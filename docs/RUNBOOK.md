# Runbook

Prerequisites: Python, Docker, Kubernetes sandbox cluster and Helm 3. The app is a legacy-style demo, not an actual proprietary legacy application.

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r app/requirements.txt
(cd app && python -m unittest discover -p 'test_*.py')
docker build -t portfolio-demo:local app
helm lint helm/legacy-app
helm template legacy helm/legacy-app
```

For a local kind cluster, load the image with `kind load docker-image portfolio-demo:local`, then `helm upgrade --install legacy helm/legacy-app`. Verify `kubectl rollout status deployment/legacy`, port-forward the service and query `/healthz`. Roll back with `helm rollback legacy 1` after checking `helm history legacy`. For remote clusters push to an authorized registry and override image repository/tag. Configure ingress and TLS separately for the environment. Teardown with `helm uninstall legacy`.
