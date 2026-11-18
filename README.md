# naftis

Managing Istio tasks means defining changes, executing them, and checking the affected services. naftis provides a web dashboard where you can create reusable task templates, run tasks, and inspect service and pod data.

## Features

- Create custom task templates and execute tasks built from them.
- Roll back specific tasks from the dashboard.
- View an Istio service graph with a selected service as the root node.
- Inspect diagnostic data for Istio services and pods.
- Access integrated real-time dashboards.

## Installation

naftis has a Go backend and runs on Kubernetes with a MySQL database. You need access to a Kubernetes cluster, a configured `kubectl`, and Istio.

**Check compatibility first:** the inherited implementation documents support for Istio 1.0. Compatibility with newer Istio releases is not established here.

For backend development, install Go, clone this standalone repository, and check that its Go packages build. Replace `<repository-url>` with this project's repository URL:

```bash
go version
git clone <repository-url> naftis
cd naftis
go build ./...
```

Building the Go packages does not deploy the UI or database. For a Kubernetes installation, obtain `naftis.yaml` and the appropriate MySQL manifest from this project's release bundle. Run these commands from the directory containing those files:

```bash
kubectl create namespace naftis

# Use this manifest for a bare-metal cluster.
kubectl apply -n naftis -f mysql.yaml

# For a cloud cluster, use mysql-cloud.yaml instead of mysql.yaml.
# kubectl apply -n naftis -f mysql-cloud.yaml

kubectl apply -n naftis -f naftis.yaml
```

Review the manifests before applying them, especially image references, storage settings, and credentials.

## Usage

Check the deployed pods, then forward the dashboard to your machine:

```bash
kubectl get pod -n naftis
kubectl -n naftis port-forward $(kubectl -n naftis get pod -l app=naftis-ui -o jsonpath='{.items[0].metadata.name}') 8080:80
```

Open [http://localhost:8080/](http://localhost:8080/). The inherited deployment uses `admin` for both the username and password. **Do not expose the dashboard with these default credentials.**

In the dashboard, create a task template, create a task from that template, and execute it. Inspect the affected services and pods through the service views. Rollback is available for specific tasks; do not assume every change can be reversed.

## License

Licensed under Apache-2.0. This project is based on [XiaoMi/naftis](https://github.com/XiaoMi/naftis). Preserve the upstream copyright and license notices, and retain any applicable NOTICE file when redistributing the code.
