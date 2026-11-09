# Task 2: Simple Jenkins Pipeline for CI/CD

## What I did
- Built a tiny Python app (`app.py`) with a pytest test (`test_app.py`).
- Containerized it with Docker (`Dockerfile`).
- Wrote a declarative `Jenkinsfile` with **Build -> Test -> Deploy** stages.
- Ran Jenkins in Docker on WSL, with the Docker socket mounted so pipelines can run Docker commands.
- Created a Pipeline job ("Pipeline script from SCM") pointing to this repo, triggered by **Poll SCM** (`* * * * *`).

## How to run
```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -u root jenkins/jenkins:lts
docker exec jenkins sh -c "apt-get update && apt-get install -y docker.io"
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```
Open http://localhost:8080, create a Pipeline job from this repo, and click **Build Now**.
Check the deployed app: `docker logs myapp`

## Screenshots
See the `screenshots/` folder.

