# ICP Certificate Policy Controller
## Description
A controller that watches certificates created and/or used within ICP/MCM to ensure they don't expire within a given amount of time. The controller shows whether or not a given `CertPolicy` is compliant.

## Usage
The controller can be run as a stand-alone program within IBM Cloud Private. Its intended usage is to be integrated with Multi-cloud Manager.

`CertPolicy` is the custom resource definition created by this controller. It watches specific namespaces and shows whether or not those namespaces and the policy as a whole is compliant.

The controller watches for `CertPolicy` objects in Kubernetes. This is an example spec of a `CertPolicy` object:

```yaml
apiVersion: mcm-grcpolicy.ibm.com
kind: CertPolicy
metadata:
  name: certificate-policy-1
  namespace: kube-system
  label:
    category: "System-Integrity"
spec:
  # include are the namespaces you want to watch certificates in, while exclude are the namespaces you explicitly do not want to watch
  namespaceSelector:
    include: ["default", "kube-*"]
    exclude: ["kube-system"]
  # Can be enforce or inform, however enforce doesn't do anything with regards to this controller
  remediationAction: inform
  # minimum duration is the least amount of time the certificate is still valid before it is considered non-compliant
  minimumDuration: 100h
```
