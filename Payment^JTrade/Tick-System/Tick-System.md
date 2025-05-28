# Tick System

Created: 2017-06-18 17:06:24 -0600

Modified: 2017-06-18 17:08:52 -0600

---

![After customer submit a request, wait some time, we have two servers: reservation serve and tick serve 2. 3. 4. 5. Around 500k request are coming in Through the loader balance send those 500k request to 500 service After insert the reservation table, send user id to ticket service, sharding by user id We have around 20 serve ticket service and each serve connect a redis database , the redis datab base like a queue store the the request send by the reversion service Ticket service will process 1000 request each time and will check the concert table, reaming ticket table first Customer sent a request and store it in a reservation table , crateat at, concer_id, user_id , tickets_countstatus We also have a concert table Id, title , description, start , tick_amount$emain_amount When customer submit a request, then we create an entity in reversion table and status is pending Jump to a waiting interface, every 10 second, send a request to check the status Ticket server are running and keep checking if there have item in the request table ](../../media/Payment^JTrade-Tick-System-Tick-System-image1.png){width="4.78125in" height="3.0in"}



