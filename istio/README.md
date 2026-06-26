# istioctl

- Endpoints — see all pods Istio knows about for a service: `istioctl proxy-config endpoints <pod.namespace> | grep <service>`
- Clusters — verify DestinationRule settings are applied: `istioctl proxy-config cluster <pod.namespace> --fqdn <service-fqdn> -o json`
- Listeners — see what ports/protocols the proxy is listening on: `istioctl proxy-config listener <pod.namespace>`
- Routes — see routing rules applied: `istioctl proxy-config route <pod.namespace>`
- Check for config issues across the mesh: `istioctl analyze -n <namespace>`
- Verify DestinationRule/VirtualService is applied to a specific pod: `istioctl experimental describe pod <pod.namespace>`
- Check proxy sync status — is Envoy in sync with istiod: `istioctl proxy-status`
- Full Envoy config dump for a pod: `istioctl proxy-config all <pod.namespace> -o json`

