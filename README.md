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
