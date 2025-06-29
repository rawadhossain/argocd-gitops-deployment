# GitOps workflow
## The GitOps Flow that's been Created:
### 1. Source Code Repository [next-app-Gitops](https://github.com/rawadhossain/next-app-Gitops/blob/main/.github/workflows/docker-build.yml) --workflow runs here
* Contains your application code (nextjs code)
* Contains the GitHub Actions workflow
* When you push code to main branch → triggers the workflow

### 2. GitOps Repository (argocd-gitops-deployment)
* Contains manifests (like manifest.yml)
* Acts as the "source of truth" for your deployment configuration
* ArgoCD watches this repository for changes

## Here's What Happens Step by Step:
```
    ┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
    │   Your Code     │    │   Docker Hub     │    │   GitOps Repo   │
    │   Repository    │    │                  │    │                 │
    │                 │    │                  │    │   manifest.yml  │
    └─────────────────┘    └──────────────────┘    └─────────────────┘
             │                       │                       │
             │ 1. Push to main       │                       │
             ▼                       │                       │
    ┌─────────────────┐              │                       │
    │ GitHub Actions  │              │                       │
    │ Workflow Runs   │              │                       │
    └─────────────────┘              │                       │
             │                       │                       │
             │ 2. Build & Push       │                       │
             └──────────────────────►│                       │
                                     │                       │
             ┌───────────────────────┘                       │
             │ 3. Update manifest                            │
             └──────────────────────────────────────────────►│
                                                             │
                                                             ▼
                                                    ┌─────────────────┐
                                                    │   ArgoCD        │
                                                    │   Detects       │
                                                    │   Changes       │
                                                    └─────────────────┘
                                                             │
                                                             ▼
                                                    ┌─────────────────┐
                                                    │   Kubernetes    │
                                                    │ Cluster/docker  │
                                                    │   (Deployment)  │
                                                    └─────────────────┘
```
## What Your Workflow Does:
### Step 1: Build & Push Docker Image

* Builds your todo-app into a Docker image
* Tags it with the Git commit SHA: rawadhossain/todo-app:224d48e406594a0275cc50dc858d90fed1b2f64e
* Pushes it to Docker Hub

### Step 2: Update GitOps Repository

* Clones your argocd-gitops-deployment repository
* Updates the manifest.yml file to use the new Docker image tag
* Before: image: 100xdevs/todo-app-week-39:54dc1a156a8b5e0ce9b5e779e5ac529550fc4ce3
* After: image: rawadhossain/todo-app:224d48e406594a0275cc50dc858d90fed1b2f64e
* Commits and pushes this change back to the GitOps repo

### Step 3: ArgoCD Takes Over

* ArgoCD monitors your argocd-gitops-deployment repository
* When it sees the manifest.yml changed, it automatically:

    * Pulls the new Docker image from Docker Hub
    * Updates your Kubernetes deployment
    * Your application gets updated with the new code!

## Finally pushed to dockerhub 
![image](https://github.com/user-attachments/assets/8a1eaab0-6fdb-4f90-8c6e-ff12812aaf6e)

