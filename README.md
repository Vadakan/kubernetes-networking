# kubernetes-networking


![image](https://user-images.githubusercontent.com/80065996/148438020-db49ce34-5759-44f7-933b-a230529c3bb3.png)

User name for Minikube: docker,tcuser

![image](https://user-images.githubusercontent.com/80065996/148438191-a0d8e707-f9b8-42c6-baba-4f1310b4727c.png)

However this has some issue in above approach. When we plan to move kubernetes cluster to cloud, we have to change volume mount in all the places it configured. so its better to use some placeholder which has same functionality and reference it. this is called "Persistent Volume claims"

![image](https://user-images.githubusercontent.com/80065996/148513435-a950d797-5295-4130-af86-3701317e1213.png)

![image](https://user-images.githubusercontent.com/80065996/148513479-9c19b01d-0016-40d8-b120-21aadcc3a766.png)
============================================================================================
Below is the persistent volume and persisent volume claim

First part "Persistent Volume claim" tells what we need.(we need to find a storage with 20GB of size.)
the second part "Persistent volume" tells how do we going to implement this.(we mention the hostpath/capacity/type of capacity/access modes.)
the size (20 GB in both the part has to match with each other. kubernetes will search for a match of size for 20GB and work on this)
suppose if we move this to cloud and we have EBS storage of 100GB (second part - "Persistent volume"),and if you mention 120GB in first part to match ""Persistent Volume claim"", then it will not work. Second part is total size and first part is how much we need in that total size.

![image](https://user-images.githubusercontent.com/80065996/148515082-d4ec611b-ee8b-4a7c-9ad6-35824b44bb6b.png)

============================
How to link both "Persistent Volume" and "Persistent Volume claim".? by using Strorage class. We need to tell explicitly what kind of storage we needs (SSD or magnetic drive,etcc. SSD will be bit faster)

![image](https://user-images.githubusercontent.com/80065996/148516398-016ac857-5111-4f98-81e3-b80eb3ef2875.png)

At run time , "first part(Persistent Volume claim)" will look for 20GB size Second part("Persistent Volume") in the node(computer),and also it will match "storage class" and "Access type" in both first part and second part. so if a match satisifed, then persistent volume will be created with path mentioned

![image](https://user-images.githubusercontent.com/80065996/148518444-3f145241-d9e0-4c98-b532-b95e32bd7691.png)

We have to mention the name in above deployment correctly. 

Commands to get Persistent Volume and Persistent volume claim:
==============================================================

**kubectl get pv  
to see persistent volume
kubectl get pvc  
to see persistent volume claim**

we use above command because we cannot see persistent volume and persistent volume claims using 
**"Kubectl get all"** command

below image stats that.

We can confirm whether the persistent volume is bound by verifying the **"status Bound" field below**

![image](https://user-images.githubusercontent.com/80065996/148521272-888dcff3-58dc-42a0-baea-f404a16597f7.png)

![image](https://user-images.githubusercontent.com/80065996/148521162-271dfbc9-e101-4e45-b457-6c0a00f40572.png)

![image](https://user-images.githubusercontent.com/80065996/148521611-c6b502cd-9cff-445d-b91c-3e58bde805f9.png)

![image](https://user-images.githubusercontent.com/80065996/148521656-b93dba3a-c378-4f85-8bfa-3e78b40cdcab.png)

![image](https://user-images.githubusercontent.com/80065996/148521693-ae1b6f3d-8717-4584-bf84-9c184ddfcfec.png)

![image](https://user-images.githubusercontent.com/80065996/148521947-aab10dab-883c-483e-9d24-87f1fc9a275e.png)

![image](https://user-images.githubusercontent.com/80065996/148522031-908fccdf-f6e1-4c65-a740-6012465ad718.png)

![image](https://user-images.githubusercontent.com/80065996/148522540-609325e4-4252-43bc-abcb-6b304bf52c51.png)

**AWS Kubernetes Deployment:
![image](https://user-images.githubusercontent.com/80065996/148523833-b5834d59-245a-44ae-9d02-15be9f0c15ec.png)

![image](https://user-images.githubusercontent.com/80065996/148524257-f4385a5a-a66a-478e-9339-1f133013a2b4.png)

**What happend if a node fails which is hosting the "API Gateway" ?. we need to architect our cluster for this scenario as well to 
make API gateway replicated in other active nodes. so we can write Yaml files in such a way that API gateway can run simultaneously 
at two nodes rather than only 1 node which if fails creates an issue.**

like below API gatway in two nodes.
![image](https://user-images.githubusercontent.com/80065996/148524717-862ae5fe-b4f4-4998-a8ff-07f7d92ebb60.png)

**Persistent storage in EBS (Elastic block store)**

![image](https://user-images.githubusercontent.com/80065996/148524805-0849ee27-8bb1-4278-98e0-abed5b6fb8fb.png)

**When node connected to EBS failed and restarted again, it will connect back to EBS again automatically and works perfectly fine**

**Proper set up of cluster like below we are going to set up:**

![image](https://user-images.githubusercontent.com/80065996/148525468-23eba117-ccdd-461f-a7df-c6150f2a414d.png)

**KOPS vs EKS:
**

KOPS -- Given by kubernetes
EKS --- Provided by amazon

In KOPS -- you can see master running on one of the nodes (EC2 machine). 
But in EKS --- you cannot even see the Master running. Master node will be managed by amazon itself.
Also in KOPS, you will get single master by defauly but we can configure another master as well on our own.
Also in KOPS, sometimes Kubernetes cluster monitoring system, will give alerts if master is failing. so we have to manually fix it

**What happens if we terminate the master:?**
**If master node is terminated, there will another master immedicately started by default. also, losing a master will not affect the worker nodes.
Just temporarily we will lose the control over the worker nodes.(just temporarily)**

![image](https://user-images.githubusercontent.com/80065996/148526577-d4618f30-79e8-4418-a5a3-d31eb2810088.png)

![image](https://user-images.githubusercontent.com/80065996/148526632-2d66c69d-e71f-4519-b6b5-79201efe14ee.png)

![image](https://user-images.githubusercontent.com/80065996/148526697-0220e6eb-7a20-44bb-9513-1e66ffbb299b.png)

![image](https://user-images.githubusercontent.com/80065996/148526773-d2479535-8986-41e9-a4c5-040e7ce3d40f.png)

![image](https://user-images.githubusercontent.com/80065996/148526888-611ff529-2e62-4dd9-93c4-b6d726178a47.png)

![image](https://user-images.githubusercontent.com/80065996/148527189-4eab0cbb-0587-49a4-88c3-302a898bec69.png)

![image](https://user-images.githubusercontent.com/80065996/148527844-e3a4b42d-b691-49a1-bc56-df09158b4e9e.png)

![image](https://user-images.githubusercontent.com/80065996/148527907-1bdf97be-5c81-441d-a2aa-ea486e55b07b.png)

![image](https://user-images.githubusercontent.com/80065996/148527940-00fad9e4-9216-4bc4-a4bf-224dece8f556.png)

![image](https://user-images.githubusercontent.com/80065996/148528079-af875c36-7c4a-42b6-b6f5-44caf47d1a27.png)

**AWS fargate:

**we can run kubernetes cluster without Ec2 instances(server) -- Severless** EKS can very well integrates with this. This is one of the
advantage of EKS

![image](https://user-images.githubusercontent.com/80065996/148528562-3afb7068-0e0a-49a8-9540-2bfdb3481d0f.png)

![image](https://user-images.githubusercontent.com/80065996/148530485-61294590-6085-4085-a81f-be0c500b853c.png)

![image](https://user-images.githubusercontent.com/80065996/148530970-4914e6bf-427d-4b5b-91a4-1c88480f9ae5.png)

**EKS kubernetes Cluster creations:**

We are not going to use User interface for creating the kubernetes cluster
We are going to use Command line tool EKSCTL. because User interface is not that friendly

EKSCTL github account : https://github.com/weaveworks/eksctl

We have to create Separate EC2 instance for only issuing commands for EKSCTL in AWS account
choose amazon linux 2

![image](https://user-images.githubusercontent.com/80065996/148533068-eddd6fee-a505-496f-9dfb-069a2fa2ee03.png)

![image](https://user-images.githubusercontent.com/80065996/148533135-1988b7c3-a3f8-41e6-a030-2d397ca01fec.png)

![image](https://user-images.githubusercontent.com/80065996/148533205-c9838ce2-9225-4198-8a47-eacba7951d4d.png)

![image](https://user-images.githubusercontent.com/80065996/148533291-a78d818f-6c83-4c30-bfb0-d924faabd8b2.png)

![image](https://user-images.githubusercontent.com/80065996/148533504-937dddff-ea1d-4980-a73a-c12f5375cf9e.png)

![image](https://user-images.githubusercontent.com/80065996/148534127-0bcb9acc-c685-4b59-9333-a5e2046e93e7.png)

![image](https://user-images.githubusercontent.com/80065996/148534283-195ad222-807a-4e0e-b6c8-8b0b533419f0.png)

Once EC2 instance is ready, issue below commands one by one to install EKSCTL command line tool:

WARNING - you MUST delete your cluster when finished.
-----------------------------------------------------

eksctl delete cluster fleetman


1: Install eksctl
------------------

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

2: Update AWS CLI to Version 2
------------------------------

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Now log out of your shell and back in again.

3: Set up a Group
-----------------

Set up a group with the Permissions of:

AmazonEC2FullAccess
IAMFullAccess
AWSCloudFormationFullAccess

You also need to create an inline policy, using the following:
--------------------------------------------------------------

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "eks:*",
      "Resource": "*"
    },
    {
      "Action": [
        "ssm:GetParameter",
        "ssm:GetParameters"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}


4: Add a user to the group
--------------------------

Use the console to add a user to your new group, and then use "aws configure" to input the credentials

5: Install kubectl
------------------

Warning: check the current default kubernetes version supplied with EKS and install a matching version of kubectl

export RELEASE=<enter default eks version number here. Eg 1.17.0>
curl -LO https://storage.googleapis.com/kubernetes-release/release/v$RELEASE/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

Check the version with "kubectl version --client"


6: Start your cluster!
----------------------

eksctl create cluster --name fleetman --nodes-min=3

**commands execution step by step screenshots:**

![image](https://user-images.githubusercontent.com/80065996/148534955-c07dd562-454f-4781-91b7-9db608b26617.png)
![image](https://user-images.githubusercontent.com/80065996/148534975-c3ff6e2a-0e53-44c0-9fac-bcf56c077fbe.png)
![image](https://user-images.githubusercontent.com/80065996/148534998-daacf6d0-5579-47f8-937f-37b2773f778f.png)

**Every Ec2 instances we are creating will have by default AWS CLI tool installed in it**
**with command starts with "AWS", we can access almost all services in AWS via command line**
![image](https://user-images.githubusercontent.com/80065996/148535125-8decfe21-7f96-4634-a09c-ba31e1beb751.png)
![image](https://user-images.githubusercontent.com/80065996/148535296-83757650-1a13-4924-8453-c1a943f4db53.png)

**Update AWS CLI to version 2:**
2: Update AWS CLI to Version 2
------------------------------

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Now log out of your shell and back in again.

![image](https://user-images.githubusercontent.com/80065996/148535506-ec6ab523-92d1-4417-9dbe-ed40b3ace63a.png)

Exit and come back

![image](https://user-images.githubusercontent.com/80065996/148535575-a598fa2c-b9b8-4fb1-8456-7685a992df05.png)

Now you can see AWS CLI version2

![image](https://user-images.githubusercontent.com/80065996/148535616-0b163344-7f6f-43d3-a1db-ecc863c57a86.png)

**Configure AWS credentials:**
![image](https://user-images.githubusercontent.com/80065996/148535802-1dac5027-fdd7-40ee-a5db-21ef203492e9.png)

**You have to set up IAM User:**
1) Create a group in IAM service and then create a user and allocate the user under the group to access the Ec2 services via AWS CLI
2) Group defines permissions - what are the services you can access with your Role.

Create a group and assign below policy
![image](https://user-images.githubusercontent.com/80065996/148536193-a03d9a98-f2c9-43de-9ae5-5188b71a76d0.png)
![image](https://user-images.githubusercontent.com/80065996/148536319-9b184710-5c6e-4195-a5bd-9171cf6e93bb.png)
![image](https://user-images.githubusercontent.com/80065996/148536382-e7f27a7d-8765-449a-8a5d-972728fa92d9.png)
![image](https://user-images.githubusercontent.com/80065996/148536404-2c9ab9ec-6423-4b3b-b999-a8dbd5205db1.png)

inside the group mentioned the inline policy

![image](https://user-images.githubusercontent.com/80065996/148536478-a0f9675d-53f5-425a-991c-9ff84b58cd2b.png)

![image](https://user-images.githubusercontent.com/80065996/148537808-ffd52d34-c048-4efb-ada2-eb777e70d127.png)

![image](https://user-images.githubusercontent.com/80065996/148537983-7e833ed8-8959-40a4-8a4f-1a23d3373332.png)

![image](https://user-images.githubusercontent.com/80065996/148538073-b826222f-c7d6-4b82-b848-414aa6acf1cc.png)

**We have to follow minimum IAM policies to be secure:**

https://eksctl.io/usage/minimum-iam-policies/

![image](https://user-images.githubusercontent.com/80065996/148538886-ec7b5559-f7ec-41dd-8d4e-0fd854f2fd10.png)

![image](https://user-images.githubusercontent.com/80065996/148538920-c40b8213-bf22-41a7-9e9f-217a75535d79.png)

![image](https://user-images.githubusercontent.com/80065996/148538958-0fde0bee-3804-4516-b615-a2c005f2e564.png)

![image](https://user-images.githubusercontent.com/80065996/148539241-43adaa32-319b-463b-9b2e-69b0bb69acb5.png)

![image](https://user-images.githubusercontent.com/80065996/148539510-7839e620-8aa9-4114-846a-ad7b32394f46.png)



**Distributed logging:**

Every container inside the pod inside the node will produce logs for sure. Even in case of failure applications running inside the pod will produce logs.
so we need to have distributed logging mechanism(software) to capture the logs in case of failure. 

**Logstash or fluentd** are most commonly used for capturing the logs from containers.

![image](https://user-images.githubusercontent.com/80065996/148655500-f91a2729-3610-46e6-a603-ab6cfb3d7dd8.png)

We are going to use fluentd. fluentd is just going to only collect the logs from pods. it will store the logs. It will just collect the data. Thats all its work is done
for storing it will send the logs to another component **called "Elastic search"** **which in turns use database to store the logs and we can query it**.


![image](https://user-images.githubusercontent.com/80065996/148655976-bcbe17e5-ce7d-4419-88dc-291b0ad951be.png)


![image](https://user-images.githubusercontent.com/80065996/148656015-ed6d1973-0ba3-45ca-9ff9-a9862ad2af4c.png)

**ELK - Logstash(or Fluentd) -- Elastic search -- Kibana**

Kibana will give frontend which is showing user friendly way of logs 

![image](https://user-images.githubusercontent.com/80065996/148656205-b4482eba-8497-48c2-b10e-6b6db78abd7f.png)

**Deamon set: This is same as replica set in kubernetes. Difference is if we mention the deamon set, we need not mention the replica set, pod will be **
created in every node of the cluster.

![image](https://user-images.githubusercontent.com/80065996/148676697-20dac312-d987-4249-a74c-91a876f18a3a.png)

**Statefulset := this is same as replica set, we mention number of replicas. Difference is pod will be generated with proper names.in replica set pods will**
**be generated with random auto generated names.**

![image](https://user-images.githubusercontent.com/80065996/148676719-8c97b7fd-9500-4bf9-9297-2a6361982847.png)

**ELK (Fluentd,Elastic search and Kibana -- kubernetes github page itself will provide the files to configure this via container. Udemy course download section**
**has combined version of this architecutre. you can use it.)**

**Configmap - this is similar to deployment file, it will create key,valye pair that every pod can access in the cluster
**

**Monitoring the kubernetes cluster using promotheus and grafana:**

**in case of any node faiure or CPU crunch or any other problem which lead to node failure, We need to monitor and get alert.**
**every cloud provider will give such a alarm service(ex: cloud watch in aws), but we will get only limited analysis by the service and **
**if we want to move the kuberneted cluster from one cloud service provider to another service provider, we need to study their monitoring system,**
**instead we can use common monitoring system. Promotheus is one of that**

**Promotheus is monitoring tool. it works similar to ELK stack. Frontend for prometheus in Grafana (provided by grafana labs)**


**Advanced kubernetes:**

Memory Requests: We can mention the pod definition, like how much space the pod would take to run comfortably.
(specifically we have to mention the RAM for the POD. if a RAM for the POD is more than RAM for the node(EC2 instace) in which it is going to run,
then it will throw the error

**Good scenario:**

![image](https://user-images.githubusercontent.com/80065996/148797817-72727ca2-3e8d-43a1-9d15-d0c531671357.png)

![image](https://user-images.githubusercontent.com/80065996/148798434-b76d22a6-fd93-4637-ba55-5aaaeec0e1fc.png)

**Tyring to deploy the fourth pod which exceeds the RAM:**

![image](https://user-images.githubusercontent.com/80065996/148798629-996a5775-6399-4500-8dcd-25ff64022dd2.png)


![image](https://user-images.githubusercontent.com/80065996/148798721-b9bb1095-eac2-40f3-a28d-7cce2f1df83e.png)

**How to see how may space and RAM left for a node (EC2 instance):**


![image](https://user-images.githubusercontent.com/80065996/148809825-d17773e6-0092-40a2-8630-6925ad75cc7b.png)


![image](https://user-images.githubusercontent.com/80065996/148809908-5ab1bcb1-2727-42c8-9b7d-c4eee7c290fb.png)

Above command shows how much RAM left in the node.

![image](https://user-images.githubusercontent.com/80065996/148811209-6af39bfb-67ce-48e7-aa95-c8286cee1bcc.png)

After checking the free memory we can mention the space needed for pods we are creating.


![image](https://user-images.githubusercontent.com/80065996/148812221-98901e6b-26bf-4c4d-8145-d0223e025c0b.png)


![image](https://user-images.githubusercontent.com/80065996/148812387-3280b908-e50c-4896-8b68-3f91cf0cc63f.png)


![image](https://user-images.githubusercontent.com/80065996/148812676-2f87f90f-9ffd-4989-b998-544f93e80a40.png)

For every field changed in our deployment and if we do 'kubectl apply -f" (re-deploy), new replica set will be created and old replicas will be 
reduced to zero pods.


![image](https://user-images.githubusercontent.com/80065996/148812897-001fd370-dc2d-46f0-8b8e-2772dd7e1126.png)

due to space issue, it is showing still pending.

**kubectl describe node nodename**

above command provides below statistics about memory usage of all the pods running inside the node.

![image](https://user-images.githubusercontent.com/80065996/148815965-69110651-af16-429e-98b0-69f94fe18217.png)


Till now we saw Memory Requests.
**Now its time for CPU Requests:**

We have 2 CPU's available in our node (2 core cpu, we can request 1 CPU for a pod or 2 CPU for a pod depends on the size of the pod
but if we ask more CPU than its available curretly in the node, then it will throw an error)

Ex: I have 2 core CPU, requesting 100m (100 mill CPU, 1000milli CPU equal to 1 CPU)

![image](https://user-images.githubusercontent.com/80065996/148913674-39659af3-e21c-4cb3-be88-74c8132e28ff.png)

Since 100m (100 milli CPU) can be allocated when we have 2 core CPU, there is no issue, it will run smoothly

![image](https://user-images.githubusercontent.com/80065996/148915061-b8dc8a60-eb8b-4b57-8bb6-e956d2874b0a.png)

Now deliberately giving more CPU,

![image](https://user-images.githubusercontent.com/80065996/148915342-f31cddc2-87be-4677-a827-a53d126a13ed.png)

50% of CPU is requested by this pod

![image](https://user-images.githubusercontent.com/80065996/148915844-b273b0d8-3978-4d4d-89c4-3da551f5d4d6.png)

Now trying to exceed the limit

![image](https://user-images.githubusercontent.com/80065996/148916020-79270b41-33f3-498c-90f8-af46e0208fe6.png)

**Pod wont be successfull as it will be in pending state for long time**

![image](https://user-images.githubusercontent.com/80065996/148916262-7a5b11d1-6191-4ede-bc2b-4172139598d3.png)

**Describing the pod to see more details:**

**Pod failed to schedule due to insufficient memory**

**1000 millicore equal to 1 CPU , 2000 millicore equal to 2 CPU**

![image](https://user-images.githubusercontent.com/80065996/148916441-4f1aae8b-2ec2-48f6-b9c0-c82df22d0d16.png)

**Memory and CPU Limits** **("Limits" is different "Request" is different. We saw "memory and CPU requests" above, now we are going to 
see "Limits")**

![image](https://user-images.githubusercontent.com/80065996/148918166-84b64608-1926-4c55-95eb-5ba10d6f6ccd.png)

![image](https://user-images.githubusercontent.com/80065996/148918462-8b0fd3b6-1c6f-4307-a638-564f610cb5d4.png)

if CPU limit is exceeded, Container will continue to run but CPU will be 'clamped' (also called as throttling), kubernetes will not allow
the container to use more CPU than the limit specified by us in the YAML file

**Scenario : Limit of CPU and Memory is same as Limit of CPU and Memory speicifed in YAML file of container going to start**

![image](https://user-images.githubusercontent.com/80065996/148919659-28e95bf1-497f-44ca-b287-0f560f59fb58.png)

**Container started without any issue:**

![image](https://user-images.githubusercontent.com/80065996/148920355-3b7853e1-e213-4fd8-9bcf-da7690abd9b6.png)

Container running smoothly

![image](https://user-images.githubusercontent.com/80065996/148920449-d63edc71-3e4f-4c93-a937-8dc5e66d92ae.png)


Memory exceeding scenario : Limit 300Mi requesting 400Mi

![image](https://user-images.githubusercontent.com/80065996/148920710-468c88fe-8501-4a9a-883e-deb833913efc.png)

**Kubectl apply -f command itself throwing error by checking limit and allowed amounts**

![image](https://user-images.githubusercontent.com/80065996/148920813-767c4455-d012-41cd-93e1-76632d5393ee.png)

**Increasing replica count to exceed limits during runtime sceanrio:**

![image](https://user-images.githubusercontent.com/80065996/148921025-80478638-c916-43d1-87c0-164b38717893.png)

But surprisingly its running well. Because this is not memory increasing at runtime scenario. Limit and resource we set is for individual pods 
and its running perfectly.

![image](https://user-images.githubusercontent.com/80065996/148922275-f9f8ff83-7548-4931-a858-3f8c7c7ee923.png)

**We can mimic that scenario in other way, We can reduce the memory request and limit to 4Mi and redeploy it (kubectl apply), so new replica set
will be created.Since we mentioned very low memory 4Mi (4 MB), memory will not be enough for the pod and container will restart it considering
it reaches the limit**

![image](https://user-images.githubusercontent.com/80065996/148922676-6cd1290a-28d2-462c-8fc9-c4fae18d0638.png)

redeploying it

![image](https://user-images.githubusercontent.com/80065996/148922777-cf45e1a4-4565-44be-80fb-b6c5a54c8799.png)

![image](https://user-images.githubusercontent.com/80065996/148922895-dc0e81bd-0f22-4edb-9d8c-562ec28a5f54.png)

**describe it to see the error:**
![image](https://user-images.githubusercontent.com/80065996/148923380-16301896-a6ae-4ec5-a228-6dacb935d775.png)

![image](https://user-images.githubusercontent.com/80065996/148923497-e65f47d8-85b3-48d7-a97b-b3141343df81.png)

**When you have microservice running in production and it has memory leak issue once in 6 months. memory leaks are 
difficult to trace and if no much developers available, so we can use this memory limit property to set the limit and if once
the pod runing that microservice detects, memory is exceeded, it will terminate the container inside of it and start a new one. 
so we will not worry for next 6 months**

**Kubernets metrics profiling:**

**(used to identify how much space your pod going to need. how to get resources limit for CPU and memory to specify in the YAML file)**

**Two commands used to identify the CPU and memory maximum used by either POD or NODE. But we need to install metrics profiling server to do that
or else below command will fail**

![image](https://user-images.githubusercontent.com/80065996/148927992-abd1356e-3c82-499d-af89-9323ea850810.png)

**Metrics profiling server is another pod which we are going to run**

**Metrics profiling is the addon in the minikube **

**to check the addons use below command**

![image](https://user-images.githubusercontent.com/80065996/148929354-b19b8e2a-ad1b-494e-85b9-607fc7761f77.png)

**We are going to enable below addon:**

![image](https://user-images.githubusercontent.com/80065996/148930053-62a864fa-c533-477b-87ae-cfc68a2e8d5f.png)

![image](https://user-images.githubusercontent.com/80065996/148930864-8dc1a84c-8b77-41ec-a876-04880f0e9c3a.png)

**two pods of Active MQ is running:**

![image](https://user-images.githubusercontent.com/80065996/148932129-3645a07d-24e9-4173-8aad-9de345110fa2.png)

we will do metrics profiling now

![image](https://user-images.githubusercontent.com/80065996/148932485-534afe4a-1f66-4872-a6bc-b5d419a10797.png)


![image](https://user-images.githubusercontent.com/80065996/148932525-d7dbb158-2b42-4331-ab2d-f65c7446b335.png)

![image](https://user-images.githubusercontent.com/80065996/148933360-d21a1615-1741-47c5-a239-13b477ebe10a.png)

Metrics profling always takes time to get the metrics,

Inside kube-system namespace you can see the metrics service and deployment available

![image](https://user-images.githubusercontent.com/80065996/148933588-937b5968-cd96-4649-a400-b35452d51863.png)


it will show maximum CPU and RAM used by each pod like below so that we can mention the amount in out Resource request in YAML

![image](https://user-images.githubusercontent.com/80065996/148934208-8f0f1372-26a7-460b-9a45-eb59b86726cd.png)


**What will happen if a node has sufficient memory. the pod we are requesting is going to occupy 300Mi (300 MB) of maximum size as per 
metrics server. But in YAML file we are mention resource request (RAM request for the same POD with 4Mi (4Mb) size. Eventhough we mention 
very little memory and try to run a pod which consumes more memory, as long as our node is having enough memory kubernetes will allow running the pod**

**mentioning 1MB which is very less in below YAML **

![image](https://user-images.githubusercontent.com/80065996/148935867-2742e0e0-2723-4ad6-9376-afc6957d8cf9.png)


![image](https://user-images.githubusercontent.com/80065996/148936669-32ce349f-1da9-406e-8fd3-27caf1300055.png)

![image](https://user-images.githubusercontent.com/80065996/148936726-e805240c-4114-4d63-877f-58bc91067aa2.png)


![image](https://user-images.githubusercontent.com/80065996/148936648-d43130a9-7439-4cbd-ab4b-cbac534e9b99.png)

**Kubernetes didnt stop the pod from running it happily allocated the space for running**

![image](https://user-images.githubusercontent.com/80065996/148936823-ba83e8ba-b3c8-4dd0-9014-b25fc3724d42.png)


**Seeing metrics on the dashboard:**

Kubernetes has in built dashboard to see the metrics. It will not be as good as promotheus but for small clusters we can use this
its an addon

![image](https://user-images.githubusercontent.com/80065996/148939756-aa454e4c-5c87-4216-94c8-8a44f21c1f20.png)


![image](https://user-images.githubusercontent.com/80065996/148939795-883883a0-f55f-43f0-8498-1c9988bbed87.png)


![image](https://user-images.githubusercontent.com/80065996/148940497-2861369a-4b54-4285-b461-3eed51515146.png)

i will open browser and will display the metrics in the dashboard format. if you press ctrl+c dashboard will go off

**Setting reasonable Resource size:**

metrics-server showing CPU and Memory below based on usage, we can consider this and provide resource based on this

![image](https://user-images.githubusercontent.com/80065996/148967524-400fa35b-d778-4044-9109-56c654cfbd28.png)



**Horizontal pod auto scaling:**


![image](https://user-images.githubusercontent.com/80065996/148968324-45544ed9-5cfe-49a0-b58d-e7e30d1d2abc.png)

![image](https://user-images.githubusercontent.com/80065996/148970484-c93ccd0c-6432-4f26-ad06-e3c8d73ce322.png)


We cannot scale all the services present in microservice architecture.

1) Suppose if a service reads a file and passing values to another service, we cannot replicate this service, because it will send
duplicate values to the destination and it will give wrong results

2) if a service reads data from database and do some calculations and returing the result, we can do replication for that service.  
    so that we can avoid CPU intensive calculation.
    

# Databases are difficult to replicate to many pods. Some database can provide their own replication system like mongo database which provides their
# own replication system that we can use to replicate databases.

**Example: ** 

# for the replication we created, loads will be shared by their respective 'services' in round robin fashion(by default, we can change that if we want)

**1) API gateway needs to be highly available, its functionalty is to just pass the routes to microservice in the cluster**
**2) SQL services cannot be replicated. if SQL permits its own replication then we can do similar to mongo database's own replication system. or else it will give 'split** **memory' issue which give unexpcted results.**
**3) Service like which reads database or file and pass the data to particulat service (only doing that work similar to position simulator passing **
   random data continously to Active Mq service), should not get replicated. Because it will pass duplicate values and will give unexpected results.
**4) Services which reads database and do some calcualtions then those kind of service can be replicated. **
**5) MQ's can be replicated or not --> needs some discssions**


# Topic : Horizontal POD auto scaling 

**Horizontal scaling : create more nodes and make the cluster powerful**
**Vertical scaling : Making the nodes more powerful **


# When we create a deployment with replica as '1' for a pod with CPU requets as 100m (100 milli core), and we can create HPA(horizontal pod auto scaling)
# some rule with condition (if cpu exceeds 50 % of usage, then deployment will be updated automaticall with replica as 2 (auto scaled))


![image](https://user-images.githubusercontent.com/80065996/149307374-30f85b97-3aff-4fc6-86d0-87d18c307b95.png)


if CPU reached 50% usage, you can see below replica updated to 2 (we gave maximum replica of 5 in auto scaling configuration)



![image](https://user-images.githubusercontent.com/80065996/149307491-fe4cc4cc-24ed-4bd0-8dac-b357cbe0281d.png)


![image](https://user-images.githubusercontent.com/80065996/149308597-dcd7319d-cd14-4775-a200-77b60df7342b.png)

# auto scaling will not only scale up the pods, but also scale down the pod count in case of under usage.

**Steps to do auto scaling :**

**Step 1:** **Deployment we are going to create**

CPU i have mentioned as 50m (50 millicore). I am going to create rule saying if our pod reached more than 20% of CPU mentioned ( 20% of 50 - around 10 ), then
auto scale needs to happen. and **maximum number of pod needs to get scaled is 2**

![image](https://user-images.githubusercontent.com/80065996/149314541-5e4b3a34-014d-4d9f-9811-932d5bf641dc.png)

**Step 2 : Deploy the deployment YAML we have created**

kubectl apply -f deplyment.yaml

![image](https://user-images.githubusercontent.com/80065996/149315089-bb2c4200-07f8-4e2c-bfb6-1072cc3f916a.png)


**step 3: **Enable the **metrics-server** to see hoe much memory and CPU the pod is using on its maximum


![image](https://user-images.githubusercontent.com/80065996/149315264-f833efb9-361e-485a-a847-5c1c1aaed3d9.png)


**step 4: now we identify maximum CPU core usage is 14m (14 milli core CPU)**
  # create auto scaling using kubectl command - option 1 --> this is not ideal way to do this. Always we have to use YAML file to achieve this
  
  
 ![image](https://user-images.githubusercontent.com/80065996/149315681-99e6a027-8c58-4270-b6ee-e4eccdf90123.png)

 
# step : 5 you can see auto scaling created from below screeshot:


![image](https://user-images.githubusercontent.com/80065996/149315782-5348e7b9-e630-411e-8f4f-812162b51fd9.png)


**step: 6 you can see pod is auto scaling because the pod reached more than 20 % of CPU usage**

![image](https://user-images.githubusercontent.com/80065996/149316064-982fc4c4-641a-435a-b6ce-ac3250b7d720.png)


**step 7: deployment auto scaled successfully **

![image](https://user-images.githubusercontent.com/80065996/149316212-1d318de1-58ba-4ca4-82fd-02848a6bc30f.png)

**step 8: We can see details of HPA(horizontal pod auto scaling) we created using below kubectl command**


![image](https://user-images.githubusercontent.com/80065996/149316704-c8cba7d9-6e0f-4114-a8a1-e933a0f61078.png)


**step 9: Now i dont want to do this via command line. because our cluser running should always match the YAML we have in definition.**
 **we need to generate the YAML file from the kubectl command**
if we use command, if we delete the cluster, again if cluster is created we neeed to remember this command to create auto scaling


![image](https://user-images.githubusercontent.com/80065996/149318769-eb31fe8d-9441-4a88-9e58-40f6e3e6d855.png)

copy the YAML generated and paste it notepad

**step 10: Delete below content (annotations and timestamp) **

![image](https://user-images.githubusercontent.com/80065996/149319214-086563a2-4076-4080-8fcf-5f31156d2a51.png)


**step 11: Remove below content as well**


![image](https://user-images.githubusercontent.com/80065996/149319395-045633b2-1550-4ec7-b96e-f9fce71476bc.png)


**step 12 : After deleting unnecessary content, below final version**


![image](https://user-images.githubusercontent.com/80065996/149322303-d7c0e5a6-dafa-4ed0-86ac-326a4f5c390d.png)


**step 13: created a file and pasted the version of api here.**


![image](https://user-images.githubusercontent.com/80065996/149321047-bbc82263-337c-4f48-a604-2a60686940f5.png)


**step 14: apply the YAML now**



![image](https://user-images.githubusercontent.com/80065996/149323048-6534e86a-2495-4cd6-9123-026d94957b4c.png)


**step 15: Result:**

![image](https://user-images.githubusercontent.com/80065996/149323125-3b386081-d626-4181-9ce9-58be53a44263.png)


**step 16 : Scaling down**

changing the perecentage target to 90 % (by changing YAML file for auto scaling rules.) so only if 90% out of 50m (50 milli core CPU) is used then scale up should 
heppen and if its less than that scale down should happen

**curent CPU usage :**

**26m (26 millicore) is used till now**


![image](https://user-images.githubusercontent.com/80065996/149325463-b806385c-e76a-4d73-a06d-760c52bef2e9.png)

now changing the rule.YAMl file so that my cluster will sacale down

![image](https://user-images.githubusercontent.com/80065996/149325583-34b5b22a-809f-447a-804d-d5923a547bcf.png)



**mentioned 90 percent, so once i apply it has to scale down since i mentioned minimum number of pods as 1**


![image](https://user-images.githubusercontent.com/80065996/149325722-567f416d-d370-4c78-9727-53c15c236d07.png)


**successfully scaled down:**


![image](https://user-images.githubusercontent.com/80065996/149326458-cc3eabbd-0ecf-400d-8e69-59e54decf146.png)


**could see only 1 pod**

![image](https://user-images.githubusercontent.com/80065996/149326823-7107b3ff-9309-4350-a9f9-09d5a9ac5abc.png)


# Readiness and Liveness probes:



![image](https://user-images.githubusercontent.com/80065996/149350103-c1af1ad6-5485-4229-aa22-59a72ea9dcef.png)



When auto scaling happens if new pods are created, it will always take time for the container running inside the auto scaled pod to recieve user 
requests. so 1/3 rd of the users will always face issues. we can avoid this by using readiness and Liveness probes

![image](https://user-images.githubusercontent.com/80065996/149358086-59006895-c0c5-46da-85af-f47318e32ad0.png)

what it does is once the pod is ready to recieve the requests, then kubernetes will consider the pod as active pod and service created for the
corresponding deployment will pass the request only when pod is ready

port mentioned here is the port in which microservices listening inside the container.

![image](https://user-images.githubusercontent.com/80065996/149358554-94cb3d56-31d2-4110-8323-ef6ab5e9563a.png)


**stimulating the readiness probe sceanrio:**


we have below 3 files:
1) Delpoyment
2) service (nodeport)
3) auto scaling rules - hpa (horizontal pod auto scaling)


![image](https://user-images.githubusercontent.com/80065996/149360540-d5a2e0c5-d7ff-4ff6-a3fd-60ffb8ade6c9.png)

1) Delpoyment:


![image](https://user-images.githubusercontent.com/80065996/149360817-e12d0bec-fd5b-453f-b7c5-31cfab2e30b0.png)


2) services:


![image](https://user-images.githubusercontent.com/80065996/149361005-6503b7fe-9731-4374-afbb-31ceb6c33cb5.png)

3) auto scaling rules - hpa (horizontal pod auto scaling)

![image](https://user-images.githubusercontent.com/80065996/149361174-f4cb4cec-e4b2-4043-bbea-406f71d30c96.png)

After applying kubectl command we have got deployments, replicasets, pods, services, HPA(auto scaling rules) configured as shown below,


![image](https://user-images.githubusercontent.com/80065996/149370233-a65ae029-bb5c-4dc9-a1c8-94704d68d434.png)

now we are going to overload this single pod with many requests so that new pod will be created as part of auto replication but pods will be only
ready after readiness probe and service created will not loadbalance requests untill its ready after readiness probe so that non-ready pod will not 
get requests and can avoid loading delay for our webapp

**opening a git bash and run a curl command infinite times to overload the pod**


![image](https://user-images.githubusercontent.com/80065996/149372208-6963cb46-2942-45ae-a58c-573bd61c32d9.png)

**command to do that below **

![image](https://user-images.githubusercontent.com/80065996/149372758-f6907737-62fd-4c15-8935-27ab53b92086.png)


below image shows, increase in hit increased memory usage and CPU usage and new replica is created as part of auto scaling but there is no delay 
in git bash nor error this is because service will load balance request only if the pod is ready

![image](https://user-images.githubusercontent.com/80065996/149372041-aa2b06b1-65b4-4c9a-984d-41627bbfed22.png)


![image](https://user-images.githubusercontent.com/80065996/149372549-c21144aa-3735-4fcb-977e-28067175e5d6.png)


![image](https://user-images.githubusercontent.com/80065996/149373040-202d7a64-c233-44e6-925a-6e3f073ecf35.png)


![image](https://user-images.githubusercontent.com/80065996/149374402-f3ade940-40ad-4630-b937-1cbf48dc6e8a.png)


**removed the "readiness probe here in the deployment"**

![image](https://user-images.githubusercontent.com/80065996/149374931-dc37bf12-4cee-4e40-b6fb-306fac626b50.png)

going to apply it now,

![image](https://user-images.githubusercontent.com/80065996/149375056-4fec9ecd-b8a4-4caf-8ba2-18571f60058b.png)


now going to bombard the server with requests:

![image](https://user-images.githubusercontent.com/80065996/149375261-80fce4ee-66ee-445b-90ea-c0a4a903cb94.png)


now there is a delay in getting repsonse due to absence of readiness probe

![image](https://user-images.githubusercontent.com/80065996/149375520-160a1720-bb77-46b7-a9ac-44c41b9967d8.png)



# Quality of service and Eviction:

**Scheduler := job of scheduler is to decide which pod needs to be scheduled in which nod. It will check memory and CPU of the RAM of the node and**
**decides to schedule the pod to make sure it will not overload the node by creating more pods **

**Fresh node:**

Whole memory is free. so scheduler can schedule new pods here


![image](https://user-images.githubusercontent.com/80065996/149525346-6045ea91-4de8-492c-a27b-fdd1e72d5ad6.png)


now developer decided to introduce new pods which requests memory RAM as 500 MB as shown below,


![image](https://user-images.githubusercontent.com/80065996/149525941-0436c215-bd18-475a-a0fd-c7b4cb6ee65e.png)


this pod will fit in so scheduler moved this pod into the node

![image](https://user-images.githubusercontent.com/80065996/149526354-897c9c49-07fe-4f1e-ae91-efc9ee2581af.png)

Current usage,

![image](https://user-images.githubusercontent.com/80065996/149526656-e8492b25-c7cb-4472-a0d1-f208112dcbdd.png)

another pod trying to start up which needs memory request of 300mb, out of 900mb in nide, 500mb is already occupied by first nod, remaining 400 mb is free
since second pod need  only 300Mb, scheduler will allocate this second pod in same nod itself


![image](https://user-images.githubusercontent.com/80065996/149527453-b3d02342-872f-48a2-8aa9-9d5a50d91011.png)


second pod allocated in same nod,

![image](https://user-images.githubusercontent.com/80065996/149528816-8b46e8ab-82e0-4fe3-81b5-98395a1ac6c2.png)


but we dont know how much memory this pod is really going to use, if 300Mb is requested as part of memory request in YAML, it does not mean that
it is going to use the entire 300MB of RAM, it may use less oy it may go high as well in runtime


![image](https://user-images.githubusercontent.com/80065996/149529219-e2edf0d9-c2ba-454f-a23b-113aa5f99e9c.png)


**Now request for scheduling 3rd pod is coming from developer, but the difference is there is no memory request mentioned in the YAML**
**its plain request. the problem is we are not really sure how much memory this third pod is going to occupy in runtime not even in approx numbers **
** so its a risk to schedule this in this node as only 100 MB memory left in this node


![image](https://user-images.githubusercontent.com/80065996/149529573-9152bebf-184e-4de1-a409-6f79c83e354f.png)

# Scheduler is not intelligent enough to analyse the memory and allocate the pod, it just allocated even if small amount of memory left in the node 
# as mentioned in the below picture. it just allocated the pod into the node


![image](https://user-images.githubusercontent.com/80065996/149530016-787fc601-89de-489b-8e01-ec2563a0d388.png)


**Pod1 - we know developer gave all the information (resource limit(pod should not exceed) as well as how much memory pod needs to run smoothly)**
**pod2 - developer have only how much memory pod needs to run smoothly and not the limit(pod should not exceed)**
**pod3 - developer didnt give how much memory pod needs to run smoothly and didnt give even the limit (memory pod should not exceed)**

**So as long as pod1 and pod2 runs with less memory than exepcted, third pod will run smoothly but we are not sure when memory will get exceeded and issue comes **
**up. **

Based on the above conditions given by the developer, we are going to label pods as,
1) nice pod - pod1 as it has all the info about memory
2) quite nice pod - pod2 as it does not have memory limit
3) rude pod - pod3 as it does not have memory info and memory limit info.

**the same thing even kubernetes tries to do --> allocates label to the pods based on the details given but in a slight technical way with technial name (giving labels)**


![image](https://user-images.githubusercontent.com/80065996/149531706-c712da03-e240-4511-b516-48b5ec6c25a1.png)


# Qos Labels: (Quality of Service) labels - kubernets allocated to pods based on memory request or cpu request details


![image](https://user-images.githubusercontent.com/80065996/149533013-230b917e-548e-4cae-9aba-6c84583f6ebe.png)



if memory request,memory limit. cpu request,cpu limit -  everything is mentioned then kubernetes will give label as **Qos: Guaranteed** label

if memory request or cpu request is given but limit (memory limit or cpu limit) is not given, then kuberneres will give label as **Qos:Burstable** label

if none of the request(cpu or memory) or limit (memory or limit) is given, then kubernetes will give label as **Qos:BestEffort** label


# Scheduler will use this label to manage the pods in case anything goes wrong. 

# Demonstration of Qos label set up in kuberenets

1) **We are setting memory requests and CPU requests and not the limits - so we should get label as 'Burstable'**



![image](https://user-images.githubusercontent.com/80065996/149537748-8291b3ea-85e1-4dc2-ac5c-77cea76f2e93.png)


![image](https://user-images.githubusercontent.com/80065996/149537805-6f40a19d-c1ce-4e24-8af8-4dd55cfd0d51.png)


![image](https://user-images.githubusercontent.com/80065996/149537983-2cb124c2-ee00-437d-83ea-3f3a29d14721.png)


**'kubectl describe the pod to see the Qos label set up':**


![image](https://user-images.githubusercontent.com/80065996/149538213-1c58f2e9-32c8-45b4-ac72-849e601efab7.png)


![image](https://user-images.githubusercontent.com/80065996/149538319-e88954a6-a3fd-407d-9b40-187b801962b5.png)


**Setting both memory,cpu requests and limits now and check the Qos label for to set "guaranteed":**


![image](https://user-images.githubusercontent.com/80065996/149539202-5183a65f-75f9-43df-8662-09c2356a3323.png)


kubectl apply this now,


![image](https://user-images.githubusercontent.com/80065996/149540082-65483cac-74ac-4ee1-aadc-280992d50b6d.png)

describe the pod,


![image](https://user-images.githubusercontent.com/80065996/149540217-725c42bb-e98e-4e17-9cf8-5f55770fb340.png)



![image](https://user-images.githubusercontent.com/80065996/149540153-ef41945b-ff7b-4c97-989f-7e570a8e96bd.png)


**Now create a pod without memory/cpu requests and limits: this should assign Qos label as "Best effort"**


![image](https://user-images.githubusercontent.com/80065996/149545443-c78ef748-d3d2-4f98-92a8-8c0d1a836f60.png)


kubectl apply it :


![image](https://user-images.githubusercontent.com/80065996/149545778-900b764f-e436-4cb1-acc5-2d901cc9afdb.png)


![image](https://user-images.githubusercontent.com/80065996/149545840-68c6adc8-f3d5-43eb-a5c5-f81f74a5d241.png)


describe the pod,


![image](https://user-images.githubusercontent.com/80065996/149545967-da417e55-6f8f-4066-aa3e-f62ee2a83486.png)


# Demonstration on how this label going to use when node is overloaded:
# Caution: We should take care of node carefully in kuberneted cluster, if we start losing nodes, then slowly your kubernestes cluster will start to fail

**from below diagram,**
**if pod1 exceeds 500 Mb, based on limit settings it will automatically evicted, **
**but the problem really lies with pod2 and pod3, it will not get automatically evicted, thats why this Qos label concept came into picture**


![image](https://user-images.githubusercontent.com/80065996/149547318-7dd07801-efcd-4d42-a89d-11a40f08e659.png)


**Now scenario := second pod getting more requests and it consumes more memory, so what kubernetes scheduler does is that,** **it will evict the pod
with label Qos as 'Best effort' because that is a rude pod** **to free up memory. (since this Qos label is the least prior label)**
**thing to note: Eviction does not mean to remove the pod completely, after few mins, this pod will get created in another node which has enough memory.**
**This is the algorith in which kubernetes works.**


![image](https://user-images.githubusercontent.com/80065996/149549309-86f9e6b8-56a0-4a5c-8cc3-ac67612289d9.png)


**pod 2 evicted and it will start in another node somewhere which is having space in the cluster**

![image](https://user-images.githubusercontent.com/80065996/149549655-83033c30-513a-40ae-85e8-b02dec10e537.png)



**Scenario: 3 suppose if first pod (qos label 'guaranteed' which is nice pod which has memory limit of 500 MB reaches its limit of 500 MB which makes node**
**overloaded, in this case pod with next least Qos label "Burstable" will be evicted by kuberenetes scheduler)**

**node total memory: 600 MB**
**Pod1 := 500MB , limit: 500MB - reached 500MB (from 450 MB) Qos - guarnatee**
**pod2 := no limit, occupied 150MB - Qos - Burstable (this pod will be reomoved by kubernetes scheduler)**


 
![image](https://user-images.githubusercontent.com/80065996/149562153-2280ce4a-8784-4ab2-8223-1eeb07bd845e.png)



# Pod priorities:

**pod priorities are mainly intended to use while creating a pod newly.**

**mostly this will not be used in prodcution because it is not quite stable** **but it may get stable in future. so better you know it**


# sceanrio: 1 if we set priority as zero (0) in the deployment.yaml file

**Higher the priority number higher the priority to pick it up**

if node is having total memory of 1000Mb, if a pod1 with priority 0 with memory request as 500Mb scheduler will schedule this pod in node
next, if second pod with priority 10 comes with 600MB (now total memory of node will be exceeded) now scheduler will consider second pod since
second pod has highest priority (priority number 10), first pod will be terminated and second pod will be scheduled in that node.Terminated pod (pod1) will be
scheduled newly in different node where it has sufficient memmory

pod1 - priority - 0
pod2 - priority - 10

![image](https://user-images.githubusercontent.com/80065996/149563425-5bdbd95a-0a7b-44bf-ab8f-7719d3904404.png)



![image](https://user-images.githubusercontent.com/80065996/149563586-5e808241-2627-410a-868a-4486b40f043f.png)


![image](https://user-images.githubusercontent.com/80065996/149563711-25d5da92-70e8-4150-826d-582e7d7a0db8.png)


![image](https://user-images.githubusercontent.com/80065996/149563764-ccdbe530-ca52-4a70-a316-cc94ffa315fb.png)


# Config Maps and Secrets:

Image is something we cannot alter. We can just use it in YAML file in the deployment. if we want to pass values to image for example passing environment variable,
we need to mention the environment variables like below,



![image](https://user-images.githubusercontent.com/80065996/149614327-b756bd6d-42ed-430a-81b3-d1061f2f2025.png)



![image](https://user-images.githubusercontent.com/80065996/149614339-33103f44-b13d-45c7-ae86-2fa1d440dd8f.png)


# like this we need to mention in the deployment we are using in YAML file. Its kind of repetitive coding
# if the image using database, then we need to mention URL value as well in the deployment YAML file everytime, this is also a repetitive coding.
# we can avoid this by mentioning only once somewhere and can refer it. This is called configm maps and secretes in kubernetes.

**Create a YAML file configmap.YAML**


![image](https://user-images.githubusercontent.com/80065996/149614568-674e917e-f7c9-469a-8424-7efa4bad8082.png)


![image](https://user-images.githubusercontent.com/80065996/149614590-bd21833a-dee8-43d4-befc-78235eb2052d.png)


# YAML file for config map

![image](https://user-images.githubusercontent.com/80065996/149614696-2a25fb59-ec4c-4a39-a90a-44b26131a9f0.png)


Kubectl apply it,

![image](https://user-images.githubusercontent.com/80065996/149614727-bf1178fd-c331-4082-bd8d-ecfe36dc8dc7.png)


![image](https://user-images.githubusercontent.com/80065996/149614779-b2281f2c-ff41-4365-b015-431a03651201.png)


# Consuming config maps as environment variables:

# we can do it in 3 ways. 
 **Way-1 : Kind of hardcoding. This is not proper way of doing it**
 
 # for every value you need you have repeat the block of lines 
 
 ![image](https://user-images.githubusercontent.com/80065996/149615356-4490631c-8ef2-414c-865b-b69c0f849a9b.png)


**and for multiple deployment we have to copy and paste the same code. Our objective is to spread across multiple pods. that is met. if suppose now,**
**any change in database URL or DATABASE password, we have to change only once in config file not in all the places inside the deploymeny.Yaml file**


![image](https://user-images.githubusercontent.com/80065996/149615568-e2ad3dff-abb2-4386-85fd-78ef640ff127.png)



![image](https://user-images.githubusercontent.com/80065996/149615526-696fc24f-4091-4112-bee3-325a81bd0e35.png)



kubectl apply it,


![image](https://user-images.githubusercontent.com/80065996/149615707-0006fd8c-3982-4da9-8395-86282da7c8bc.png)


![image](https://user-images.githubusercontent.com/80065996/149615739-90145774-d33d-494b-be10-914236d82073.png)


now if you are chaning database URL or password change it in config.YAMl so that it will get applied to all the pod spreaded across.


![image](https://user-images.githubusercontent.com/80065996/149615912-6076f4ae-d8d4-4be3-8280-7ce8e93c61e3.png)


![image](https://user-images.githubusercontent.com/80065996/149616287-2f44e307-b8ef-4084-bc60-94f515e5dd6d.png)


**demonstartion:**

**enter into the pod and check whether environmant variable is set or not**


![image](https://user-images.githubusercontent.com/80065996/149616348-21e9263a-6e86-41cf-8dfa-aa28657ac52c.png)


# experiment : changing the value of configmap and check inside the pod whether it chanded un RUNTIME

#  step :1 changing the value of configmap database URL value 

**Initial value:**


![image](https://user-images.githubusercontent.com/80065996/149616419-5137e479-1dc7-44fd-a45d-3f1cad86ad53.png)


**changed Value:**


![image](https://user-images.githubusercontent.com/80065996/149616429-47061cea-886c-47a3-bfae-7f9876805167.png)


**kubectl apply it,**


![image](https://user-images.githubusercontent.com/80065996/149616487-f6719675-ae61-4a17-9232-c053b10ef4a8.png)


![image](https://user-images.githubusercontent.com/80065996/149616505-b3fe6dbb-d1b9-476a-9aec-46f4327eb219.png)


# Error --  new config map value not reflcted inside the pod still old configmap value is present


![image](https://user-images.githubusercontent.com/80065996/149616562-fe91e041-6aab-4c8c-b856-dce474f04aa2.png)

# Existing pod running will not pickup the configmap changes done during the run time. Only way it can sense the change is we have to kill the pod
# have to recreate it so that it can pick up the new changes of config map


![image](https://user-images.githubusercontent.com/80065996/149616742-efd1d48c-3455-442a-8996-25d691a2aea5.png)

deleted the pod and as per replicaset, new pod is created, we are going to get inside the pod by exec into it

We could see new value is updated now. Only the new pod created will pick up the changes in configmap file

![image](https://user-images.githubusercontent.com/80065996/149616805-0a2bd0b2-3b68-477d-b6c7-a4fb2de24ccc.png)


# This problem is resolved by kubernetes recently. this was a bug. Check this and work on it https://github.com/gopaddle-io/configurator

# Temporary solution all the projects are using to avoid this problem:
# We need to get rid of old configmap yaml file and create a new file and change the reference of that config yaml in all the deployment files 
# so that new deployment will be created (old replica sets will be terminated and new pods will be created by picking up the configmap changes)
# note: any change to the deployment file will always create new version (new replica set creation)


# demonstration:

**step1 := going to change the config map yaml file for the change in URL value:**

**Current config yaml file:**


![image](https://user-images.githubusercontent.com/80065996/149617328-7ac69ec7-805f-4e4a-b499-538d78621931.png)


**changed file:**

Changed password so changed the name of configmap as well as highlighted below,


![image](https://user-images.githubusercontent.com/80065996/149617368-9559e562-a201-42a8-b29c-e99601fb4dc9.png)


**step 2: We have to replace the name of configmap used in delpoyment yaml file by checking manually**

Kubectl apply it 


![image](https://user-images.githubusercontent.com/80065996/149617458-8f129c6e-ec1b-4128-a133-924d91fdec50.png)


**Repalced the name of configmap in deployment Yaml file**


![image](https://user-images.githubusercontent.com/80065996/149617756-475f5201-348c-4e82-8303-576dda61bfad.png)



**step : 3 kubectl apply it with new changed in deployment YAML file with changed configmap name so that new replicasets will be created to pick new change**


![image](https://user-images.githubusercontent.com/80065996/149617627-3dc1d9eb-7a3c-4c0f-8e29-132689a0e19b.png)


**step 4: you could see new version of deployment is created as shown below in the form replica set.**


![image](https://user-images.githubusercontent.com/80065996/149617658-c04bb349-9f90-4231-8b4c-13b4aa6067e6.png)


**Step 5: Enter into the new pod by using 'kubectl exec' command and check the environment variable inside whether it picked the new changed in the configmap**

**New changes picked as shown in below image**

![image](https://user-images.githubusercontent.com/80065996/149617822-e404c565-a37f-4423-af2b-bf5392366077.png)




