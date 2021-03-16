# Containerise a Maven Project

### Step 1: Create a sample Maven Project

### Step 2: Add this build feature to pom.xml
````
<build>  
    <plugins>  
        <plugin>  
            <!-- Build an executable JAR -->  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-jar-plugin</artifactId>  
            <version>3.1.0</version>  
            <configuration>  
                <archive>  
                    <manifest>  
                        <mainClass>com.sample.HelloWorld.App</mainClass>  
                    </manifest>  
                </archive>  
            </configuration>  
        </plugin>  
    </plugins>  
</build>
````
 
###### Note: This is done so that it could find the main class

### Step 3: Create a Docker File(dockerfile)
````
FROM maven:3.5.4-jdk-8-alpine as maven

COPY ./pom.xml ./pom.xml

COPY ./src ./src

RUN mvn dependency:go-offline -B

RUN mvn package

FROM openjdk:8u171-jre-alpine

WORKDIR /adevguide

COPY --from=maven target/*.jar ./HelloWorld.jar

CMD ["java", "-jar", "./HelloWorld.jar"]
````

### Step 4: Build the docker file

````
docker build -t mavenproject007 .
````

### Step 5: Run the docker file by mounting the data outside the container
````
docker run -v mavenVolume:/adevguide/ -it mavenproject007
````