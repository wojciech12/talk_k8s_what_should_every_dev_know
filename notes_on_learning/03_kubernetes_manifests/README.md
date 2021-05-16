# Learning

Start with:

0. Deployment and service
1. Simple templating of k8s to inject version of the docker, configmap and secret created before
2. Helm with minimum templating
3. Secret management or deployment strategies
4. ArgoCD, more helm? Choose your journey!

## Steps in Learning

1. v0.1 - deployment and service.

   ```bash
   $ kubectl apply -f deployment_v0.1
   ```

   We can start learning Kubernetes with deployment and service.

2. v0.2 - assume that secrets and configmaps are in the cluster, you can use a simple templating to inject your version:

   ```yaml
   envFrom:
   - configMapRef:
       name: configmap-my-app
   - secretRef:
       name: secrets-my-app
   ```

   Notice, to KISS just 'my-app' would be much better name.

   ```bash
   # somebody before
   $ kubect create configmap my-app --from-literal=PSQL_USER=user
   $ kubect create secret generic my-app --from-literal=PSQL_PASSWORD=XYZ

   # our CI/CD
   $ kubectl apply -f deployment_v0.2
   ```

3. v1 - minimal helm, just template what you need. Still assume secrets are in the k8s, if possible move configMaps to the component repo.

   ```bash
   $ export APP_NAME=my-app
   $ export APP_VERSION=snapshot-9911
  
   $ helm template "${APP_NAME}" deployment_v2 \
      --set image.tag="${APP_VERSION}" \
      --version "${APP_VERSION}"

   # in CI/CD, dry run for safety :)
   $ helm template "${APP_NAME}" deployment_v2 \
      --set image.tag="${APP_VERSION}" \
      --version "${APP_VERSION}" | kubectl apply -f - --dry-run=server


   # in CI/CD, again --dry-run for safety
   $ helm install "${APP_NAME}" deployment_v2 \
      --set image.tag="${APP_VERSION}" \
      --version "${APP_VERSION}" --dry-run --debug
   ```

   Lean towards:

   1. Not templating if it is not going to change
   2. Push as many setting as possible to the component repo

4. v2? Secret management... deployment strategies.

5. v3? Choose your journey: argoCD? more helm? 

## Demo

```bash
$ brew install k3d
```

```bash
$ k3d cluster create talk-demo-cluster
$ k3d kubeconfig merge talk-demo-cluster --kubeconfig-switch-context
```
