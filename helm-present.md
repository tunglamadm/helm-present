# Introduction to Helm - Instructor Guide

## 1. What is Helm
Helm is a package manager for Kubernetes that simplifies the deployment and management of applications. It allows you to define, install, and upgrade complex Kubernetes applications using Helm charts.

Charts are like packages, these charts will have all the template files and the configuration required to create Kubernetes resources.
![image](https://github.com/user-attachments/assets/42066b2b-fae0-4994-b800-8b13bffcb45d)

For example to install MongoDB application to the Kubernetes, we need to create all the resources that are required to configure containers, networking, volumes,...

 ![image](https://github.com/user-attachments/assets/de5e16f2-eb14-4fea-a3c4-4a0e7ae0da63)

Using Helm this can be accomplished with a single command, helm will pull a MongoDB chart from a centralized then install with a single command (This is one of the template charts public in the internet, you can create your own charts).

### Revision History
Helm maintains a revision history after installations and upgrades.
When upgrades, it will create another revision for it.
![image](https://github.com/user-attachments/assets/19b6aef8-059d-4ac9-8e3a-273e9debf1bd)

### Dynamic Configuration
Helm uses file called Values.yaml,  which can pass parameters to template manifests in Helm Chart

![image](https://github.com/user-attachments/assets/e4ac3384-4d93-4ea1-a58b-d6f3e0a9b55c)

### Intelligent Deployments
It knows the order in which Kubernetes resources should be created. It will automatically deploy with that order.
![image](https://github.com/user-attachments/assets/5df24831-1be0-47e1-9e0d-27b5aab817da)

### Life Cycle Hooks
If there is any work which is not directly related to Kubernetes, but it has to be done during the installation or upgradation helm allows us to write hooks that can be hooked into the lifecycle events like installation, upgrade, uninstallation,... This could be writing data to database, backing up a database or making sure that the Kubernetes cluster is in a required state.

## 2. Charts and Repos
Charts are stored in a chart repository.
![image](https://github.com/user-attachments/assets/b054c033-37c6-4fc9-aff4-6fe711e552a5)
                                  https://bitnami.com/stacks/helm




