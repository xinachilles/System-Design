# Retry patterns



---

35

<https://youtu.be/N82W4xigYwA>

enable application handle transient failure (the failure occur temporarily and for a short period of time) .when application try to connect to service or new work resource

You are communicating with some kind of service and the network failure, or they reboot the service whatever. The retry pattern said let me try silently communicating again. Let me sleep for a couple second; call it again maybe it's up if it's not and Let me try 5 second later. See it's up if not, 10 second. If 10 second it's still not uplet'sme give a failure to my user. Create a better user experience to the user. So you have a timer running through the application and calling the service basically.



When to use it

Use retry for only transient failure there is possibility. circular break is a little more complex than his one. Retry is very easy

Retry only for transient failure and more than like to resolve themselves very quickly. Like it might have done for like couple of seconds, the you try and it's up.

~~Match the retry police with the operation, otherwise use the circular break~~

When not use it

Don't case a chain reaction without components; don't do retry for internal exception
