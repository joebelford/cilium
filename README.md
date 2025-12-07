# Install Cilium on Talos

## Install Gateway CRD's

```bash
kubectl apply -k 'https://github.com/kubernetes-sigs/gateway-api//config/crd?ref=v1.4.0'
```

**NOTE** Use helm chart and [values](./cilium.values.yaml) file instead.

```bash
helm repo add cilium https://helm.cilium.io/
helm repo update
helm install cilium cilium/cilium --version 1.18.4 --namespace kube-system --atomic -f cilium.values.yaml
```

**NOTE** Leaving this here for reference

```bash
helm install \
    cilium \
    cilium/cilium \
    --version 1.18.2 \
    --namespace kube-system \
    --set ipam.mode=kubernetes \
    --set kubeProxyReplacement=true \
    --set securityContext.capabilities.ciliumAgent="{CHOWN,KILL,NET_ADMIN,NET_RAW,IPC_LOCK,SYS_ADMIN,SYS_RESOURCE,DAC_OVERRIDE,FOWNER,SETGID,SETUID}" \
    --set securityContext.capabilities.cleanCiliumState="{NET_ADMIN,SYS_ADMIN,SYS_RESOURCE}" \
    --set cgroup.autoMount.enabled=false \
    --set cgroup.hostRoot=/sys/fs/cgroup \
    --set k8sServiceHost=localhost \
    --set k8sServicePort=7445
```

## Roll out update for ingress

```bash
helm upgrade cilium cilium/cilium --version 1.14.6 \
    --namespace kube-system \
    --reuse-values \
    --set ingressController.enabled=true \
    --set ingressController.loadbalancerMode=dedicated
```

```bash
kubectl -n kube-system rollout restart deployment/cilium-operator;
kubectl -n kube-system rollout restart ds/cilium
```

## References

<https://allanjohn909.medium.com/harnessing-the-power-of-cilium-a-guide-to-bgp-integration-with-gateway-api-on-ipv4-7b0d058a1c0d>  
<https://isovalent.com/blog/post/connecting-your-kubernetes-island-to-your-network-with-cilium-bgp/>  
<http://docs.frrouting.org/en/latest/bgp.html#autonomous-systems>  
<https://docs.cilium.io/en/stable/network/lb-ipam/>  
<https://www.talos.dev/v1.5/talos-guides/configuration/editing-machine-configuration/>  
<https://dariomader.io/post/how_i_moved_from_metallb_to_cilium/>  
<https://www.talos.dev/v1.5/kubernetes-guides/network/deploying-cilium/#installation-using-helm>  
<https://www.clouditlab.com/deploying-a-single-node-kubernetes-cluster-in-aws-using-talos/>  
