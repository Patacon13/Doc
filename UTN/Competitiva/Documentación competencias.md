# Notebook Sir-Lock 221

## Cabecera

```c++
#include <iostream>
#include <algorithm>
#include <map>
#include <vector>
#include <deque>
#include <set>
#include <cmath>


typedef long long tint;
using namespace std;

#define forsn(i,s,n) for(int i = (s);i < (int)(n);i++)
#define forn(i,n) forsn(i,0,n)
#define dforsn(i,s,n) for(int i = (n) - 1;i >= (int)(s);i--)
#define dforn(i,n) dforsn(i,0,n)
#define all(v) (v).begin(), (v).end()
```

## Criterio en sort

```c++
bool criterio(const quimico&a, const quimico&b) {
    if(a.num!=b.num) {
        return a.num<b.num;
    }
    else {
        if(a.ganancia!=b.ganancia) {
            return a.ganancia<b.ganancia;
        }
    }
}
```

## Binary Search

```c++
int a = 0, b = n-1;
while (a <= b) {
    int k = (a+b)/2;
    if (array[k] == x) {
        // x found at index k
    }
    if (array[k] > x) b = k-1;
    else a = k+1;
}
```

## Grafos 
### BFS

Verifica cuáles nodos son vecinos. También da mínima distancia en grafos no ponderados (todos los vértices tienen la misma distancia).
```c++
/*
 *@param graph listas de adyacencia.
 *@param v numero de nodo a analizar sus vecinos/distancia contra vecinos.
  *@return d retorna vector de enteros indicando distancia a vecinos. Si la distancia es = -1, entonces no es                                                         
  *vecino.
*/
vector<int> bfs(vector<vector<int>>& graph, int v){
    vector<int> d(graph.size(), -1);
    queue<int> q;
    d[v] = 0;
    q.push(v);
    while(!q.empty()){ //mientras haya nodos por procesar
        int x = q.front();
        q.pop();
        for(int y: graph[x]){ // para cada y vecino de x
            if(d[y] == -1) { // si y no fue encolado aun
                d[y] = d[x] + 1;
                q.push(y);
            }
        }
    } //d contiene las distancias desde v
    return d; // (o -1 para los nodos inalcanzables)
}
```

### DFS

```c++
#define TAMANO 10
vector<bool> visitado(TAMANO, false);
vector<vector<int>> graph; //las listas tienen que estar completadas, no podes hacer DFS de la nada :P
/*
 *@param node el nodo a analizar
*/
void DFS(int node) {
    visitado[node] = true;
    for(int y: graph[node]) { //para cada y vecino de node
        if(!visitado[y]) { //si y no fue visitado
            DFS(y); //realizo el DFS correspondiente
        }
    }
}
```

DFS - Bipartito (colores)

```c++
bool dfs(int node, int clr) {
    color[node] = clr;
    for(int i = 0; i < n; i++) if(matAdy[node][i] == 1) {
        if(color[i] == color[node]) return false;
        if(color[i] == -1 && !dfs(i, 1-clr)) return false;
    }
    return true;
}
```

DFS - Kosaraju
```c++
void dfs1(int node) {
    visitado[node] = true;
    for(int y: vecinos[node]) {
            if(!visitado[y]) { 
                dfs1(y);
        }
    }
    orden.push_back(node);
}

void dfs2(int node) {
    visitado[node] = true;
    componentes.push_back(node);
    for(int y: vecinosReversa[node]) { 
            if(!visitado[y]) {
                    dfs2(y);
            }
    }
}
```

En el código se debe tener en cuenta que, en primer lugar se debe /*realizar un dfs1 en todos los nodos. Luego, invertir el “orden”, y, finalmente, realizar el dfs2. El algoritmo devuelve todas las componentes fuertemente conexas. Sin embargo, entiende a una componente simple como fuertemente conexa. Por eso se tendría que mirar si está detectando más de 1 nodo.

### Algoritmos distancias en grafos ponderados

### Dijkstra

```c++
//Calcula distancia mínima de todos a un nodo, incluso si es ponderado. Las aristas NO pueden ser negativas.
//seteo la distancia de todos a INF
for (int i = 1; i <= n; i++) distance[i] = INF;
//la distancia a sí mismo, es 0
distance[x] = 0;
q.push({0,x});
while (!q.empty()) {
    int a = q.top().second;
    q.pop();
    if (processed[a]) continue;
    processed[a] = true;
    for (auto u : adj[a]) {
        int b = u.first, w = u.second;
        if (distance[a]+w < distance[b]) {
            distance[b] = distance[a]+w;
            q.push({-distance[b],b});
        }
    }
}
```
### Bellman-Ford - Complejidad: O(n*m)

```c++
//Calcula ponderados que tienen aristas negativas.

vector<int> bellmanFord(int n, int m, vector<edge> &e) {
    vector<int> d (n, INF);
    d[v] = 0;
    for (int i=0; i<n-1; ++i)
        for (int j=0; j<m; ++j)
            if (d[e[j].a] < INF)
                d[e[j].b] = min (d[e[j].b], d[e[j].a] + e[j].cost);
    return d;
}
```

### Floyd-Warshall - es con dp. Puede que no se pueda usar en competitiva - Complejidad: O(n3)


```c++
//Lo mismo que el de arriba, pero con programación dinámica.

void floyd(vector<vector<int> > &d){
// d -> matriz inicial de distancias
    for(int k=0;k<n;k++)
        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

# Estructuras de Grafos
Es importante entender que existen estructuras que puedan requerir de otras. Kruscal necesita en sí mismo, de un union-find

## Union-Find

En primer lugar, hay que inicializar los sets link y size

```c++
for(int i = 0; i <= n; i++) link[i] = i;
for(int i = 0; i <= n; i++) size[i] = 1;
```

### Union

```c++
bool union(int a, int b) {
    a = find(a);
    b = find(b);
    if(a == b) return false;
    if(size[a] < size[b]) swap(a,b);
    size[a] += size[b]
    link[b] = a;
    return true;
}
```

### Find

```c++
void find(int x) {
    while(x != link[x]) x = link[x];
    return x;
}
```

### Averiguar si están en la misma componente

```c++
bool same(int a, int b) {
    return find(a) == find(b);
}
```

# Árboles de recorrido máximo

## Kruskal

Siendo aristas un vector de pares donde se ve la unión de primero-segundo, ordenado de menor a mayor según el peso. Requiere union-find

```c++
for(unsigned i = 0; i<aristas.size(); i++) {
    if(!same(aristas[i].first,aristas[i].second) union(aristas[i].first,aristas[i].second);
}
```

# Teoría de números

```c++
int mcd(int a, int b)  {  
    if (a == 0)
        return b;  
    return gcd(b % a, a);  
}  
```

```c++
int mcm(int a, int b)  {  
    return (a*b)/gcd(a, b);  
} 
```
