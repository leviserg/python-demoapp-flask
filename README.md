# Python Flask - Demo Web Application

This is a simple Python Flask web application. The app provides system information and a realtime monitoring screen with dials showing CPU, memory, IO and process information.

The app has been designed with cloud native demos & containers in mind, in order to provide a real working application for deployment, something more than "hello-world" but with the minimum of pre-reqs. It is not intended as a complete example of a fully functioning architecture or complex software design.

Typical uses would be deployment to Kubernetes, demos of Docker, CI/CD (build pipelines are provided), deployment to cloud (Azure) monitoring, auto-scaling


## Building & Running Locally
Install Scoop to make Make available on your Windows11 PC {https://get.scoop.sh}
Run from Power Shell (no admin rights required):
  - Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  - Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
Close terminal and run Git Bash:
	- scoop bucket add main (probably could be added)
	- scoop install main/make
	- scoop install main/gcc
Go to your app root directory where "makefile" is stored. Run Git Bash commands:
```
make venv
make test
make run
```
### Pre-reqs

- Be using Linux, WSL or MacOS, with bash, make etc
- [Python 3.9+](https://www.python.org/downloads/) - for running locally, linting, running tests etc - with PATH set in Environment variables
- [Docker](https://docs.docker.com/get-docker/) - for running as a container, or image build and push
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux) - for deployment to Azure

Clone the project to any directory where you do development work

```
git clone https://github.com/leviserg/python-demoapp-flask.git
```

### Makefile

A standard GNU Make file is provided to help with running and building locally.

```text
help                 💬 This help message
lint                 🔎 Lint & format, will not fix but sets exit code on error
lint-fix             📜 Lint & format, will try to fix errors and modify code
image                🔨 Build container image from Dockerfile
push                 📤 Push container image to registry
run                  🏃 Run the server locally using Python & Flask
deploy               🚀 Deploy to Azure Web App
undeploy             💀 Remove from Azure
test                 🎯 Unit tests for Flask app
test-report          🎯 Unit tests for Flask app (with report output)
test-api             🚦 Run integration API tests, server must be running
clean                🧹 Clean up project
```

Make file variables and default values, pass these in when calling `make`, e.g. `make image IMAGE_REPO=blah/foo`

| Makefile Variable | Default                 |
| ----------------- | ----------------------- |
| IMAGE_REG         | ghcr<span>.</span>io    |
| IMAGE_REPO        | leviserg/python-demoapp |
| IMAGE_TAG         | latest                  |
| AZURE_RES_GROUP   | temp-demoapps           |
| AZURE_REGION      | uksouth                 |
| AZURE_SITE_NAME   | pythonapp-{git-sha}     |

The app runs under Flask and listens on port 5000 by default, this can be changed with the `PORT` environmental variable.

# Containers
Should you want to build your own container, use `make image` and the above variables to customise the name & tag.

## Kubernetes

The app can easily be deployed to Kubernetes using Helm, see [deploy/kubernetes/readme.md](deploy/kubernetes/readme.md) for details

# GitHub Actions CI/CD

A working set of CI and CD release GitHub Actions workflows are provided `.github/workflows/`, automated builds are run in GitHub hosted runners

## Running in Azure App Service (Linux)

If you want to deploy to an Azure Web App as a container (aka Linux Web App), a Bicep template is provided in the [deploy](deploy/) directory

For a super quick deployment, use `make deploy` which will deploy to a resource group, temp-demoapps and use the git ref to create a unique site name

```bash
make deploy
```

## Running in Azure App Service (Windows)

Just don't, it's awful
