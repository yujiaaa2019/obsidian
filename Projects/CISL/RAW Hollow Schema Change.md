1. can only delete top-level type
	1. to delete other type, they will always be referenced by some other types, which will throw error (validation error: who is referening this)
2. Can delete a field inside a type
	1. that field cannot be referenced or indexed (otherwise will throw error)
`RawHollowSchemaClient`
