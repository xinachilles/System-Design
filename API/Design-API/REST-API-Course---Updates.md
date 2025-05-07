# REST API Course : Updates

Created: 2020-05-02 13:12:55 -0600

Modified: 2021-04-04 16:05:02 -0600

---

Restively or security basic authentication in this lecture follow the concept of basically syndication

for a piece of cake.

You will learn about some of the issues related to basic authentication scheme.

Also a walk you could record for implementing the basic old core basically basic attorney equation is

the easiest way you can protect your computers in basic authentication.

The consumer sends the credentials to the EPA.

So in the issue do we have to authorize mission the data in the authorization header is a base 64 encoded

string.

The format of the string is user Holland Pasley the server or receiving the credentials issue that we

have of course the value in the head and then checks the user and the password if everything is good

it sends back 200.

OK.

If the user id and password are not good then it sounds like a 401.

If you are using SATB that means you are sending the data in clear text or plain text format.

Anyone can read that to you.

That means the user id and password that are being sent as part of the authorization however are going

to be visible to anyone who's getting older man in the middle attack.

So the bottom line is basically turned occasion it's OK to use that SSL restricted areas but not could

be used and instantly be physical turned to cash scheme has some inherent issues.

Let me explain to you what these issues that some examples let's say Achmet travel has partnered with

a company called dreams and dreams runs a website that lets the users bid on vacation packages when

the user needs information on the vacation package.

The website for Bream's will allow the user to go through this path.

Here is you can see Dreamz is making a car to the Acme here using the credentials that have been provided

by the Akman genius to be dreams and genius.

This is all well and good but as I explained earlier if Dreamz is using Esther ATP then the man in the

middle attack is possible.

The clear text credentials can be read by anyone who has access to the data flowing from dreams to Akman.

So this is one issue.

The issue is that basic authentication requires the caller or the consumer to send the credentials every

request not to make things easier.

Actually engineers may decide to use sessions but that's not allowed from the rest of us practice.

So this is another problem the caller has to send the credentials that every request and other issue

is with the mobile applications that may be directly invoking the API.

So let's assume that Bream's ingenious decided to build a mobile application for building happening

in this scenario the vacation building application is expected to go directly to the Acme.

If so this would require the dreams ingenues to hard code the credentials in the application and anybody

can break into the application and take all the credentials and use it for whatever purpose.

So these are some of the common issues that are highlighted for the basic authentication scheme.

Let me walk you through the code for Burling it appears that support basic authentication.

The code for this data is available on get up in this.

Just damn.

I will be using NPM package called Passport.

You can read about passport passport just knock on the passport package provides an authentication their

it's nonintrusive you really don't go for it.

If that statement in your code so the code becomes very maintainable it supports multiple forms of authentication

basic authentication just one of those or two tokens you name it and it's supported.

There is evidence support for social occasion.

Or is it that aspect is more for the consumer.

Not from the provider perspective.

And at this point there are over three hundred strategies available from authentication perspective

impossible.

So I would suggest that if you are into node programming look into this package.

It seems to be very effective and highly supported in terms of implementing any kind of authentication.

You may be looking for so for any indication you need to think about where you are going to store the

user information whether it's going to be in a database in a file system or on Elda in this damn well

I have stored the data in a file called users or just it's hardcoded.

It can easily be changed to a database.

You can store all you use as needed.

And this uses storage.

You will find no function call or check credentials and what it is doing it is simply getting the username

and password and checking if the username and pass don't exist in the user's update and that's what

it is doing so to protect the source in this case private I need to insert the passport basic authentication

middleware callback.

So that's what this all test for here.

What's happening is that going to get us called on slash private the first callback that will be called

is up and will check if the credentials are good and is good if the credentials are good only then this

main function will be core for getting the resource back to the caller.

The basic pilot set up is done in five core basic.

So next we'll look at Doc certain basic blocks.

You'll find two packages from Passport perspective passport and passport badge is to give you what is

down.

I'll be using SCDP not devious.

And that's the reason I had needed passport dash S&P I'm indicating here that I'm interested in basic

strategy.

And then the users data is also important.

From the start trajectory perspective you need to provide a functional callback function that takes

username password and don't call back.

And here if the user is following we make a card the don't call back with the truth.

Otherwise we make a call to the done with the Fox and then the middle real function is created by calling

passport not a ticket and using basic hair to indicate that it's basic the session management calls

indicate that there will be no session manager.

Let's start to happen try code first I'll hit the private Tressel's you with the browser.

And as you can see we received a dialog box indicating that 12:56 unauthorized had a seat.

So here we can provide the password and the user ID.

I know the has the user IDs time and some one two three.

And as you can see access was turned over to the which was to dig a little deeper into what's happening

under the colors.

We will try this game you tell the postman Klein So I'm doing a get on this resource obviously I get

back a photo on an authorized and unauthorized message in order to provide the authentication credentials

to select the basic health and provide the ID and password and just do a search at this time.

Access was granted not if we look at how the postman learned this walking it will give us a better idea.

So let's go to the headers and under headers you will see the authorization had which has basic.

And then it has a weird looking string let's call it a string.

Now this training if you remember is the string that has the big 64 and quartered user id and password.

So what I'm going to show you next is they will decode the string to see what's there.

I'm going to Web site which allows you could D-CT been 64 string.

So I'm just going to paste this thing here and see what we get.

De-code And as you can see we received some Kallen Sam 2:59 which is the user name called Passement.

Let's summarize in this lecture.

I covered the use of basic authentication prosecuting its basic authentication is part of the protocol

specifications.

The specification requires the request to use the DP had record authorization the authorization to carry

the credentials in the 64 and cordoned format.

A couple of things to keep in mind.

[You must use STAPPER for basic authentication because you don't want the credentials to be visible to]{.underline}

anyone.

So the EPA has the credentials to her nose and the body is encrypted.

So the man in the middle a lot the possible the basic authentic Asian scheme requires that the are of

the EPA as the credential every call because you can not manage sessions or you should not manage sessions

in the state you.
