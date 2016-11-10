---
layout: post
title: "Topological Sorting"
date: 2016-11-10 16:28:45 -0500
comments: true
categories: Algorithms

tags: [graph, algorithms]
keywords: graph, algorithms, lintcode, leetcode
description: topological sorting
---


Given an directed graph, a topological order of the graph nodes is defined as follow:

For each directed edge A -> B in graph, A must before B in the order list.
The first node in the order can be any node in the graph with no nodes direct to it.
Find any topological order for the given graph.  

~~~
Notice
You can assume that there is at least one topological
order in the graph.
~~~

Analyse:
入度为0的点拿出来做为第一个点。用hashmap来存一个点的入度。  
1. Using hashmap to store every node's indegree;     
2. Find the node whose indegree is 0.     
3. BFS from the node. When traverse to a neighbor of the node, minus 1 the value of map[neighbor], when the value was minus to 0, add it to the queue and the result vector.

扩展：通过入度可以判断一个有向图有没有环。 环中所有点入度都不为0.

~~~c++
/**
 * Definition for Directed graph.
 * struct DirectedGraphNode {
 *     int label;
 *     vector<DirectedGraphNode *> neighbors;
 *     DirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
public:
    /**
     * @param graph: A list of Directed graph node
     * @return: Any topological order for the given graph.
     */
    vector<DirectedGraphNode*> topSort(vector<DirectedGraphNode*> graph) {
        unordered_map<DirectedGraphNode*, int> map;
        for(DirectedGraphNode* node : graph){
            for(DirectedGraphNode* neighbor: node->neighbors){
                if(map.find(neighbor) == map.end()){
                    map[neighbor] = 1;
                }
                else{
                    map[neighbor] = map[neighbor] + 1;
                }
            }
        }
        
        vector<DirectedGraphNode*> res;
        queue<DirectedGraphNode*> q;
        for(DirectedGraphNode* node : graph){
            if(map.find(node) == map.end()){
                q.push(node);
                res.push_back(node);
            }
        }
        
        
        while(q.size() != 0) {
            DirectedGraphNode* node = q.front(); q.pop();
            for(DirectedGraphNode* neighbor: node->neighbors){
                map[neighbor] -= 1;
                if(map[neighbor] == 0){
                    q.push(neighbor);
                    res.push_back(neighbor);
                }
            }
        }
        return res;  
    
    }
};
~~~
