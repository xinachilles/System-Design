# BASE 62 Encode and Decode TinyURL 



---

![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image1.png)



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image2.png)



![private int toBase62Cchar c){ if(c return c- return 1Ø+(c-'a'); return 36+(c-'A'); return e; private int ShortKeyToId(String shortKey){ int id O; for(int i e; i<shortKey. id id *62 + toBase62CshortKey. charAt(i)); return id; // Decodes a shortened URL to its original URL . public String decode(String shortUr1) { int id ShortKeyT01dCshortUr1); i f(idToString. containsKey(i return idToString.get(id); return ](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image3.png)



Accepted Solution



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image4.png)



![private int toBase62Cchar c){ if(c return c- return 1Ø+(c-'a'); return 36+(c- return e; private int ShortKeyToId(String shortKey){ int id 0; for(int i --- e; id id *62 + toBase62(shortKey. charAt(i)); return id; // Decodes a shortened URL to its original URL . public String decode(String shortUr1) { int id ShortKeyToId(shortUr1. substring(URL length())) ; i f(idToString. containsKey(i return idToString. get(id); return ](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image5.png)

![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image6.png)



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image7.png)



![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image8.png)



先计算的 放在 最后

![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image9.png)![](../../media/TinyURL^MID-gen-TinyURL-BASE-62--Encode-and-Decode-TinyURL-image10.png)

不用 for loop










