---
layout: post
title:  "BFS dan DFS"
date:   2018-10-18 21:00:00 +7
categories: [Graph]
author: Tony
profpic: default
author-description: Informatika 2016
---

Breadth-First Search (BFS):
BFS merupakan algoritma yang mengtransversal pada graf yang memilih node terdekat terlebih dahulu.

Depth-First Search (DFS):
DFS merupakan algoritma yang mengtransversal pada graf yang memilih anak node terlebih dahulu.

Persoalan:
Diberfikan graf berarah dan sebuan simpul (vertex) S, tentukan banyak sisi (edge) terdikit yang harus dilalui dari S ke semua simpul pada graf.

Solusi:
```c++
vector<int> adj[MAXN]; /* adjecency list dari graf */
int dist[MAXN];

int dfs(int node){
    for(int next_node : adj[node]){
        if(dist[next_node]==-1){
            dist[next_node] = dist[node]+1;
            dfs(next_node);
        }
    }
}

int main(){
    int start;
    dist[start] = 0;
    dfs(start);
}
```

Implementasi diatas tidak menghasilkan jawaban yang minimal.
Untuk dapat mendepatkan jawaban optimal menggunakan dfs, kompleksitas waktu akan eksponensial.


Hal ini dapat dapat diselesaikan dengan optimal secara linear dengan menggunakan algoritma Breadth-First Search.

```c++
vector<int> adj[MAXN]; /* adjecency list dari graf */
int dist[MAXN];
/* dist di set dengan -1 untuk membedakan apakah node i sudah pernah dilewati atau belum dengan jarak terdekat */
memset(dist, -1, sizeof dist);
int start;
queue<int> q;
q.push(start);
dist[start] = 0;
while(!q.empty()){
    int now = q.front();
    q.pop();
    for(int next_node : adj[node]){
        if(dist[next_node]==-1){
            dist[next_node] = dist[now]+1;
            q.push(next_node);
        }
    }
}
/*
    dist[i] = jarak terdekat dari start ke i, jika dist[i] != -1
*/
```
