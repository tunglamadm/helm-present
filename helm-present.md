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
                                  
![image](https://github.com/user-attachments/assets/9622ebb7-b5b2-49e3-9a16-e91209154074)

You can look at the source of the charts here: https://github.com/bitnami/charts
![image](https://github.com/user-attachments/assets/d45627f1-3a31-423d-986e-2a8869fcc390)

Each chart will have a templates folder, a Chart.yaml file and the values.yaml
![image](https://github.com/user-attachments/assets/609fa330-fdf0-4bae-be49-2eb8db60d096)

## 3. Work with chart repositories
helm repo list

helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo remove bitnami
![image](https://github.com/user-attachments/assets/40cddfea-e79a-4849-b002-b5cb44112983)


- Search the repository:

helm search repo mysql

helm search repo apache (show only latest version)

![image](https://github.com/user-attachments/assets/e7d4b2e9-f7b5-467e-af19-1449003db107)

helm search repo apache --versions (show older versions)

![image](https://github.com/user-attachments/assets/571ea5dd-6407-429a-aee5-50aa44080fda)

### 3.1. Install Helm chart
- Install Helm chart
helm install mydb bitnami/mysql
![image](https://github.com/user-attachments/assets/90581501-c8a4-4b7a-a422-6dcdd37f9fe9)
Successfully deployed helm chart, it will show the result and also the related instructions about the chart installed
![image](https://github.com/user-attachments/assets/30af28ff-cf1c-41f7-b7a7-90dfdfc7da2e)

Check the pod:

![image](https://github.com/user-attachments/assets/c4ebe947-a603-4bb6-83c4-2e59d6bbf16e)
![image](https://github.com/user-attachments/assets/63746bc8-2745-46a0-b029-b248218f163f)


- Install to another namespace:

helm install myapache bitnami/apache -n myapache-test-2
![image](https://github.com/user-attachments/assets/2237de4d-cd4f-46fa-86ac-488647a2f68f)

The installation name should be unique per namespace.


- List and Uninstall:

  helm list
![image](https://github.com/user-attachments/assets/b4482fe7-ada3-455a-865e-49d7bebb47c7)

  helm uninstall mydb
![image](https://github.com/user-attachments/assets/a74d6a7d-d954-4ffe-94fd-75d6502b2c86)
![image](https://github.com/user-attachments/assets/b254a483-fa75-485c-914a-6c95806403d8)


### 3.2. Custom Values

- Option 1 pass in the command line: helm install mydb bitnami/mysq --set auth.rootPassword = test1234

![image](https://github.com/user-attachments/assets/b1a8b14f-f472-4371-96a4-44258f44fb19)

- Option 2: Pass in file values.yaml
  
![image](https://github.com/user-attachments/assets/22faf298-a544-4d5f-a031-7445c5c90483)

  Inside values.yaml: helm install mydb bitnami/mysql --values values.yaml
  
  ![image](https://github.com/user-attachments/assets/359696f7-3528-4923-a51e-63470c32cb72)

  Check new password applied:
  
  ![image](https://github.com/user-attachments/assets/e2b73b93-c805-4a4f-8c45-077b3f02f4c3)

  




