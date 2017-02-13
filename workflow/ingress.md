# Workflow + Kubernetes Native Ingress

### Feature Flag

##### Setting within the Deis controller chart.

```yaml
# controller/values.yaml

# ...
global:
# ...
  experimental_native_ingress: true
```

### Experimental Native Ingress (ENABLED)

 - Will **NOT** deploy the traditional Deis router
 - Will **REQUIRE** a hostname set 
 - Will create Kubernetes ingress for existing Deis services
  - Controller
  - Grafana
  - More?
 - Will have **NO** concept of verifying an ingress controller is in place

##### Example deployment with nginx ingress

```bash

```
 
### Experimental Native Ingress (DISABLED)

- No changes. Workflow should behave exactly as it always has.

