# Crossplane Deep Dive Demo

Originally created for KubeCon EU 2022

## Prerequisites

* Working Crossplane installation with provider-jet-aws

## Demonstrate core functionality with MR provisioning

* Apply barebone Managed Resource
```sh
kubectl apply -f examples/managed-resources/aws/s3.yaml
```
* Go to AWS console
* Amazon S3->Buckets->crossplane-deepdive-demo-bucket->Properties. Click Edit in the Tags sectiona
* Change the Value of Name tag to another one like `CONFIGURATION DRIFT`, click 'Save changes'
* Demonstrate that with the next reconciliation loop the configuration drift is mitigated
  and Name tag value is set back to `CrossplaneDeepDiveDemoBucket`
* Clean up the MR
```sh
kubectl delete -f examples/managed-resources/aws/s3.yaml
```

## Demonstrate the Claim

* Compare barebone `examples/managed-resources/aws/rds-instance.yaml`
  with the Claim `examples/claims/claim-aws.yaml`
* Discuss custom API, exposing only required API fields,
  show all config options of [RDSInstance Spec][rds-spec]
* Claim intentionally goes first before diving into Compositions
  to immediately demonstrate the value to the customers

## Demonstrate the XRD and Composition

* Show and discuss contents of `examples/compositions/definition.yaml`
* Show and discuss contents of `examples/compositions/composition-aws.yaml`
* Apply XRD and Composition
```sh
kubectl apply -f examples/compositions/definition.yaml
kubectl apply -f examples/compositions/composition-aws.yaml
```
* Look around
```sh
kubectl get xrd
kubectl get compositions
```
* Apply the Claim
```sh
kubectl apply -f examples/claims/claim-aws.yaml
```
* Explore `db-conn` secret created by the Claim
* Go to the AWS console, demonstrate that values from Claim/XRD were properly propagated
  to the configuration of cloud resource

[rds-spec]: https://doc.crds.dev/github.com/crossplane-contrib/provider-jet-aws/rds.aws.jet.crossplane.io/Instance/v1alpha2@v0.4.2
