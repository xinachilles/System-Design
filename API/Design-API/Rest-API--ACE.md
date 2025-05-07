# Rest API -ACE

Created: 2021-04-23 23:22:35 -0600

Modified: 2021-04-23 23:40:26 -0600

---

![核 心 原 则 无 状 态 Stateless 统 一 的 交 互 界 面 Uniform Interface ](../../media/API-Design-API-Rest-API--ACE-image1.png){width="10.083333333333334in" height="7.364583333333333in"}

Stateless -- don't need to remeber the previous operation and previous status





![统 一 的 交 互 界 面 以 资 源 为 核 心 资 源 的 表 现 形 式 描 述 性 信 息 0 允 许 返 回 链 接 ](../../media/API-Design-API-Rest-API--ACE-image2.png){width="10.083333333333334in" height="5.083333333333333in"}





![以 资 源 为 核 心 GET /users/123 ](../../media/API-Design-API-Rest-API--ACE-image3.png){width="10.083333333333334in" height="6.708333333333333in"}



![GET /users/123 Accept: text/plain <- GET /users/123 Accept: application/json <- JSON ](../../media/API-Design-API-Rest-API--ACE-image4.png){width="10.083333333333334in" height="7.541666666666667in"}

Return a jason format or txt format





![GET /departments/le "departmentld": 10 "departmentName": "Administration" , "locationld" : 1700, "managerld" : 200, " links " : "href": "10/emp10yees" , "rel": "employees" , "type " : "GET" ](../../media/API-Design-API-Rest-API--ACE-image5.png){width="10.083333333333334in" height="8.875in"}

Query a department, but if user want to know more about this department and employee in this department, we can send a "links" to user..



信息描述形式



![](../../media/API-Design-API-Rest-API--ACE-image6.png){width="10.083333333333334in" height="7.291666666666667in"}



![](../../media/API-Design-API-Rest-API--ACE-image7.png){width="10.083333333333334in" height="6.802083333333333in"}



![](../../media/API-Design-API-Rest-API--ACE-image8.png){width="10.083333333333334in" height="6.583333333333333in"}

Post = create new

![](../../media/API-Design-API-Rest-API--ACE-image9.png){width="10.083333333333334in" height="6.270833333333333in"}

Put = update idempotent







![](../../media/API-Design-API-Rest-API--ACE-image10.png){width="0.22916666666666666in" height="0.22916666666666666in"}



![](../../media/API-Design-API-Rest-API--ACE-image11.png){width="10.083333333333334in" height="6.09375in"}![](../../media/API-Design-API-Rest-API--ACE-image12.png){width="10.083333333333334in" height="6.635416666666667in"}



![思 考 题 八 对 于 资 源 users or [user 如 果 在 跟 该 资 源 有 关 的 一 系 列 API 中 可 能 返 回 多 个 资 源 ， 就 优 先 选 用 [users ](../../media/API-Design-API-Rest-API--ACE-image13.png){width="10.083333333333334in" height="10.104166666666666in"}



![/users/:id/email or /email /users/:id/email ](../../media/API-Design-API-Rest-API--ACE-image14.png){width="10.083333333333334in" height="9.979166666666666in"}

Nest resource



![](../../media/API-Design-API-Rest-API--ACE-image15.png){width="10.083333333333334in" height="5.375in"}![](../../media/API-Design-API-Rest-API--ACE-image16.png){width="0.4479166666666667in" height="0.625in"}



ID should only put the request body

![](../../media/API-Design-API-Rest-API--ACE-image17.png){width="10.083333333333334in" height="5.71875in"}





![](../../media/API-Design-API-Rest-API--ACE-image18.png){width="10.083333333333334in" height="7.979166666666667in"}

Consider search and like are nouns



取消like



Delete like ...


















