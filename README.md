## Flux Deployments
Flux instances are configured to monitor their cluster's corresponding environment branch and cluster directory.

For example, the Flux instance running in the dev EKS cluster will monitor and deploy resources from `./clusters/dev/` in the `dev` branch only. Kustomizations in this directory will specify which workloads will be deployed to the cluster.

### Adding a new service

1. Add base manifests and kustomization.yml to `./workloads/applications/base/{service-name}/`
2. Add patches to `./workloads/applications/{environment}/patches/`
3. Add resource and patch entries to `./workloads/applications/{environment}/kustomization.yaml`

## Flux Troubleshooting
### CRDs
Check `kustomizations` in `flux-system`
```
❯ kubectl get kustomization -n flux-system
NAME          READY   STATUS                                                           AGE
cluster-tools        True    Applied revision: dev/ccf6f7d653d359ce11f7cdf8023a242658524da3   13d
flux-system   True    Applied revision: dev/ccf6f7d653d359ce11f7cdf8023a242658524da3   13d
applications      True    Applied revision: dev/ccf6f7d653d359ce11f7cdf8023a242658524da3   124m
```

Check `helmreleases`
```
❯ kubectl get helmreleases --all-namespaces
NAMESPACE     NAME                           READY   STATUS                             AGE
kube-system   aws-load-balancer-controller   True    Release reconciliation succeeded   13d
kube-system   external-dns                   True    Release reconciliation succeeded   13d
kube-system   kubernetes-external-secrets    True    Release reconciliation succeeded   13d
```

Check `helmrepo`
```
kubectl get helmrepo --all-namespaces
```

### Pod Logs
There are several deployments in the `flux-system` namespace that may have useful logs
```
❯ kubectl get deploy -n flux-system
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
helm-controller           1/1     1            1           13d
kustomize-controller      1/1     1            1           13d
notification-controller   1/1     1            1           13d
source-controller         1/1     1            1           13d
```

## Docs
- [https://github.com/fluxcd/flux2-kustomize-helm-example](https://github.com/fluxcd/flux2-kustomize-helm-example)
- [Kustomize](https://kubectl.docs.kubernetes.io/guides/config_management/)
- [Helm Release](https://fluxcd.io/docs/guides/helmreleases/)

