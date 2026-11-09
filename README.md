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

## Interview Questions
1. **What is Jenkins and how is it used in CI/CD?** An open-source automation server. It automatically builds, tests and deploys code whenever changes are pushed, giving fast feedback and repeatable releases.
2. **What is a Jenkinsfile?** A text file stored in the repo that defines the pipeline as code (stages and steps), so it is versioned with the application.
3. **How do you create and configure pipelines?** New Item -> Pipeline -> choose "Pipeline script from SCM" -> add repo URL and branch -> set a build trigger (webhook or Poll SCM) -> save and build.
4. **Common stages?** Checkout, Build, Test, Code analysis, Package/Docker build, Push artifact, Deploy, Notify.
5. **Declarative vs scripted?** Declarative uses a structured `pipeline { }` syntax that is simpler and has built-in validation. Scripted is Groovy-based (`node { }`), more flexible and powerful but harder to read and maintain.
