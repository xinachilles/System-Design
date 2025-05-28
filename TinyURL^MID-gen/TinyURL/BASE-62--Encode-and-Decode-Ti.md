# BASE 62 Encode and Decode TinyURL 

Created: 2017-04-30 15:58:43 -0600

Modified: 2017-10-21 09:04:17 -0600

---

![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image1.png){width="5.0in" height="2.423611111111111in"}



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image2.png){width="5.0in" height="4.1875in"}



![private int toBase62Cchar c){ if(c return c- return 1Ø+(c-'a'); return 36+(c-'A'); return e; private int ShortKeyToId(String shortKey){ int id O; for(int i e; i<shortKey. id id *62 + toBase62CshortKey. charAt(i)); return id; // Decodes a shortened URL to its original URL . public String decode(String shortUr1) { int id ShortKeyT01dCshortUr1); i f(idToString. containsKey(i return idToString.get(id); return ](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image3.png){width="5.0in" height="4.5625in"}



Accepted Solution



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image4.png){width="5.0in" height="4.868055555555555in"}



![private int toBase62Cchar c){ if(c return c- return 1Ø+(c-'a'); return 36+(c- return e; private int ShortKeyToId(String shortKey){ int id 0; for(int i --- e; id id *62 + toBase62(shortKey. charAt(i)); return id; // Decodes a shortened URL to its original URL . public String decode(String shortUr1) { int id ShortKeyToId(shortUr1. substring(URL length())) ; i f(idToString. containsKey(i return idToString. get(id); return ](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image5.png){width="5.0in" height="5.159722222222222in"}

![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image6.png){width="1.2777777777777777in" height="0.8125in"}



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image7.png){width="1.3680555555555556in" height="0.625in"}



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image8.png){width="2.9305555555555554in" height="1.0138888888888888in"}



先计算的 放在 最后

![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image9.png){width="3.7708333333333335in" height="0.8541666666666666in"}![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image10.png){width="3.423611111111111in" height="0.9236111111111112in"}

不用 for loop










