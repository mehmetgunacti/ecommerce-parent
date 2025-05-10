## Run Jenkins inside Docker

Create a custom Jenkins image (Dockerfile);
```bash
# Use the official Jenkins image with JDK 21
FROM jenkins/jenkins:jdk21

# Switch to root user to install packages
USER root

# Update package lists and install xmllint
RUN apt-get update && apt-get install -y libxml2-utils && rm -rf /var/lib/apt/lists/*

# Switch back to Jenkins user
USER jenkins

# Expose Jenkins port
EXPOSE 8080

# Start Jenkins
CMD ["java", "-jar", "/usr/share/jenkins/jenkins.war"]
```

Build:
```bash
docker build -t jenkins-xmllint:jdk21 .
```

Run:
```bash
docker run -d -p 8090:8080 -v /mnt/data/ecommerce/jenkins_home:/var/jenkins_home jenkins-xmllint:jdk21
```

Access:
```bash
[http://ecommerce.local:8090](http://ecommerce.local:8090)
```