# Bloom Filter 



---

whether an element is present in a set



can help us find out whether the key in the hashtable or not











A large bit vector

represents the set. An element is added to the set by computing 'n' hash functions of the element and setting the corresponding bits. An element is deemed to be in the set



if the bits at all 'n' of the element's hash locations are set.



The disadvantage to using a bloom filter for the URL seen test is that each false positive will cause the URL not to be added to the frontier, and therefore the document

will never be downloaded. The chance of a false positive can be reduced by making the bit vector larger.





that means "chao deng" may in the set



if 2 or 4 is 0 menas "chao deng" 100% not in the set.



use mutil hash function











![> r 7 ](../media/Basic-Bloom-Filter-image1.png)





![上 来 说 我 们 来 个 简 单 的 预 热 一 下 ， 难 题 留 在 后 面 几 轮 吧 。 让 我 写 个 Remove Duplciates from Array, 我 fiHashSet- 做 完 之 后 让 我 解 了 一 下 ， 然 后 问 你 知 道 b m filterß? 我 说 听 说 过 ， 然 后 给 我 大 概 讲 解 了 一 下 ， 讠 上 我 实 现 函 数 b 》 ean bloomfilter(int n)- 实 现 很 简 单 ， 是 每 调 用 一 次 这 个 函 数 ， 把 input n5--- 个 global 姹 丽 b 进 行 & 操 作 ， 然 后 冉 把 g ℃ ba 、 丽 ab 更 新 一 下 ， 最 后 返 回 & 操 作 的 结 果 。 然 后 问 我 b 》 m fi r 怎 么 样 应 用 到 刚 才 的 rem e duplicate 中 去， 我 在 检 查 数 组 元 素 是 否 在 hash 中 存 在 前 用 m ter （ nums [ 刂 〕 ](../media/Basic-Bloom-Filter-image2.png)





判断 int n 的 bloom filter, 比 string 简单



![class HashFunction public int cap, seed; public HashFunction(int cap, this. cap - cap; this. seed seed; int seed) { public int hash(String value) { int ret 0; int n = value. length(); for (int i ret +2 seed * ret + value.charAtCi); ret cap; return ret; ](../media/Basic-Bloom-Filter-image3.png)



![public class StandardBloomFilter { public BitSet bits; public int k; public List<HashFunction» hashFunc; public StandardBloomFi1ter(int k) { // initialize your data structure here this. k k hashFunc new for (int i hashFunc.add(nen HashFunction(1ØOØØØ + i, bits - - new BitSet(1ØØOØØ + k); public void add(String word) { // Write your code here for (int i int position hashFunc bi ts. set(position) ; public boolean contains(String word) { // Write your code here for (int i int position hashFunc.getCi).hash(word); if (!bits.get(position)) return false; return true; ](../media/Basic-Bloom-Filter-image4.png)



![](../media/Basic-Bloom-Filter-image5.png)![](../media/Basic-Bloom-Filter-image6.png)![](../media/Basic-Bloom-Filter-image7.png)![](../media/Basic-Bloom-Filter-image8.png)![](../media/Basic-Bloom-Filter-image9.png)![](../media/Basic-Bloom-Filter-image10.png)![](../media/Basic-Bloom-Filter-image11.png)![](../media/Basic-Bloom-Filter-image12.png)![](../media/Basic-Bloom-Filter-image13.png)













