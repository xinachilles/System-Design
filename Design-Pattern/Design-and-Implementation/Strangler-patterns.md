# Strangler patterns

Created: 2019-10-28 23:01:52 -0600

Modified: 2019-12-30 12:36:59 -0600

---

42

<https://youtu.be/N82W4xigYwA>



~~"incrementally migrate a legacy system " you actually work with legacy system. It's way migrate to new system.~~

"gradually replace feature" even if you want to new system basial struggle you kill off the old system.

~~Most of us live in a monolithic application these days. A lot of you may be moved to microservice but your application was successful. It's probably a monolithic solution there as somehow successful but now you try to break into microservice.~~

What the strangler pattern does:you haveaapplication connect to you database and you try to introduce a new application. It's next to you application but this s a load balance for both of them. The new application may have certain feature.

So you still have people use the old system. So transfer them migrate them to new system so then can use the new feature over here. So you actually allowing them to fork off and then kike up see hit our new system.

You see the old system is bigger and new system is smaller. Slowly implement the new features; then you would basically strangle it. The old system is being circled and everything else will be move to new system. So we will start to use new system and people happy with it and we killed off the old application.

In really life, it is hard to kill off the old system since a lot of customer use it. But it is way to introduce old feature into new system which use different technology.

When to use it

When you want to gradually migrate backend application to new architecturemaybe use microservice.

When to use

When the backend service cannot intercept for small system where complex of the whole replacement is very slow










