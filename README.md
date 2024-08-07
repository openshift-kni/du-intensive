## Kube-burner-ocp based template for a RAN DU workload

This repository holds the templates for a DU workload that is deployed with [kube-burner](https://github.com/kube-burner/kube-burner)


| Pod | Number of Pods | Specs | Stress |
|-----|----------------|-------| ------- |
| Guaranteed | 1 pod | - 32 CPU, 1 GiB Memory, 16 GiB HP<br>- 2 configmaps and 4 secrets<br> 1 svc<br> | 32 threads of 100% CPU stress, 512mb vm stress |
| BestEffort - web_server | 10 pods | - 100 mc CPU, 128 Mib Memory<br>- 2 configmaps and 4 secrets<br>- 10 svc<br> | Exposes 8080 port for probes|
| BestEffort - curl_app | 10 pods | - 100 mc CPU, 128 Mib Memory<br>- Liveness Probes (every 10 secs)| Kubelet stress with probes, ~10-30% increase |
| BestEffort - kubectl_pods | 10 pods | - 100 mc CPU , 128 Mib Memory<br>- kubectl get (every 1 sec) | Kube-api-server stress with kubectl get, ~50% increase |

This workload is intended to be deployed by the ocp-far-edge-vran pipeline but the templates can be run locally using kube-burner


### Steps to run workload with cpu_util tests:

* Run the MIRROR_SPOKE_OPERATOR_IMAGES stage in ocp-far-edge-vran-deployment pipeline to mirror necessary test images
* Run the cpu_util test using ocp-far-edge-vran-tests pipeline

### Steps to run locally
* export REGISTRY=quay.io/rh_ee_apalanis
* Deploy DU workload with `kube-burner init --config du-intensive.yaml`
