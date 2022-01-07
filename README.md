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

