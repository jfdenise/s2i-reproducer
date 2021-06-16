# s2i-reproducer

Reproducer for issue: https://github.com/shipwright-io/build/issues/810

The image builder image quay.io/jfdenise/wildfly-s2i-v2:latest contains the s2i labels although the s2i build doesn't take them into account.

0) Kubernetes (minikube v1.11.0, kubernetes v1.18.3 on Docker 19.03.2)

1) Follow the initial steps in https://github.com/shipwright-io/build
NB: You will have to update the push-secret and output image repo location.

2) Add the source-to-image-jboss strategy (a copy of https://github.com/shipwright-io/build/blob/main/samples/buildstrategy/source-to-image/buildstrategy_source-to-image_cr.yaml) 
 to workaround: https://github.com/shipwright-io/build/issues/811
   ./kubectl apply --filename buildstrategy_source-to-image_cr.yaml

3) Create the build: 
   ./kubectl apply --filename build.yml

4) Create the buildRun:
   ./kubectl create --filename build-run.yml

5) Wait to give time to kubernetes to pull the builder image.

6) Check the log:
 ./kubectl --namespace default logs wildfly-s2i-buildrun<your id> --container=step-build-and-push

You will find the following trace:

Running: [/bin/sh -c /usr/libexec/s2i/assemble] 
/bin/sh: /usr/libexec/s2i/assemble: No such file or directory
error building image: error building stage: failed to execute command: waiting for process to exit: exit status 127
