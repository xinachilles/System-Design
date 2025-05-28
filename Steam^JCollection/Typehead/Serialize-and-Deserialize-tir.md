# Serialize and Deserialize tire

Created: 2017-10-12 16:00:14 -0600

Modified: 2017-10-12 16:05:17 -0600

---

![10 * Definition of TrieNode: 11 * public class TrieNode 12 public NavigableMap<Character, TrieNode> children; 13 public TrieNode() 14 children --- new TreeMap<Character, ) ; 15 16 18 class Solution { 19 20 This method will be invoked first, you should design your own algorithm 21 * to serialize a trie which denote by a root node to a string which 22 23 24 25 26 27 2B 29 30 31 32 33 34 35 36 37 3B 39 40 41 can public be easily deserialized by your own "deserialize" method later. Str ing serialize (TrieNode root) { Write your code here (root null) yet urn StringBuffer sb = flew StringBuffer(); sb. append( Iterator iter --- root. children. entryset( ) . iterator(); while (iter .hasNext()) { Map.Entry entry = (Map. Entry) iter .next( ) ; Character key Tr ieNode child sb. append (key) = (Character ) entry. getKey() ; = (TrieNode )entry. getValue(); sb. append (ser ialize(child) ) ; sb. append( sb. tostring( ) ; zetu:n ](../../media/Steam^JCollection-Typehead-Serialize-and-Deserialize-tire-image1.png){width="5.0in" height="4.666666666666667in"}



![data is what exactly the data is not given by So the format of data is you serialize it in 50 51 52 53 54 57 59 60 61 62 63 64 67 69 70 71 72 73 74 * This method will be invoked second, the argument * you serialized at method "serialize' that means * system, it's given by your own serialize method. * designed by yourself, and deserialize it here as * " serialize" method. public TrieNode deseria1ize(String data) { // Write your code here (data null I data. length( ) zee urn null; Tr ieNode root TrieNode(); TrieNode current = root; Stack<TrieNode> path --- Stack<TrieNode>( ) ; foy (Character c : data. toCharArray()) { path. push (current) ; break; path. pop( ) ; break; deLaaLC: TrieNode() ; current new path. peek ( ) . children. put (c, zetuyn root ; current) ](../../media/Steam^JCollection-Typehead-Serialize-and-Deserialize-tire-image2.png){width="5.0in" height="4.6875in"}




