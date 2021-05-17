# Logging with fluent-bit
We're going to create a logging system to collect logs only from the default namespace and post it into a slack channel through a webhook

## Create the main application
* Create the hello application
`$ kubectl create -f 01_hello_app.yaml`
* Expose the hello app
`$ kubectl expose deployment hello --type=NodePort`
* Right now you can check the created service and access through the service port
`$ kubectl get services`
`$ curl http://192.168.99.100:31366`

## Create the logging system
* Create the logging namespace
`$ kubectl create namespace logging`
* Create the service account and role setup
`$ kubectl create -f 02_fluent-bit_rbac.yaml`
* Create the configmap with all the fluent-bit configuration (Update the output-slack.conf with your webhook URL)
`$ kubectl create -f 03_fluent-bit_configmap.yaml`
* Create the daemon
`$ kubectl create -f 04_fluent-bit_daemon.yaml`

## Post example on slack
```
["timestamp": 1620993109.434713215, {"log"=>"2021/05/14 11:51:49 Serving request: /
", "stream"=>"stderr", "time"=>"2021-05-14T11:51:49.434713215Z", "kubernetes"=>{"pod_name"=>"hello-579d48586d-vctzf", "namespace_name"=>"default", "pod_id"=>"ec14cd1c-a53f-4ac9-b223-41c6b042ad3b", "labels"=>{"app"=>"hello", "pod-template-hash"=>"579d48586d"}, "host"=>"minikube", "container_name"=>"hello", "docker_id"=>"1d9bedb0aa258b587671e7da3eaf64ffe55d06a1c16dc80f5068214c80908e4f", "container_hash"=>"gcr.io/google-samples/hello-app@sha256:c62ead5b8c15c231f9e786250b07909daf6c266d0fcddd93fea882eb722c3be4", "container_image"=>"gcr.io/google-samples/hello-app:1.0"}}]
```
