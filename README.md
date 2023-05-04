Download Link: https://assignmentchef.com/product/solved-csc212-programming-assignment-4-recommending-friends-on-social-networks
<br>
<h1>1             Introduction</h1>

One of the main functionalities offered by online social platforms such as Facebook and Twitter is the recommendation of new friends. This is achieved by utilizing various information about the users, but the main factor used for recommending a new friend to a user is how well these two users are connected. A social network such as Facebook can be represented as undirected graph such as the one shown in Figure 1. We can use the information contained in the graph to select the top candidate friends for a given user. There are many ways to do this, but we will focus on two methods:

<ol>

 <li><strong>Popular users</strong>: In this method, we recommend the most popular users in the graph, that is nodes with the highest degrees (number of neighbors).</li>

</ol>

<strong>Example 1. </strong><em>If we want to recommend 4 new friends for user 3 using the popular users method, we recommend:</em>

<ul>

 <li><em>User 8, which has degree 7.</em></li>

 <li><em>User 12, which has degree 5.</em></li>

 <li><em>User 4, which has degree 3.</em></li>

 <li><em>User 6, which has degree 3 (we break ties according to user ID).</em></li>

</ul>

<ol start="2">

 <li><strong>Common neighbors</strong>: In this method, we recommend users who have the most common friends with the user.</li>

</ol>

<strong>Example 2. </strong><em>If we want to recommend 4 new friends for user 3 using the common neighbors method, we recommend:</em>

<ul>

 <li><em>User 4, which has 2 common neighbors with 3, nodes 1 and 5. (b) User 6, which has 2 common neighbors with 3, nodes 2 and 5.</em></li>

 <li><em>User 12, which has 1 common neighbor with 3, node 1.</em></li>

 <li><em>User 8, which has 0 common neighbors with 3 (we break ties according to user ID).</em></li>

</ul>

Figure 1: Example of a graph representing a social network.

In this assignment, your are going to create data structures to represent graphs and use them to implement these friend recommendation methods.

<h1>2             The data structures</h1>

In this section, we present the data structures necessary for this assignment.

<h2>2.1           Implementing a top <em>k </em>priority queue</h2>

To recommend top <em>k </em>users, we use a priority queue that keeps only the top <em>k </em>elements and serves them in decreasing order of priority. For this, write the class PQKImp that implements the interface PQK below.

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>public interface </strong>PQK&lt;P <strong>extends </strong>Comparable&lt;P&gt;, T&gt; {<em>// Return the length of the queue </em><strong>int </strong>length();<em>// Enqueue a new element. The queue keeps the k elements with the highest priority. In case of a tie apply FIFO. </em><strong>void </strong>enqueue(P pr, T e);<em>// Serve the element with the highest priority. In case of a tie apply</em><em>FIFO.</em>Pair&lt;P, T&gt; serve();}</td>

  </tr>

 </tbody>

</table>

The class PQKImp takes the parameter <em>k </em>as parameter in the constructor:

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>public class </strong>PQKImp&lt;P <strong>extends </strong>Comparable&lt;P&gt;, T&gt; <strong>implements </strong>PQK&lt;P, T&gt; { <strong>public </strong>PQKImp(<strong>int </strong>k) {…}…}</td>

  </tr>

 </tbody>

</table>

<h2>2.2           Implementing a map</h2>

In this step, you write a BST implementation of a map. The Map interface is as follows:

<em>2.2     Implementing a map</em>

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>public interface </strong>Map&lt;K <strong>extends </strong>Comparable&lt;K&gt;, T&gt; {<em>// Return the size of the map. </em><strong>int </strong>size();<em>// Return true if the map is full. </em><strong>boolean </strong>full();<em>// Remove all elements from the map. </em><strong>void </strong>clear();<em>// Update the data of the key k if it exists and return true. If k does not exist, the method returns false.</em><strong>boolean </strong>update(K k, T e);<em>// Search for element with key k and returns a pair containing true and its data if it exists. If k does not exist, the method returns</em><em>false and null.</em>Pair&lt;Boolean, T&gt; retrieve(K k);<em>// Insert a new element if does not exist and return true. If k already exists, return false.</em><strong>boolean </strong>insert(K k, T e);<em>// Remove the element with key k if it exists and return true. If the element does not exist return false.</em><strong>boolean </strong>remove(K k);<em>// Return the list of keys in increasing order. </em>List&lt;K&gt; getKeys();}</td>

  </tr>

 </tbody>

</table>

Notice that this interface does not have a current element. Write the class BSTMap that implements the interface Map:

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>public class </strong>BSTMap&lt;K <strong>extends </strong>Comparable&lt;K&gt;, T&gt; <strong>implements </strong>Map&lt;K, T&gt; { <strong>public </strong>BSTNode&lt;K, T&gt; root; <em>// Do not change this</em>… <strong>public </strong>BSTMap() { …}}</td>

  </tr>

 </tbody>

</table>

The interface List is defined as follows:

<strong>public interface </strong>List&lt;T&gt; {

<strong>boolean </strong>empty(); <strong>boolean </strong>full(); <strong>void </strong>findFirst(); <strong>void </strong>findNext(); <strong>boolean </strong>last(); T retrieve(); <strong>void </strong>update(T e); <strong>void </strong>insert(T e);

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>void </strong>remove();<em>// Return the number of elements in the list. </em><strong>int </strong>size();<em>// Searches for e in the list. Current must not change.</em><strong>boolean </strong>exists(T e);}</td>

  </tr>

 </tbody>

</table>

<h1>3             Representing the social network</h1>

To represent the friendship graph, we use the following interface:

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>public interface </strong>Graph&lt;K <strong>extends </strong>Comparable&lt;K&gt;&gt; {<em>// Add a node to the graph if it does not exist and return true. If the node already exists, return false.</em><strong>boolean </strong>addNode(K i);<em>// Check if i is a node </em><strong>boolean </strong>isNode(K i);<em>// Add an edge to the graph if it does not exist and return true. If i or j do not exist or the edge (i, j) already exists, return false. </em><strong>boolean </strong>addEdge(K i, K j);<em>// Check if (i, j) is an edge. </em><strong>boolean </strong>isEdge(K i, K j);<em>// Return the set of neighbors of node i. If i does not exist, the method returns null.</em>List&lt;K&gt; neighb(K i);<em>// Return the degree (the number of neighbors) of node i. If i does not exist, the method returns -1.</em><strong>int </strong>deg(K i);<em>// Return a list containing the nodes in increasing order.</em>List&lt;K&gt; getNodes();}</td>

  </tr>

 </tbody>

</table>

We will use adjacency list representation, but instead of an array of lists, we use a map of lists. Each list in the map represents the neighbors of a node. Write the class MGraph that implements the interface Graph using this representation:

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>public class </strong>MGraph&lt;K <strong>extends </strong>Comparable&lt;K&gt;&gt; <strong>implements </strong>Graph&lt;K&gt; {<strong>public </strong>Map&lt;K, List&lt;K&gt;&gt; adj; <em>// Do not change this </em><strong>public </strong>MGraph() { …}…}</td>

  </tr>

 </tbody>

</table>

<h1>4             The friends recommender</h1>

Write the class Recommender that implements the two friends recommendation methods discussed above:

<table width="628">

 <tbody>

  <tr>

   <td width="628"><strong>import </strong>java.io.File; <strong>import </strong>java.util.Scanner; <strong>public class </strong>Recommender {<em>// Return the top k recommended friends for user i using the popular nodes method. If i does not exist, return null. In case of a tie, users with the lowest id are selected.</em><strong>public static </strong>&lt;K <strong>extends </strong>Comparable&lt;K&gt;&gt; PQK&lt;Double, K&gt; recommendPop(Graph&lt;K&gt; g, K i, <strong>int </strong>k) { <strong>return null</strong>;}<em>// Return the top k recommended friends for user i using common neighbors method. If i does not exist, return null. In case of a tie , users with the lowest id are selected. </em><strong>public static </strong>&lt;K <strong>extends </strong>Comparable&lt;K&gt;&gt; PQK&lt;Double, K&gt; recommendCN(Graph&lt;K&gt; g, K i, <strong>int </strong>k) { <strong>return null</strong>;}}</td>

  </tr>

 </tbody>

</table>


