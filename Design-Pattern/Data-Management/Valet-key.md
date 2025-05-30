# Valet key



---

**22**

<https://youtu.be/N82W4xigYwA>

~~The pattern used a taken to provide the client restrict or direct specific resource. It provide offload the data from application min cost max scalability in performance.~~



The client talk to the application and the application generate a limit time key for client to use. This token also can predefined operations such as reading and writing to storage, the scope of the data or only specific rows in a table. ~~In cloud storage systems the key can specify a container, or just a specific item within a container.~~

So the application sent the key back to client The client with that token can read or write the data from storage directly. The application will not directly handle the data transfer



Here is access token or whatever token you want to use. The client go to upload information with the key to other data service. For example,youtube, you want to upload some video to theyoutube. Soyoutubecreate a valet key for the user and said go to the data service you can upload the image, video directly and the application could be serving other traffic: like people isbrowserlingor vising video. ~~They could see all the traffic while you uploading take too much bandwidth you go over there~~



Data stores have the ability to handle upload and download of data directly, without requiring that the application perform any processing to move this data.

But, this typically requires the client to have access to the security credentials for the store

How to user it

When the application have limited resource; to minimize operation cost, you application is in theAzueoramazoeaws. It could be expensive . You storage could be cheap. Why don't use this pattern offload the data from the application. And allow theclinetto connect it with token.

~~If many of interactions are for uploadedordownload when data is store in remote from different data center.~~



When not useit

Data need transformation.

If the application must perform some task on the data before it's stored or before it's sent to the client

![](../../media/Design-Pattern-Data-Management-Valet-key-image1.png)![](../../media/Design-Pattern-Data-Management-Valet-key-image2.png)![](../../media/Design-Pattern-Data-Management-Valet-key-image3.png)



