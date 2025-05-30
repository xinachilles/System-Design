# new 



---

![Solution with sleep Crawler Tasks & Pages Crawler W h 'eltrue) L (taskTabIe) taskTabIeT ' NULL •(taskrable) L .(taskTab1e) task taskTable.F ' task.state •working• page • H task.tvpe "list' LOL For newTask task Table. r (newTask) task.state •done" Loc • (pageTabIe) pageTable.;. 'Apage) (pageTabIe) L '(taskTable) task_state "done" He; c(taskTaBe) ](../../media/Web-crawler-^MP2p-Web-Crawler-new-image1.png)



![Solution with conditional variable Tasks & Pages Crawler Crawler = c' (taskTa&e) While( taskTabIe. NULL ) Cond_Wait (cond, taskTabIe) task taskTabIe.i •neOaeCstatezne#) task-state "working" s (taskTabIe) page 'f task_tvpe •list" k(taskTab1e) newTask page: : (newTask) Cond _ task.state •done" L U V (pageTabIe) pageTable. (page) k (taskTabIe) task_state • •done" AA" (taskTab/e) ](../../media/Web-crawler-^MP2p-Web-Crawler-new-image2.png)



![Conditional variable Cond_Wait(cond, mutex) Lock (cond.threadWaitList); cond.threadWaitList.Add(this.thread); Release(cond.threadWaitList); Rejease(mutex) ; Block(this.thread); Lock(mutex); Cond_Signal(cond) Lock(cond. threadWaitList); cond.threadWaitList.size()>O ); thread = cond.threadWaitList.Pop(); Wakeup(thread); Re tease(cond.threadWaitList); ](../../media/Web-crawler-^MP2p-Web-Crawler-new-image3.png)



![Solution with semaphore Tasks & crawler Crawler W h' Icttrue) v o' (taskTabIe) task • taskTable.C task. state • "working• r(taskTabIe) page C' task_type "list- (taskTaBe) ' newTask task Table. 1 d: t (newTask) Sign a I I numberOfNewTask) task.state JW(taskTable) L OL V (page Table) pageTabIe_ r • I pageTabIe) k (taskTaE*e) task,state -done- Re T .3 co(taskTabIe) ](../../media/Web-crawler-^MP2p-Web-Crawler-new-image4.png)



![Semaphore Wait(semaphore) Lock(semaphore); semaphore. value"; semaphore.value<O ) semaphore.processWaitList. Ad d(this.process); Release (semaphore); Block(this.process); Re Lease (semaphore); Signal (semaphore) Lock(semaphore); semaphore -value"; process semaphore.processWaitList.Pop(); Wakeup (process); ](../../media/Web-crawler-^MP2p-Web-Crawler-new-image5.png)



![sender recewer = ConnectVecetverAddress) Whop' (true) task Send(receiver. par. task) Sender 'Z J t (numberOfF L '(task 'able') La: •(crawIerTabIe) task z task Table - task state • •working" crawler crawler. state = "busy" (crawler Table) task) Receiver page, crawler. task • Receive() crawler.state • fee" R A (crawler Table) n um b Of F ](../../media/Web-crawler-^MP2p-Web-Crawler-new-image6.png)



![](../../media/Web-crawler-^MP2p-Web-Crawler-new-image7.png)![](../../media/Web-crawler-^MP2p-Web-Crawler-new-image8.png)








