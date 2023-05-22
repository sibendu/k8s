Following experiment is done using Microk8s on WSL2

1. Disable HA mode for Microk8s. Microk8s uses dqlite by default. Disabling HA will use etcd. THis will reset cluster, and remove all other things.

sudo microk8s disable ha-cluster 

2.  Download etcd (ref. https://github.com/etcd-io/etcd/releases). It will have etcdctl. Assuming it is inside /tmp/etcd-download-test 

export CERT_LOCATION=/var/snap/microk8s/current/certs

 /tmp/etcd-download-test/etcdctl --cacert=$CERT_LOCATION/ca.crt --cert=$CERT_LOCATION/server.crt --key=$CERT_LOCATION/server.key --endpoints=https://127.0.0.1:12379  member list --write-out=table

To get keys ->  /tmp/etcd-download-test/etcdctl --cacert=$CERT_LOCATION/ca.crt --cert=$CERT_LOCATION/server.crt --key=$CERT_LOCATION/server.key --endpoints=https://127.0.0.1:12379  get --prefix /registry/pods --keys-only


To Watch Changes being streamed - 
/tmp/etcd-download-test/etcdctl --cacert=$CERT_LOCATION/ca.crt --cert=$CERT_LOCATION/server.crt --key=$CERT_LOCATION/server.key --endpoints=https://127.0.0.1:12379  watch --prefix /registry/pods/default --write-out=json

Now ceate a pod in default namespace -> kubectl run --namespace=default --image=nginx nginx  
As the pod gets created by API server in etcd, it watches the change