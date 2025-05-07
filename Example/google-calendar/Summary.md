# Summary 

Created: 2018-01-30 18:41:09 -0600

Modified: 2018-01-30 19:01:18 -0600

---

1. function



What kind of function it need have,



1.Create anevent

2.Event have different type

3.Request to approve the event

4.Reminder, can reminder user at specific time or periodically, by phone or by email





we haveaevent table,

eventkey, eventtype( stick, vacation) begin time and end time, title, user, create time ( who approveand approve time, whoreject,rejecttime, ),andremindetime, who create the reminder

and who create this reminder



another table for event approve request

event key, submit date, who need approve, message and time stamp



subscription table

user, event type, whocreate, timestamp



queue table

queue key, event key, event ,soucetask, destination task, queued time,processdstatart,proceesend , hold timefstrdata, who create and when create



if it is daily event, insert it to queue table, there are will be a job run every night or every moning to pick up the event.



if it need send today or next hours,



for example if the event need be trigger next few hours, we can create a array or cycle array in memory,



the size is 60 and value is a pair, (task and cycle number)



we have a current index will every 1 minute



scan current bucket ( or set), if the task`s cycle number is 0, create a another thread to execute this task, delete this task from memory and set the database status to be done



if the cycle number is not 0, minus one



there should be one job continue grape the event from database insert to memory



base on the current index and current time and the event time we can figurate out which slot and the cycle time



current index + ( time -current time ) %60 is slot index



(time - current) / 60 is cycle number


