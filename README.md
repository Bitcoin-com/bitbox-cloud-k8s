## BITBOX Cloud Kubernetes Configs

### Steps

1. Add a new blank disk on GCE called bitcoin-data that is 200GB. You can always expand it later.
2. Save the following code snippets and place them in a new directory kube.
3. Change the `rpcuser` and `rpcpass` values in `bitcoin-secrets.yml`. They are base64 encoded. To base64 a string, just run `echo -n SOMESTRING | base64`.

### Create K8s cluster

Create gcloud k8s cluster called `bitbox-cloud`

```
> gcloud container clusters create bitbox-cloud --num-nodes=3

Creating cluster bitbox-cloud...done.
Created [https://container.googleapis.com/v1/projects/big-earth/zones/us-central1-b/clusters/bitbox-cloud].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/us-central1-b/bitbox-cloud?project=big-earth
kubeconfig entry generated for bitbox-cloud.
NAME          LOCATION       MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
bitbox-cloud  us-central1-b  1.8.8-gke.0     xx.xxx.xxx.xxx n1-standard-1  1.8.8-gke.0   3          RUNNING
```

### Create Bitcoin Cash containers

```
> kubectl create -f bitbox-cloud-k8s/

deployment "bitcoin" created
secret "bitcoin" created
service "bitcoin" created
```

### Delete and clean up

```
> kubectl delete deployment bitcoin
deployment "bitcoin" deleted

> kubectl delete secret bitcoin
secret "bitcoin" deleted

> kubectl delete service bitcoin
service "bitcoin" deleted

> gcloud container clusters delete bitbox-cloud

The following clusters will be deleted.
 - [bitbox-cloud] in [us-central1-b]

Do you want to continue (Y/n)?  Y

Deleting cluster bitbox-cloud...done.
Deleted [https://container.googleapis.com/v1/projects/big-earth/zones/us-central1-b/clusters/bitbox-cloud].
```

### Credit

Forked from [these steps](https://gist.github.com/zquestz/0007d1ede543478d44556280fdf238c9)
