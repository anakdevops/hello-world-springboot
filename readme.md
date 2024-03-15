java 17

```
mvnw clean
mvnw package
java -jar target/spring-boot-hello-world-devops-0.0.1-SNAPSHOT.jar
```

```
docker build -t anakdevops .
docker run -d --name spring -p 82:8080 anakdevops
curl localhost:82
```
