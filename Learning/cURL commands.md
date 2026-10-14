```
metatron curl -a cindercloudservicecontrolplane -X POST "https://hybriddev-yujiax.cindercloudservicecontrolplane.test.netflix.net:7004/REST/gradle-task-errors" -d '{
      "runtimeName": "referenceApp",
      "pluginVersion": "1.0.0",
      "taskName": "edprGenerate",
      "errorMessage": "Failed to generate code",
      "stackTrace": "java.lang.RuntimeException: Failed to generate code\n\tat com.netflix.edpr.Example.run(Example.java:42)",
      "errorType": "RuntimeException",
      "buildEnvironment": "CI",
      "gradleVersion": "8.5",
      "jdkVersion": "21.0.1",
      "metadata": {
        "gitBranch": "main",
        "gitCommit": "abc123"
      }
    }' -H 'Content-Type: application/json'
```