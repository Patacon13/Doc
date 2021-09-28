# Notebook Sir-Lock 221B

## Cabecera

```c++
#include <iostream>
#include <iomanip>
#include <algorithm>
#include <map>
#include <vector>
#include <deque>
#include <set>
#include <cmath>
#include <queue>

typedef long long tint;
using namespace std;

#define forsn(i,s,n) for(int i = (s);i < (int)(n);i++)
#define forn(i,n) forsn(i,0,n)
#define dforsn(i,s,n) for(int i = (n) - 1;i >= (int)(s);i--)
#define dforn(i,n) dforsn(i,0,n)
#define all(v) (v).begin(), (v).end()

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    return 0;
}

```

## Comparación de flotantes

Usar == puede ser un problema para comparar dos números ya que no siempre las operaciones con flotantes son exactas. Una estrategia útil es comparar contra un ε

```c++
if (abs(a-b) < 1e-1) {
    // a == b
}
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

En el código se debe tener en cuenta que, en primer lugar se debe realizar un dfs1 en todos los nodos. Luego, invertir el “orden”, y, finalmente, realizar el dfs2. El algoritmo devuelve todas las componentes fuertemente conexas. Sin embargo, entiende a una componente simple como fuertemente conexa. Por eso se tendría que mirar si está detectando más de 1 nodo.

### Orden topológico

```c++
vector<int> topSort(vector<vector<int>> g, vector<int> inGrade) {
	int n = inGrade.size();
	vector<int> topSorted;
	queue<int> q;
	for(int i = 0; i< n; i++) if(inGrade[i] == 0) q.push(i);
	while(!q.empty()) {
		int node = q.front(); q.pop();
		topSorted.push_back(node);
		for(int y : g[node]) if(--inGrade[y] == 0) q.push(y);
	}
	if(topSorted.size() < n) topSorted.clear();
	return topSorted;
}
```

### Algoritmos distancias en grafos ponderados

### Dijkstra

```c++
void init(int src) {
	for(int i = 0; i<dist.size(); i++){
		padre[i] = i;
		dist[i] = INF;
	}
	dist[src] = 0;
}

int getCercano() {
	int valorMinimo = INF;
	int nodoMinimo = 0;
	for(int i = 0; i<dist.size(); i++) {
		if(!visitados[i] && dist[i] < valorMinimo) {
			valorMinimo = dist[i];
			nodoMinimo = i;
		}
	}
	return nodoMinimo;
}

void dijkstra() {
	for(int i = 0; i<dist.size(); i++) {
		int cercano = getCercano();
		visitados[cercano] = true;
		for(int adj = 0; adj < dist.size(); adj++) {
			if(costos[cercano][adj] != 0 && dist[adj]>dist[cercano]+costos[cercano][adj]) {
				dist[adj] = dist[cercano]+costos[cercano][adj];
				padre[adj] = cercano;
			}
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
bool unite(int a, int b) {
    a = find(a);
    b = find(b);
    if(a == b) return false;
    if(size[a] < size[b]) swap(a,b);
    size[a] += size[b];
    link[b] = a;
    return true;
}
```

### Find

```c++
int find(int x) {
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

Arista es un vector de par de pares. aristas[i].first -> peso. aristas[i].second.first -> primer nodo conexión. aristas[i].second.second -> segundo nodo conexión. Requiere union-find. Devuelve el tamaño total del mst.

```c++
int kruskal(vector<pair<int,pair<int,int>>> grafo) {
	int ret = 0;
	for(unsigned i = 0; i<grafo.size(); i++) {
		if(!same(grafo[i].second.first, grafo[i].second.second)) {
			unite(grafo[i].second.first,grafo[i].second.second);
			mst.push_back(grafo[i]);
			ret += grafo[i].first;
		}
	}
	return ret;
}
```

# Teoría de números

## Potencia binaria

```c++
long long binpow(long long a, long long b) {
    long long res = 1;
    while (b > 0) {
        if (b & 1)
            res = res * a;
        a = a * a;
        b >>= 1;
    }
    return res;
}
```

## MCD

```c++
int mcd(int a, int b)  {  
    if (a == 0)
        return b;  
    return mcd(b % a, a);  
}  
```

## MCM


```c++
int mcm(int a, int b)  {  
    return a*(b/mcd(a, b));  
} 
```

## MCM para un vector

```c++
int mcmvector(vector<int> a){
	int aux=mcm(a[0],a[1]);
	for(int i=2;i<a.size();i++){
		aux=mcm(aux,a[i]);
	}
	return aux;
}
```

## Verificar si un número es primo

```c++
bool esPrimo(int n) {
    if(n<2) return false;
        for(int x = 2; x*x <= n; x++) {
	    if(n%x == 0) return false;
	}
	return true;
}
```

## Criba de Erastótenes

```c++
vector<int> primos;
bool criba[33000];

for(int i=2;i<33000;i++){
	criba[i]=true;
}

for(int i=2;i<33000;i++){
	if(criba[i]==false){
		continue;
	}
	else{
		primos.push_back(i);
	}
	for(int j=2*i;j<33000;j+=i){
		criba[j]=false;
	}
}
```

## Factorización

```c++
vector<int> factors(int n) {
    vector<int> f;
    for(int x = 2; x*x <= n; x++) {
        while(n%x == 0) {
	    f.push_back(x);
	    n /= x;
	}
    }
    if(n > 1) f.push_back(n);
    return f;
}
```
# Range queries

## Queries estáticas

### Suma de prefijos

```c++
int sum(int a, int b) {
    int s = 0;
    for(int i = a; i<= b; i++) {
        s += array[i];
    }
    return s;
}	
```

### Longest Increasing Subsequence

```C++
LIS[1] = 1;

//Busco todos los casos
for(int i=2; i<=n; i++){

	//Consideramos a 0 como el menor valor posible
	maximo = -1;
	for(int k=1; k<i; k++){
		if(a[k] < a[i]){
  			int maximoLocal = LIS[k] + 1;
  			if(maximoLocal > maximo){
    				maximo = maximoLocal;
  			}
		}
	}
	LIS[i] = maximo == -1 ? 1 : maximo;
}
```

## Queries dinámicas

### Fenwick tree

Dado un vector de enteros llamado tree generamos dos funciones que nos servirán para "hacer una suma de prefijos dinámica"

Obtener suma:

```c++
int sum(int k) {
    int s = 0;
    while(k >= 1) {
        s += tree[k];
	k -= k&-k;
    }
    return s;
}
```

Añadir un número X al elemento en K

```c++
int add(int k, int x) {
    while(k <= n) {
        tree[k] += x;
	k += k&-k;
    }
}
```

### Segment tree

El segment tree soporta rangos, a diferencia de el fenwick que realizará la consulta desde el primer elemento hasta un n. Es una estructura de datos mucho más general.

```
El tamaño del árbol será 2n
```

Obtener suma:

```c++
int sum(int a, int b) {
    a += n;
    b += n;
    int s = 0;
    while(a <= b) {
        if(a%2 == 1) s += tree[a++];
	if(b%2 == 0) s += tree[b--];
	a /= 2;
	b /= 2;
    }
    return s;
}
```

Añadir un número X al elemento en K

```c++
void add(int k, int x) {
    k += n;
    tree[k] += x;
    for(k /= 2; k >= 1; k /= 2) {
        tree[k] = tree[2*k]+tree[2*k+1];
    }
}
```

# Programación dinámica

## Ejemplo mochila

```c++
int mochilaDinamica2(int itemsC, int pesoMaximo) {
	if(itemsC == 0) return 0;
	if(dp[itemsC][pesoMaximo] != -1) return dp[itemsC][pesoMaximo];
	dp[itemsC][pesoMaximo] = mochilaDinamica2(itemsC-1, pesoMaximo);
	if(pesoMaximo >= items[itemsC].peso) {
		dp[itemsC][pesoMaximo] = max(dp[itemsC][pesoMaximo], items[itemsC].valor + mochilaDinamica2(itemsC - 1, pesoMaximo - items[itemsC].peso));
	}
	return dp[itemsC][pesoMaximo];
}
```
