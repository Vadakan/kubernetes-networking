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

