# quad tree



---

quad tree



[[Quadtrees](https://en.wikipedia.org/wiki/Quadtree) are a data structure that encode the who earth into a two-dimensional space with multiple cells.]{.mark}



[every non-leaf node has exactly four children.]{.mark}



In the context of location, these nodes represent the four quadrants: NW, NE, SW, and SE.



Each node in a quadtree can be recursively subdivided, with ~~successive subdivisions~~ resulting in smaller and smaller cells.



the idea is take the space and section into 4, 4 section, quad, and each section, could be sectioned into 4, and again each section can be into 4...





[Each point of interest should be in one cell and each cell can be present by a code]{.mark}

Each cell has limitation, if any cell reach the limitation the cell will be subdivided into smeller and smaller cell





We use 0 for left , 1 for right, 0 for top and 1 for bottom





Level 1 of quad tree should have 4 cells and his their code

00 -- top left,

01 -- top right,

10 -- bottom left

11 -- bottom right



Each level has 2 more digital compared with his upper level



0101 --- 0100, 0110, 0111 maybe also have 0001 from the his left section ,











the running time will reduce from n2 n square, to nlogn, if we want to check one given area, it just need long n time



~~wouldn't you have to restructure the quadtree every time ,an object moves and~~

~~in fact the answer to that question is absolutely 100% yes,~~





take all the point in the map and insert into the quadtree





![class Solution { public Node constructCintOC] grid) { return helperCgrid, O, O, grid. length); // x, y is Coordlinates in the upper left corner private Node grid, int x, int y, int ten) { if Clen - return new NodeCgridCxJ[y] O, true, null, null, null, null); Node Node Node Node Node to the value result new Node(); topLeft helper(grid, x, y, len / Z); topRight = helper(grid, x, y + len / Z, len / Z); bottomLeft = helper(grid, x + len / Z , Y, len / Z); bottomRight helper(grid, x + len Y + len / Z, len the current grid has the same value (i -e all I's or all Ø's) set tsLeaf True and set val of the grid and set the four children to Nun and stop. if CtopLeft. isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight. isLeaf && topLeft. val --- topRight.va1 && topRight.va1 - bottomLeft . val bottomLeft.va1 --- bottomRight. vat) { result.isLeaf true; result.val topLeft.va1; } else { result. topLeft = topLeft; result. topRight topRight; result. bottomLeft = bottomLeft; result. bottomRight bottomRight; return result; ](../../media/Location-Service-Basic-quad-tree-image1.png)



![Example 1: 0 0 Input: grid output: [[0,1], [1,0], Explanation: The explanation of this example is shown below: Notice that 0 represnts False and 1 represents True in the photo representing the Quad---Tree. Example 2: ](../../media/Location-Service-Basic-quad-tree-image2.png)





Don't store the objects itself in the quadtree. Put them into a flat structure (array, linked list, etc.) and have the Quadtree just keep a pointer to the actual objects. If the quadtree has a certain depth (on all nodes), you could flatten it as well.


