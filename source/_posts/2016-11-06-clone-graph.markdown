---
layout: post
title: "Clone Graph"
date: 2016-11-06 18:32:33 -0500
comments: true
categories: Algorithms
---

Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

How we serialize an undirected graph:

Nodes are labeled uniquely.

We use # as a separator for each node, and , as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph {0,1,2#1,2#2,2}.

The graph has a total of three nodes, and therefore contains three parts as separated by #.

1. First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
2. Second node is labeled as 1. Connect node 1 to node 2.
3. Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.
Visually, the graph looks like the following:  

~~~
   1  
  / \  
 /   \
0 --- 2
     / \
     \_/
~~~ 

``Analyse:``  
1. Find all nodes in the graph (BFS) ``vector<UndirectedGraphNode *> findAllNodes(UndirectedGraphNode *node){}``
2. Deep copy the nodes  ``void copyNodes(vector<UndirectedGraphNode *> nodes){}``
3. Link node with its neighbors  ``void findNeighbors(vector<UndirectedGraphNode *> nodes){}``

~~~C++
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
private:

    vector<UndirectedGraphNode *> findAllNodes(UndirectedGraphNode *node){
        
        //vector<UndirectedGraphNode *> res;
        queue<UndirectedGraphNode *> q;
        set<UndirectedGraphNode *> set;
        q.push(node);
        set.insert(node);
        
        while(q.size() != 0){
            UndirectedGraphNode *tmp = q.front(); q.pop();
            for(UndirectedGraphNode * n : tmp->neighbors){
                if(set.find(n) == set.end()){
                    q.push(n);
                    set.insert(n);
                }
            }
        }
        vector<UndirectedGraphNode *> res(set.begin(), set.end());
        return res;
        
    }
    
    unordered_map<UndirectedGraphNode *, UndirectedGraphNode *> map;
    
    void copyNodes(vector<UndirectedGraphNode *> &nodes){
        for(UndirectedGraphNode* n : nodes){
            map[n] = new UndirectedGraphNode(n->label);
            //cout<<map[n]->label<<endl;
        }
    }
    
    void findNeighbors(vector<UndirectedGraphNode *> nodes){
        for(UndirectedGraphNode* nd:nodes){
            UndirectedGraphNode* newNode = map[nd];
            vector<UndirectedGraphNode *> neighbors = nd->neighbors;
            for(UndirectedGraphNode* n : neighbors){
                newNode->neighbors.push_back(map[n]);
            }
        }
    }
public:
    /**
     * @param node: A undirected graph node
     * @return: A undirected graph node
     */
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if(node == NULL)
            return NULL;
        vector<UndirectedGraphNode *> nodes;
        //BFS find all nodes in the graph
        nodes = findAllNodes(node);
        //mapping old nodes to new nodes
        copyNodes(nodes);
        //link nodes
        findNeighbors(nodes);
        return map[node];
        
    }
};
~~~

problems that I met:  
1. set. Using set can eliminate the visited nodes. Like 0's neighbor is 1 and 2, while 1's neighbor is 0 and 2. But the 0 and 2 is already found.
2. deep copy. Pay attention to the neighbors of old nodes and new nodes. ``newNode->neighbors.push_back(map[n]);``  

There are other methods like recursion DFS, non-recursion DFS and using array instead of queue.

