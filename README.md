servicemesh_with_tempo
==============================
### This is all about how to install the tempo with servicmesh on Rosa
### Followed the below docs.
Main Doc :- https://docs.openshift.com/container-platform/4.15/service_mesh/v2x/ossm-observability.html#ossm-configuring-distr-tracing-tempo_observability
### Supporting Docs
###  Servicemesh
https://docs.openshift.com/rosa/service_mesh/v2x/ossm-create-smcp.html
### Tempo Installation
https://docs.openshift.com/container-platform/4.13/observability/distr_tracing/distr_tracing_tempo/distr-tracing-tempo-installing.html

### Install the following operators

    Red Hat OpenShift Tempo Operator

    Kiali Operator provided by Red Hat

    Red Hat OpenShift Service Mesh
You have installed the Tempo Operator and Red Hat OpenShift Service Mesh Operator in the openshift-operators namespace
```
### You have created namespaces such as istio-system and tracing-system
oc new-project istio-system
oc new-project tracing-system

### Create the smcp
oc create -f smcp_final.yaml

### create the smmr
oc create -f smmr_final.yaml

### Deploy tempostack in the tracing-system namespace
### Create the secret for s3 bucket
### create s3 bucket in AWS and make a secret with access key and secret key
oc create -f metrics-tempo-s3_final.yaml
oc create -f tempostack_final.yaml

### Edit the CRD Kiali with the below changes and let the kiali pod reconcile

  external_services:
    tracing:
      query_timeout: 30
      enabled: true
      in_cluster_url: 'http://tempo-simplest-query-frontend.tracing-system.svc.cluster.local:16686'
      url: '[Tempo query frontend Route url]'
      use_grpc: false

### Create telemetry object in the istio-system namespace
oc create -f telemetry_final.yaml 
```
