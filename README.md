# Powered By OneKloud 

### Project Setup

- Enable Github Actions On This Repository
- Create The Following Repository Secrets
  - DOCKERHUB_USERNAME : DockerHub Username
  - DOCKERHUB_TOKEN : DockerHub Token
  - GH_PAT : Github User PAT
- Clone this repository

### Environments
This project supports the following 3 environments
- developement
- staging
- production
The development environment either points to a docker image tag of dev-short_sha or semantic version tag. Which every is most recent.
The staging and production environments always point to the most recent tagged release. Docker images tagged latest always point to
the last tagged release.

### Conventional Commits
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) are encouraged as they provide an easy set of rules
for creating an explicit commit history which can then be used by automated tools to generate a detailed changelog for this
project. The commit message should be structured as follows:
- <type>[optional scope]: <description>
- [optional body]
- [optional footer(s)]

Here's some example pr titles:
- BREAKING CHANGE: (Major version bump) vX.Y.Y
- feat: (Minor version bump) vY.X.Y
- fix: (Patch version bump) vY.Y.X

### Semantic Versioning
[SemVer](https://semver.org/)
vMajor.Minor.Patch
This project uses Semantic Versioning to denote version number and begins with an intial version of v0.0.0.
The semantic version of the project can be incremented by creating a tag and pushing it main.
- Get most recent tag: git tag --sort=committerdate | tail -1
- Create tag: git tag v0.1.0
- Push tag: git push origin v0.1.0

### Build Test Release-Cycle:
Background: This project comes with Github Actions implementing the following release cycle in the following order:
- pull-request
- merge-to-main
- pre-release
- deploy-to-dev
- tagged-release
- deploy-to-staging
- deploy-to-prod

#### pull-request
Runs on every commit to a pull request on the main branch. 
- Executes unit and other tests.

#### pre-release
Runs on every merge to main.
- Executes unit and other tests.
- Builds Docker image
- Tags Image with short sha from this commit
- Pushes docker image to dockerhub
- Creates a pre-release tagged latest
- Deploys docker image to dev by updating dev helm repo associated with this project

#### tagged-release
Runs on every tag push to main with the pattern v*
- Executes unit and other tests.
- Builds Docker image
- Tags Image with pushed tag
- Pushes docker image to dockerhub with this tag and latest
- Deploys docker tagged image to dev by updating dev helm repo associated with this project

#### deploy to staging / production
Runs via workflow dispatch after selecting target environment
- Deploys docker tagged image to env by updating env helm repo associated with this project

### Minor Version Bump Example
Assuming starting version tag v0.0.0
- git checkout -b feature/new_endpoint
- git commit -m "feat: New Endpoint"
- git push origin feature/new_endpoint
- Create pull request
- Pull request runs unit tests on this and subsequent commits

### Pre-release and deloy to dev environment
pre-release workflow runs on merges to main.
- Executes unit and other tests.
- Builds Docker image
- Tags Image with short sha from this commit
- Pushes docker image to dockerhub
- Creates a pre-release tagged latest
- Deploys docker image to dev by updating dev helm repo associated with this project
- If helm repo is watched by [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) app is updated on k8s cluster

### Release and deploy to dev environment
Runs on every tag push to main with the pattern v*
- git tag v0.1.0
- git push origin v0.1.0
- Executes unit and other tests.
- Builds Docker image
- Tags Image with pushed tag
- Pushes docker image to dockerhub with this tag and latest
- Deploys docker tagged image to dev by updating dev helm repo associated with this project
- If helm repo is watched by [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) app is updated on k8s cluster

### Deploy to staging / production
Runs via workflow dispatch after selecting target environment
- Deploys docker tagged image to env by updating env helm repo associated with this project
