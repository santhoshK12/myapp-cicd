End-to-End CI/CD Pipeline with GitHub Actions + Kubernetes (Minikube)

Project Overview



This project demonstrates a complete CI/CD pipeline for a containerized Python Flask application deployed to Kubernetes using Minikube.



The pipeline is built using:



GitHub Actions (CI + CD)



Self-hosted runner (Windows)



Docker



Kubernetes (Minikube)



Rolling updates



Smoke testing



Automatic rollback on failure



This setup mirrors modern DevOps practices used in real-world production environments.



Architecture Overview



Developer Push →

GitHub Actions CI →

Build Docker Image →

CD Triggered via workflow\_run →

Deploy to Kubernetes →

Rolling Update →

Smoke Test →

Rollback if failure



CI Workflow (Continuous Integration)



Triggered on:



Push to main



Pull Requests



CI performs:



Checkout source code



Install Python dependencies



Run unit tests (pytest)



Build Docker image



(Optional) Push image to registry



CI ensures:



Code correctness



Test validation



Build integrity



Artifact reproducibility



Artifact Strategy:



Docker images are tagged using commit SHA



Ensures immutability



Prevents environment drift



CD Workflow (Continuous Deployment)



Triggered only after:



CI workflow completes successfully



Using workflow\_run



CD runs on:



Self-hosted GitHub runner (Windows)



Directly connected to Minikube cluster



CD performs:



Build Docker image using CI-tested commit SHA



Load image into Minikube



Apply Kubernetes manifests



Update deployment image



Perform rolling update



Wait for rollout completion



Execute smoke test (/health endpoint)



Auto-rollback if deployment fails



☸ Kubernetes Configuration



Deployment:



2 replicas



Rolling update strategy



Readiness and liveness probes



Service:



NodePort (for Minikube access)



Health endpoint:



/health



Ensures application availability before marking deployment successful.



Smoke Testing Strategy



Instead of relying on Minikube service tunnel, the pipeline:



Uses kubectl port-forward



Validates /health endpoint



Retries up to 20 times



Fails deployment if health check fails



This ensures zero false positives in CI/CD validation.



Rollback Mechanism



If rollout or smoke test fails:



kubectl rollout undo deployment/myapp



Rollback ensures:



High availability



Reduced downtime



Safer deployments



Concurrency Control



To prevent multiple deployments running simultaneously:



concurrency:

&nbsp; group: cd-minikube

&nbsp; cancel-in-progress: true



This ensures:



Only one deployment per branch at a time



Old runs auto-cancelled



Production-Grade Practices Implemented



Build once, deploy same artifact



SHA-based image tagging



CI → CD chaining



Automated health validation



Deployment rollback



Concurrency protection



Self-hosted runner integration



Infrastructure reproducibility



How to Run Locally



Start Minikube:



minikube start



Build image:



docker build -t myapp:local .

minikube image load myapp:local



Deploy:



kubectl apply -f k8s/

kubectl rollout status deployment/myapp



Access:



kubectl port-forward svc/myapp-svc 5000:80



Open:



http://localhost:5000

Key Learning Outcomes



This project demonstrates:



Real-world CI/CD pipeline structure



Event-driven automation



Immutable artifact strategy



Kubernetes rolling updates



Deployment validation patterns



Failure handling with rollback



Windows self-hosted runner integration



Future Enhancements



Push image to GHCR or ECR



Deploy to cloud Kubernetes (EKS)



Helm-based deployments



Security scanning (Trivy)



Environment-based deployments (dev/stage/prod)



Infrastructure provisioning via Terraform



Author

SANTHOSH REDDY KISTIPATI

DevOps Engineer Portfolio Project

Built to demonstrate production-ready CI/CD implementation.

