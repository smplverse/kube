# kube

The deployment repo for all of the services in the SMPLverse cluster.

Provisioned through linode and pretty much merging all of the existing,
service-specific configs.

## Secrets that require setting

- metadata-key (the auth key for the main api server to insert metadata entries)

  ```bash
  kubectl create secret generic metadata-api-key --from-literal METADATA_API_KEY=[secret]
  ```
