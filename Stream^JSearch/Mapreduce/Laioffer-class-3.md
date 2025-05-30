# Laioffer class 3 



---

![!춏하국느다蕣 ! ](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image1.png)



![](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image2.png)





Original file -> chunk, mapper -> map output file -> partition,sort and shuffer -> reducer ->final file





Word count, partition by hash(words)

All the apple, count go to the same partition

![](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image3.png)











Sort , partition by the

![](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image4.png)



![](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image5.png)

map partition the input data

partition



![](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image6.png)



![](../../media/Stream^JSearch-Mapreduce-Laioffer-class-3-image7.png)





If sorted by value



1.  Word count
2.  Different reducer... 1-100 ->reducer 1 , 100-200 reducer 2, then sort in the different reducer and merge
3.  Into the output file







