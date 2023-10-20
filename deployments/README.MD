#### Controllers
- Controllers in Kubernetes watches the state of the cluster through the API server and makes desired changes to ensure the defined desired state in maintained.
- Controllers use two mechanisms:
1. Watch mechanism
2. Poll mechanism: continuosly polls for updated information when needed

#### Types of controllers in k8s
1. ReplicaSet: Ensures that a specified number of pod replicas are running at all times. It is often used to guarantee high availability and scalability.

2. Deployment: Manages ReplicaSets and provides declarative updates for Pods and ReplicaSets. It allows you to easily roll out changes to your application by creating new ReplicaSets and gradually scaling down the old ones.

3. StatefulSet: Manages the deployment and scaling of stateful applications. It ensures that each pod in the set has a unique identity and stable network identity.

4. DaemonSet: Ensures that a copy of a specific pod is running on each node in the cluster. It is commonly used for running monitoring agents, log collectors, or other system-level daemons.

5. Job: Creates one or more pods and ensures that a specified number of them successfully terminate. It is useful for running batch or one-time tasks.

6. CronJob: Creates Jobs on a schedule defined by the cron format. It allows you to run Jobs periodically, similar to cron jobs in Unix-like systems.

#### Replicaset controller
- If a pod dies/deleted, the replicaset submits a new pod to be created to the API server to maintain the defined desired state.

##### Types of Node failures
1. Transient failures: node becomes temporarily unavailable
- If this happens, the pods in that node are marked with an Error status, on the API server, because the controller manager can not reach those pods, to get their status.
- Kubelet restarts the containers in the Node, once is become ready again.
2. Permanent failures
- Pod eviction timeout (~5 minutes): if a Node become unavailable for around 5 minutes, the pods in that Node are terminated and rescheduled in other Nodes in the cluster.

#### Configurating and Managing Application state with Deployments
- Key concepts:
1. Updating deployments
- To rollout a new app version
```
kubectl rollout restart deployment/<deployment-name> -n <namespace>
```
- To check the  deployment rollout status
```
kubectl rollout status deployment <deployment-name>
```
2. Controlling rollouts: controlling rollouts of the application version being deployed.
3. scaling applications

#####  Update Strategies
- k8s gives us configurations options for controlling rollouts on new apps versions
- how k8s manages the transition between versions of replicasets, when deploying a new application version
- controls how new pods are rolled out.
- Update Strategies used:
1. ```RollingUpdate``` (default Update Strategy) --> Replace the old ReplicaSets by new one using rolling update i.e gradually scale down the old ReplicaSets and scale up the new one. --> also manages the transition from the old ReplicaSet to the new one. 
2. ```Recreate``` --> Terminates all pods in the current ReplicaSet prior to scaling up the New ReplicaSet

###### Controlling the ```RollingUpdate``` Strategy
- config options available:
1. ```maxUnavailable```: specifies the maximum number of olds pods that can be taken down at a time while new pods are being created.
2. ```maxSurge```: specifies the number of new pods that can be created at a time while olds pods are being terminated.
- k8s deployment will stop a rollout if the number of pods, not passing the Readiness Probes, goes below the ```maxUnavailable``` value. (25% is the default value) --> protects the app and ensures a set of pods are always running.
- Always use Readiness Probes to ensure the apps are ready to server user requests.
- To pause a deployment rollout
``` 
kubectl rollout pause deployment <deployment-name>  -n <namespace> 
```
- To resume a deployment
``` 
kubectl rollout resume <deployment-name>  -n <namespace> 
```
- To view a deployment history
```
kubectl rollout history deployment <deployment-name> -n <namespace>
```
- To rollback to a previous version
```
kubectl rollout undo deployment <deployment-name> -n <namespace> 
```
- To rollback to a specific version
```
kubectl rollout undo deployment <deployment-name> --to-revision=1 -n <namespace> 
```
- To restart a deployment (no new pods are recreated, just new replicaset with same pod spec)
``` 
kubectl rollout restart deployment/<deployment-name> -n <namespace> 
```
``` 
kubectl rollout restart deployment <deployment-name> -n <namespace> 
```

- You can:
1. Pause a rollout to make corrections
2. Rollback to an earlier version
3. Restart a deployment

##### Deployments
- Deployments can be used to manage:
1. ReplicaSets: core building block of deployments.
2. Pods
- Updating a deployment is usually triggered by changing the pod template.
- Deployment statuses:
1. Complete: all update work is finished
2. Progressing: update in flight
3. Failed: update could not complete


