# Associate Cloud Engineer

## Section 1: Setting up a cloud solution environment
1.1 Setting up cloud projects and accounts. Activities include
- Creating projects
- Assigning users to pre-defined IAM roles within a project.
- Linking users to G Suite identities
- Enabling APIs within projects
- Provisioning one or more Stackdriver accounts

1.2 Managing billing configuration
- Creating one or more billing accounts
- Linking projects to a billing account
- Establishing billing budgets and alerts
- Setting up billing exports to estimate daily/monthly charges

1.3 Installing and configuring the command line interface, specifically the Cloud SDK (e.g., setting the default project)

## Section 2: Planning and configuring a cloud solution
2.1 Planning and estimating GCP product use using the Pricing Calculator

2.2 Planning and configuring compute resources. Considerations include:
- Selecting appropriate compute choices for a given workload (e.g., Compute Engine, Kubernetes Engine, App Engine)
- Using preemptible VMs and custom machine types as appropriate

2.3 Planning and configuring data storage options. Considerations include:
- Product choice (e.g., Cloud SQL, BigQuery, Cloud Spanner, Cloud Bigtable)
- Choosing storage options (e.g., Regional, Multi-regional, Nearline, Coldline)

2.4 Planning and configuring network resources. Tasks include:
- Differentiating load balancing options.
- Identifying resource locations in a network for availability
- Configuring Cloud DNS

## Section 3: Deploying and implementing a cloud solution
3.1 Deploying and implementing Compute Engine resources. Tasks include:
- Launching a compute intance using Gloud Console and Cloud SDK (gcloud) (e.g., assign disks, availability policy, SSH keys)
- Creating an autoscaled managed instance group using an instance template
- Generating/uploading a custom SSH key for instances
- Configuring a VM for Stackdriver monitoring and logging
- Assessing compute quotas and requesting increases
- Installing the Stackdriver Agent for monitoring and logging

3.2 Deploying and implementing Kubernetes Engine resources. Tasks include:
- Deploying a Kubernetes Engine clster
- Deploying a container application to Kubernetes engine using pods.
- Configuring Kubernetes Engine application monitoring and logging

3.3 Deploying and implementing App Engine and Cloud Functions resources. Tasks include:
- Deploying an application to App Engine (eg., scaling configuration, versions, and traffic splitting)
- Deploying a Cloud Function that receives Google Cloud events (e.g., Cloud Pub/Sub events, Cloud Storage object change notification events)

3.4 Deploying and implementing data solutions. Tasks include:
- Initializing data systems with products (e.g., Cloud SQL, Cloud Datastore, BigQuery, Cloud Spanner, Cloud Pub/Sub, Cloud Bigtable, Cloud Dataproc, Cloud Storage)
- Loading data (e.g., Command line upload, API transfer, import/export, load data from Cloud Storage, streaming data to Cloud Pub/Sub)

3.5 Deploying and implementing networking resources. Tasks include:
- Creating a VPC with subnets (e.g., Custom-mode VPC, Shared VPC)
- Launching a Compute Engine instance with custom network configuration (e.g., Internal-only IP address, Google private access, Static external and private IP address, network tags)
- Creating ingress and egress firewall rules for a VPC (e.g., IP subnets, Tags, Service Accounts)
- Cerating a load balancer to distribute application network traffic to an application (e.g., Global HTTP(s) load balancer, Global SSL Proxy load balancer, Global TCP Proxy load balancer, Regional Network load balancer, Regional Internal load balancer)

3.6 Deploying a Solution using Cloud Launcher. Tasks include:
- Browsing Cloud Launcher catalog and viewing solution details.
- Deploying a Cloud Launcher marketplace solution

3.7 Deploying an Application using Deployment Manager. Tasks include:
- Developing Deployment Manager templates to automate deployment of an applicaiton
- Launching a Deployment Manager template to provision GCP resources and configure an application automatically


## Section 4: Ensuring successful operation of a cloud solution
4.1 Managing Compute Engine resources. Tasks include:
- Managing a single VM instance (e.g., start, stop, edit configuration, or delete an instance)
- SSH/RDP to the instance
- Attaching a GPU to a new instance and installing CUDA libraries
- Viewing current running VM Inventory (instance IDs, details)
- Working with snapshots (e.g., createa a snapshot from a VM, view snapshots, delete snapshot)
- Working with Images (e.g., create an image from a VM or a snapshot, view images, delete an image)
- Working with Instance Groups (e.g., set auto scaling parameters, assign instance template, create an instance template, remove instance group)
- Working with management interfaces (e.g., Cloud Console, Gloud Shell, GCloud SDK)

4.2 Managing Kubernetes Engine resources. Tasks include:
- Viewing current running cluster inventory (nodes, pods, services)
- Browsing the container image repository and viewing container image details
- Working with nodes (e.g., add, edit, or remove a node)
- Working with pods (e.g., add, edit, or remove pods)
- Working with services (e.g., add, edit, or remove a service)
- Working with management interfaces (e.g., Cloud Console, Cloud Shell, Cloud SDK)

4.3 Managing App Engine resources. Tasks include:
- Adjusting application traffic splitting parameters
- Setting scaling parametres for autoscaling instances
- Working with management interfaces (e.g., Cloud Console Cloud Shell, Cloud SDK)

4.4 Managing data solutions. Tasks include:
- Executing queries to retrieve data from data instances (e.g., Cloud SQL, BigQuery, Cloud Spanner, Cloud Datastore, Cloud Bigtable, Cloud Dataproc)
- Estimating costs of a BigQuery query
- Backing up and restoring data instances (e.g., Cloud SQL, Cloud Datastore, Cloud Dataproc)
- Reviewing job status in Cloud Dataproc or BigQuery
- Moving objects between Cloud Storage buckets
- Converting Cloud Storage buckets between storage classes
- Setting object lifecycle management policies for Cloud Storage buckets
- Working with management interfaces (e.g., Cloud Console, Cloud Shell, Cloud SDK)

4.5 Managing networking resources. Tasks include:
- Adding a subnet to an existing VPC
- Expanding a CIDR block subnet to have more IP addresses
- Reserving static external or internal IP addresses
- Working with management interfaces (e.g., Cloud Console, Cloud Shell, Cloud SDK)

4.6 Monitoring and logging. Tasks include:
- Creating Stackdriver alerts based on resource metrics
- Creating Stackdriver custom metrics
- Configuring log sinks to export logs to external systems (e.g., on premises or BigQuery)
- Viewing and filtering logs in Stackdriver
- Viewing specific log message details in Stackdriver
- Using cloud diagnostics to research an application issue (e.g., viewing Cloud Trace data, using Cloud Debug to view an application point-in-time)
- Viewing Google Cloud Platform status
- Working with management interfaces (e.g., Cloud Console, Cloud Shell, Cloud SDK)

## Section 5: Configuring access and security
5.1 Managing Identity and Access Management (IAM). Tasks include:
- Viewing account IAM assignments
- Assigning IAM roles to accounts or Google Groups
- Defining custom IAM roles

5.2 Managing service accounts. Tasks include:
- Managing service accounts with limited scopes
- Assigning a service account to VM instances
- Granting access to a service account in another project

5.3 Viewing audit logs for project and managed services