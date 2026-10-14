How Hollow read LONG; Hollow has compression logic that allocates number of bits based on the longest LONG in the dataset, so it does not have to allocate 64 bits everytime. This is done by the `ZigZag.encodeLong` and `ZigZag.decodeLong` here
This is in **HollowObjectTypeReadState.class**
```java
@Override  
public long readLong(int ordinal, int fieldIndex) {  
    sampler.recordFieldAccess(fieldIndex);  
  
    HollowObjectTypeShardsHolder shardsHolder;  
    HollowObjectTypeReadStateShard shard;  
    long value;  
  
    do {  
        shardsHolder = this.shardsVolatile;  
        shard = shardsHolder.shards[ordinal & shardsHolder.shardNumberMask];  
        value = shard.readLong(ordinal >> shard.shardOrdinalShift, fieldIndex);  
    } while(readWasUnsafe(shardsHolder, ordinal, shard));  
  
    if(value == shard.dataElements.nullValueForField[fieldIndex])  
        return Long.MIN_VALUE;  
    return ZigZag.decodeLong(value);  
}
```

And in the WriteEngine: **HollowObjectWriteRecord.class**
```java
public void setLong(String fieldName, long value) {  
    if(value == Long.MIN_VALUE) {  
        setNull(fieldName);  
    } else {  
        int fieldIndex = getSchema().getPosition(fieldName);  
        FieldType fieldType = getSchema().getFieldType(fieldIndex);  
  
        if(fieldType == FieldType.LONG) {  
            ByteDataArray buf = getFieldBuffer(fieldIndex);  
            // zig zag encoding  
            VarInt.writeVLong(buf, ZigZag.encodeLong(value));  
        } else if(fieldType == FieldType.UUID_LONG) {  
            ByteDataArray buf = getFieldBuffer(fieldIndex);  
            // fixed-length encoding for UUID components  
            writeFixedLengthLong(buf, value);  
        } else {  
            throw new IllegalArgumentException("Attempting to serialize LONG in field " + fieldName + ".  Carefully check your schema for type " + getSchema().getName() + ".");  
        }  
    }  
}
```

And writeFixedLengthLong will encode into something totally different than the `ZigZag.encodeLong` 


  The Issue

  When you write a UUID_LONG field:
  1. You write the raw long value using writeFixedLengthLong(buf, value) - this writes the value as-is in big-endian format (8 bytes)
  2. The value is stored directly: 0x90b9d6e9fd2e4941 for example

  When you read it back:
  1. The readLong() method retrieves the raw value: 0x90b9d6e9fd2e4941
  2. Then it applies zig-zag decoding: ZigZag.decodeLong(value)
  3. Zig-zag decoding transforms the bits, giving you a completely different number!

  Zig-Zag Encoding Explained

  Zig-zag encoding is used for variable-length compression of signed integers. It maps:
  - Positive values: 0, 1, 2, 3... → 0, 2, 4, 6...
  - Negative values: -1, -2, -3... → 1, 3, 5...

  The formula:
  - Encode: (n << 1) ^ (n >> 63)
  - Decode: (n >>> 1) ^ -(n & 1)

  So if you zig-zag decode a value that was never zig-zag encoded, you get garbage!

  For example:
  Original value:  0x90b9d6e9fd2e4941
  After ZigZag decode: 0xb7a3148b0168db5f (completely different!)

