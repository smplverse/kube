# kube

The deployment repo for all of the services in the SMPLverse cluster.

Provisioned through linode and pretty much merging all of the existing,
service-specific configs.

## Secrets that require setting

- metadata-key (the auth key for the main api server to insert metadata entries)

  ```bash
  kubectl create secret generic metadata-key --from-literal METADATA_API_KEY=[secret]
  ```

  ```bash
  kubectl create secret generic api-secrets --from-env-file .env
  ```

## Deploy to the cluster

1. Install ingress

   ```sh
   helm upgrade --install ingress-nginx ingress-nginx \
     --repo https://kubernetes.github.io/ingress-nginx \
     --namespace ingress-nginx --create-namespace
   ```

2. To get the IPv4 of the Ingress

   ```sh
   kubectl get services \
     --namespace ingress-nginx \
     -o wide \
     -w \
     ingress-nginx-controller
   ```

3. Create A-Records for all of the subdomains (`api.smplverse.xyz`,
   `metadata.smplverse.xyz`, `smplverse.xyz`) to point to the Ingress.

4. Install cert-manager

   ```sh
   kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml
   ```

5. Apply the configurations

   ```sh
   kubectl apply -f ingress.yaml -f api.yaml -f metadata.yaml -f interface.yaml
   ```

_I ended up making the api a standalone deployment since I don't have
linode-lke gpu access and it is to late before launch to mess with migrating to
gke/eks, so api can be skipped_

## License MIT
