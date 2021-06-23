REF: https://www.jenkins.io/doc/book/installing/docker/

```yml
version: "2.4"
services:
  blueocean:
    container_name: blueocean
    build:
      context: ./jenkins
    environment:
      - DOCKER_HOST=tcp://docker:2376
      - DOCKER_CERT_PATH=/certs/client
      - DOCKER_TLS_VERIFY=1
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - type: volume
        source: dind-certs
        target: /certs/client
      - type: volume
        source: data
        target: /var/jenkins_home
    networks:
      jenkins:
  dind:
    container_name: dind
    image: docker:dind
    privileged: true
    environment:
       - DOCKER_TLS_CERTDIR=/certs
    ports:
      - "2376:2376"
    volumes:
      - type: volume
        source: dind-certs
        target: /certs/client
      - type: volume
        source: data
        target: /var/jenkins_home
    networks:
      jenkins:
        aliases:
          - docker
volumes:
  dind-certs:
  data:
networks:
  jenkins:
```
