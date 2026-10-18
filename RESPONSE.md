## Part 1

> What does a Deployment give you that a bare Pod doesn't? Why do we need a separate Service object instead of talking to a Pod's IP directly?

Deployments give you automatic management for a set of pods, such that you can just declare what state you want your pods to be in, and then the deployment will just "get" your pods to be in that state. One important state is replicas, where you can declare how many pods you want running. If a pod fails or get deleted, the deployment can automatically spin up another pod to ensure the replica count is matched.

Vs when you use a bare pod, you have to manually manage the state, such as spinning up new pods when they get deleted and making sure you have the number of pods that you want.

For services, it allows pods to do inter-pod communicate easily without knowing things such as a Pod's IP. Since pods can be created/destroyed a bunch of times, it is not guaranteed that their IPs are static at all times. This means that all pods that need to talk to each other need to keep track of other active pods and their IPs, which is messy and confusing. If you instead can talk to pods through some static interface without needing to know about pod IPs, its a lot cleaner.

If you have a frontend pod that needs to talk to some backend pods, the frontend shouldn't need to know how many backend pods there are, or the IPs of each pod, just that its request can get routed to some backend pod. You can put each backend pod in one service so that the frontend can just talk to that single backend service for a stable networking interface.

## Part 2

> What is a Helm chart, and how do `values.yaml` plus `-f`/`--set` let the same chart produce different Kubernetes resources for different environments? Why is this better than keeping separate copies of your Part 1 manifests for staging and production?

A Helm chart basically creates a "package" of Kubernetes manifest templates and makes them reusable in different scenarios by applying specific configurable parameters, or values to the manifest templates (using a values.yaml file). For example, if you want to apply the same kubernetes manifests in part-1 in different use cases, like for a local cluster vs the actual production cluster, you can just apply the Helm chart with different values files for production values vs local cluster values, instead of keeping 2 different sets of kubernetes manifests.

Use `-f` for applying a different values file (values.yaml is the default config), and --set lets you manually override specific values in the cli

The main advantage for using Helm is for reducing duplication and making configuration management easier.
