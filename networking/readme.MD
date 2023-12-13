###### Ingress 
- Kubernetes API object that manages the routing of external HTTP/s traffic to services running in a cluster. (Layer 7 traffic)
- opening a k8s cluster to receive external traffic.
- traffic routing : defining traffic routes to backend services

- Ingress architecture in kubernetes consists of:
1. Ingress Resource
- defines http rules on how our apps can be acccessed
2. Ingress Controller
- Actual Ingress Resource implementation
- Acts as a http reverse proxy
- receives the connection, finds a matching rule and passes the traffic directly to the Services pod endpoints to the respond to the request
3. Ingress Class
- Associates an Ingress resource with a specific ingress controller
- You can defines a default Ingress Class in a Kubernetes cluster.

##### Why Kubernetes chose to allow 3rd party Ingress controllers, rather than creating a specific one for k8s
1. Allowing the k8s cluster administrator to choose what works best for them.
2. Allowing Infrastructure providers can create ingress controllers optimized for their envionments.

##### Proxy based Ingress controllers
1. Nginx --> maintained by the k8s community.
2. Traefik --> Based on a cloudnative edge router.
3. Contour --> Uses a popular Layer 7 proxy, Envoy service proxy.

##### Characteristics of the Ingress API
1. Defines traffic routes between external clients and services.
2. Allows encrpyted communication using TLS: TLS termination happens at the Ingress controller.
3. Load balances client traffic across a service's endpoints (pods).

##### Ingress capabilities:
1. Name based virtual hosts
2. Path based routing
3. TLS termination
- You can define a certificate config in our ingress configuration and use that to provide encryption from the client browser to the ingress controller.

##### Types of Ingress Controllers implementations
1. Running as a pod in a Kubernetes cluster - eg Nginx Ingress Controller.
2. Hardware external to the cluster - eg Citrix, F5 etc
3. Cloud Ingress Controllers - eg APPGW, Google Load Balancer and AWS ALB Ingress

- Ingress controllers have a defined specification

##### Advantages of Ingress over Load Balancers
1. Ingress works at layer 7, giving Ingress the ability to do things like path based routing and name based virtual hosts.
2. Ingress can implement higher level capabilities such as url re-routing, session persistency and dynamic weighting. --> Traditional LoadBalancers do not have these capabilities and can only load balance traffic based on the ip address and the port
3. Ingress controllers are a single resource that can provide access to multiple internal services, whereas LoadBalancers expose a single service  on a single IP and port --> This reduces resources consumed, which can even save you on your cloud bill. 
4. Reduced latency -> Ingress controllers forwards traffic directly to pod endpoints, rather than to the kube proxy, which would then route to the pod endpoints or perhaps even routing traffic between nodes.

##### Limitations of the Service API (using LoadBalancer type in general)
1. Manual configuration of load balancer whe using NodePort type
2. Potential latency due to the network hops introduced by kube proxy: LoadBalancer --> Kube proxy--> Service --> Pod
3. A LoadBalancer per service can quickly escalate operational(cloud costs) costs.
4. The Service API can not cater for advanced ingress traffic patterns.

##### Kubernetes Networking Rules
1. All pods can communicate with all other pods without Network Address Translation (NAT).
2. All nodes can communicate with all pods (and vice-versa) without Network Address Translation (NAT).
3. The IP address that a Pod sees itself as, is the same IP address that other see it as.
(All provide, that provide implementation for node-node network communication must abide to these rules.)

##### Container Networking Interface (CNI)
- CNI consists of a specification and libraries for writing plugins to configure networks interface in linux containers, along with a number of supported plugins

##### Service discovery mechanism in k8s
1. Environment variables defined in a Pod.
2. DNS implementation

##### TLS Secret Considerations
1. Secret must contain a certifcate (tls.cert) and a private key(tls.key).
2. Intermediate certificates must be bundled with public key Infrastructure.
3. TLS secrets are namespaces: TLS secrets are referenceable by Ingress objects in the same namespace.

#### NB:
- Ingress can only be used for http based traffic, you will need LoadBalancers for TCP/UDP based traffic functioning at layer 4.