# Testing Locally

The following aadsyncClient must be uncommented

cmd/aadsync-controller/aadsync-controller.go
38: aadsyncClient := aadsyncclient.NewClientForLocal(controllerConfig.Namespace, log)

Need controller config file location set via AADSYNC_CONTROLLER_CONFIGFILE environment variable:

```yaml
#
# AzureAD Sync Controller Config
#
namespace: "openshift"
groups:
- "464e7cdd-b431-4e49-9aa7-8c6ef24c9dbc"
- "ca65a5de-3ca5-474a-8fc5-bee95dd3e335"
```

Need the following environment variables set for MS Graph API:
- AZURE_TENANT_ID
- AZURE_CLIENT_ID
- AZURE_CLIENT_SECRET

This will test retrieving a Azure AD token for accessing the MS Graph API

```bash
curl -d "client_id=$AZURE_CLIENT_ID&scope=https://graph.microsoft.com/.default&client_secret=$AZURE_CLIENT_SECRET&grant_type=client_credentials" -H "Content-Type: application/x-www-form-urlencoded" -X POST https://login.microsoftonline.com/$AZURE_TENANT_ID/oauth2/v2.0/token
```

Need the following environment variables set for Kubernetes API:
- KUBERNETES_SERVICE_HOST
- KUBERNETES_SERVICE_PORT
- KUBERNETES_SERVICEACCOUNT_TOKENFILE (Found incluster at /var/run/secrets/kubernetes.io/serviceaccount/token)
- KUBERNETES_SERVICEACCOUNT_ROOTCAFILE (Found incluster at /var/run/secrets/kubernetes.io/serviceaccount/ca.crt)

# Testing in Kubernetes

The following aadsyncClient must be uncommented

cmd/aadsync-controller/aadsync-controller.go
39: aadsyncClient := aadsyncclient.NewClient(controllerConfig.Namespace, log)

Need controller config at /etc/aadsynccontroller/config.yaml

```yaml
#
# AzureAD Sync Controller Config
#
namespace: "openshift"
groups:
- "464e7cdd-b431-4e49-9aa7-8c6ef24c9dbc"
- "ca65a5de-3ca5-474a-8fc5-bee95dd3e335"
```

Need the following environment variables set for MS Graph API:
- AZURE_TENANT_ID
- AZURE_CLIENT_ID
- AZURE_CLIENT_SECRET

Run aadsync-controller binary with loglevel flag

```bash
aadsync-controller --loglevel debug
aadsync-controller --loglevel info
aadsync-controller --loglevel error
```

Debug prints out sensitive details for debugging ...


