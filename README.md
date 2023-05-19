#
# When not to use Microservices

We should not use microservices when there is no requirement for scalability within a project. Using microservices over monolith architecture requires more maintenance and a creates a higher cost to the business. Microservices are best suited for complex, large-scale applications that require independent scalability and have diverse functional components. If you have a small, simple application with limited functionality, adopting a microservices architecture may introduce unnecessary complexity and overhead. In such cases, a monolithic or modular architecture might be more appropriate.

#
# When not to use Kubernetes/K8

- Simple applications: If you have a small, simple application that doesn't require complex scaling or orchestration capabilities, deploying it on Kubernetes might introduce unnecessary complexity.

- Limited resources: Running a Kubernetes cluster requires a certain amount of computing resources, including memory, CPU, and storage. If you have limited resources available, running Kubernetes may not be feasible or efficient. 

- Cost considerations: Running a Kubernetes cluster can incur additional costs. Apart from the hardware resources required, there may be expenses associated with networking, storage, monitoring, and managing the cluster.

- Learning curve: Kubernetes has a steep learning curve, and it requires a significant investment of time and effort to become proficient in managing and operating Kubernetes clusters. 

#
# How does K8 work?

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It provides a set of tools and APIs for managing and coordinating containers across a cluster of nodes.

Kubernetes simplifies the management of containerized applications by providing a scalable and resilient platform. It abstracts the complexity of infrastructure management, allowing developers to focus on building and deploying applications, while operators can efficiently manage and scale the underlying infrastructure.

#
# K8 Architecture diagram:
![full-kubernetes-model-architecture](https://github.com/JakeGillatt/Kubernetes/assets/129315605/76e169b1-2147-4cd8-b880-823eb94ac85b)

#
# What are some of the benefits of using K8?

Here are some benefits of using Kubernetes:

Scalability: Kubernetes enables horizontal scaling of applications. You can easily scale your application by increasing or decreasing the number of replicas (Pods) based on the demand. This allows you to handle varying levels of traffic and ensure high availability and performance.

High Availability: Kubernetes provides features for ensuring high availability of applications. It automatically restarts failed containers, replaces failed nodes, and reschedules Pods on healthy nodes. This helps in maintaining the overall health of the application and minimizing downtime.

Fault Tolerance: Kubernetes offers self-healing capabilities. If a container or node fails, Kubernetes automatically detects the failure and takes corrective actions, such as restarting the container or rescheduling it on a different node. This improves the fault tolerance of your applications.

Resource Efficiency: Kubernetes optimizes resource utilization by scheduling Pods based on available resources and resource requests. It ensures that each container receives its allocated resources, avoiding resource contention and maximizing the efficiency of the underlying infrastructure.

Deployment Flexibility: Kubernetes supports various deployment strategies, including rolling updates, blue-green deployments, and canary releases. This allows you to update or roll back your application versions with minimal disruption and risk.

Infrastructure Agnosticism: Kubernetes abstracts the underlying infrastructure, allowing you to run your applications on different cloud providers, virtual machines, or on-premises environments. This flexibility avoids vendor lock-in and enables workload portability.

Service Discovery and Load Balancing: Kubernetes provides a built-in service discovery mechanism. It assigns a stable network endpoint (Service) to a group of Pods, enabling other services or Pods to easily discover and communicate with them. Kubernetes also automatically load balances the traffic across the available replicas of a service.

Storage Orchestration: Kubernetes has integrated storage orchestration capabilities. It can dynamically provision and manage storage volumes for containers, ensuring that containers have persistent and reliable storage available when needed.

Extensibility and Ecosystem: Kubernetes has a large and vibrant ecosystem with a wide range of third-party tools, extensions, and plugins. This allows you to extend Kubernetes functionality with additional features like monitoring, logging, security, and more, tailored to your specific needs.

Community Support: Kubernetes has a large and active community of developers and users who contribute to its development, provide support, and share best practices. The community-driven nature of Kubernetes ensures that it continues to evolve, improve, and stay up-to-date with the latest advancements.

#
# Who is using K8?

Kubernetes has gained significant adoption and is used by numerous major companies across various industries. Here are some notable companies that use Kubernetes:

Google: Kubernetes was originally developed by Google and is still heavily used internally within the company for managing its vast infrastructure and workloads.

Amazon: Amazon Web Services (AWS) offers Amazon Elastic Kubernetes Service (EKS) as a managed Kubernetes service. Many companies running their workloads on AWS utilize EKS, including popular brands like Netflix, Adobe, and SoundCloud.

Microsoft: Microsoft provides Azure Kubernetes Service (AKS), a managed Kubernetes offering on the Azure cloud platform. Companies like Siemens, Accenture, and ASOS utilize AKS for their container orchestration needs.

IBM: IBM offers IBM Cloud Kubernetes Service (IKS) as a managed Kubernetes service. Companies like Nestle, H&R Block, and Bosch use IKS for deploying and managing their containerized applications.

Salesforce: Salesforce, a leading customer relationship management (CRM) platform, leverages Kubernetes for managing and scaling its infrastructure. They have a Kubernetes-based platform called Salesforce Heroku.

Spotify: The popular music streaming service Spotify relies on Kubernetes to manage its complex and dynamic infrastructure. Kubernetes enables them to handle massive traffic and scale their services efficiently.

Twitter: Twitter adopted Kubernetes to manage its microservices-based architecture. Kubernetes allows them to scale their infrastructure, manage deployments, and ensure high availability of their services.

#
# nginx-deployment.yml

1. In VS Code create a new nginx-deployment.yml file
2. Add the following code to the file:
```
apiVersion: apps/v1 # which api to use for deployment

kind: Deployment # what kind of service/object do you want to create?

# what would you like to call it - name the service/object


metadata:
  name: nginx-deployment
spec:
  selector: 
    matchLabels:
      app: nginx
  replicas: 1 # you can scale this in or out

  template:
    metadata:
      labels:
        app: nginx
    
    spec: 
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
3. Run the command `kubectl create -f nginx-deploy.yml`
4. Now create another YAML file called `nginx-service.yml` and add the following code:
```
---
# select the type of API version and type of service
apiVersion: v1
kind: Service

# metadata for name
metadata:
  name: nginx-svc
  namespace: default # sre

# specification to include poirts selector to connect to the
spec: 
  ports:
  - nodePort: 30001 # range is 30000 - 32768
    port: 80

    targetPort: 80

# define the selector and label to connect to nginx
  selector:
    app: nginx

# creating a nodeport type of deployment
  type: NodePort # also use loadbalancer - for local use cluster
```
5. Run the command `kubectl create -f nginx-service.yml`
5. Now run `kubectl get pods`, you should see that both pods are running

#
# 
