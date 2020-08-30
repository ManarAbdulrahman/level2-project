FROM maven

COPY . /role-service

WORKDIR /role-service

RUN mvn clean package && ls target

cmd ["java", "-jar", "target/role-service-1.0.0.jar"]

EXPOSE 80
