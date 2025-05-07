# Federated identity pattern

Created: 2019-10-24 22:11:49 -0600

Modified: 2019-12-20 13:02:32 -0600

---

**18**

(It delicate authority to external identity provider; simply the developers; requirement for user administration; improve for user experiments for the application; centralize MFA for user application)

The application may have register user, your system has hr system require you register then you have your fanical payroll system required your registration. So you got account anywhere and you don't even remember the password.

User registration could be pain. It hard to keep security. John left company. We had remove him for everything.

Everyone need to log in again to different system. For example. if you go the HR system. You cannot go to your other system like payroll system. You have to sign in again

Someone called IDP. identity provider, you may actually see it already. Sometimes you go on this website. You can see your google id to log in to the system.

In Canada, if you want to pay you tax staff, you can user you bank to log in. so you can said you use the bank account. You click that and you direct to another page. You log in with your current credential then you direct back to you system you using.

You navigate to application and you re-direct to your IDP. You log in in to IDP. It actually signed back to your application with the token, Ok here is the person and he is implicated

When to use it

When you have multiple application want to provide "SSO". If you build mutiple applications, you need SSO using IDP.



You want to Federated identity for multiply partners some of the partner is going to join you



The consider,

The identity server need to be high available.You can't just one identity service. It could be sign point failure.

(authentication and authorization)

The identity service usually not do the authorization information. It just authentication nor authorization

Do you have access to the HR system? It is usually not identity system IDP problem.

Basic IDP problem is here is him and this is authentication with thismutifactor authentication password and his good to go.



From document

If the client access a service that requires authentication. The authentication is performed by anIdPthat works in concert with an STS ( security token service). TheIdPissues security tokens that provide information about the authenticated user. This information, referred to as claims, includes the user's identity, and might also include other information such as role membership and more granular access rights.

[STS return the token to client and client pass the token to service he want to login in]{.mark}
