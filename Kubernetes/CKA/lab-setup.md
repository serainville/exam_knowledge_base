# Lab Setup
It is highly advisable to have hands-on experience with running a Kubernetes cluster in order to pass the CKA exam. Provided
below are three lab setups you can use. Which one you choose will be dependent on what resources you have available and affordability.
For the best experience possible you will want to run machines with at least 2 vCPUs and 7.5GB of RAM.

Possible lab environmetns:
- Virtual servers (VirtualBox)
- AWS
- GCP

The following is the recommened server hardware configuation for all nodes. You may run Kubernetes on much smaller hardware, such as 1GB of RAM and 1 vCPU, however, performance may become an issue.

Recommended Server Configuration[^1]

| Component   | Amount |
| ----------- | ------ |
| Storage     | 10 GB  |
| CPU \ Cores | 2      |
| RAM         | 7.5 GB |

[^1]: Much smaller nodes can be used, however, you may experience performance issues if there is not enough resoures for the Kubernete pods and any pods you deploy. 
