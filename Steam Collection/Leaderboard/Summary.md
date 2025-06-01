# Summary 



---

[The root node has 3 bucket for the range and each bucket will have a point point to the next level]{.mark}



[Each bucket will store the number of player in this range]{.mark}



[The leaf node will be the store the partical score]{.mark}



To determine the rank for a player ,who has a score of 30, the library only needs to read four entities ---



to add up the number of players who have a score higher than 30. With 22 players to the "right" in four entities, the player rank is 23rd.

Likewise, a call to SetScore only needs to update four entities. Even if you have a large number of different scores, Datastore access will only increase at O(log n) and is not affected by the number of players. In practice, [the Code Jam ran]{.mark}


