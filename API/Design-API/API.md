
---

# Rest API:

## Key Concepts
## State Transfer

- REST (Representational State Transfer) is a stateless communication method between client and service
- No need to track state over time
- Each request contains all necessary information

## Evolution from Traditional Methods
- Traditional software used procedure calls
- SOAP (Simple Object Access Protocol) is procedure-based
- REST changed the paradigm from procedures to resources

## Resource-Based Approach
- Resources are accessible via URLs
- Resources can be accessed through web browser address bars
- Leverages built-in HTTP protocol methods (GET, POST, PUT, DELETE, etc.)

## Key Differences
**Old Approach**: Procedure-oriented (like SOAP)

 **REST Approach**: Resource-oriented

- Focuses on resources rather than procedures
- Uses standard HTTP methods
- Stateless communication
- URL-based resource access

## Side effect:

We can do the same operation over and over. I am not going to see different result each time. For example, when you get some from rest endpoint, you should expert to be the same except some come to modify behind the screen.


## Hateoas

~~everything what wegonado is descript by the way with interface with the rest API endpoint~~

~~We should able to operate the resource understand what operate are available and make modification, do everything we need do just through thecommunication of resource of URL. And the verb you are done that. One key ofHateOSis you return the information you can do with dataset. If I go out the high level dataset. I am not just give the information back. I am also getting the location information and operation information~~

~~This is I can do find the subset of the data.. is I can do to ..add subset of the data . It's really just put some format around way you can expose rest endpoint.~~

# API design 2

## Why move WFC to the REST API

Used the browser's HTTP mechanism. It's a well-known protocol. Typically, firewalls are already open to that. There's security around it, and Secure Sockets Layer is so. It just makes for it a good transport, if you will, a means for setting up APIs

But the two real drivers that we saw we're


### Number one

### Number two 

is that rest facilitates this content negotiation and exposing Json data instead of XML


It turns out that Jason was designed with JavaScript in mind. I say that sort of tongue-in-cheek is Jason really is Javascript but it makes it very easy for JavaScript whether it's a a browser client code excetera to take that payload and process it


jason data is smaller than XML


Finding the resource

~~The remote procedure calling is not the way we design we design the rest API. For the RPC: there is resource which is Noun. We have verb added to the Noun.~~

The rest API web have the URL to represent the resource. If you can type in the browser that is the idea of the resource. It should be able to get to it from the browser windows.

It's noun it's wefindits object in question and the verb is going to identify what we want to do.

If I say get, I am looking for download --- GET

If I say put, I am looking for making a modification. -- PUT

If I say post, I am looking for creating a new one. -- POST

When you create a API, you have to ask yourself.



~~Did you build the ways that consumer has figure out all the logic around hosing the process or you build the way you can capture the business logic which were you know and you are expert with you application~~.



Tweet API. Status is a collection. We negative with the status to user timeline, home timeline or user timeline.

Verb:

Get is just simple read. Post, the idea of post is we posting something new to the collection. Use the endpoint will be products and I have the information about new product I go to post.



For post, what should be returned. There is a design question behind the API weather you return the product back with the ID populated or just say ok I got the post



The put is replacement that when I am using for update.



Patch :just modify the part of the object. Maybe you can take partial representation



**How the resource are represent.** Think about the from the perspective who initial the transaction; how much the business logic are you really posting about consume the API vs you are service manage because that's what you know yourapibecause you know your business. You allow people to interact with that business you know.



We are talking about theHateoas. All I had is I got top of the API. I got everything I need. What I receive back is anything possible for me to do with mygithubrepository here.

I can find myfollowee.. get all information in one shot. I don't have to middle multiple call for that.

It is stateless. I don't have to maintain state information even my client giveto me each call.

Avoid of round trip. The more calls you want to made on HTTP. The slow isgonna

That's much better just sent on too much information and avoid all of round trip than sendingdownsmaller or trunk information and let user keep coming back for more data.



[Lesson 3]{.mark}

Role template part of theurland map to thesegmatsyou have. what's template could be match theurlhas the result do?? Couldn't be ambiguous.

If find the template model. Itgonnatake the control piece of thatwheatheryou specific in the template that brace control. Or it's actual had could you may say the default control. But it will take that. It's will appended the control on the end look for the type



The first thing is let's give the control segment find the control. The next thing is find the action because when they come to find the action. It'sgoonaapply all of the rules order/

On the top of the web APIdesployto use HTTP webs to match action. If you don't have action or your action is optimal. It's not go to try to find the method that has the same name you put intourlinstead it's go to the control look the verb userwheatherget, put...

1/There is attribute you can take say it is http put ,get whatever

2/ there is conversation of name method with method started with get.. got

3/if we have dpcrazy thing. It doesn't care about it. It will look theirparamenters. It's going to match upalthe methods areeligiablebase on the controller and action (get postdelet) then it'sgoonafind one wherever the mostparamentersmach

Control really don't carewha'tthe method name what they are about is how the parameters match. What's the action are



4

200 is thing create. typical you retrun somthing had or you don't have to

ussually if you send the thing a great, you have noting to show for. you gonna to use 204; 204 is thing is generated and that is noting to show you;



201 which is create. That create can do a number of things; mayretrun a bad item you just create or even in the header return the location you can ngative the item. So i can say it is crated but i am not give you the header because you already know. you already post to me, if you want to negate have is the location



400 we got bad request. it's goona tell you tell how we actually look the data, valiation. what's going on inside the payload.

401 we go unauthoried. unauthoried is mean who are you , you cann't get resouces at all.

403 - Forbidden me yes, we know who you are , we still not get you get in you haven't get the permession.

404 not found that was code in the delte , we cannot found issue on the id you return back 404

you can build a javascirpt frame work read the status message use then high line and sent the class on the clint site and say this si error. that is what i show in the message.



whay just simple set the status message rather than sending on 404, 400 message.

the main reason is on clinet side if that somthing wrong, they except 400, there is going to throw a exception. ti's sort of over simply that.



if you just simply sent a 200 . it wil simply miss. it's will very simply sikp over gross up by sneding dow the 400. you are making sure the clinet. we have problem have you need to dress us



you API is going to be consume internally or exterally if exteranlly, if exterally more standly you can made, extrally clinet know what is 400 is; what is 404 is; much easy then you set up you onw code with you app.



when you doing the validation .look for good value, don't look for bad value. make sure been provied is inside the good range. if it not inside the good range. it's bad reject. In that point, because problem for bad value is list of bad value is in finitely long that list of good value. look for good, if it is not inside the good, it is must be bad, you can go ahead, throw your exception return back you bad request





Security

We do a "get" for example. We gone return everything. It go to return a list of entries. IfI set up a get method inside these. It's going to return backaindividual entry: most of dome you have been doing you've ben return back a "httpresult" why not just simple returnaobject: the key is what I am assuming. If I assume everything find Igonnacreate a message signature with that object. Because once you have the signature, that is what you have to return.



There a few thing you can do. For example: if I have issue maybe I return none. I can go into the response header or request header and I can set the status. It's still available but I don't have the flexibility I had - return different type. If I useHTTPresult. I can do thing like return mode status instead of the object. serialize some different base on how I can handle that.

If I returnaobject. If something got wrong I have to open the response object andstart to tweaking manually made the code more readable.





Authentication and authorization

Authentication is who are you ; what is your identify I am know to system or not;

Authorization is who you allow to do . what's my permission.



Why I want Facebook authorization on my APP. There are couple of different reason. The bigest part of it is because it'sgonato be much similar to user. The trends are your usergonnahave F account or T account. Those 4 providers. Igonnamake up of this status on top of my head like 87% or other stats made up on thespot I am willing to bet those4 providers probably cover 99.99% of population going to be using your application. So by allowing them user name and password, they currently have ? you are moving abarrier. Honestly a lot of two. I got to say ?? I would look to say sign up for brand-new user nameand new password . I just go to I am not interesting. You are removing thatbarrier and allow people to log in usercredentials they already have.

The another reason I like to user Fauthorization is because there is less security data for me to have to worry about. The reason way to security datais doesn't store it.

So come to F or come to T will all i get is token. That if turn out my database get hack, all the token are released. It's very easy I just go back to F invalidate those token. Problem solve. I don't need worry aboutexposing someone else password. if you use password cross multiple site. You really kind ofopeningyourself up to that. I don't want one of responsible for exposing out your password which the same password that I have in my luggage . I don't want to worry about it I just want to hold on the token,

Authorized attribution. The action will not run unless we have ??untlfor the individual they areauthorized

[if you got the API token and secrets, you can pass them right into the developer API configuration, and then it just know it can handle that type of login]{.mark}



Overrwirtethesyntry to get the user name out of the header. If it is existed then I'm create anidentifyusing ?? you can create a customeridentifytoo. There are a lot offlexablearound it passing in who is user and I'm set theprinclablethere and Http context



token base Authentication

we you give permission to your app,

your app and it says this app would

like to Tweet on your behalf and post Facebook updates without

you knowing and things like that .



It protects revealing the password to the app, so when it's generated

by a third party, I never get the password. I just get the token, and that's all I'm dealing with.



Token is passed in the header



it's going to be through a secured sockets layer ((SSL) and managed

by your code



encrypted in SSL



cross-site request forgery.



Yes. This is kind of a big thing where somebody could potentially simulate my form and be sending in a request from somewhere else that I'm not expecting.

That's a problem.



attack takes advantage of the fact that you may not be testing where your form posts came from, so I stand up a page





on my site, I take your information and make it look like we're on a good site, but then, under the covers, I might be setting some parameters like delete user

or something that you don't want to happen, and then I'm posting

it to the site that's the





ultimate site we're going to. And

that site is just looking for



00:46:23.880 --> 00:46:26.690

a post. It's not necessarily looking

where it's coming form,











lesson 6



/stand/{name} or / stand/name



I'm gonna create a placeholder for name; so that rather than



you always having the same name equals... name = that you can just simply send it as part of the path.



we can pass the name from the body



but you only can have one parameters was passed from body and multiple from URL



Model Bind





a very explicit way to inform the system how you want to deal with

certain types of complex objects.









































![](../../media/API-Design-API-API-image1.png){width="0.22916666666666666in" height="0.5833333333333334in"}![](../../media/API-Design-API-API-image2.png){width="0.22916666666666666in" height="0.5833333333333334in"}


