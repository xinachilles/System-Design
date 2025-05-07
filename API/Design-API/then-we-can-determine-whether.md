# then we can determine whether use partial Get request

Created: 2019-06-11 23:55:42 -0600

Modified: 2019-06-11 23:55:54 -0600

---

then we can determine whether use partial Get request

for example, if the image is huge, we can use the partial request get the a small chunk image first



GET <https://adventure-works.com/products/10?fields=productImage> HTTP/1.1

Range: bytes=0-2499



In the result, We can included an another link pointed to another source (HATEOAS)



We also can add the version on theurland like /v2/.. and router or API gateway?? Will sent the request to different end pointor we can put the version number to parameter







There are two types of API restful and RPC.If the API is client side, most of time they use REST API over the HTTP since it must compatible with browser or mobile application

For the backend side, I think RPC is more efficiency?? than REST API because, most company build own protocol instead of Http. Those protocol supportbinary serializeand more efficiency than HTTP





API gateway is between client and API service. It can be used identify use, provide the SSL offloading, routing, rate limited

Gateway rote is route the request to different backend service , aggregate is combine the multiple service to single one, we also can put some function in the gateway such as identify the user
