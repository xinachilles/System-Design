# Consistent Hash

Created: 2017-04-29 17:37:22 -0600

Modified: 2020-12-21 17:26:58 -0600

---

![Consistent Hashing 一 个 简 单 的 一 致 性 Hash 算 法 ， 将 key 模 一 个 很 大 的 数 ， 比 如 360 · 将 360 分 配 给 n 台 机 器 ， 每 个 机 器 负 责 一 段 区 间 · 区 间 分 配 信 息 记 录 为 一 张 表 存 在 Web Server 上 · 新 加 一 台 机 器 的 时 候 ， 在 表 中 选 择 一 个 位 置 插 入 ， 匀 走 相 邻 两 台 机 器 的 一 部 分 数 据 · 比 如 n 从 2 变 化 到 3 ， 只 有 1 / 3 的 数 据 移 动 DBI: DBO: DBI: 180 ～ 359 DBO: 0 ～ 179 240 、 359 0 、 1 19 DB2: 120 ～ 239 禁 止 录 像 与 传 播 录 像 ， 否 则 将 追 究 法 律 责 任 和 经 济 赔 第 44 页 ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image1.png){width="5.0in" height="2.798611111111111in"}





































![Consistent Hashing Il Description D Notes Testcase Judge Consistent Hashing I micro-shards 95", I. * A 0-359 O- n-l MIE], k micro-shards, 3. hash function micro-shard n NoSQL 1000. micro-shard fi consistent hashing 1 . create(int n, int k) 2. addMachine(int machine_id) // add a new machine, return a list of shard ids. 3. getMachineldByHashCode(int hashcode) // return machine id ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image2.png){width="4.458333333333333in" height="6.5in"}



![i Notice 2A64fi, Have you met this question in a real interview? Yes Example create (lee, 3) addMachine(1) [3, 41, gø] getMachineIdByHashCode (4) addMachine(2) [11, 55, 831 getMachineIdByHashCode (61) getMachineIdByHashCode (91) ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image3.png){width="4.965277777777778in" height="5.4375in"}



![public class Solution { public int n, k; public Set<lnteger» ids null; public Map<lnteger, List<lnteger» machines // @param n a positive integer // @param k a positive integer // @return a Solution object getMachi neldByHashCode = null; public static Solution create(int n, int k) { // Write your code here Solution solution new Solution(); solution.n = n; solution. k solution. ids new solution. machines new HashMap<Integer, return solution; // @param machine_id an integer // @return a list of shard ids public List<lnteger» addMachine(int machine_id) { // Write your code here Random ra -new Random(); List<lnteger> random_nums = new for (int i int index ra.nextlnt(n); while (ids. contains(index)) index ra.nextlnt(n); ids . add(index); random_nums. add(i ndex) ; Coll ect ions. ; machines. random_nums); return random---nums; ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image4.png){width="5.0in" height="5.076388888888889in"}



![// @param hashcode an integer // @return a machine id public int getMachineIdByHashCode(int hashcode) { // Write your code here int distance n + 1; int machine_id Ø; for (Map. Entry<lnteger, entry : machines. entrySet()) { int key = entry .getKey(); List<lnteger> random_nums entry.getVa1ueC); for (Integer num . random_nums) { int d num - hashcode; if (d < 0) if Cd < distance) { distance = d; machine_id key; return ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image5.png){width="5.0in" height="3.0694444444444446in"}



![](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image6.png){width="5.0in" height="2.7152777777777777in"}





Consistent Hash

Traditional way just use value % size

Two problems:

if add new machine that mean the hash function is changed . We need re-calculated all the mapping, move almost everything to the new machine.

it is not loaded balance. the data was not distributed equality between the machines

For the **consistent hash**, almost k/n keys need to be remapped.

'k' is the total number of keys and 'n' is the total number of servers





Further

suppose there is a ring and we divided the ring into n part (2^64)

- every add a new machine, sign k( 1000) randomly to the ring. Those k called virtual node ( the number should less then 2^64)

**To map key to the machine:**

Hash it to a single integer ( value % 2 ^64)

Move clockwise on the ring until finding the first virtual node ~~or first machine~~ it encounters. we will that machine to store than value.

**Added a new machine:**

To add a new server, say D, keys that were originally stored his neighbor (next key, clockwise) will shifted to D

For example 1-2-A->3->C->4-B

C is new machine and will ask B for node 3

**Delete a new machine:**

To remove a machine or if a machine failed, say A, all keys or data that were originally mapping to A will fall into B, and only those keys need to be moved to B, other keys will not be affected.





![东 邪 黄 药 用 是 顺 时 针 没 错 。 蛤 你 举 个 例 子 ， 加 入 说 环 上 目 前 顺 时 针 的 情 况 分 别 是 ： 一 > 2 那 么 此 时 数 据 1 和 2 是 存 在 机 器 A 的 ， 数 据 3 和 4 是 存 在 机 器 B 的 。 然 后 此 时 新 加 入 一 个 机 器 C 。 这 个 机 器 hash 之 后 被 分 配 在 数 据 3 和 4 之 间 。 2016 ． 09 ． 07 也 就 是 说 ， 在 新 的 结 中 ， 3 需 要 存 在 机 器 C 上 ， 3 原 本 是 存 在 机 器 B 上 的 ， 所 以 他 要 顺 时 针 问 B 要 数 据 。 而 不 是 逆 时 ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image7.png){width="5.0in" height="2.8541666666666665in"}

![](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image8.png){width="5.0in" height="2.7916666666666665in"}





Bittger CS503 week2 C22



![Consistent Hashing 15 14 13 12 11 oe3 10 ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image9.png){width="5.0in" height="3.3541666666666665in"}



c1 is for 3 to 15

c2 is for 8 to 4

c3 is for 10 to 9

c4 is for 14 to 11



![](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Consistent-Hash-image10.png){width="1.4305555555555556in" height="2.3194444444444446in"}



if the value after hash is 12, we need find the first value bigger than 12, is db 4, so this value is stored in db4 ( clockwise around to find the first db he meet)










