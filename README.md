
# How to Containerize a Spring Boot Application

If you installed Docker Desktop, make sure it’s running. You need to have Docker Engine installed and running locally (you should be able to run docker -v and get a Docker version).




## Dockerfile

```
FROM openjdk:12
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Docker uses the .jar file, so you need to tell Gradle to build it.
```bash
./gradlew buildCode
```
Build the Docker image. Here I’m applying the tag splitdemo/spring-boot-docker to the image.
```bash
docker build --build-arg JAR_FILE="build/libs/*.jar" -t com.example/spring-boot-docker .
```
Alternative, just so you know, you could both build the project and the Docker image simultaneously using Gradle (you don’t need to run the following command, it’s just an alternative to the previous two).
```bash
./gradlew bootBuildImage --imageName=com.example/spring-boot-docker
```

Run the Docker image.
```bash
docker run -p 8080:8080 com.example/spring-boot-docker
```

If you want to push your image on the docker hub, just run the command
```bash
docker push com.example/spring-boot-docker:tagname
```

If the tagname is not informed, dockerhub interprets it as latest