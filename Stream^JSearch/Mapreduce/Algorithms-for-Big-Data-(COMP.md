# Algorithms for Big Data (COMPSCI 229r), Lecture 25



---

[Algorithms for Big Data (COMPSCI 229r), Lecture 25](https://www.youtube.com/watch?v=CwSbDshioQY)







Map: each item is processed by some map function, emit set of key value pair



Shuffle: all items emitted in the map phase grouped by key, item the same key

Sent to the same reducer



Reducer: machine received key,<value1, value2 ....> emit new set of items



Terasort

Input <i,a[i]> -- unsorted array

Want to use p machine

1.  Try to find smallest n/p elements and next smallest n/p elements ...
2.  Send jth batch of n/p to machine j to be sorted



Round 1

![de-c Md? '1 Ah) > ' NemoteAccess • Ittps //cratrdstatemrnuq!à: Play (k) ](../../media/Stream^JSearch-Mapreduce-Algorithms-for-Big-Data-(COMPSCI-229r),-Lecture-25-image1.png)



Mapper:

Partition i % p

Sample: base on P(T/n), if the we decided the element we need to be sampled

We sent it to reducer j



reducer



![、 5 s 。 丶 十 冖 ](../../media/Stream^JSearch-Mapreduce-Algorithms-for-Big-Data-(COMPSCI-229r),-Lecture-25-image2.png)




