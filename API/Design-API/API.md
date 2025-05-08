
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

## Data Format

- REST primarily uses JSON instead of XML
- JSON advantages:
  - Designed with JavaScript compatibility in mind
  - Easier for browser/client code to process
  - Smaller data size compared to XML

## Resource Identification
- Resources are represented by URLs
- Resources should be accessible via browser
- Follows noun-based resource naming
- HTTP methods (verbs) define actions on resources
  
## HTTP Methods

- GET: Download/retrieve data
  
- PUT: Modify existing data.The put is replacement that when I am using for update.
  
- POST: Create new data. The idea of post is we posting something new to the collection.  
   - For post, what should be returned. There is a design question behind the API weather you return the product back with the ID populated or just say ok I got the post
  
- Patch: Just modify the part of the object. Maybe you can take partial representation
  
## API Design Considerations
- Focus on business logic and application expertise
- Design for resource accessibility
- Consider how consumers will interact with the API

## Example: Twitter API
- Uses collections (e.g., "status")
- Resources include:
    - User timeline
    - Home timeline

## Key Differences from RPC
- REST is resource-oriented (nouns)
- RPC is procedure-oriented (verbs added to nouns)
- REST uses standard HTTP methods for actions
- Resources are directly accessible via URLs

## How the resource are represent.

- Consider the transaction initiator's perspective
- Balance between:
  - Business logic exposed to API consumers
  - Service-managed business logic
- Focus on your core business expertise
 
## HATEOAS (Hypermedia as the Engine of Application State)
- API provides all necessary information in responses
- Clients can discover available actions
- Example: GitHub repository API
  - Single call provides complete repository information
  - Includes related resources (followers, etc.)
  - Eliminates need for multiple API calls

## Performance Optimization
- Stateless Design
  - No state maintenance required
  - Each request contains complete information
  - Client provides all necessary data per call
- Round Trip Avoidance
  - Minimize HTTP calls
  - Better to send more data in one call than multiple smaller calls
  - Reduces latency and improves performance
  - Prefer comprehensive responses over multiple small requests



# Lesson 3

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


