# Install Cilium on Talos

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
