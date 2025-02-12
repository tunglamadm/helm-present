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

  
### 3.3. Helm Upgrade

- Update repo:

![image](https://github.com/user-attachments/assets/b1f588c7-9c43-47ee-817e-8fe498ce7a77)


- Upgrading chart: helm upgrade mydb bitnami/mysql --values values.yaml
![image](https://github.com/user-attachments/assets/da2fa1a6-71ef-4944-8318-fccd41a641cd)

What will be upgraded are new chart version due to repo update above and what you want to change in values.yaml.
Helm will generate the Kubernetes templates. It will compare them to the old templates that are already deployed, and only the changes that are required will be pushed to the Kubernetes cluster.

- Check revision:

![image](https://github.com/user-attachments/assets/a6a1233b-26da-4476-9f04-273310a59e6e)

- Helm upgrade note: if you simply do upgrade without any configuration, then it will use the default configuration, it won't use the configuration that was used during the first installation.
  
   +) Default config will be use: helm upgrade mydb bitnami/mysql
   
   +) Pass the same configuration or updated configuration: helm upgrade mydb bitnami/mysql --values values.yaml
   
   +) Reuse previous configuration: helm upgrade mydb bitnami/mysql --reuse-values
 

### 3.4. Release Records

kubectl get secret

![image](https://github.com/user-attachments/assets/d300d0e6-58b6-4c6c-84ec-b345f37bdc6a)
Everytime we do installation or upgrade, it will create a secret, this is release record. This secret record will have the entire information about installation.

When you uninstall a chart, if you want to retain the secret records for rollback,... you can use --keep-history: helm uninstall mydb --keep-history

![image](https://github.com/user-attachments/assets/f071f326-52bd-4526-a712-45ae229666e1)


## 4. Advanced commands

### 4.1. helm release workflow

helm install mydb bitnami/mysql (https://github.com/bitnami/charts/tree/main/bitnami/mysql)

- Load the chart and its dependecies: the entire chart will be loaded and also its dependecies if needed.
  
- Parse the values.yaml.
  
- Generate the yaml: take the values provided, override the default values, generate the template yaml using those values by substituting values in the template.
  
- Parse the YAML to kube objects and validate: it will parse those yamls into Kubernetes objects, validate those objects against Kubernetes schema to ensure that they can be created and they are complying with the kubernetes schema.
  
- Generate YAML and send to kube cluster.

### 4.2. helm --dry-run

helm install mydb bitnami/mysql --values values.yaml --dry-run

This option will load this chart, substitute the values through values.yaml. Render the required kubernetes templates yaml file. But it will not creat resource in K8s cluster. Useful for debugging and seeing what we configured in the chart.

![image](https://github.com/user-attachments/assets/5aa40033-f6e2-4b41-9a83-b88dbe541d10)


### 4.3. helm template
In some cases, user will use all generated template files from --dry-run command above to apply to K8s cluster. But the dry run command include some non yaml syntax or elements into templates that get generated so it will have error. --dry-run can also use with both installation and upgradation, upgrade output will be completely different from installation because only the differences will be generated. Using --dry-run, we need connection to K8s cluster so template yaml files are validated.

helm template mydb bitnami/mysql --values values.yaml

helm template only generated yaml files format for new installation, never communicates with the kubernetes api server, it doesn't validate these templates, so it doesn't need a active Kubernetes cluster to work. This is great for your ci/cd tools where you don't have access to a kubernetes cluster, but you still want to generate these templates and use them later.
![image](https://github.com/user-attachments/assets/e341d3fb-12cb-4b23-a419-4ebafa8bee5e)


### 4.4. More about Release Records
- values.yaml changed and upgrade the chart:

![image](https://github.com/user-attachments/assets/5963b791-bf15-4e99-a915-2d451d7146f5)

- new revision is created:
  
![image](https://github.com/user-attachments/assets/dd86f64b-60c6-4043-ab3a-061044d4f2fd)

- view 1 secret: kubectl get secret -o yaml sh.helm.release.v1.mydb.v1
  
  all the data is encoded:
  
![image](https://github.com/user-attachments/assets/ad5aaa50-d29c-4b3c-9223-58181180c35c)
![image](https://github.com/user-attachments/assets/c89958dd-e0fb-48ff-8968-e4a2cd84e9bf)


### 4.5. helm get

If helm list does not give you enough information, you can use helm get to have more details. It contains sub commands and flags

![image](https://github.com/user-attachments/assets/fd1ad25c-af4a-41de-b6d4-e73af0a6cd77)

- Example:
helm get values mydb: It only shows the custom values , not the default values.

helm get values mydb --all: will show default values.

![image](https://github.com/user-attachments/assets/71ab3ffc-4fed-4ba3-952e-d6cc13ca7223)


![image](https://github.com/user-attachments/assets/9e455e87-3bd3-4dad-bd28-28d929d07f5b)


### 4.6. helm history

helm history mydb

![image](https://github.com/user-attachments/assets/ec2ac213-d00b-4b23-9403-ddc51cbe2139)

Even when a upgrade or installation fails the version and history for it will be maintained.
![image](https://github.com/user-attachments/assets/825687ac-7799-4fb2-ade8-dfd8b3659d67)


### 4.7. helm rollback

helm rollback myapache 1

![image](https://github.com/user-attachments/assets/e3ad4489-587e-4301-8000-b1265c8bde03)

Even if we uninstall helm chart but we still keep helm history, we can still rollback to previous versions. (helm uninstall mydb --keep-history)


### 4.8. create namespace

Helm has option or flag called --create-namespace. This will automatically create new namespace, we don't have to crate new namespace then apply to helm command.

- Example: helm install myapache bitnami/apache --namespace myapache --create-namespace

### 4.9. helm install or upgrade

Example: helm upgrade --install mywebserver bitnamy/apache

The way this command works, it will first check if the installation is already there. If it is there it will do the upgrade, otherwise it will do a installation.

![image](https://github.com/user-attachments/assets/20f545fe-72be-4773-bff6-887e2b8ad081)
![image](https://github.com/user-attachments/assets/4de0fb08-4c50-4664-9d8b-a7f2ed32d51e)

### 4.10. Generate Release Names

helm install bitnami/apache --generate-name

![image](https://github.com/user-attachments/assets/8eec18c7-5cc1-4d41-a1d3-f8cdd01a4fae)
![image](https://github.com/user-attachments/assets/b9ec1391-b39d-4106-b919-6edec7beeb43)

### 4.11. Wait and Timeout
The helm install command considers the installation to be successful as soon as the manifest is received by the kubernetes API server it doesn't wait for the pods to be up and running.
To wait until helm successful install, use --wait option. Default time is 5 minutes, if the installation doesn't complete, if the pods are not up and running within this time the installation will be marked as a failure.

       helm install mywebserver2 bitnami/apache --wait --timeout 5m10s


### 4.12. Forceful upgrades
When using helm upgrade, it will restart those pods whose values have changed, it will not restart all the pods all the time. But if you want to forcefully restart the pods when you do a upgrade, use --force. Helm will delete the current deployment instead of modifying the deployment and it will recreate the deployment (there will be some downtime for the application)

       helm upgrade mywebserver2 bitnami/apache --force

## 5. Create Charts

### 5.1. Create first chart

helm create firstchart

By default helm create command uses the nginx chart to create chart.

![image](https://github.com/user-attachments/assets/52f0de38-24ea-49f9-97eb-4d4cadad893a)

- Chart.yaml file contains the metadata about the chart.
![image](https://github.com/user-attachments/assets/768c441c-f415-4faa-843d-c23d969302e6)

- charts folder, it will empty when we first create the chart, ut if this chart depends on any other charts those charts will be pulled and stored inside charts folder.
  
- templates folder:
  
![image](https://github.com/user-attachments/assets/ef3b7a46-99aa-48ae-a602-ffd1f3c81721)

- values.yaml: contains all the values, the default values that will go into the template yaml files

![image](https://github.com/user-attachments/assets/93486c98-d1ff-401e-bc41-7c777c5bdba0)

### 5.2. Install the chart

helm install firstapp firstchart/

Instead of point to bitnami repo, we point to the folder of the chart.

![image](https://github.com/user-attachments/assets/290cdc13-2ac5-4e14-a2f4-7975a0471542)

The NOTES show in the screen comes from NOTES.txt insde templates folder:

![image](https://github.com/user-attachments/assets/cf590128-776e-441c-895a-2e4ae332f943)



### 5.3. Chart YAML 
This is the file that has the metadata about project

![image](https://github.com/user-attachments/assets/03b450e7-7903-480f-8df5-fe45b78fa5a3)

- apiVersion: determines the rest of the document, that is the structure that needs to be followed, the elements that can be used, the mandatory elements and the optional elements are determined by the version. In the future, if this version changes, there might be some new mandatory fields that will be included.
  
- name: the name of the chart
  
- description: information about what this chart does or what this chart is responsible for.
  
- type: can be application or library
  
  +) when we use a chart to deploy application type will be application
  
  +) library will be use in case project will have reusable functions that can be used across charts.   When define a chart as a library project, it will not have any templates and there will be no releases or installations done using that chart. It simply is to define some reusable functions that can be used across other charts, other library charts or other application charts.

- version: the version of the chart starts with zero point 0.1.0 (version number is not fixed, can be any numer)

- appVersion: the application that is being packaged through this chart, depending on the version of application.

- More optional elements in Chart.yaml: https://helm.sh/docs/topics/charts/z

### 5.4. Templates 

![image](https://github.com/user-attachments/assets/8f4badda-7f83-44b9-8f22-eb5fb51c23d1)

All the manifest files here has placeholder inside. This is Google Go templating syntax. You can delete any file that you think it doesn't need for your project or you can add more file inside templates folder.

- deployment.yaml

![image](https://github.com/user-attachments/assets/1a63c7c3-ec68-41ce-9b6f-10455cbb30be)

- Some files will have conditional statement on the 1st line
  
![image](https://github.com/user-attachments/assets/f6e166c6-7d38-498b-ba94-bfb2c596c34a)
![image](https://github.com/user-attachments/assets/d7addad2-f3b7-46ba-a409-8c17a53c7e25)

### 5.5. Helpers File
.tml stands for template

![image](https://github.com/user-attachments/assets/f8b36310-26da-4a8c-89d3-9794638c28e2)

All the other yaml files in this folder are used to generate Kubernetes manifest, which will be used to create Kubernetes resources. This .tpl will not generate any Kubernetes manifest. It will simply have some methods that will be used in all manifest files. So any template methods we want to reuse across these templates can be put into this template file.









