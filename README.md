# Myfiles-acn
-------------------
Cogniz - 22/22/2023 - Int Ques
1) What is PR Decoration in SQ?
	You can also report the pull request analysis and quality gate status directly in your DevOps platform's interface.
	Pull request analysis shows your pull request's quality gate and analysis in the SonarQube interface
	You can see your pull requests in SonarQube from the branches and pull requests dropdown menu of your project.
	In addition to appearing in the SonarCloud interface, the quality gate status and a summary of the results also appear in your DevOps platform 
	interface (that is, in the pull request view of GitHub, Bitbucket Cloud, Azure DevOps or GitLab). 
	This is referred to as pull request decoration.

2) What is HPA in GKE?
	The Horizontal Pod Autoscaler changes the shape of your Kubernetes workload by automatically increasing or decreasing 
	the number of Pods in response to the workload's CPU or memory consumption, or in response to custom metrics reported 
	from within Kubernetes or external metrics from sources outside of your cluster.
	*kubectl autoscale deployment hello-world-rest-api --max=10 --cpu-percent=70
	*Also called horizontal pod autoscaling - HPA - kubectl get hpa
	
	
	apiVersion: autoscaling/v2beta2
	kind: HorizontalPodAutoscaler
	metadata:
	  name: my-app-hpa
	spec:
	  scaleTargetRef:
		apiVersion: apps/v1
		kind: Deployment
		name: my-app-deployment
	  minReplicas: 1
	  maxReplicas: 3
	  metrics:
		- type: Resource
		  resource:
			name: cpu
			target:
			  type: Utilization
			  averageUtilization: 50
	
3) What is the difference between Statefulset and Deamonset?
	StatefulSets are used to manage stateful applications which wants to persist some data and DaemonSets ensure that all (or a subset of) nodes run a copy of a pod.

4) How to enable branch protection rules?
	Under your repository name, click Settings. If you cannot see the "Settings" tab, select the dropdown menu, 
	then click Settings. 
	In the "Code and automation" section of the sidebar, click Branches. Next to "Branch protection rules", click Add rule.

5) How do you connect config maps to the Pod running in GKE?
	Mount the ConfigMap through a Volume
	Attach to the created Pod using `kubectl exec -it pod-using-configmap sh`. 
	Then run `ls /etc/config` and you can see each key from the ConfigMap added as a file in the directory. 

6) How did you create GKE cluster?

7) How do you create a project in SQ? 
	- Using Jenkins Pipeline- 
	sh "sonar-scanner-4.7.0.2747/bin/sonar-scanner -X -Dsonar.login=$env.SONARQUBE_API_KEY -Dsonar.projectKey=dwsam-t-star-gx:gen3 -Dsonar.inclusions=**/*.sql -Dsonar.host.url=http://sonarqube-ent-sonarqube.sonarqube.svc.cluster.local:9000 && \
			    curl -v -u $env.SONARQUBE_API_KEY: -X POST -d 'almSetting=sonarqube-dwsam-t-star-gx' -d 'repository=https://github.com/dwsam-t-star-gx/gen3' -d 'monorepo=false' -d 'project=dwsam-t-star-gx:gen3' 'https://sonarqube.gcp.prod.dws.com/api/alm_settings/set_github_binding'"
		    }

8) How do you remove history in Github Commits?
	Ans - Once you've identified the commits that you want to delete, 
	you can use the git rebase command to remove them. This will open an interactive rebase window, 
	where you can select the commits that you want to delete.
	For example, if you want to remove the last three commits, you can run the following command: git rebase -i HEAD~3
	Git revert can be used as well.


9) What is Continuous Delivery vs Continuous Deployment?
	Key Differences. Simply put, Continuous Delivery focuses on ensuring software is always release-ready with manual approval, 
	while Continuous Deployment automates the release process, deploying changes to production automatically once tests pass.

10) What is NoSQL service in GCP?
	NoSQL databases use a non-tabular format to store data rather than in rule-based, relational tables like relational databases.
	In GCP its - Datastore or Cloud Bigtable
	In Azure - Azure Cosmos DB for NoSQL

11) Write a simple dockerfile
	FROM : 18-alpine
	WORKDIR /usr/src/app
	COPY package*.json ./
	RUN npm install
	COPY . .
	EXPOSE 3000

12) Can we use multiple agent in Jenkinsfile?
	Yes we can use.
	pipeline {
		agent none
		stages {
			stage('Build') {
				agent any
				steps {
					checkout scm
					sh 'make'
					stash includes: '**/target/*.jar', name: 'app' 
				}
			}
			stage('Test on Linux') {
				agent { 
					label 'linux'
				}
				steps {
					unstash 'app' 
					sh 'make check'
				}
				post {
					always {
						junit '**/target/*.xml'
					}
				}
			}
			stage('Test on Windows') {
				agent {
					label 'windows'
				}
				steps {
					unstash 'app'
					bat 'make check' 
				}
				post {
					always {
						junit '**/target/*.xml'
					}
				}
			}
		}
	}

12) Can you retrive a instance if its accendentialy terminated
	Generally, snapshots are used to backup data from your persistent disks and you can create an instance.

13) What is git stash and rebase?
	The git stash command takes your uncommitted changes (both staged and unstaged), saves them away for later use,
	Rebasing is the process of combining or moving a sequence of commits on top of a new base commit.
	a rebase means moving your branch's starting point to a different commit. 
	It's like pretending you began your work from that new point.

14)  What is DAST and SAST?
	SAST - SAST is open box testing that scans a software application from the inside out before it is compiled or executed. 
	DAST -  DAST tests the running application and has no access to its source code.

15) What is cloudsql Auth Proxy?
	Cloud SQL Auth proxy is used as an interface between your Database and instance to establish authorized, encrypted, and secured connections from 
	Database to your instance. It acts as a sidecar container.



TCS

1) Which mode used for creating GKE cluster?
	Standard - Gives you to customize your cluster.
	
2) Difference between Standard and Autopilot Gke Cluster?
	Autopilot: Defaults to the Regular release channel. You can optionally set the release channel or GKE version in channels. 
	Standard: You can set the release channel, choose a static GKE version, or even opt for Alpha clusters for Kubernetes Alpha APIs.

3) What is Node?
	A Node is a worker machine in Kubernetes were the workloads run like pods, ingress, etc. and may be either a virtual or a physical machine, depending on the cluster. 
	Each Node is managed by the control plane. 

4) Ingress and Workloads
	Kubernetes Ingress is an API object that helps developers expose their applications and manage external access by 
	providing http/s routing rules to the services within a Kubernetes cluster.
	A workload is an application running on Kubernetes. 
	Whether your workload is a single component or several that work together, on Kubernetes you run it inside a set of pods.

5) HPA and VPA?
	Vertical Pod autoscaling lets you analyze and set CPU and memory resources required by Pods.
	Vertical Pod autoscaling is enabled by default in Autopilot clusters.
	Due to Kubernetes limitations, the only way to modify the resource requests of a running Pod is to recreate the Pod.
	
	The Horizontal Pod Autoscaler changes the shape of your Kubernetes workload by automatically increasing or decreasing the number of Pods in response 
	to the workload's CPU or memory consumption, or in response to custom metrics reported from within Kubernetes or external metrics from sources outside of your cluster.
	Both Autopilot clusters and Standard clusters with node auto provisioning automatically scale the number of nodes in the cluster based on changes in the number of Pods.

6) What are the parameters defined to trigger HPA and VPA?

7) What is CI/CD and CD?
	CI focuses on preparing code for release (build/test),
	Continuous Delivery focuses on ensuring software is always release-ready with manual approval, 
	while Continuous Deployment automates the release process, deploying changes to production automatically once tests pass.
	
8) How to deploy in GKE using Jenkins?
	Github(SCM)-->Jenkins-->Test-->SonarQube-->Docker(build image)-->Artifact registry or container registries-->K8s(a helm chart or a manifest to fetch and create pod)

9) What is Helm charts?
	A Helm chart is a package of yaml and k8s manifest that contains all the necessary resources to deploy an application to a Kubernetes cluster. 
	This includes YAML configuration files for deployments, services, secrets, 
	and config maps that define the desired state of your application.
	
10) How does Cloud SQL proxy communicate with instance and database?
	The Cloud SQL Auth Proxy uses a secure tunnel to communicate with its companion process running on the server. 
	Each connection established through the Cloud SQL Auth Proxy creates one connection to the Cloud SQL instance.
	A SVC account is created and attached to side car container with required permission to communicate with database and instance.

11) What is service account and how does a service account communicate with App and database?
	Create an service account for the app and attach roles to it that can communicate with the DB.
	
12) How do you attach Pubsub to a pod?


13) what is mvn deploy and mvn install?
	mvn:install compiles and installs your component in your local Maven repository, 
	so that you can use it when other components used and developed locally depend on it. 
	mvn:deploy deploys your (previously installed) component to a remote repository
	
14) What are the build phases in Maven?
	validate - validate the project is correct and all necessary information is available
	compile - compile the source code of the project
	test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
	package - take the compiled code and package it in its distributable format, such as a JAR.
	verify - run any checks on results of integration tests to ensure quality criteria are met
	install - install the package into the local repository, for use as a dependency in other projects locally
	deploy - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.
	
Genpact:

1) How to create a branch in github and restrict access to few users?
	Can be done - https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule
	select repo --> under Code and automation select branch --> Next to "Branch protection rules", click Add rule.
	--> Branch name pattern --> Restrict who can push to matching branches --> Restrict pushes that create matching branches

2) What is shared library in Jenkins?
	A shared library in Jenkins is a collection of Groovy scripts shared between different Jenkins jobs. 
	To run the scripts, they are pulled into a Jenkinsfile.

3) How to run parallel stages in Jenkins file?
	pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building the application"'
                // Add commands to build application
            }
        }
        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'sleep 5s'
                        sh 'echo "Running unit tests"'
                        // Add commands to run unit tests
                    }
                }
                stage('Integration Tests') {
                    steps {
                        sh 'echo "Running integration tests"'
                        // Add commands to run integration tests
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying the application"'
                // Add commands to deploy application
            }
        }
    }
}

4) What is Webhook and how does it work?
	Webhooks let you subscribe to events happening in a software system and automatically receive a delivery of
	data to your server whenever those events occur.
	Webhooks are used in a wide range of scenarios, including:

	Triggering CI (continuous integration) pipelines on an external CI server. For example, to trigger CI in Jenkins or CircleCI when code is pushed to a branch.
	Sending notifications about events on GitHub to collaboration platforms. For example, sending a notification to Discord or Slack when there's a review on a pull request.
	Updating an external issue tracker like Jira.
	Deploying to a production server.
	Logging events as they happen on GitHub, for audit purposes.
	
5) can we change the name of pom.xml file? And what is settings.xml?
	Yes it can be changed but mvn command always look for pom.xml file
	Maven provides a settings file, settings.xml, which allows us to specify which local and remote repositories it will use. 
	We can also use it to store settings that we don’t want in our source code, such as credentials.
	
6) What is docker commit?
	docker commit allows you to create a new image from an existing container. 
	This means that you can modify the container and then save it as a new image. 
	You can then use this image to recreate the container or create new containers based on it.
	
7) If you have a docker container running and you what to change the version of the image how can this be done if there is no dockerfile?
	Using docker commit - https://phoenixnap.com/kb/how-to-commit-changes-to-docker-image
   
   how to delete all paused containers in docker?	
   Use the docker container prune command to remove all stopped containers

8) What is du and df in linux command?
	du reports files' and directories' disk usage, df reports how much disk space your filesystem is using. 
	The df command displays the amount of disk space available on the filesystem with each file name's argument.

9) Is sonarqube SAST or DAST?
	SAST
	
10) What is CIDR?
11) What is the difference between app server and web server?
	Web servers deliver static content, like HTML pages, images, videos, and files. 
	Application servers deliver dynamic content, like real-time updates, personalized information, and customer support.
	
12) Write a manifest?
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-rest-api
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello-world-rest-api
  template:
    metadata:
      labels:
        app: hello-world-rest-api
    spec:
      containers:
      - image: in28min/hello-world-rest-api:0.0.3.RELEASE
        name: hello-world-rest-api
	
13) What is the difference between ALB and NLB?
	ALB specializes in intelligent routing at the application layer to distribute the traffic uniformly. 
	NLB, the Network Load Balancer, focuses on high-speed, low-latency traffic handling at the network layer.
	https://blog.cloudcraft.co/alb-vs-nlb-which-aws-load-balancer-fits-your-needs/

14) What is apiVersion in the K8s manifest file? can this be changed?
	This field specifies the version of the Kubernetes API that this definition file is using.
	yes it can be changed for eg: extensions/v1beta1
	
15) Which is the gke version used?
	1.29.4

	
	
16) What is cherrypicking in GitHub?
	Cherry picking is the act of picking a commit from a branch and applying it to another. 
	git cherry-pick can be useful for undoing changes. For example, say a commit is accidently made to the wrong branch. 
	You can switch to the correct branch and cherry-pick the commit to where it should belong.
	-git cherry-pick commitSha
	
17) Where does the Kubectl commands store its configuration file?
	in the home directory in a hidden folder : $HOME/.kube/config
	It contains the list of clusters and the credentials that will be attached to each of those clusters.
	GKE provides them through the gcloud command.
	To view the configuration, either open the config file or use the kubectl command: “config view”.
	
18) How does Kubectl command works?
	For example, let’s say an administrator wants to see a list of Pods in a cluster.
	After connecting kubectl to the cluster with proper credentials, the administrator can issue the kubectl “get pod” command.
	kubectl then converts this command into an API call, which it sends to the Kube API server through HTTPS on the cluster’s control plane server.
	From there, the Kube API server processes the request by querying etcd. The Kube API server then returns the results to kubectl through HTTPS.
	Finally, kubectl interprets the API response and displays the results to the administrator at the command prompt.
	
19) what is predictive scaling vs scheduled scaling?
	Load Forecasting: This predictive method analyzes history for up to 14 days to forecast what demand for the following two days.
	Updated every day, the data is created to reflect one-hour intervals. 
	Scheduled Scaling Actions: This option adds or removes resources according to a load forecast.
	
Cloudifyops:

1) Why only one replica for SonarQube, if there is numerous jobs running will it not crash?
	Firstly, the client barely had 700 users out of which 150 users were onboarded to sonarqube and hardly 100 users were actively working on code scanning on a monthly basis. 
	Since there was no huge incoming to the sonarqube instance the client decided to go with the enterprise edition of sonarqube. 
	But for example the load is huge, the best way to come out is to enable VPA in kubernetes cluster, and that exactly what we have done.

2) Why have you used cloudsql proxy as a side car container?
	To secure the communication between the instance and database. This is a recommended way by Google.

3) What is the difference between K8s manifest and helm charts?
	manifest is a single file whereas helm is a bundled of different manifest.
	
4) Explain Jenkins pipeline work flow?
5) Can we use open source tools to minimize the cost for the clients? How and what can you do to do so?
6) 


Dover - 1
1) What is application Gateway?
	Azure Application Gateway is a web traffic (OSI layer 7) load balancer that enables you to manage traffic to your web applications.

2) What is VPC Peering?
	A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private 
	IPv4 addresses or IPv6 addresses.

3) What is the difference between RBAC & Policy?
	RBAC grants access to users or groups within a subscription whereas policy is defined within the resource group or subscription

4) What are the different types of storages in Azure?
	Azure Blob storage.
	Azure File storage.
	Azure Table storage.
	Azure Queue storage.
	Azure Disk storage.
	Azure Data Lake storage.

5) What variable groups and library?
	Variable Groups allow you to manage and share variables across multiple pipelines, making it easier to maintain and update your configurations.
	A library is a collection of build and release assets for an Azure DevOps project. 

6) Where do you store Api tokens?
	Consider using Azure KeyVault. 

7) What is API management Service?
	Azure API Management is a hybrid, multicloud management platform for APIs across all environments. 
	As a platform-as-a-service, API Management supports the complete API lifecycle.

8) What are stages?
	A stage is a logical boundary in the pipeline. It can be used to mark separation of concerns (for example, Build, QA, and production). 
	Each stage contains one or more jobs. When you define multiple stages in a pipeline, by default, they run one after the other.

9) What are the monitoring tools you have used?
	Azure Monitor.

10) What is the difference between Replica-set, statefulset and deployment?
	StatefulSets are ideal for stateful applications needing stable identities and data persistence. 
	Deployments are better suited for stateless applications requiring rolling updates and scaling capabilities.
	DaemonSets should be used when a single copy of your application must run on all or a subset of the nodes in the cluster.
	A ReplicaSet (RS) is a Kubernetes object that ensures there is always a stable set of running pods for a specific workload.

11) What is control plane?
	The Kubernetes control plane manages clusters and resources such as worker nodes and pods.

12) What is Let’s Encrypt?
	Let's Encrypt is a global Certificate Authority (CA).

13) What are 400 & 401, 502 error?
	400 bad request: All errors with the status code 4xx indicate an invalid request from a client to a server.
	401 unauthorized: This request to the server requires the client to authorize.
	A 502 bad gateway message indicates that one server got an invalid response from another.
	You are not authorized to view this page (HTTP Error 403 - Forbidden) Or. 
	The page cannot be found (HTTP Error 404 - File not found)

14) What is terraform init, plan and apply?
	init: initializes a working directory and downloads the necessary provider plugins and modules and setting up the backend for storing your infrastructure's state.
	plan: Terraform plan: creates a dry-run, determining what actions are necessary to achieve the desired state defined in the Terraform configuration files.
	apply: The terraform apply command executes planned actions, creating, updating, or deleting infrastructure resources to match the new state outlined in your IaC

15) What is terraform provider?
	A provider in Terraform is a plugin that enables interaction with an API. This includes Cloud providers and Software-as-a-service providers.

16) What are groups?
	Active Directory (AD) groups enable administrators to bring together and manage a set of users, computers, or other groups as a single object.

17) What are LRS, ZRS and GRS?
	Microsoft Azure supports 4 different replication services, locally-redundant storage, zone-redundant storage, geo-redundant storage and read-access geo-redundant storage 
	(also known as LRS, ZRS, GRS and RA-GRS respectively.)
	LRS: 3 copies in a physical location within a region
	ZRS: 3 copies across zones within a region
	GRS: LRS in a primary and secondary region
	GZRS: ZRS in a primary region and LRS in a secondary region
18) 


Testing experts.

1) How will you add more volumes to an ebs block and attach to an instance?
2) How will AKS cluster communicate with Azure Devops pipeline?
	To create a Service Connection for a private AKS cluster in Azure DevOps, you need to follow these steps:

	In Azure DevOps, open the Service connections page from the project settings page.
	Choose + New service connection and select Kubernetes.
	Fill in the parameters for the service connection.
	
3) How will you define sonarqube rule to azure pipelines?
	Using service connection.

4) How will a pod scheduled in a particular node?
	You can control which node a specific pod is scheduled on by using node selectors and labels.
	Taint and tolerance

5) There is a pods of 5gb limit and you have two nodes of 25 GB and 32GB, where will the pod gets created?
	It will choose the later one.

6) Tell me the steps to import resources using terraform.
	a) Identify the existing infrastructure you will import and get the resource ID.
	b) Define an import block for the resources.
	import {
	id = "id-1234abcd"
	to = aws_instance.example
	}
	c) Run terraform plan to review the import plan and optionally generate configuration for the resources.(terraform plan -generate-config-out=main.tf)
	d) Apply the configuration to bring the resource into your Terraform state file.
		terraform apply


7) steps to upgrade a K8s cluster.
	a) from the azurecli, view the current version
	   az aks show -g <resource group name> -n <AKS cluster name>| grep -E "orchestratorVersion|kubernetesVersion"
	b) check the available upgrade version
	   az aks get-upgrades --resource-group <RG name> --name <Cluster name> --output table
	c) Setup pod disruption budget for the pods which are critical and maxnode surge(will create a extra node and evict node as per the maxsurge defined lets say you have 5 node than it will drain only two first and 4 running)
		** Please make sure there is no Pod Disruption Budget (PDB) that would be potentially blocking AKS cluster upgrade process. 
		Essentially, “maxUnavailable” and “minAvailable” would need to be set according to your workload. For example, if you have only 1 Pod hosting the application, 
		the option is either not set a PDB or set “maxUnavailable” at “1” and plan the application down time with the application team.
	d) upgrade the cluster using az cli:
		az aks upgrade \
		--resource-group <RG name> \
		--name <AKS cluster name> \
		--kubernetes-version <target K8s version>
		This option will ask two questions: are you sure you want to update the cluster? (y/n)? 
		AND 
		Since you have not defined argument control-plane-only, this will upgrade both CP and node, to version 1.2x.x. (y/N)
	e)  Look at the AKS cluster there is already one node created with latest version and drain node as defined in maxsurge. Atlast the newly created node will be deleted.
	f)	Optionally if you choose only control plane for the upgrade in previous option, Check AKS Node Image Version
		az aks nodepool show \
		 --resource-group <RG name> \
		 --cluster-name <AKS cluster name> \
		 --name <target node pool name> \
		 --query nodeImageVersion
	g) parallelly you can also check the event of the cluster upgrade.
		kubectl get events.
	h) 
8) How can vnet peering be done?


Impetus:

1) How do you upgrade your AKS cluster?
	steps to upgrade a K8s cluster.
	a) from the azurecli
	   az aks show -g <resource group name> -n <AKS cluster name>| grep -E "orchestratorVersion|kubernetesVersion"
	b) check the available upgrade version
	   az aks get-upgrades --resource-group <RG name> --name <Cluster name> --output table
	c) Setup pod disruption budget for the pods which are critical and maxnode surge(will create a extra node and evict node as per the maxsurge defined lets say you have 5 node than it will drain only two first and 4 running)
		** Please make sure there is no Pod Disruption Budget (PDB) that would be potentially blocking AKS cluster upgrade process. 
		Essentially, “maxUnavailable” and “minAvailable” would need to be set according to your workload. For example, if you have only 1 Pod hosting the application, 
		the option is either not set a PDB or set “maxUnavailable” at “1” and plan the application down time with the application team.
	d) upgrade the cluster using az cli:
		az aks upgrade \
		--resource-group <RG name> \
		--name <AKS cluster name> \
		--kubernetes-version <target K8s version>
		This option will ask two questions: are you sure you want to update the cluster? (y/n)? 
		AND 
		Since you have not defined argument control-plane-only, this will upgrade both CP and node, to version 1.2x.x. (y/N)
	e)  Look at the AKS cluster there is already one node created with latest version and drain node as defined in maxsurge. Atlast the newly created node will be deleted.
	f)	Check AKS Node Image Version
		az aks nodepool show \
		 --resource-group <RG name> \
		 --cluster-name <AKS cluster name> \
		 --name <target node pool name> \
		 --query nodeImageVersion
	g) parallelly you can also check the event of the cluster upgrade.
		kubectl get events.

2) What are node pools?
	A node pool is a group of nodes within a cluster that all have the same configuration. Node pools use a NodeConfig specification. 

3) How to auto scale AKS cluster?
	az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --vm-set-type VirtualMachineScaleSets --load-balancer-sku standard --enable-cluster-autoscaler --min-count 1 --max-count 3

4) What is HPA and VPA?

5) How do you manage monitoring for your cluster and logging?

6) How does a node autoscale?
	Using cluster autoscaler.

8) What is the difference between Route table and Security Groups?
	NSGs act like a firewall, controlling inbound and outbound traffic based on security rules. On the other hand, 
	A route table contains a set of rules, called routes, that determine where network traffic from your subnet or gateway is directed.

9) What is the differece between Application gateway and load balancer?
	Azure Application Gateway is a web traffic (OSI layer 7) load balancer that enables you to manage traffic to your web applications. 
	Traditional load balancers operate at the transport layer (OSI layer 4 - TCP and UDP) and route traffic based on source IP address and port, to a destination IP address and port.

10) What are backups and Snapshot?
	Admins can't use snapshots as backups because snapshots rely on delta files to temporarily store VM data locally. Backups 
	store VM data as a direct copy in a separate location, such as the cloud, which enables admins to restore the original VM for disaster recovery (DR) purposes.

11) What is Stakeholder and basic in Azure Devops?
	Stakeholder: Provides partial access, can be assigned to unlimited users for free
	Basic: Provides access to most features.

12) How to use Test Plan in azure devops?
	

13) What are webapps? Have you used? 

14) What are SKU and different types and definetion?
	SKU refers to a specific version or offering of a resource within Azure. It defines the characteristics, capabilities, features, performance levels, and pricing of various
	Azure resources and services like virtual machines, storage accounts, databases, and more.
	For eg: Azure Load Balancer has 3 SKUs - Basic, Standard, and Gateway.differences in scale, features, and pricing.

15) How is memory and cpu utilization defined in a node?

16) How to set GRS for a storage account?
	Navigate to your storage account in the Azure portal.

	Under Data management select Redundancy.

	Update the Redundancy setting.

	Select Save.

17) What is blue green deployment and canary deployment? What are the different types of deployments?
	
	Recreating : Recreating deployment terminates all the pods and replaces them with the new version. 
				 This can be useful in situations where an old and new version of the application cannot run at the same time. 
	
	Rolling: A rolling deployment replaces pods running the old version of the application with the new version without downtime.
	
	Blue/Green : A Blue/Green deployment involves deploying the new application version (green) alongside the old one (blue).
				 A load balancer in the form of the service selector object is used to direct the traffic to the new application (green) 
				 instead of the old one when it has been tested and verified. 
	
	Canary : A Canary deployment can be used to let a subset of the users test a new version of the application or when you are not fully confident about the 
			 new version’s functionality. This involves deploying a new version of the application alongside the old one, with the old version of the application 
			 serving most users and the newer version serving a small pool of test users. The new deployment is rolled out to more users if it is successful.
	A/B
	Ramped Slow Rollout
	Best-Effort Controlled Rollout
	Shadow Deployment

18) How to get logs of a pod deleted 7 days back?
	To retain logs of deleted pods, you can configure log retention for the Log Analytics workspace. 
	This will allow you to store logs for a specified period, even if the pods that generated the logs have been deleted.


Telus
1) Write a script on how to find a text file in all link directory.
	In the provided example, it seeks a file named “sample.txt” within the “GFG” directory.
	find ./GFG -name sample.txt 

2) Write a simple steps for CICD

3) How do you integrate SonarQube with azure devops and github?
	In Azure DevOps, go to Project Settings > Service connections. Select New service connection and then select SonarQube from the service connection list. 
	Enter your SonarQube Server URL, an authentication token, and a memorable Service connection name. Then, select Save to save your connection.

4) How do you tag a github commit with a version?
	Tag the commit with this command: git tag -a M1 e3afd034 -m "Tag Message"
	
4) How to scale a kubernetes deployment?
	kubectl scale --replicas=3 deployment/mysql
	
5) how to see the deployment history in k8s?
	kubectl rollout history deployment/myapp-deployment

	
6) Prometheus & grafana?

7) How to share the config with user when using terraform?

   using env variable.
   can use hashicorp vault to store secrets they just have to define the variable in main.tf --> main.tf will look for variables.tf --> terraform.tfvars will help you to fetch the details.
   can use terraform cloud, in a particular workspace and mark the credentials as sensitive.
   you can also use azure keyvault.
   just have to define the variable in main.tf --> main.tf will look for variables.tf --> terraform.tfvars will help you to fetch the details.
   
8) What is the difference between Azure Pipelines & Releases?
	The Azure DevOps Server provides two different types of pipelines to perform build, deployment, testing and further actions. 
	A Build Pipeline is used to generate Artifacts out of Source Code. 
	A Release Pipeline consumes the Artifacts and conducts follow-up actions within a multi-staging system.

9) What is deployment gate in azure devops?
	Gates allow automatic collection of health signals from external services and then promote the 
	release when all the signals are successful or stop the deployment on timeout.

10) What are different services in K8s?
	ClusterIP: ClusterIP is used for Pod-to-Pod communication within the same Kubernetes cluster.
	NodePort: External client-to-Pod
	LB: External client-to-Pod (Load balancing between nodes)
In contrast, NodePort and LoadBalancer Services are used for communication between applications within the cluster and external clients outside the cluster.

11) What are the vulnerability options for docker ?
	We can integrate synk with the container registry.
	

Hexaware:
1) What are the services you have used in azure?
2) Give a CICD sample setup for a javawebapp?
3) How envs you had?
4) How were you hadling the evn configurations?
5) What were the runners/agents in azure devops you were using in deployments? Who took care of the installations?
   https://www.youtube.com/watch?v=Hy6fne9oQJM&ab_channel=DevOpsCoach
   
6) How did you configure your AZURE DEVOPS Org with different services in azure like AKS, ACR?
	Create a service connection
	a) Complete the following steps to create a service connection for Azure Pipelines.

	b) Sign in to your organization (https://dev.azure.com/{yourorganization}) and select your project.

	c) Select Project settings > Service connections.

	d) Select + New service connection, select the type of service connection that you need, and then select Next.

	e) Choose an authentication method, and then select Next.

	f) Enter the parameters for the service connection. The list of parameters differs for each type of service connection. 
	
	g) Select Save to create the connection.
	
7) Have you used YAML based pipelines?
8) How will you configure a pipeline based on another jobs or stages? Lets say Maven should pass to trigger SonarQube job?
	https://www.youtube.com/watch?v=Tv1bQZqaE7s&ab_channel=DasLearning
	
9) How will you configure a pipeline which is dependent on another pipeline? Lets say one pipeline to another pipeline?
	https://learn.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers?view=azure-devops
	
10) Did you use the concept of template in the pipeline anywere ?
	https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops&pivots=templates-includes
	Templates can help you speed up development. For example, you can have a series of the same tasks in a template and then include the template 
	multiple times in different stages of your YAML pipeline.
	
11) Were did you store the state file for terraform?
	In storage account-> container-> enable versioning.
	
12) What is the commands to check the resources in a statefile?
	The terraform state list command is used to list resources within a terraform state file.
	
13) How was handing the statefile, lets say it got corrupted or some time so how was handling it?
	ALWAYS USE REMOTE STATE – it goes without saying that the local state is planning to fail, especially when working inside a team. 
	With remote state and locking mechanisms in place, you ensure that collaboration can be implemented and there won’t be any race conditions happening.
	IMPLEMENT STATE ENCRYPTION – encryption should be enabled for state files at rest and in transit, and if your remote backend supports it, 
	enable server-side encryption. 
	REVIEW TERRAFORM PLANS BEFORE DOING APPLIES – this will ensure you understand the impact your changes have on your infrastructure and your state.
	VERSION YOUR CONFIGURATION AND USE MODULES WHEREVER POSSIBLE – Your configuration should always be versioned because it will make rollbacks easier. 
	Using modules and versioning them, will also ensure that you can easily roll back to the previous configuration.

14) Tell me one instances were your statefile got corrupted and how did you resolve it?
15) Can two team use the same terraform cloud?
	yes
	
16) how to bring the resources that were manually created in terraform configuration? 
	Using terraform import
	
17) How do you create a resource group for AKS for three envs?
18) What are dynamic blocks in terraform?
	Dynamic Blocks in Terraform let you repeat configuration blocks inside a resource/provider/provisioner/datasource based on a variable/local/expression 
	you use inside them. dynamic blocks are supported inside of resource, data, provider, and provisioner blocks.
	
19) have you worked on modules in terraform? how is the structure?
	a module allows you to group resources together and reuse this group later, possibly many times. 
	Let's say we have two different modules: a "server" module and a "network" module. The module called "network" is where we define and configure 
	our virtual network and place servers in it:
	
		module "server" {
		source        = "./module_server"
		some_variable = some_value
	}

	module "network" {  
		source              = "./module_network"
		some_other_variable = some_other_value
	}
	
20) How will you call the child modules from root modules?
	in the root modules define the path of child module.
	child module may contain your resource block, root module will only have the source path for child module.
	source = "path to child module"
	source = "../../module/ec2"
	
21) What are the different services in AKS?
22) you have to two pods and services how will you route the traffic to the pods?
23) What is the ingress conttroller in K8s? Have you created any ingress controller?
    An API object that manages external access to the services in a cluster, typically HTTP.
	Ingress may provide load balancing, SSL termination and name-based virtual hosting.
	Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. 
	Traffic routing is controlled by rules defined on the Ingress resource.
	
24) Have you worked on URI mapping and disk-mapping in ingress ?
25) How can a K8s secrets access keyvaults for a application in a secure and automated way?
26) How will you debug a pod when it is down?
27) With what you were monitoring this AKS clusters?
28) How will you setup applications in AZure Entra ID?
	
------------

Infogain:
1) Lets say you have an ec2 instance and you want to change the image of instance, so I want to spinup the new ec2 instance first and than terminate the older one. Is it possible with terraform?
	The only way it is possible is to:
	Spin up the new EC2 instance with the desired AMI.
	Verify that the new instance is functioning correctly.
	Once you’re confident, terminate the older EC2 instance.
	
2) What is the use of locals in terraform?
	By using locals, you can reference the same expression or value multiple times without duplicating it in your code.
	This reduces redundancy and makes your configuration more concise.
	
3) How do you fetch secrets from secret manager, and use it in terraform?
4) what if you have not stored the secret in keyvault? How do you fetch it in you code?
5) Lets say we have a main.tf for NSG and lot of rules are defined how can we optimize the code?
	Use Dynamaic blocks.
	
6) What is livenessprobe and readynessprobe?
	The livenessProbe serves as a diagnostic check to confirm if the container is alive. 
	On the other hand, readinessProbe ensures that the container is healthy to serve incoming traffic.
	The kubelet uses startup probes to know when a container application has started. If such a probe is configured,
	liveness and readiness probes do not start until it succeeds, making sure those probes don't interfere with the application startup.
	
7) What you you mean by statefulset and how is it different from deploymentset?
	Deployments are ideal for stateless applications, where each instance is essentially identical and interchangeable. 
	On the other hand, StatefulSets are designed for stateful applications that require stable, unique identities and persistent storage.
	
8) What are different type of docker volumes?
	Remote Volumes: Created and managed on a remote host. 
	Host Volumes: Created and managed on the host machine.
	Third-Party Volume Plugins: Enables the use of external storage systems like cloud storage or 
	distributed file systems as backing storage for Docker volumes.
	
9) What is kubernetes API and what are the different types of API available?
	The Kubernetes API is a critical component of the Kubernetes control plane. 
	It serves as the front end through which users interact with their Kubernetes clusters.
	The Kubernetes API is an application that provides Kubernetes functionality via a RESTful interface.
	It stores the state of the cluster and allows users to manage, create, and configure Kubernetes resources.
	Users can interact with the API directly or use tools like kubectl to communicate with the control plane.
	Types of Kubernetes API:
	The Kubernetes API is organized into different groups based on their purpose:
		Cluster-Level API: Defines resources related to the entire Kubernetes cluster itself.
		Resource-Level API: Focuses on managing workloads and services running on the cluster.
		Extension-Level API: Extends Kubernetes functionality with additional resources and capabilities.
		The Discovery API provides information about the Kubernetes APIs, including names, resources, versions, and supported operations.
		The Kubernetes OpenAPI Document offers comprehensive schemas (OpenAPI v2.0 and v3.0) for all Kubernetes API endpoints, 
		including extensibility components.
		
10) What are CRD's? 
	CRDs enable you to create custom API objects with your own defined structure and behavior.
	Unlike built-in resources, which are predefined by Kubernetes, CRDs let you extend the Kubernetes API with your own types of objects.
	For example, if you need a new resource type specific to your application, you can define it using a CRD.
	When you create a new CustomResourceDefinition, the Kubernetes API Server automatically generates a new RESTful resource path for each version 
	specified in the CRD.
	CRDs can be either namespaced (scoped to a specific namespace) or cluster-scoped.
	Deleting a namespace also deletes all custom objects within that namespace.
	
11) What is Hub and Spoke concept?
	In the hub and spoke model, you have a centralized “hub” that connects to various “spoke” networks.
	The hub acts as a central point of connectivity, while the spokes represent isolated workloads or environments.
	This model simplifies network management, enhances security, and optimizes resource organization.
	
12) You have a 3-tier application in k8s and we want to ensure that all pods should communicate with each other but I want to ensure the podA should not talk to podC, how can this be done?
	To achieve this network isolation in your Kubernetes cluster, you can use Network Policies. 
	Network Policies allow you to control traffic flow at the IP address or port level for specific applications within your cluster. Let’s break down the steps:

	Understanding Network Policies:
		Network Policies are an application-centric construct in Kubernetes.
		They specify how a pod is allowed to communicate with other network entities (such as pods or namespaces) over the network.
		NetworkPolicies apply to connections with a pod on one or both ends and are not relevant to other connections.
	Creating a Network Policy:
		You’ll create a NetworkPolicy that restricts communication between pods A and C.
		The policy will allow communication between all other pods (including A and B).
		apiVersion: networking.k8s.io/v1
		kind: NetworkPolicy
		metadata:
		  name: allow-pod-a-to-b
		spec:
		  podSelector:
			matchLabels:
			  app: my-app
		  ingress:
			- from:
				- podSelector:
					matchLabels:
					  app: my-app
				- podSelector:
					matchLabels:
					  app: pod-b

Q) What is ingress and ingress controller?

   Just like other objects in K8s ingress is also a type of object K8s which is mainly referred as set of redirection rules.
   Where as ingress controller is like other deployment objects(could be demon set as well) which listen and configure those ingress rules.
   
Q) what is quota and limit range in k8s?
	Resource Quotas:
	A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace.
	It can limit:
	The quantity of objects (such as pods, services, etc.) that can be created in a namespace by type.
	The total amount of compute resources (CPU, memory) that may be consumed by resources in that namespace.
	
	How it works:
	Different teams work in different namespaces (which can be enforced with RBAC).
	The administrator creates one ResourceQuota for each namespace.
	Users create resources within the namespace, and the quota system tracks usage.
	If creating or updating a resource violates a quota constraint, the request fails.
	For CPU and memory resources, every new pod must set a limit or request for that resource.
	Other resources (e.g., storage) are enforced by ResourceQuota but allow pods without explicit limits or requests.
	
	Example policies:
	Allocate specific CPU and memory resources to different teams or namespaces.
	Reserve resources for future allocation.
	Enforce limits on specific namespaces (e.g., “testing” namespace using 1 core and 1GiB RAM).
	Contention for resources is handled on a first-come-first-served basis.
	Remember that neither contention nor changes to quota affect already created resources.
	
	Below is an example of a Kubernetes manifest for a ResourceQuota:
	
	apiVersion: v1
	kind: ResourceQuota
	metadata:
	  name: my-resource-quota
	  namespace: my-namespace
	spec:
	  hard:
		pods: "10"  # Maximum number of pods allowed
		requests.cpu: "2"  # Total CPU requests (sum of all pods)
		requests.memory: 4Gi  # Total memory requests (sum of all pods)
		limits.cpu: "4"  # Total CPU limits (sum of all pods)
		limits.memory: 8Gi  # Total memory limits (sum of all pods)

	
	Resource Limits:
	Requests and limits control resources (CPU, memory) for containers within pods.
	Requests:
	Guaranteed resources for a container.
	The container will only be scheduled on a node that can provide these requested resources.
	
	Limits:
	Maximum allowed resources for a container.
	Ensures a container never exceeds a certain value.
	Specify resource requests and limits in your pod definitions.
	
	Below is an example of a Kubernetes manifest that combines both Resource Limits and Resource Requests for a pod:
	
	apiVersion: v1
	kind: Pod
	metadata:
	  name: my-pod
	spec:
	  containers:
	  - name: my-container
		image: nginx:latest
		resources:
		  limits:
			cpu: "500m"  # Maximum CPU usage (0.5 cores)
			memory: "512Mi"  # Maximum memory usage
		  requests:
			cpu: "200m"  # Minimum CPU required (0.2 cores)
			memory: "256Mi"  # Minimum memory required

	
	
Q) Help me to create a ingress in kubernetes?
	Certainly! In Kubernetes, an Ingress is an API object that manages external access to services within a cluster, typically for HTTP traffic. It provides features 
	like load balancing, SSL termination, and name-based virtual hosting. Let’s walk through the steps to create an Ingress:
	
	1) Prerequisites:
		You need an Ingress controller to handle the Ingress rules. Simply creating an Ingress resource won’t have any effect.
		Deploy an Ingress controller such as ingress-nginx. There are various Ingress controllers available, but they should ideally fit the reference specification.
		
	2) Create an Ingress Resource:
		An Ingress resource defines how external traffic should be routed to services within the cluster.
		Here’s a minimal example of an Ingress resource YAML:
		
		apiVersion: networking.k8s.io/v1
		kind: Ingress
		metadata:
		  name: minimal-ingress
		  annotations:
			nginx.ingress.kubernetes.io/rewrite-target: /
		spec:
		  ingressClassName: nginx-example
		  rules:
			- http:
				paths:
				  - path: /testpath
					pathType: Prefix
					backend:
					  service:
						name: test
						port:
						  number: 80

--------------------------
Azure-Pipeline
---------------
# Comcast GenLite Genie
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml
 
trigger:
 - dev
 - main
 
pool:
  vmImage: ubuntu-latest
 
variables:
  ${{ if eq( variables['System.PullRequest.TargetBranch'], 'dev' ) }}:
    env: 'Development'
  ${{ else }}:
    env: 'PROD'
 
stages:
- stage: BuildAndPushImages
  displayName: Build And Push Images
  dependsOn: []
  jobs:
  - deployment: BuildDockerImages
    environment:
     name: $(env)
    strategy:
      runOnce:
        deploy:
          steps:
          - checkout: self
            clean: true
          - task: Docker@2
            displayName: Docker Build and Push Dev
            condition: |
              and(
                or(eq(variables['System.PullRequest.TargetBranch'], 'dev'), eq(variables['Build.SourceBranch'], 'refs/heads/dev')),
                succeeded()
              )
            inputs:
              containerRegistry: 'GenLite-ACR-DEV'
              repository: 'genlite-genie'
              command: 'buildAndPush'
              Dockerfile: '**/Dockerfile.Dev'
              tags: "$(Build.BuildNumber)"
          - script: |
              curl -LJO https://unified-agent.s3.amazonaws.com/wss-unified-agent.jar
              echo Unified Agent downloaded successfully
              java -jar wss-unified-agent.jar
            env:
              WS_APIKEY: '$(APIKEY)'
              WS_USERKEY: '$(USERKEY)'
              WS_WSS_URL: 'https://saas.whitesourcesoftware.com/agent'
              WS_PRODUCTNAME: 'GTO-AIX-Genlite'
              WS_PROJECTNAME: 'WS-genlite'
            displayName: 'Mend Unified Agent Scan'
          - task: Docker@2
            displayName: Docker Build and Push Prod
            condition: |
              and(
                or(eq(variables['System.PullRequest.TargetBranch'], 'main'), eq(variables['Build.SourceBranch'], 'refs/heads/main')),
                succeeded()
              )
            inputs:
              containerRegistry: 'GenLite-ACR-PROD'
              repository: 'genlite-genie-prod'
              command: 'buildAndPush'
              Dockerfile: '**/Dockerfile.Prod'
              tags: "$(Build.BuildNumber)"
 
- stage: DEV
  displayName: DEV
  dependsOn: [BuildAndPushImages]
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/dev'))
  jobs:
  - deployment: Refresh_Dev_WebApp
    environment:
     name: Development
    strategy:
      runOnce:
        deploy:
          steps:
          - checkout: self
            clean: true
          - task: AzureWebAppContainer@1
            inputs:
              azureSubscription: 'IXT-GenAI-DEV'
              appName: 'genlite-genie'
              containers: 'genlitecrdevwest.azurecr.io/genlite-genie:$(Build.BuildNumber)'
              containerCommand: 'python src/main.py'
 
- stage: PROD
  displayName: PROD
  dependsOn: [BuildAndPushImages]
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: Refresh_Prod_WebApp
    environment:
     name: PROD
    strategy:
      runOnce:
        deploy:
          steps:
          - checkout: self
            clean: true
          - task: AzureWebAppContainer@1
            inputs:
              azureSubscription: 'IXT-GenAI-prod'
              appName: 'genlite-genie-prod'
              containers: 'genlitecrprodwest.azurecr.io/genlite-genie-prod:$(Build.BuildNumber)'
              containerCommand: 'python src/main.py'


    ---------------------------

    Terraform:
    -----------------

    Terraform Interview Question:
-----------------------------

1) How does terraform import command works?
   terraform import aws_instance.foo i-abcd1234 (In this case you may have to write down the resource block with required options)
   Another option is to use import block under terraform:
   import{
   to = resource_name #aws_instance.example
   id = "abcd1234" # your instance id
   }
   
   Save the configuration.
   Terraform init
   if you want to generate the configuration automatically, you can run the following command:
   terraform plan -generate-config-out=generated_resources.tf
   terraform apply --auto-approve
   
2) Were did you store the state file for terraform?
	In storage account-> container-> enable versioning.
	terraform {
	  backend "azurerm" {
		resource_group_name  = "StorageAccount-ResourceGroup"  # Can be passed via `-backend-config=`"resource_group_name=<resource group name>"` in the `init` command.
		storage_account_name = "abcd1234"                      # Can be passed via `-backend-config=`"storage_account_name=<storage account name>"` in the `init` command.
		container_name       = "tfstate"                       # Can be passed via `-backend-config=`"container_name=<container name>"` in the `init` command.
		key                  = "prod.terraform.tfstate"        # Can be passed via `-backend-config=`"key=<blob key name>"` in the `init` command.
		use_azuread_auth     = true                            # Can also be set via `ARM_USE_AZUREAD` environment variable.
	  }
	}

	
3) What is the commands to check the resources in a statefile?
	The terraform state list command is used to list resources within a terraform state file.
	
4) How was handing the statefile, lets say it got corrupted or some time so how was handling it?
	ALWAYS USE REMOTE STATE – it goes without saying that the local state is planning to fail, especially when working inside a team. 
	With remote state and locking mechanisms in place, you ensure that collaboration can be implemented and there won’t be any race conditions happening.
	IMPLEMENT STATE ENCRYPTION – encryption should be enabled for state files at rest and in transit, and if your remote backend supports it, 
	enable server-side encryption. 
	REVIEW TERRAFORM PLANS BEFORE DOING APPLIES – this will ensure you understand the impact your changes have on your infrastructure and your state.
	VERSION YOUR CONFIGURATION AND USE MODULES WHEREVER POSSIBLE – Your configuration should always be versioned because it will make rollbacks easier. 
	Using modules and versioning them, will also ensure that you can easily roll back to the previous configuration.
	Restore from Backup:
	If you have a backup of your previous working state file, you can restore it. Here’s how:
	Move your current state file to a different safe folder or directory (just in case).
	Rename the backup state file to be your new terraform.tfstate file.
	Run terraform plan again to verify that everything is in order.

5) Tell me one instances were your statefile got corrupted and how did you resolve it?
6) Can two team use the same terraform cloud?
	yes
	
7) how to bring the resources that were manually created outside terraform configuration? 
	Using terraform import
	
8) How do you create a resource group for AKS for three envs?
9) What are dynamic blocks in terraform?
	Dynamic Blocks in Terraform let you repeat configuration blocks inside a resource/provider/provisioner/datasource based on a variable/local/expression 
	you use inside them. dynamic blocks are supported inside of resource, data, provider, and provisioner blocks.
	Dynamic blocks in Terraform allow you to dynamically construct repeatable nested blocks within your Terraform configurations. 
	They are particularly useful when you need to manage configurations that can vary in size or composition
	
	
	
10) have you worked on modules in terraform? how is the structure?
	a module allows you to group resources together and reuse this group later, possibly many times. 
	Let's say we have two different modules: a "server" module and a "network" module. The module called "network" is where we define and configure 
	our virtual network and place servers in it:
	
		module "server" {
		source        = "./module_server"
		some_variable = some_value
	}

	module "network" {  
		source              = "./module_network"
		some_other_variable = some_other_value
	}
	
11) How will you call the child modules from root modules?
	in the root modules define the path of child module.
	child module may contain your resource block, root module will only have the source path for child module.
	source = "path to child module"
	source = "../../module/ec2"   
	
12) Tell me the steps to import resources using terraform.
	a) Identify the existing infrastructure you will import and get the resource ID.
	b) Define an import block for the resources.
	import {
	id = "id-1234abcd"
	to = aws_instance.example
	}
	c) Run terraform plan to review the import plan and optionally generate configuration for the resources.(terraform plan -generate-config-out=main.tf)
	d) Apply the configuration to bring the resource into your Terraform state file.
		terraform apply

---------------------------------
Helm
----------

Q) Looping in helm and flow control?

In Helm, you can control the flow of template generation using various control structures. Let’s explore two important ones: if/else and range.

1) If/Else:
	The if/else block allows you to conditionally include or exclude blocks of text in a template.
	The basic structure for a conditional looks like this:
	
	{{ if PIPELINE }}
	  # Do something
	{{ else if OTHER PIPELINE }}
	  # Do something else
	{{ else }}
	  # Default case
	{{ end }}
	
	In Helm, we use pipelines instead of simple values. A pipeline can execute an entire sequence, not just evaluate a value.
	For example, consider a ConfigMap with a conditional for a coffee mug:
	
	apiVersion: v1
	kind: ConfigMap
	metadata:
	  name: {{ .Release.Name }}-configmap
	data:
	  myvalue: "Hello World"
	  drink: {{ .Values.favorite.drink | default "tea" | quote }}
	  food: {{ .Values.favorite.food | upper | quote }}
	  {{ if eq .Values.favorite.drink "coffee" }}
	  mug: "true"
	  {{ end }}
	
	If the drink value is set to “coffee,” the output will include a mug: "true" flag.
	
Q) What are pipelines in helm?
   	A pipeline lets you connect multiple functions together, passing the output of one function as the input to the next.
	
	In Helm templates, you create pipelines using the vertical bar character (|).
	
	# Example: Quoting a value from .Values.favorite.drink
	drink: {{ .Values.favorite.drink | quote }}
	
	In this example, we use the quote function to wrap the value from .Values.favorite.drink.
    The pipeline sends the result of .Values.favorite.drink to the quote function.
	
	Pipelines allow you to chain multiple functions together:
	food: {{ .Values.favorite.food | upper | quote }}
	Here, we first apply the upper function (which converts the food value to uppercase) and then pass the result to quote.
	
	Suppose your .Values.favorite.drink is “coffee” and .Values.favorite.food is “pizza.”
	Using pipelines, the template would produce:
	
	drink: "coffee"
	food: "PIZZA" # Since in the food: {{ .Values.favorite.food | upper | quote }} we have applied upper function.
	
Q) What is template engine in helm?
	The template engine within Helm allows you to encapsulate variables and functions within your Kubernetes manifests.
	It fosters adaptability and efficiency by injecting dynamic values from the values.yaml file into your manifests.
	
	In a Helm chart, you might have a Deployment manifest with placeholders like:
	
	spec:
	  replicas: {{ .Values.replicaCount }}
	  containers:
		- name: my-app
		  image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
	
	Here, .Values.replicaCount and .Values.image are placeholders that get replaced with actual values during rendering.
	
Q) How to manage whitespaces in helm?
	Let’s explore a few techniques for handling whitespace effectively:
	
	Chomping Whitespace:
	--------------------
	   Helm provides special characters to control whitespace around template actions.
	   The delimiters {{ and }} can be augmented with a dash (-):
		{{- will consume all whitespace to the left of the action, including newlines.
		-}} will consume all whitespace to the right of the action, including newlines.
	   Whitespace includes spaces, tabs, and newlines.
	   Use these modified delimiters to manage indentation and spacing in your templates15.
		
	   Example:
	   {{- define "example.volumemounts.gcs" -}}
	   {{- if not .Values.local -}}
	   - name: /example/gcs-creds
	     value: gcs-key
	   {{- end -}}
	   {{- end -}}
	   
	   Make sure there is a space between the - and the rest of your directive. {{- 3 }} means "trim left whitespace and print 3" while {{-3 }} means "print -3".

Q) What is _helpers.tpl in helm?
	Purpose of _helpers.tpl:
	_helpers.tpl is a conventionally named file within a Helm chart.
	It serves as a central location for defining reusable template functions, variables, or other utilities that can be shared across multiple templates within the chart.
	Think of it as a place to store helper functions that you can re-use throughout your chart.
	
	Inside _helpers.tpl, you can define named templates using the define action.
	These named templates can encapsulate logic, calculations, or any other functionality that you want to use across different templates.
	For example:
	{{- define "mychart.myhelper" }}
	{{- printf "Hello from my helper!" }}
	{{- end }}

In this example, we’ve defined a named template called "mychart.myhelper" that simply prints a message.

Q) What is dryrun in helm?

	In Helm, the dry run feature allows you to test a Helm chart template rendering without actually installing anything on your Kubernetes cluster. Here are the relevant commands and their explanations:

	helm install --dry-run:
	Use this command to simulate an installation without actually deploying the resources.
	Helm will render the templates, generate the YAML manifests, and display them, but it won’t create any actual Kubernetes objects.
	Example:
	helm install <chartName> <repoName>/<chartPath> --dry-run
	
	helm template --validate:
	This command renders the chart templates and validates them against the Kubernetes API server.
	It checks whether the produced YAML is valid for the objects supported by the cluster.
	If the chart includes custom resource definitions (CRDs), this command ensures they are processed correctly.
	Example:
	helm template <chartPath> --validate

	helm lint:
	The helm lint command is different from the above two.
	It runs additional checks on the unrendered chart (before template rendering).
	For example, it verifies that files in the templates/ directory have valid extensions (e.g., .tpl, .yml, .yaml, or .txt).
	Example:
	helm lint <chartPath>
------------------------------------
Telus
-------------------
Q) How to create release pipeline with multiple Environments in azure devops?

	Creating a release pipeline with multiple environments in Azure DevOps involves defining stages for each environment (e.g., development, testing, production) 
	and configuring the necessary deployment steps. Let’s break down the process:
	
	Create a New Release Pipeline:
	In your Azure DevOps project, navigate to Releases.
	Click New and then select New release pipeline.
	Choose Empty job since we’ll add the tasks later.
	
	Define Stages:
	Name your first stage (e.g., Development).
	Add two more stages (e.g., Staging and Production).
	For each stage, select Empty job and rename it accordingly.
	
	Configure Artifacts:
	Next to Artifacts, click Add.
	Specify the project, the build pipeline, and the default version.
	Click Add when you’re done.
	
	Customize Each Stage:
	Within each stage, add the necessary tasks for deployment.
	For example:
	In the Development stage, deploy to a development environment.
	In the Staging stage, deploy to a staging environment.
	In the Production stage, deploy to the production environment.
	
	Variables and Conditions:
	Define environment-specific variables (e.g., connection strings, API keys) in your pipeline.
	Use conditions to control when each stage runs. For example:

	stages:
	- stage: Development
	  jobs:
	  - job: DeployDev
		steps:
		  # Deployment tasks for dev environment

	- stage: Staging
	  condition: succeeded('Development')
	  jobs:
	  - job: DeployStaging
		steps:
		  # Deployment tasks for staging environment

	- stage: Production
	  condition: succeeded('Staging')
	  jobs:
	  - job: DeployProd
		steps:
		  # Deployment tasks for production environment
		  
	Triggering Builds:
	Configure triggers to start the release pipeline when changes occur (e.g., commits to specific branches).
	Approvals and Monitoring:
	Set up pre-deployment approvals if needed (e.g., manual approval before deploying to production).
	Monitor the progress of each stage and approve deployments when ready.
	
Q) How would you set up a continuous integration and continuous delivery (CI/CD) pipeline in Azure DevOps? 
	What are the main components and stages involved in this process?

1. Create an Azure DevOps project and repository: Store your codebase in the repository (e.g., Git) for version control.
2. Configure build pipelines: Define tasks to compile, test, and package your application using YAML or classic editor.
3. Set triggers: Enable continuous integration by setting up branch policies and pull request validation.
4. Create release pipelines: Define stages, environments, and deployment strategies for continuous delivery.
5. Link artifacts: Connect build output to release pipelines as input artifacts.
6. Add tasks: Configure pre- and post-deployment conditions, approvals, and gates for each stage.
7. Monitor and optimize: Use analytics and dashboards to track pipeline performance and improve processes.

Main components and stages:
– Build stage: Compile, test, and package the application.
– Release stage: Deploy the packaged application to various environments (e.g., dev, QA, prod).
– Artifacts: Intermediate files generated during the build process, used as inputs for release pipelines.
– Triggers: Events that initiate pipeline execution, such as commits or pull requests.
– Tasks: Individual actions performed within each stage of the pipeline.
– Environments: Target locations where the application is deployed.
– Approvals and gates: Controls to ensure quality and compliance before proceeding with deployments.

Q) How to resolve merge conflict?

Resolving a merge conflict in Git involves addressing conflicting changes made by different contributors. 
Here are the steps to resolve a merge conflict:

Identify the Conflict:
	Merge conflicts occur when competing changes are made to the same lines in a file or when one person edits a file while another person deletes the same file.
	Git will mark the file as conflicted and halt the merging process.
	Open the Conflicted File:
	Use your favorite text editor (such as Visual Studio Code) to open the file with conflicts.
	Look for conflict markers in the file:
	<<<<<<< HEAD
	Your changes
	=======
	Other changes
	>>>>>>> BRANCH-NAME

Resolve the Conflict:
	Decide which changes to keep:
	Keep only your branch’s changes (HEAD section).
	Keep only the other branch’s changes (BRANCH-NAME section).
	Make a brand new change that incorporates both sets of changes.
	Remove the conflict markers (<<<<<<<, =======, and >>>>>>>).

Commit the Resolved File:
	After resolving the conflict, add the file to the staging area:
	git add <file-name>

	Commit the resolved changes:
	git commit -m "Resolve merge conflict in <file-name>"
	
Push or Merge:
	If you’re working on a local branch, push your changes to the remote repository.
	If you’re resolving a pull request, the conflict resolution commit will be included in the pull request.
	
Q) What is git merge and rebase?

	Git Merge: Merging combines changes from one branch into another branch.
	Git Rebase: Rebasing is the process of combining or moving a sequence of commits on top of a new base commit.
				a rebase means moving your branch's starting point to a different commit. 
				It's like pretending you began your work from that new point.
				
Q) What is git stash?
	The git stash command takes your uncommitted changes (both staged and unstaged), saves them away for later use,
	It’s like putting your work aside on a shelf and then later taking it back when needed.
	
	Common Git Stash Commands:
	git stash save "My changes": Save your changes with a descriptive message.
	git stash list: List all stashes.
	git stash apply: Apply the most recent stash (restores your changes).
	git stash pop: Apply the most recent stash and remove it from the stash stack.
	git stash drop: Remove a specific stash.
	git stash branch my-feature: Create a new branch from a stash.
	
Q) Git Pull vs Fetch?

	Git Fetch: Fetch retrieves changes from a remote repository to your local repository without modifying your working directory.
	Git Pull : Pull combines fetching changes with merging them into your current branch.
	
Q) conditions in a azure pipeline to skip a stage?

	In Azure Pipelines, you can use the condition property to control whether a stage runs based on specific conditions. 
	Here’s how you can skip a stage using conditions:

	Using Conditions in YAML:
	In your YAML pipeline definition, specify the condition property for the stage you want to conditionally skip.
	The condition can be based on variables, expressions, or predefined functions.
	
	stages:
  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - script: echo "Building..."
  - stage: Test
    condition: succeeded()  # This stage will run only if the previous stage succeeded
    jobs:
      - job: TestJob
        steps:
          - script: echo "Testing..."
  - stage: Deploy
    condition: failed()  # This stage will run only if the previous stage failed
    jobs:
      - job: DeployJob
        steps:
          - script: echo "Deploying..."

	Available Conditions:
	You can use predefined functions like succeeded(), failed(), canceled(), and custom expressions.
	For example:
	condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
	Remember to adjust the conditions based on your specific requirements. 
	This allows you to skip stages based on the outcome of previous stages or other conditions.
	
Q) SAST vs DAST?
	SAST analyzes an application’s source code, configuration files, and related dependencies without executing the application.
	It identifies flaws, weaknesses, and vulnerabilities in the code itself.
	It scans for issues like insecure coding practices, buffer overflows, and other code-related vulnerabilities.
	SAST tools perform a “white box” analysis, meaning they have access to the underlying framework, design, and implementation.
	Use SAST during the pre-runtime phases of the SDLC (e.g., design and coding) to identify vulnerabilities before code execution.
	
	DAST assesses an application while it is running to find vulnerabilities from an external perspective.
	It simulates attacks and examines the application’s behavior in response.
	DAST tools perform a “black box” analysis, meaning they have no knowledge of the application’s technologies or frameworks.
	It tests the application from the outside in, just like a real attacker would.
	Identifies vulnerabilities in the running application.
	Provides insight into how the application responds to attacks.
	Useful for assessing runtime vulnerabilities.
	Use DAST to assess runtime vulnerabilities after the application is deployed and actively running (usually in a pre-production environment).
	
Q) What is multi tenant application?
	A multi-tenant application is a software architecture where a single instance of an application serves multiple customers or tenants. 
	Each tenant is distinct, and they may belong to different organizations, companies, or groups. 
	
	Examples:
	Business-to-Business (B2B) Solutions: Accounting software, work tracking tools, and other Software as a Service (SaaS) products.
	Business-to-Consumer (B2C) Solutions: Music streaming services, photo sharing platforms, and social networks.
	Enterprise-Wide Platforms: Shared Kubernetes clusters used by multiple business units within an organization.
	Customization: While tenants share the core functionality of the application, they may be given the ability to customize certain aspects, 
	such as the user interface (UI) color or specific business rules. However, they cannot modify the application’s underlying code.
	
	Each tenant might get a unique subdomain under a common shared domain name, using a format like tenant.provider.com. us.contoso.com and eu.contoso.com domains.
	
Q) What is DMZ, how to use DMZ and what are the components used?
	A Demilitarized Zone (DMZ) is a network architecture that provides an additional layer of security between an organization’s internal network 
	(Local Area Network or LAN) and untrusted external networks (such as the internet). 
	The DMZ acts as a buffer zone, allowing controlled communication between internal resources and qualified traffic from the internet. 
	Here are the key points about DMZs:

	Purpose of a DMZ:
	A DMZ isolates external-facing services and resources (such as web servers, DNS servers, FTP servers, and proxy servers) from the internal LAN.
	It allows these services to be accessed via the internet while ensuring that the internal LAN remains secure.
	
	How a DMZ Works:
	The DMZ is typically isolated by a security gateway (such as a firewall) that filters traffic between the DMZ and the LAN.
	It provides a safe space for external-facing servers and resources.
	Ideally, the DMZ is located between two firewalls, ensuring that incoming network packets are observed by a firewall 
	(or other security tools) before reaching the servers hosted in the DMZ.
	
	Components Used in a DMZ:
	Firewalls: Used to filter traffic between the DMZ and the LAN.
	Web Servers: Host external-facing websites and applications.
	Proxy Servers: Centralize web content filtering and simplify monitoring of internet traffic.
	DNS Servers: Resolve domain names to IP addresses.
	FTP Servers: Facilitate file transfers.
	Mail Servers: Handle email communication.
	VoIP Servers: Manage voice over IP services.
	
	In summary, a DMZ provides a secure intermediary network between internal resources and untrusted external networks, ensuring efficient communication while maintaining security. 
	
10)	Explain the key components of K8s?
	Control Node or master node:
	•	etcd is a configuration database stores configuration data for the worker nodes. 
	•	API Server to perform operation on cluster using api calls. 
	•	Controller Manager manages the worker nodes and the Pods in a cluster. 
	•	Scheduler responsible for distribution of the workload.
	Worker Node:
	•	Docker run time environment to run containerized application for eg Docker.
	•	Kubelet service to communicate with controller manager & to read configuration from etcd database. 
	•	Kube Proxy Service to route traffic to the right container.
	
11) Branching strategies, versioning, CICD, Jenkins and Azure Devops, K8s, terraform, docker
12) Azure Services.
13) Microservices arch deployment in k8s.
14) Networking questions.
15) Octopus Deploy
16) azure KeyVault: Azure Key Vault is a cloud service for securely storing and accessing secrets. 
	A secret is anything that you want to tightly control access to, such as API keys, passwords, certificates, or cryptographic keys.
17) Blue-Green Deployment.
	Recreating : Recreating deployment terminates all the pods and replaces them with the new version. 
				 This can be useful in situations where an old and new version of the application cannot run at the same time. 
	
	Rolling: A rolling deployment replaces pods running the old version of the application with the new version without downtime.
	
	Blue/Green : A Blue/Green deployment involves deploying the new application version (green) alongside the old one (blue).
				 A load balancer in the form of the service selector object is used to direct the traffic to the new application (green) 
				 instead of the old one when it has been tested and verified. 
	
	Canary : A Canary deployment can be used to let a subset of the users test a new version of the application or when you are not fully confident about the 
			 new version’s functionality. This involves deploying a new version of the application alongside the old one, with the old version of the application 
			 serving most users and the newer version serving a small pool of test users. The new deployment is rolled out to more users if it is successful.
			 
18) User Permission.
19) VPN: Azure VPN Gateway is a service that can be used to send encrypted traffic between an Azure virtual network and on-premises locations over the public Internet.
		You can also use VPN Gateway to send encrypted traffic between Azure virtual networks over the Microsoft network.
    WAN: Azure Virtual WAN is a networking service that brings many networking, security, and routing functionalities together to provide a single operational interface.
	DMZ: A Demilitarized Zone (DMZ) is a network architecture that provides an additional layer of security between an organization’s internal network 
	(Local Area Network or LAN) and untrusted external networks (such as the internet). 
20) Powershell, bash.
21) What are rolebinding and clusterrole bindings?
Role bindings can exist in separate namespaces to service accounts. Role bindings can link cluster roles, but they only grant access to the namespace of the role binding.
Cluster role bindings link accounts to cluster roles and grant access across all resources.

22) startupprobe vs readinessprobe vs livenessprobe:
    The livenessProbe serves as a diagnostic check to confirm if the container is alive. 
	On the other hand, readinessProbe ensures that the container is healthy to serve incoming traffic.
	The kubelet uses startup probes to know when a container application has started. If such a probe is configured,
	liveness and readiness probes do not start until it succeeds, making sure those probes don't interfere with the application startup.
	
	
23) Azure pipeline flow to deploy a microservice in multiple k8s cluster such as dev, stage, prod?

	scm-->maven build--> push artifact build to artifact --> build and push docker image to ACR --> use env stages to push image to k8s-->
	
	stages:
	- stage: Development
	  jobs:
	  - job: DeployDev
		steps:
		  # Deployment tasks for dev environment

	- stage: Staging
	  condition: succeeded('Development')
	  jobs:
	  - job: DeployStaging
		steps:
		  # Deployment tasks for staging environment

	- stage: Production
	  condition: succeeded('Staging')
	  jobs:
	  - job: DeployProd
		steps:
		  # Deployment tasks for production environment
		  
		  -------------------
		  
		  stage: DEV
		  displayName: DEV
		  dependsOn: [BuildAndPushImages]
		  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/dev'))
		----
		stage: PROD
		displayName: PROD
		dependsOn: [BuildAndPushImages]
		condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
		jobs:







