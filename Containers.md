# What is a container?

In the simplest of terms a container is a **sandbox** that helps us **isolate**, **run** and **monitor** a specific process in an operating system.

**Sandbox** implies that the process has its won process namespace, whereas in a usual operating system environment process share namespaces.

Typically once container has once container process. The container process and the container lifecycle is linked. When one ends the other one is also terminated.

## Container Image
The **binary** representation of the runtime environment (the one thats desicribed above :point_up: ) of a container (so basically a binary file). This binary representation follows a **parent-child model** and **image layering**

Lets say there is just a scratch image that we start with which is just an empty file system. Then we add on top of it is a basic operating system (say Debian). This is a child of scratch. Let's add something else on top of that, say Perl, Ruby environment. This will act as another layer.

#### Advantages
- This gives us a tree-like structure of different OS environments from which we can pull any of the images and run our applications (sharing images by pulling branches of a tree).
- This tree also follows inheritance. Let's say if we discovered a security vulnerablity in Debian and we put a path in at the debian image then that patch will be inherited by all its descendents with having to fix the vulnerablity in all the child images.

If we look at it from an OO perspective then an image is a sort of a class (a template for creating runtime OS envs) and the runtime representation of a container is an instance of a class.

## DockerFile
It is the text file used to create images. The parent image is the one mentioned in the `FROM:` field. Every line in a dockerfile is a layer (or cache layer). From `Dockerfile -> Image -> Runtime container`. We can also generate an image from a runtime env.

## Dependencies
All the dependencies that are above the kernel are packaged inside a container. So running a container doesn't install the dependecies in the OS. The container sits on top of the OS and contains its dependencies. This enables us to run different versions of different applications with different dependencies without having to pollute our OS env by installing different versions of a dependency.

## The Docker Architecture
- DockerFile
- Docker Host (which contains)
  - Daemon
  - Image Cache
  - Volume
- Registry
- Docker Client

The client talks to the daemnon, the registry to the cache
