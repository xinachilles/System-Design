# materialize view

Created: 2019-11-10 23:57:31 -0600

Modified: 2019-12-06 15:53:38 -0600

---

<https://www.youtube.com/watch?v=06HlvmB8mDk>



materialize define a database object contains the result of query . if you want to gain the performance advantage, you might need create a materialize view.



~~the database management system will actually store the result data in the table as structure call materialize view. it is not simply the windows of base table. it's separately object holding data in himself just like table.~~



since it's object holding data. it's not window anymore in the base table. now we have a refresh of picture. refresh has to take picture periodically then will reflect the changes of base table into the materialize view. you define the refresh rate in the materialize view command to create the materialize view. the bottle line is it is lag now in that refresh.



in case of view, whatever you make the change to the base table. it will be reflect immediate in the view. if you delete the record in the base table. it will remove from the view immediately. if you update the value. it will reflect immediately in the view



in case of materialize view in the lag and the lag is depend on the refresh rate you have selected so in case of a materialized real you can do the same query



but now there's a difference the difference is underline query is not run by database management system. underlined query only run once when we was created. it will be flash but not every time when you query the materialized view.so this result into tremendous performance advantage.



and indexes on the base table are obviously not being used to materialize view. you can however create indexes on the View itself because now like a table structure holding data



~~so if you need up or performance games you can consider creating indexes on relevant columns relative within the materialize view. table is holding the data. you may want to create a view holding the same data in the table. some reason and the data in the table copy over to materialized view and any changes in the base table would be reflected periodically in the materials view.~~



other than that you can create a materialized view to reflect summarization of the detail database table or to create a subset window on the base table



~~The window is not the right term here because now it is holding a separate copy .so if they base table 20 columns. the materialized view can reflect 10 of those columns and maybe a filtered date in there in terms of rows well~~





and materialized view can hold of join multiple table this is where the Real Performance Advantage comes in because if you create a view join multiple table performance cold be very slow. you might be forced into creating a materialized for you to fix the performance issues. there is a quick comparison with materialized and view.



~~view basically gives you up to date data so this is the biggest Advantage of view but gives you a slower performance for the reason I already explained~~



~~because every time you call view, underline SQL execute all the time .so if you are joining multiple tables in a view then every time the joint statement will be called . it will slow down the performance.~~



in case of materialized view, the data is not so much up-to-date because there's a lag and you can control the refresh rate however the performance is much much faster. Keep in mind that materialized views could be created on tables which are residing outside the database.in different database it would be called a snapshot



~~and materialized view that is reflecting a data in a table which is within the same database simply be called a materialized view that is used do reflect this scenario where materialized view for you is created on a table which is in a different database the future of materialized view is available in all.~~












