
#### How to troubleshoot Broken OpenShift 4.6x Cluster

```
All worker pods were unable to be seen
Master related pods were stuck in a terminating status
SSHing to the master nodes 
we can now see that the master pods are not coming online 
restarting the node did not help
restarting the kubelet service did not help
Checking the ETCD container we see the following results 
```

```
oc login --loglevel=10 -u kubeadmin --password='NsJsv-esw2P-LGBUC-dFu5H'
Unable to connect to the server: EOF

https://www.ibm.com/support/pages/node/6398264

oc adm must-gather

oc get nodes --kubeconfig /var/Install/CP4D/auth/

$INSTALL_DIR/auth/kubeconfig
oc get nodes --kubeconfig /var/lib/kubelet/kubeconfig
oc pods --all-namesapces --kubeconfig /var/lib/kubelet/kubeconfig
oc get csr  --kubeconfig /var/lib/kubelet/kubeconfig
oc adm must-gather  --kubeconfig /var/lib/kubelet/kubeconfig


ssh core@master2.peanysrv7cbi.pe.glencore.net 
sudo -i
export KUBECONFIG=/etc/kubernetes/static-pod-resources/kube-apiserver-certs/secrets/node-kubeconfigs/lb-int.kubeconfig
oc config use-context system:admin
oc get nodes -owide  > nodes.out
oc get po -owide -A > pod-all.out
oc get csr > csr.out
oc get co > co.out 
oc describe co >> co.out 
oc get secret -A -o json | jq -r ' .items[] | select( .metadata.annotations."auth.openshift.io/certificate-not-after" | .!=null and fromdateiso8601<='$( date --date='+1year' +%s )' ) | "expiration: \( .metadata.annotations."auth.openshift.io/certificate-not-after" ) \( .type ) -n \( .metadata.namespace ) \( .metadata.name )" ' | sort | column -t > certificates.out

