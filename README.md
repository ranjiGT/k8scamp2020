# Kubernetes Summer Camp 2020 exam


## Emojivoto
The three services are the following:

* [emojivoto-web](https://github.com/BuoyantIO/emojivoto/tree/main/emojivoto-web/): Web frontend and REST API. This is the only service you directly interact with from your browser.
* [emojivoto-emoji-svc](https://github.com/BuoyantIO/emojivoto/tree/main/emojivoto-emoji-svc/): Internal API for finding and listing emoji. The web service is contacting this service internally and returning the results via its REST API.
* [emojivoto-voting-svc](https://github.com/BuoyantIO/emojivoto/tree/main/emojivoto-voting-svc/): Internal API for voting and leaderboard. The web service is contacting this service internally and returning the results via its REST API..

![Emojivoto Topology]()

## Running

To try out the service locally, you can run the following command to start the three services.

```
$ docker-compose up
```

Then you should be able to open the web application in your browser via this URL:

```
http://localhost:8080
```

## Task

The current folder also contains a minimal [Helm chart](./helm), not containing any resources. Your task is now add all the required resources to the Helm chart, so the same emojivoto application is running inside a Kubernetes cluster.

### Requirements

The task is considered successfully done when the following requirements are fulfilled:

* The helm chart can be deployed via `helm upgrade --install --wait --namespace your-name emojivoto .` from inside the helm directory.
* The helm chart contains a deployment and service for each of the three emojivoto services.
* All deployments have resource requests and limites configured.
* An ingress is provided and access via http is possible. It is configured to use the host `$FIRSTNAME-$FAMILYNAME.emojivoto.kubernetes.camp`. Certificate/https is not required.
* The environment variables in the docker-compose file are also available as environment variables in the deployment.
* All deployments must provide readiness and liveness probes using [tcp probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-tcp-liveness-probe) (instead of [http probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-http-request) as shown in the summer camp examples).
* The `emojivoto-emoji-svc` and `emojivoto-voting-svc` services are *not* reachable from the outside.
* The `emojivoto-emoji-svc` and `emojivoto-voting-svc` services are supposed to have a replica count of exactly 1.
* The `emojivoto-web` service has a default replica count and it is automatically scaled via a `HorizontalPodAutoscaler`.
* The following settings are configurable via values:
  * Port of each deployment is configurable
  * Requests and limits of each deployment are configurable
  * The docker images are configurable per service
  * The host of the ingress is configurable
  * The replica count and min and max replica count of HPA of `emojivoto-web` are configurable
* The name of the resources contains the helm chart name and deployment name.
* No configuration for RBAC or PodSecurityPolicy are required.

## Hints

* You can assume that an ingress-controller is already running inside the cluster, so you don't have to provide one.
* On SysEleven, don't run `kubectl` with `--all-namespaces` as you have access to your namespace only.
* The variables `EMOJISVC_HOST` and `VOTINGSVC_HOST` of the `emojivoto-web` service must contain the port.

## Glimpse of the deployments

