# Powered By OneKloud 

### Project Setup

- Enable Github Actions On This Repository
- Create The Following Repository Secrets
  - DOCKERHUB_USERNAME : DockerHub Username
  - DOCKERHUB_TOKEN : DockerHub Token
  - GH_PAT : Github User PAT
- Clone this repository

### Semantic Versioning
[SemVer](https://semver.org/)
vMajor.Minor.Patch
This project uses Semantic Versioning to denote version number
and begins with an intial version of v0.0.0
The semantic version of the project can be incremented by using the following 
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) in the title of your pr
- BREAKING CHANGE: (Major version bump)
- feat: (Minor version bump)
- (Patch version bump automatically on all other pr titles.)

### Release-Cycle: Pull Request --> Merge to main --> Pre-Release --> Deploy To Dev Env --> Release --> Deploy to Dev Env
Background: This project comes with the following Github Actions
- pull-request
- ci
- release
pull-request: Runs on every commit to a pull request on the main branch. Executes unit and other tests.
ci: Runs on every push to main. In addition to above builds and pushes container image to dockerhub, creates a pre-release, and updates helm manifest for deployment containing the pre-release tags
release: runs on creation of a release starting with tag v*

### Minor Version Bump Example
Assuming starting version tag v0.0.0
- git checkout -b feature/new_endpoint
- git commit -m "feat: New Endpoint"
- git push origin feature/new_endpoint
- Create pull request
- Pull request runs unit tests on this and subsequent commits

### Pre-release and deloy to dev environment
CI workflow runs on merges to main.
- merge pr to main
- Re-runs unit tests
- bumps semantic version number to v0.1.0-prerelease(number)
- builds docker image
- tags docker image as v0.1.0-prerelease(number)
- pushes docker image to dockerhub
- creates a pre-release in this project with same tag v0.1.0-prerelease(number)
- updates related helm repository Chart.yaml/values.yaml with tag v0.1.0-prerelease(number)
- If helm repo is watched by [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) app is updated on k8s cluster

### Release and deploy to dev environment
Release workflow runs on creation of a release in Github UI
- Select actions tab
- From the list of workflows select ci
- Select most recent successfull workflow run
- Select docker-build-push
- Expand Semantic Versioning task
- Select url to create a new relese
- Update release title to match tag name
- Include release description
- Publish release
- Release workflow activates
- builds and tags docker image with tag v0.1.0
- Pushes Image to docker hub as imageName:v0.1.0
- Creates a v0.1.0 release in this project
- Updates releated helm repository Chart.yaml/values.yaml with tag v0.1.0
- If helm repo is watched by [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) app is updated on k8s cluster
- enjoy!


