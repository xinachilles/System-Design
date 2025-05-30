# another way shading database



---

if we have two database, all of them responsible for read and write, there are also have two shadow database



if we want to add new machine, we don`t just add one , we add two or we can use those two shadow directly



before: mod 2, 0 will go to machine 1 and 1 will go to machine 1



after: mode4 0 and 2 will go to machine 1 and machine 3; 1 and 3 will go to machine 2 and 4



there are no any conflict we just need delete some data, for example, for machine 0, we need delete all data belong to machine 2
