## Run Jenkins inside Docker

```bash
docker run -d -p 8090:8080 -v $HOME/dev/ecommerce/jenkins_home:/var/jenkins_home jenkins/jenkins:jdk21
```

Then : `http://localhost:8090`