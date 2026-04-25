# Kubernetes

## Events

A resource type that acts as a transaction log of what is happening inside the cluster from the control plane. It gives a general overview of the state of the cluster.

## kubectl

This is basically the CLI for Kubernetes. Let's you communicate with the Kubernetes API Server.

**Useful Commands:**

`kubectl top node` - Checks the health and resource load of nodes
`kubectl top pod` - Checks the health and resource load of pods
`kubectl describe pod <pod-name>` - Shows the configuration and history of "Events"
`kubectl logs -f <pod-name>` - Streams logs in realtime
`kubectl get events --sort-by='.lastTimestamp'`
