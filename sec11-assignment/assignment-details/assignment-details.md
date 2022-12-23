## Mongo:

- Listens on port: `27017`
- We need a persistent storage
- Refer to below ConfigMap format to inject multiple files

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: [ name ]
data:
  file1.txt: |
    line 1
    line 2
    line 3
  file2.txt: |
    line 1
    line 2
```

## Job Service

- It is a Spring boot Microservice
- Listens on port `8080`
- health endpoint: `/health`
- Both startup and readiness probe can use same endpoint for this assignment
- Min required replica: 1
- CPU / Memory Usage
```
CPU => min: 100m , max: 2000m
Memory => min: 100Mi, Max: 2000Mi 
```
- Endpoints to test if you are curious
```
http://localhost/job/all
http://localhost/job/search?skills=java
```

## Candidate Service
- It is a Spring boot Microservice
- Listens on port `8080`
- health endpoint: `/health`
- Both startup and readiness probe can use same endpoint for this assignment
- Min required replica: 1
- CPU / Memory Usage
```
CPU => min: 100m , max: 2000m
Memory => min: 100Mi, Max: 2000Mi 
```
- Endpoints to test if you are curious
```
http://localhost/candidate/all
http://localhost/candidate/1
```

## Frontend Service
- It contains static content
- Listens on port `80`
- health endpoint: `/`
- It depends on Job and Candidate services
- When the home page `index.html` is loaded, it makes a call to Job-service to fetch all the jobs available.
```
http://localhost/job/all
```
- When we click on the `Candidates` tab, it makes a call to Candidate-service to fetch all candidates
```
http://localhost/candidate/all
```

## HPA
- Both Job and Candidates services are expected to auto scale based on CPU utilization
  ```
  Avg CPU Utilization : 30%
  Max Replicas: 2
  Scale Down Behavior Window: 300 seconds
  ```

## Routing Rules
  - `/` should load home page
  - `/candidate` should go to candidate service
  - `/job` should go to job-service