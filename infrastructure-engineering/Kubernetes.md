Container images can be run using container runtimes like rkt, runC, etc (only
capable of running the images on a single host)

**Container Orchestrator:** Provides a scalable, fault-tolerant solution by creating a single controller/management (the Orchestrator) unit, after connecting multiple nodes together, to manage multiple such runtimes.

**Containers** are an application-centric way to deliver high-performing, scalable applications on the infrastructure of your choice.

**Container Image** is a bundle of an app, its runtime and dependencies.

The image runs to create an isolated executable environment also called a *container*
Then that container can be deployed on several hosts (VMs, Cloud, Desktop).

Container Orchestrators manage images deployed on various hosts (that for a
cluster). They have the following advantages:

- Are fault-tolerant
- Can scale, and do this on-demand
- Use resources optimally
- Can discover other applications automatically, and communicate with each other
- Are accessible from the external world
- Can update/rollback without any downtime.

Examples of Orchestrators: DockerSwarm by Docker, Kubernetes by CNCF, ECS by Amazon
Mesos Marathon by Apache Mesos and Nomad by Hashicorp.

**Use cases:** Container Orchestrators can:
- Form a cluster with multipe hosts
- Container scheduling on different hosts
- Let containers running on different hosts communicate
- Bind containers and storage.
- Bind similar containers to form services
- Resource usage monitoring and optimization
- Provide secure access to applications running inside containers

Kubernetes can be deployed on a single workstation or a datacenter, or a cloud Like
AWS or Openstack.
