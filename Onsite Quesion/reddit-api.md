# reddit api



---

What's the question hi, so I want you to design an API for reddit



and specifically the core flow of interacting with a sub reddit



so to get you started I wrote down a couple pieces of information on the screen and I've given you the schema for a user and 4 subreddit this is kind of what they look like when they're stored on the back end ,but you'll notice the only really have IDs, which are unique stream obviously they have more stuff on the back end, but that information is not really useful ,for you during this design question is what you have on the screen is really all you need



I'm giving this I want you to design me exactly what happens with a subreddit



okay so we're designing the API within a subreddit ,and I guess the to clarify to make sure that we're on the same page as far as our understanding of a subreddit, a subreddit is an online community, where users can write posts ,they can comment on posts ,they can vote on posts ,either up vote or downvote, they can share ,reported posts, they can even become moderators ,are we designing the API for all of this functionality, or are we limiting ourselves to something ,



so you covered most of it but let's keep things simple and focus only on writing posts writing comments and upvoting and downvoting ,you can forget about all the extra auxiliary features like sharing or reporting or moderating



so we're really just focusing on the the very narrow but core functionality of a subreddit writing post, commenting on ,them end of voting or down voting them, right yep okay today I'm thinking that given the information, that you've given me here ,in a given the user and the subreddit, I'm probably going to want to define, a few more entities of the entities that we find in a subreddit ,things like posts or comments and there might be a couple more, and for each of those entities ,I'm going to define all of the methods or API methods or endpoints that we might have, liked create post, list post that kind of stuff ,is this in line with what you're asking me to design



yes but just make sure you include the signatures of the endpoints ,so what each method takes in, and return, and the types for all the arguments that they're taking in and returning,



create post you want me to also write know what I'm passing into that method, like maybe the contents of the post, and the fact that would be like at the string or something is that what you're saying



exactly I think I have most of what I need to get started ,perhaps be the best way to jump into this ,would be to really nail down the entities, that I'm going to be defining then figure out after that what methods each entity, is going to support to ,hear we've got user and subreddit ,that you said we don't need to worry about those anymore just the fact that they have a user ID and a subreddit ID ,So within a subreddit like I hinted at before, there are posts ,there are comments,comments live on post and then is there anything else that would make sense as a as a first-class citizen ,so to speak in our API as a primary entity



this may be votes, but both seem like they there a kind of attached to a post or to comment ,so for now I don't think I'm going to categorise votes as an entity, I might come back to that later, I'm thinking of is we have posts, alright post ,and then we have comments, I'll write comment ,these would be are two primary entities ,so what I've written in red, here these are kind of siblings of user and subreddits here in our API there's a main entities



and then each of these is going to have a bunch of method ,so I'm thinking that it would have basic curd operations we have : for you so we would have created post, create comment ,get post, get comment, edit, delete, list, those are the five of primary method that I can think of right now off the top of my head



~~so I guess I lost her by writing them down create create get and then list~~ some of these might be very redundant , some of these might look very similar, like for instance I'm thinking, that deletes and get might look very similar ,in that they might have very similar signatures but I suppose you will see that in a second



so maybe I will I'm going to move comment to the top here for now, and we'll deal with comments later, let's start going going through all the post ,and and then going into comments ,



okay alright so before I get into the signatures of the methods, if I be easier to define the scheme of the post ,so what you gave here would like user ID, and then you said there was more stuff, let's do that for a post, so what would a post look ,like getting that we see that we have subreddit IDs ,that tells me that we probably want a post id, I think that every entity on Reddit is going to have some sort of a unique identifier ,that's all I'll put post you would have a post ID ,and you said you want me to define the type, so this would be a string,the post ID string, then what else we would supposed to be the poster id if this is a unique identifier ,this would not be like the name or the title of the post, this would be something that is unique ,and that is generated on the backend on the server side not the client site



there the second thing that we probably want is a user ID, or Creator ID for a post, cuz when you display a post on Reddit, you know or you can see who posted that post right, so we need a think a Creator ID ,I'm going to add Creator ID , and this would be a string, and I think this would just be the this user ID, this would be what shows up here user ,if I'm a user and I have a special ID that's like 123 the and then I post a post then this Creator ID would be one 123



then I suppose a post lives under a subreddit, so we may want to keep track of the subreddit id as weel with the subreddit that are post lives in, okay to add that and I think this might actually be really useful yeah I for I think this will come into play as well in the listing ,is when we're going to list like a listing will likely take a subreddit id as parameter but I guess that every post that's returned would have that subreddit ID ,letting it makes sense to have a sub in a subreddit ID



~~and like I said this is a string and this is the same thing that we had up here in the subreddit~~ and then is there anything else that we want on a post, I think we want we probably want when the post was created, right cuz you can see the date, like if I go on Reddit right now, I'm pretty sure that I can see when a post was first written, so I think I'll store of a timestamp for when something was created, ~~maybe I'll maybe I'll give myself that so that we can see the entire signature on one screen I'm going to give myself some space remove this down here and~~ now,



here and now we're going down to the next line so put created at ,created at and this would be a timestamp ,representing when this was posted



I think this might be all that we need for a post what else do you see on a post, I guess you know you do see you see how many votes supposed has ,right you can see on the home page of Reddit ,you can see that that post has a thousand of votes ,or a thousand downvotes ,so I think we're going to want a vote count as well, I might be the last parameter votes count,let's count and this would be an integer.



and I think this might be it for post unless we also do we want to show the number of comments on a post, at any point in time, like we certainly want to shows bee comments but do we want to show the number of comments, I think we do ,we want to be able to say that this post is popular as in or very controversial right,so even a post that might be at a low vote count might have a lot of comments



and so I'm so we probably want to be able to display that, I'll put comments count here



in this would be an integer as well as having this would be tracked by the back and basically like we'll probably have something where if we query the post that maybe the backend , and computes the number of comments that it has, or maybe when we add a comment if updates the commons count in the database, something like that ,when is it would be obviously created server-side, is it that is what I think a post looks like





as far as the method, let's see with create maybe, like that the natural one to start with creating a post what would it take we said of, the post ID is generated server-side ,so we're not going to pass in a post ID here ,we would want to pass in the subreddit ID, that were that we are at ,or that we are in, rather and we would want to pass in the Creator ID, because that's how we get this creator ID and the Creator ID we said is just a user ID ,so here I'm going to pass in both the user ID and a subreddit ID, and the user ID would become Creator ID in the backend, case of is it going to be yeah he is going to be user ID, string, and this user ID becomes the Creator ID on the post entity,





and then we can take in a subreddit ID and this is just the separated ,okay and this is very easily accessible on the client side because we probably just have them at all points in time we're in the subreddit ,we have the separate ID, and we always have to user id and I suppose as far as the contents of the post ,can we just assume that for now we have like a title and a description? both string



are fine or do we want images string, okay images we could have a another field ,but it's it's roughly it's similar to what you have so ,we don't need to



have a title and this is a string again, and then we'll go with desc, and this will be a string, I think that ends the create method for the "create"method is fairly straightforward, and I suppose it return want ,I was going to I was going to say cuz you wanted me to return, it to the final return, I think for all of our methods we can probably go we can probably go with the standard, of just for returning the entity that were interacting with ,so if you're creating a post you return the post ,you're getting a post past of course return the post editing and deleting a post ,you also return the post, I even for deleting I think that that by certain API standards you still want to return whatever entity you've deleted, I guess basically all 4 the message that deal with one entity ,will just return that entity, is that okay



~~then this will return and I'll write it and read post so it's returning this entity type~~ then as far as getting a post, and deleting a post, I said before that I think these are going to be very similar, so they're going to be kind of identical ,so I will say both of them taken a post ID, by its post ID, you delete a post by its post ID, although the only problem is getting a post is fine, right so I guess getting a post we can write post ID, the only problem was deleting a post is that, l~~et's train here and hear the post ideas this post ID up~~ for delete post here, I can't delete your post or any other users posts on Reddit, so we need some kind of need an ACL system, access control system or permission system, and if you want me to like design the permitting system, or can I assume that like the backend can handle permissions based on a user ID



so you can assume that as long as you give the backend, the date that needs to perform the access control, then that's fine, just make sure you pass in whatever the backend need



following that I'm going to assume that all that the backend needs is a user ID ,and then you could have image the user ID might be the user ID might be a little bit more than just some arbitrary string, maybe it has some authentication information, and then that is in it maybe it is generated by our authentication Service or something, so that's what we would use for our access control, so I suppose maybe like every method, every method passes that user ID, and that way the user ID is used for create, has the dual purpose of being used as the Creator ID and for a bunch of other methods of also used as an access control check and that way maybe you know we can make sure that we're not dealing with Bots as well and stuff like that

so I'll just passing user ID to every single method and with the understanding that the server always uses the user ID for ACL stuff and perhaps other duties,I will pass in user ID user ID string ,and I believe that that's it for get



then I think that I think that for delete, just paste this again ,and have the same thing because when you delete, let's see when you delete, you need to pass anything else you don't need to pass in a subreddit ID ,you're like so long as you got to the post ID ,you know what post be dealing with this it's a unique identifier I think this is good



editing is going to look yeah everything is going to look very similar the only thing it'll take an extra or the fields that you can actually edit, and those will be title and description ,~~I think I'm actually going to I'm actually going to paste to be the same thing that we were dealing with here, so they taste that again I'll put this up here and I will move are (here at the end move it to the side and get myself tied up more space and here~~ it'll take in a title ~~this is going to be a string~~ and then it'll take him a description and this will be a string, okay I think that's it for editing, these guys are very they're very clean very simple and they all return a post,



~~right so maybe I'll grab this and try to copy it here we would have a post here would have a post here and then we would have a post down here after~~ for listing it's going to be kind of different cuz we will be returning a list of post, not single post ,and if we're on reddit reddit has like a huge scale, right we're dealing with, I don't know if it's in the billions but at least in the hundreds of millions of users, that doesn't even matter to be honest, like it if we were dealing with just a few hundred thousands of users, the point is we can assume that a post or rather of subreddits might have hundreds of thousands of post ,and if we do assume that which I think would be a correct assumption, we're going to want our listing to be paginate, so we can just have the API return like 200 posts, probably wanted to return a page or pages of post



I think the way all I find this is going to be ,we will take user id this is the standard ,then we will take in a post ID, or we know, we're listing post ,so we're going to take in a subreddit ID, you were listening post for subreddit, and this would be the string then, if we are dealing with pagination, I don't think there's anything else apart from the pagination ,that we need to put here cuz like listing is just a fancier get method, so we're taking the same thing that we have for "get", but we also specify the page size , so the page size and here you know I think a standard API would it would allow this page size parameter to be optional, and it would default to





like 50 posts or 10 posts, you would Define some default page size on the backend and it's an integer, and then you have to pass in a page token, when you're when you're dealing with a paginate API, you pass in a page token ,so this is like page number one, page number 2,this is also optional ,and it's a or an integer to be honest and depending on how you like is that, okay I know that sometimes page tokens will be string,but from experience I've had them be integers, sometimes literally it has to be integer, just one two three four five six



the idea is you need backend know where to start from, when giving you the next page, and then here if we close this then we would just be returned from this method you would return a list of post



we would have post, here draw an array ,to show or an empty array, to show that this is a list of post, as well as the next page token ,next page token ,and the next page token,I don't think this would be optional. Optional if you have only a single page, so maybe I'll put it as optiona, l but I guess, since this is a return statement it is not really optional. then we just go to the next page token, and the idea is that this is a integer



so now I think we can move on to comments, so let me go up here, we had our comment up here, let's grab this and find comment API, I think for comments we're going to be dealing with something very similar to the post, I will have will start by defining the the comment entity, and then I'll go through the methods, and for the methods ,I think it'll be the same thing to create, get, delete list



let me start with the with the comment entity, so this is going to be Let's see we want an ID, or unique identifier, so this is going to be comment ID, then we're going to want a string, then we are going to want a Creator ID ,similar to what we had here for the post ,so this is going to be a Creator ID, ~~and maybe I'll write ask your cuz I'm running out of space~~ then we would have a comment, doesn't live in a subreddit while he does in theory live in a subreddit, but it more specifically lives on a post, so I'll put host ID, and this would be a string, then after that we would probably want the same time stamp, that we had here right the created at timestamp, and we can vote ,we can vote on a comment, right so we probably want the votes count ,as well, is that correct, yep okay so we don't I don't think we need commons count ,cuz we don't count the number of comments , I guess there are replies ,let's deal with replies in a second ,cuz you can reply on a comment that's getting that might be a little bit tricky



but here we said, we're going to want to "create it at", so created at which is a timestamp and then and then there's the votes count ,votes count is going to be an integer ,and then oh I guess the same way that we have a title and a description ,for a post we need some kind of content for comments ,do comments have titles and descriptions, or can we capture that in the single content field ,just use a single content field ,content string,and



then what's the last thing that we need here ,we said that we might need a replies,lets you would we actually want replies, cuz we need to I think on the Reddit UR you want to show like if you are dealing with us thread of comments, you want to indent replies, and you want to keep inventing them as you go deeper and deeper and deeper into a Common Thread, so we have probably two ways of handling this ,basically we need to tell the UI that we're dealing with a Common Thread ,and to do so we need to either her store on a comment, all of its replies, so you know maybe it would be applies or children ,if you want to structure this kind of like a tree data structure ,or maybe we can have a parent point , like we point to the parents comments ,so so a comment would either not have a parent it's if it's a top-level comment, or if it's the reply to a comment ,it would have comments ID as it's parent ID



cuz let's see what would be simpler ,typically for dealing with a tree data structure ,you prefer going top to bottom, so you would prefer storing replies ,but I do feel like replies might be tricky ,because that means I post a comment, you have to update or potentially update its parents comment, on the parent store reply, so I actually think that we can just store parent ID ,and have the UI reconstruct the comment thread, or common graph from bottom up ,using the parent ID ,and the you UI sort comments within a thread based on created at timestamp does that kind of makes sense.



I think this is good and you will always have the guarantee or the invariant that a parent comments has a created at that comes before a child comment, so that's the UI,for the UI to be able to use a comment and



~~then you've got these three replies that are indented ,would have this as the parents ,right but then and you guys decided this would be created at before these three ,and then these three would be sorted within themselves based on create at~~



so I think this is basically store parent ID ,I suppose the parent could be optional if there is no parent ID, we just assume that it's a top-level comment. Write this down parentid optional, and it is going to be a string of that points to the comments ID



there's another thing that I'd like you to consider, so you mentioned that you're later going to have implement to delete comments, since comments can have parent comments, you might not want to opt into actually deleting any comment ,but rather marking it as deleted soft to delete and still return it from the API ,so that the UI can construct the tree and assume that the parent comments always exists, even if it was deleted



I see because if you have a common that's deleted with a full comment thread below, it like a bunch of reply, to the replies not dead deleted, they do not ,they do not, okay yeah so that's interesting ,so that means of the okay after you I would basically have the handle that where is it it has to be a comment would be like in is deleted field, or something and if it's deleted you just don't show it ,or something like that



or the you I could show that there was a comment there, but it was deleted, the contents basically , you show the replay be low it, okay yeah that's a good call, I guess I'll extend this with an is deleted ,is deleted field, and this would be like a boolean, and I think and I supposed that means of the content here, the backend would be in charge of empty string if it is deleted is true, because we wouldn't want to expose at deleted comments network request or something like that



so that is going to be our comment entity, ~~and then we are going to have so I might run out of space here but I'll try to do my best~~ we have creates here, we'll have to get, we have edits ,we have delete and we'll have list, now the more I think about it do we have get for a comment ,like if we if we're designing a robust API ,and we want to abide by certain standards, we probably do want to have "get"yet just in general ,but I'm not really sure where we would use that on the UI, cuz I think you just see the list of common, are you can click on a comment do you want me to design the get?



as a matter of fact ,you can kind of forget about both get comment, and delete, we just talked about it so just worried about create and list





I think edit would be identical to the edit one for the for the Post, right meaning we would take in like a user ID a post ID or content ID, and we would take not title, description but this content, that's the only thing you can edit it ,that is about a comment, you also want me to write that down or you can skip it



yeah but yeah I just wanted you to go through the you don't have to write it down



so then I'll just strike through this one as well and I'll go through a create and list, if those are a little bit more interesting,so create would taken ,we said to use IDs for a ACL and ~~then it would take for post did we pass him anything else~~ I think we passed in a subreddit ID ,to hear we would pass in the post ID, post ID, would we I guess, if you if you are doing a reply to a comment, you would pass in post id and parent ID, the parent id would be the id of the comment that you're replying to



so I guess those will be the three parameters in the content ,so yeah I'll put I'll put user ID, this is a string then put post ID this is also string ,then I'll put a content is mandatory so content is going to be a string and then we'll have the parent ID at the end



content string and then parent ID which is optional, that is the create and point I think similar to post ,it Returns the entities that returns a commons a comment okay the comment that you just wrote,



and then for listing comments and listing comments would also be identical to the post here, so we would list we would pass in a user ID, we will pass that's my post ID and then pay size and Page token, because here within a comment, it's from UI point of view like conceptually it's not really a page ,but at the backend level, it is effectively a page like it's your are limiting how many comments to returning, so you still use this concept is pagination



you return instead of post, you were trying to a list of comments and the next page token you want me to write this down or can I eat just like skip it ,skip it okay alright strikethrough it ,the idea is it's very similar to posting







so that is it for the commenting API kind of things ,so we do still have to cover voting ,how would votes work, I said in the very beginning that, I don't think having an entity for votes makes that much sense ,maybe it does, actually because if you vote on a post, and you can

vote on comments , if you vote on posts and comments, and you don't have an entity for votes ,then you are effectively editing a comment or post ,so maybe maybe actually these edit and points are a little bit more complicated, when you edit a comment or you edit a post ,you can edit it with a votes



we said that editing specifically comes with the ACL check, like a user can't edit another user's post right, but a user can vote on a another user's post ,so maybe it's not super clean to have like a voting the editing a comment or post ,maybe it does actually make sense to just have Vote be a completely separate entity similar to two comments and similar to post



they live under something so I know comments live under a post ,post live under a subreddit ,votes live under either a post or a comment ,so maybe they have like a Target ID instead of a post ID or a subreddit ID ,would that kind of makes sense ?



what's go over what different field, how would be like



vote vote would have I suppose you would have a vote ID ,vote ID ,this is also generated by the backend it's just the string ,I don't know why was it seems a little bit awkward to me to have those be a separate entity, but now I'm starting to think ,that it just makes them makes the as clean as it could be ,so we would have a Creator ID, this would be a string just how posts and comments have a Creator ID, and this would have a Target ID, and Target ID would either be the ID of a post or the comment, that is vote on



we need the type of the votes, so what does vote ,because you either have a downvote or upvote , either be a number and like an upvote would be plus one and a downvote would be -1 ,and we can keep like a tally like that, or maybe it's just like an enum, up vote ,down vote I think I'm going to go with an enum both approaches could maybe work but I think enum known as a little bit more explicit



this is an upvote versus a downvote, the idea is you have a type here, and it is an enum where that you know I'm has either up vote or down vote,

35



go up down up down does this make sense I think that's all for vote as far as the methods sweats you would quit would you ever get a vote or list of those I don't think you would ever get or less votes at least not on this subreddit on a subreddit would just you would allow users to create vote certainly users are able to that you're able to change their vote on Reddit right right okay so that I think we want to support like I am going to skip debt unless you know again depending on our API standards we may want to have get and list methods to find the so that the ATI fits those standards but for the purpose of in a subreddit I think we just need create and edit create and edit maybe we do need delete as well as he when you are so creating would be called when you go on a post or comment and you click the little up Arrow or the down arrow that would be creating we can when you click it the first time but then you have the arrow and this is the same concept as like liking or disliking on a YouTube video for example you got these two binary options and only one or the other can happen at that one time so if you liked a post and it's already like four of voted and you go to downvote it you are effectively removing the of vote and creating a downvote so that would be an edit that would be an added it but you can also I can also just retract an upvote without creating a vote I can click on the road again and that removes the vote but doesn't create a downvote I think we do need create a delete and we would use them in those three cases that I said create is when you cast a vote for the first time you cast a vote with no vote cast it at that point in time edit is when you cast the opposite vote that you have just done so you turn up voltage down vote or vice versa and delete is this when you retract an upvote or retract a downvote and then as far as like what these methods look like I guess they take in the user ID and always passing the user ID they taken the target ID so the user ID would be the Creator ID here then they taken a Target ID and or gets no edit and delete don't take in a tent Target idea creates a Target ID so this takes in a Target ID which would be the idea of the post or comments that you're voting string here and then we have to just taken a type so I'll pour down this would be type and I'm going to put up / down up and down and then we are going to have a delete and edit message so for these are they going to take me to take user ID or going to take a vote idea that you're editing and then they're going to take to the user ID again it's always for ACL text someone else's vote and then the edit one would taken the type cuz just added it to another type so yeah this would be like I'll just read them really quickly this would be user ID I'll skip the types of time user ID vote ID and this would be and then this would be a user ID and voter ID that's it cool so I do have one more question we need to be able to display whether or not you've liked or you've upvoted or downloaded a certain comment or post so would that be in Agatha call or would that be in the respective get API for the comment on the post you're saying like if you are honest subreddit as a user you should see like a little of the fact that has voted like blue or whatever showing that you have passed it a vote is that what you mean yes that's exactly what I mean vote comes into play although if you getting of getting a vote means that you have to issue a call every comments and post that doesn't seem like a good idea then what is that I guess you could say then that's where listing comes into play so maybe we have a list of oats and you list all of your votes voting like list votes method for a subreddit for a post then you have to mix and match and find all the comments I think maybe what makes the most sense is what you you I guess hinted at that you said it does it was on a vote call or on the respective Post in common call idiot lives here so maybe but maybe a post of post has also something like has my vote or currents vote as a parameter and the back and generates that when you get a poster when you list hey everybody Welcome to systems expert what's Ivan's question hi, so I want you to design an API for Reddit and specifically the core flow of interacting with a subreddit so to get you started I wrote down a couple pieces of information on the screen and I've given you the schema for a user and 4 subreddit this is kind of what they look like when they're stored on the back end but you'll notice the only really have IDs which are unique string obviously they have more stuff on the back end but that information is not really useful for you during this design question is what you have on the screen is really all you need I'm giving this I want you to design a exactly what happens with dinner subreddit okay so we're designing the API within a subreddit and I guess just to clarify to make sure that we're on the same page as far as our understanding of a subreddit a subreddit is an online community where users can write posts they can comment on post they can I get the boat on post either of the downvote they can share reported posts they can even become moderators are we designing the API for all of this functionality or are we limiting ourselves to something so you covered most of it but let's keep things simple and focus only on writing posts writing comments and uploading and downloading you can forget about all the extra auxiliary features like sharing or reporting or moderating cetera so we're really just focusing on the the the very narrow but core functionality of a subreddit writing post commenting on them end of voting or downloading them right yep okay so then I'm thinking that given the information that you've given me here in a given the user and the subreddit I'm probably going to want to define a few more entities of the entities that we find in a subreddit things like posts or comments and there might be a couple more and then for each of those entities I'm going to Define all of the methods or API methods or any points that we might have liked create post with post that kind of stuff is this in line with what you're asking me to design yes but just make sure you include the signatures of the endpoints so what you should method takes in and return in the types for all the arguments that they're taking in and returning create post you want me to also write know what what I'm passing into that messes like maybe the contents of the post and the fact that I would be like at the screen or something is that what you're saying exactly and I have started perhaps be the best way to jump into this would be to really nail down the the entities that I'm going to be defining and then figure out ask for that what methods each entity is going to support to hear we've got user and subreddit that you said we don't need to worry about those anymore just the fact that they have a user ID and a subreddit ID So within a subreddit like I hinted at before there are posts there are comments comments live on post and then is there anything else that would make sense as a as a first-class citizen so to speak in our API as a primary entity this may be votes but both seem like they are there a kind of attached to a post or to comment so for now I don't think I'm going to categorize votes as an entity I might come back to that later we have posts alright post and then we have comments I'll write comment these would be are two primary entities so what I've written in red here these are kind of siblings of user + subreddits here in our API there's a main entities and then each of these is going to have a bunch of methods so I'm thinking that it would have basic credit operations for these so we would have create post create comment get post get comment edit the weeds and West those are the five primary methods that I can think of right now off the top of my head so I guess I'll All Star by writing them down create create get and then list some of these might be very redundant some of these might look very similar like friends since I'm thinking that delete and get might look very similar you know they might have very similar signatures but I suppose we'll see that in a second so maybe I will I'm going to move comment to the top here for now and we'll deal with comments later what Starbucks is going going through all of the post and and then going into comments okay alright so before I get into the signatures of the methods you might be easier to define the scheme of the post so what you gave here with like user ID and then you said there was more stuff let's do that for a post so what would a post look like getting that we see that we have subreddit IDs that tells me that we probably want a post i d I think that every entity on Reddit is going to have some sort of the unique identifier that's all I'll put post you would have a post ID and you say you want me to find a type so this would be a strength K2 post ID string then what else we would have an I supposed to post ideas if this is a unique identifier this would not be like the name or the title of the post this would be something that is unique and that is generated on the back end on the server side. The client site he's a second thing that we probably want is a a user ID right Creator ID for a post cuz when you display a post on Reddit you know or you can see who posted that post right so we need a think a Creator ID I'm going to add Creator ID Creator ID and this would be a string and I think this would just be the this user ID this would be what shows up here user if I know user and I have a special ID that's like 123 the and then I post a post then this Creator ID would be 123 then I suppose supposed lives under a subreddit so we may want to keep track of the subreddit ideas well with the subreddit that are post lives in okay and I think this might actually be really useful yeah I for I think this will this will come into play as well in the listing is when we're going to list like a listing will likely take a subreddit ideas a parameter but I guess the and then every post that's returned would have that subreddit ID butting in I'll put subreddit ID and like I said this is a string and this is the same thing that we had up here in the subreddit I do and then is there anything else that we want on a post I think we want we probably want when the post was created right as you can see the date like if I go on Reddit right now I'm pretty sure that I can see when a post was first written so I think I'll store of a timestamp for when something was created maybe I'll maybe I'll give myself that so that we can see the entire signature on one screen I'm going to give myself some space your move this down here and now, here and now we're going down to the next line so put created at created ass and this would be a timestamp representing when this was posted and then I'm trying to think I think this might be all that we need for a post what else do you see on a post I guess you know you do see you see how many votes supposed has right you can see from the home page of Reddit you can see it so that post has a thousand of votes or a thousand downvotes so I think we're going to want a vote count as well I might be the last parameter votes count let's count and this would be an integer I'll put it and I think this might be it for post unless we also do we want to show the number of comments on a post at any point in time like we certainly want to show Z comments but do we want to show the number of comments I think we do it yeah we we want to be able to say that this post is is very popular as and or very controversial right so even a post that might be at a low VOC out my have a lot of comments and so I'm so we probably want to be able to display that okay so then I'll put I'll put comments count here Commons count in this would be an integer as well as I think this would be tracked by the back and basically like we'll probably have something where if we query a post then maybe the backend computes the number of comments and it has or maybe when we add a comment in updates Commons count of a database something like that when is it would be obviously created server-side is it that is what I think a post looks like as far as the message let's see if we can for with create maybe crazy like that the natural one to start with creating a post what would it take we said of the post ID is generated server-side so we're not going to pass in a post ID here we would want to pass in the subreddit ID that were that we are at or that we are in rather and we would want to pass in the Creator ID because that's how we get this crater ID and the Creator ID reset is Gus the user ID so here I'm going to pass in both the user ID and the subreddit ID and the user ID would become Creator ID in the case of is it going to be yeah he is going to be user ID string and this again this user ID becomes the Creator ID on the post entity and then we can take in a subreddit ID and this is just the separated okay and this is very easily accessible on the client side because we probably just have them at all points in time we're out of stock in the subreddit we have the separate ID and we always have to use your ID and I suppose as far as the contents of the post can we just assumed that for now we have like a title and a description sharpener both Springs screens are fine or do we want images Springs Springs okay we could have another field but it's it's roughly it's similar to what you have so we don't need to have a title and this is a string again and then we'll go with desk DSD SC and this will be a string I think that ends the the create a crate mat that is fairly straightforward oh and I suppose I want to yeah I was going to I was going to say cuz you wanted me to return it to the final return I think for all of our methods we can probably go we can probably go with the standard of guest for returning of the entity that were so if you're creating a post you return the post you're getting a post to have to force return the post editing and deleting a post you also return the post I even for deleting I think that that by certain standards you still want to return whatever entity you've deleted so I guess basically they call for the message that deal with one entity will just return that entity is that okay then this will return and I'll write it and read a post so it's returning this entity type then as far as getting a post and deleting a post I said before that I think these are going to be very similar so they're going to be kind of identical so I will say both of them taken a post ID by its post ID you delete a post by post ID although the only problem is getting a post is fine right so I guess getting a post we can ride post ID the only problem was deleting a post is that let's train here and hear the post ideas this post ID up here was deleting a post I can't delete your post or any other users posts on Reddit so we need some kind of need an ACL system access control system and if you want me to to like design the permissioning system or can I assume that like the back end can handle permissions based on a user ID so you can assume that as long as you give the back in the day. That needs to perform the access control then that's fine just make sure you pass in whatever the back in these seem that all that the backend needs is a user ID and then you could have had him the user ID might be the user ID might be a little bit more than just you know some arbitrary string maybe it has some some Haitian information for them bedded in it maybe it's generated by our authentication Service or something so that's what we would use for our access control so I suppose maybe like every method every method passes the user ID and that way the user ID is used for created has the dual purpose of being used as the Creator ID before a bunch of other methods it's also used as an access control check and that way maybe you know we can we can like make sure that we're not dealing with Bots as well and stuff like that so I'll just passing user ID to every single method and with the understanding that the server always uses the user ID for ACL stuff and perhaps other other duties move this here will pass send user ID user ID string and I believe that that's it for debt then I think that I think that for delete just paste this again and have the same thing because when you delete let's see when you delete you need to pass anything else you don't need to pass in a subreddit ID you're like so long as you got to the post ID you know what post be dealing with this it's a unique identifier I think this is good thing is going to look yeah everything is going to look very similar the only thing it'll take an extra or the the fields that you can actually edit and those will be title and description and am actually going to I'm actually going to Pace to be the same thing that we were dealing with here so they taste that again I'll put this up here and I will move are (here at the end move it to the side and get myself tied up more space and here it'll take in a title this is going to be a string and then it'll take him a description and this will be a string okay I think that's it for editing these guys are very they're very clean very simple and they all return a post right so maybe I'll I'll grab this and try to copy it here we would have a post here would have a post here and then we would have a post down here after listing is going to be kind of different cuz we will be returning a list of post not single post and if we're on reddit reddit has like a huge scale right we're dealing with I don't know if it's in the billions but at least in the hundreds of millions of users right I suppose like that doesn't even matter to be honest like you did if we were dealing with just a few hundred thousands of users the point is we can assume that a post or rather a subreddit might have hundreds of thousands of post and if we do assume that which I think would be a correct assumption we're going to want our listing to be patinated so we can just have the API return like 200 post will probably wanted to return a page or or pages of posts but in the way all all Define this is going to be we will take in as user ID this is the standard then we will take in a post ID or we know we're listing post so we're we're going to take in a subreddit ID were listening post for a subreddit and this would be the string and then if we are dealing with pagination I don't think there's anything else apart from the pagination that we need to put here cuz like listing is just a fancy or get method so we're taking the same thing that we have for yet but we also specify the page size for the page size and here you know I think a standard API wood would allow this page size parameter to be optional toes and it would default to like 50 posts or 10 posts you you would Define some default page size on the back end if it's not better and it's an integer and then you have to pass in a page token when you're when you're dealing with a paginate is API you pass in a page token so this is like page number one page number to this is also optional and it's a string or an integer to be honest and actually going to go with integer depending on how you like is that okay to do an integer I know that x 8 seconds will be scratching his butt from experience I've had them be integers sometimes literally just one two three four five six integers it's roughly the same but they're the guys there that you if you need a token to let the back I know where to start from when giving you the next page and so then here if we close this then we would just be returned from this method you would you return a list of post to we would have post hiralal draw an array to show or an empty array to show that this is a list of post as well as the next page token next page token and the next page cocaine I don't think this would be optional. Optional if you have only a single page so maybe I'll put it as as optional well I guess since this is a return statement and the realize that this is an integer so now I think we can move on to comments so let me go up here we had our comment up here let's grab this and find the, I think for comments we're going to be dealing with something very similar to The Post so we'll have will start by defining the the the comment entity and then I'll go through the message and for the methods I think it'll be the same thing created yet at a delete list let me start with the with the the, intensity so this is going to be Let's see we want an ID or unique identifier so this is going to be comment ID then we're going to want a Creator ID similar to what we had here for the post so this is going to be a Creator ID and maybe I'll write ask your cuz I'm running out of space then we would have a comment doesn't live in a subreddit while he does in theory live in a subreddit but it more specifically lives on a post so I'll put ink host ID and this would be a string then after that we would probably want the same time stamp that we had here right the created at timestamp and we can vote we can vote on a comment right so we probably want the votes count as well is that correct yep okay so we don't I don't think we need commons count cuz we don't count the number of comments on Mike although I guess there are replies with replies in a second cuz you can reply on a comment that might be a little bit but here we said we're going to want to create it at so created at which is a timestamp and then and then there's the votes Council votes count is going to be integer and then oh I guess the same way that we have a title and a description for a post we need some kind of content for comments do comments have titles and descriptions or can we capture that in the single content field let's just use a single content field content strength and then what's the last thing that we need here we said that we might need a replies let's see would we actually want replies cuz we need to I think on the Reddit URI you want to show like it's your dealing with this thread of comments you want to indent replies and want to keep indenting them as you go deeper and deeper and deeper into a Common Thread so we have probably two ways of handling this basically we need to tell the UI that we're dealing with a Common Thread and to do so we need to eat her store on a comment all of its replies so you know maybe it would be wise or children if you want to structure this kind of like a tree data structure maybe we can have a parent pointer like we pointed a parent comments so so a comment would either not have a parent is if it's a top-level comment or if it's the reply to a comment it would have that comments ID as it's parent ID cuz but see what would be simpler typically for dealing with a tree data structure you prefer going top to bottom so you would prefer storing replies but I do feel like replies might be tricky because that means that if you post a comment you have to update or potentially update its parents comment if you want the parents store replies so I actually think that we can just store parent ID and have the UI reconstruct the the comment thread or common grass big from bottom up using the parent ID and then the you I can sort comments within a thread based on created at timestamp is that kind of makes sense. I think this is good and and you will always have the guarantee or the invariant that a parent comments has a created at that comes before a child comment so let's study for the UI to be able to use that a comment and then you've got a few replies right these three replies that are indented would have this as the parents right but then and you like you said that this would be created at before these three and then these three would be sorted within themselves based on credit so I think this is basically was the parent ID can be optional in if there is no parent ID because assume that it's a top-level comment. Write this down parent ID parent ID optional and it is going to be a string of that points to the comments ID the parents guide you would be there's another thing that I'd like you to consider so you mentioned that you're later going to have plenty delete comments since comments can have parent comments I'm you might not want to opt into actually deleting any comment but rather marking it as deleted Microsoft to delete and still return it from the API so that the you I can construct the tree and assume that the parent comments always exists even if it was deleted I see because if you have a comment that's deleted with a full comment thread below it like a bunch of reply to the replies not dead deleted they do not know they do not okay yeah so that's interesting so that means that the okay after you I would basically have the handle that where it says it has to be a comment would be like an is deleted field or something and if it's deleted you just don't show it or something like that yes or the you I could show that there was a comment there but it was deleted the contents basically lies below it okay yeah that's a good Call Saul I guess I'll extend this with an is deleted is deleted field and this would be like a bully and I think and I supposed that means of the content hear the charge of returning an empty string if it is deleted is true because we wouldn't want to expose at deleted comments like you're at work or class or something like that okay great so that is going to be our comment and two teeth and then we are going to have so I might run out of space here but I'll try to do my best will have create here will have get will have edits will have delete and we'll have list now the more I think about it do we have get for a comment like if we if we're designing a robust API and we want to buy by certain standards we probably do want to have get just in general but I'm not really sure where we would use that on the UI cuz I think you just see the list of comments maybe you can click on a comment you want me to design the get the get as a matter of fact you can kind of forget about both get comment and delete, cuz we just talked about it so just worried about create edit unless I think that it would be identical to the edit one for the for the Post right meaning we would take in like a user ID a post ID or Content ID and we would take not title and description but this content cuz that's the only thing that you can edit a comment you also want me to write that down or can I skip it you you can skip it yeah but yeah I just wanted you to go through the you don't have to write it down so then I'll just strike through this one as well and I'll go through a creation list if those are a little bit more and sustain so create would taken we said the user IDs for acl's and then We Takin for post did we pass him anything else I think we passed in a subreddit ID so here we would pass in the post ID Albert post ID would we I guess if you if you are doing a reply to a comment you would pass in both supposed ID and the anti-d the parent ID would be the the idea of the comment that you're replying to so I guess those will be the three parameters in the content so yeah I'll put I'll put user ID this is a string that'll put post ID this is also a spring then I'll put a content content is mandatory so content is going to be a string and end, contest
