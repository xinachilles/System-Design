# nginx  and LVS f5



---

reverse proxy server

1.distribute the load from incoming requests to several servers

2.hide the existence origin server or servers.

3.caching static content, as well as dynamic content

~~layer 7 and layer 4 ,layer 4 is useipaddress and port numberas the input and get a hash value and sent it to really service~~

~~layer 7 is more expensive than layer 4 and the routing decisions on manycharacteristics of the HTTP header and on the actual contents of the message, such as the URL, the type of data (text, video, graphics), or information in a cookie.~~









we also can use mutiple nginx and using Round Robin DNS to distribute different request to different nginx



drawbacks



1.  client side will cache the ip address and not ways go to DNS
2.  if one service is failed, DNS may continue send the request to that service
3.  the service chosed by DNS is not the best chose





Nginx bittiger



![](../../media/FrontEnd-Service-Gateway-or-Web-Service^J-LB-nginx--and-LVS-f5-image1.png)

reverse proxy



Hide original server

![Reverse Proxy ](../../media/FrontEnd-Service-Gateway-or-Web-Service^J-LB-nginx--and-LVS-f5-image2.png)



SSL termination https, the reverse proxy will Encrypt and decrypt the message





![• Reverse Proxy Hide Original Server Application Firewall SSL Termination Load Balance Cache Compression ](../../media/FrontEnd-Service-Gateway-or-Web-Service^J-LB-nginx--and-LVS-f5-image3.png)









![Load Balancing Load Balancing Methods • Round Robin, optionally weighted • Least Connected, optionally weighted • IP Hash • Generic Hash • Least Time (NGINX Plus), optionally weighted ](../../media/FrontEnd-Service-Gateway-or-Web-Service^J-LB-nginx--and-LVS-f5-image4.png)









least connected : the least connected service will get the next request

IP hash, the same ip address will go to the same service all the time, unbalanced






