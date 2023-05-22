FROM openjdk:11
ARG JAR_FILE=build/libs/samplecontroller-1.0.jar
COPY ${JAR_FILE} app.jar
ENV STORE_ENABLED=true
ENV WORKER_ENABLED=true
ENTRYPOINT ["java","-jar","/app.jar"]