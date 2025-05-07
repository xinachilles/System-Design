# External configuration store

Created: 2019-10-27 23:34:24 -0600

Modified: 2020-01-25 14:09:57 -0600

---

6: 45

<https://youtu.be/N82W4xigYwA>

Help you configure information out of your application. Application deploy



Pattern can be provided easy management. Control of configuration data like app doc config, web doc config.

It's allow sharing the data cross application all instance. Typical when you build a application. You got user

Application has database, cache store, you may have some media store for photo or sth stuff.

You build a web app you put into web cofig how to connect the database. You put all information you need into this configuration.

What happen when you actually become successful you actually have multiple application running, each one has own configuration each one may point to the same database.

Develop guy modify one but forgot another 3 of them. One machine active funny. You just speed whole day debug. ~~Jony was here last night and he modify it~~

The problem is your configuration is part of your application. We deploy your application, the configuration goes with it. There multiple application can shared the same configure since the same database they point to. How to access, control it. You don't know who last tough it. Some one actually modify it. You don't know.



What external configuration does is you have a centralize configuration. You can just think a database has key value staff.. centralize, audit. You can have develop modified it. All application share the same information. The application just said give my configuration.

Centralize management system

When use this pattern.

When you share the same configuration by multiply application you want to manage your configuration central in one point

You want to proved a audit for each configuration change.



When not use it

Only one application
