# jenkins-playground

## Installation

### create docker network
```bash
docker network create jenkins

# verify
docker network ls
```

### Run a docker:dind container
```bash
docker run \
  --name docker-in-docker \
  --detach \
  --restart always \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2
```

#### Test the docker:dind container
```bash
docker exec -it docker-in-docker /bin/sh
/ # 
/ # docker run -it python sh
# 
# python
Python 3.12.1 (main, Dec 19 2023, 20:14:15) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
# exit
/ # 
/ # exit
```

### Running a Jenkins controller (and agent) container
Option 1
```bash
FROM jenkins/jenkins:2.479.3-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
```
```bash
docker build . -t jenkins_with_docker:one
```
```bash
docker run \
  --name jenkins_docker \
  --restart always \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  jenkins_with_docker:one
```

Option 2
```bash
FROM jenkins/jenkins:2.479.3-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"
```
```bash
docker build -t myjenkins-blueocean:2.479.3-1 .
```
```bash
docker run \
  --name jenkins-blueocean \
  --restart=on-failure \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.479.3-1 
```


## Credentials
To make pipelines available and faster, suggested to use credentials.

To create a new credentials follow the following instructions:

### GitHub PAT (Personal Access tokens)
1. Go to GitHub
- Log in to your GitHub account. 
- Click on your profile picture (top right) → Settings. 

2. Navigate to Developer Settings
- In the left sidebar, click Developer settings.
- Select Personal access tokens → Tokens (classic).
- Click Generate new token → Generate new token (classic).

3. Configure the Token
- Token Name: Give it a descriptive name like Jenkins Access Token.
- Expiration: Choose an expiration date (recommended: 90 days).
- Select Scopes (Permissions):
  - ✅ repo — Full control of private repositories (if needed).
  - ✅ workflow — If you’re triggering GitHub Actions.
  - ✅ admin:repo_hook — For managing webhooks (optional).
  - ✅ read:org — If accessing organizational data.

4. Generate the Token
- Click Generate Token.
- Copy the token immediately—you won’t be able to view it again.


### Add the PAT to Jenkins as Credentials
1. Open Jenkins Dashboard
- Go to http://localhost:8080.
- Click Manage Jenkins → Manage Credentials → (global) → Add Credentials.

2. Add the GitHub Token
- Kind: Select Username with password.
- Username: Your GitHub username.
- Password: Paste the Personal Access Token here.
- ID: (Optional) Set an ID like github-pat for easy reference.
- Click OK to save.


