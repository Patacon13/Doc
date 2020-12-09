# Fenwick de suma

```c++
int suma(int k){
	int s=0;
	while(k>=1){
		s+=fenwick[k];
		k-= k&-k;
	}
	return s;
}



void add(int k, int x){
	while(k<=cantidad){
		fenwick[k] += x;
		k += k&-k;
	}
}
```

# Segment de suma

```c++
//query
int suma(int a, int b) {
	a += n; 
	b += n;
	int s = 0;
	while (a <= b) {
		if (a%2 == 1) s += tree[a++];
		if (b%2 == 0) s += tree[b--];
		a /= 2;
		b /= 2;
	}
	return s;
}

//actualizacion
void add(int k, int x) {
	k += n;
	tree[k] += x;
	for (k /= 2; k >= 1; k /= 2) {
		tree[k] = tree[2*k] + tree[2*k+1];
	}
}
```

# Segment genérico

```c++
//query
char query(int a, int b) {
	a += n; 
	b += n;
	int s = 'valorinicial';
	while (a <= b) {
		if (a%2 == 1) s = 'funcion'(s, tree[a++]);
		if (b%2 == 0) s = 'funcion'(s, tree[b--]);
		a /= 2;
		b /= 2;
	}
	return s;
}

//actualizacion
void update(int k, int x) {
	k += n;
	tree[k] = 'funcion(x)';
	for (k /= 2; k >= 1; k /= 2) {
		tree[k] = 'funcion'(tree[2*k],tree[2*k+1]);
	}
}
```
# Segment de multiplicación

```c++
//query
int producto(int a, int b) {
	a += n; 
	b += n;
	int s = 1;
	while (a <= b) {
		if (a%2 == 1) s = s*tree[a++];
		if (b%2 == 0) s = s*tree[b--];
		a /= 2;
		b /= 2;
	}
	return s;
}

//actualizacion
void update(int k, int x) {
	k += n;
	tree[k] = tree[k]*x;
	for (k /= 2; k >= 1; k /= 2) {
		tree[k] = tree[2*k] * tree[2*k+1];
	}
}
```

# Segment de mínimo

```c++
//query
int minimo(int a, int b) {
	a += n; 
	b += n;
	int s = 100000000;
	while (a <= b) {
		if (a%2 == 1) s = min(s,tree[a++]);
		if (b%2 == 0) s = min(s,tree[b--]);
		a /= 2;
		b /= 2;
	}
	return s;
}

//actualizacion
void update(int k, int x) {
	k += n;
	tree[k] = min(tree[k],x);
	for (k /= 2; k >= 1; k /= 2) {
		tree[k] = min(tree[2*k],tree[2*k+1]);
	}
}
```

# Segment de máximo

```c++
//query
int maximo(int a, int b) {
	a += n; 
	b += n;
	int s = -100000;
	while (a <= b) {
		if (a%2 == 1) s = max(s,tree[a++]);
		if (b%2 == 0) s = max(s,tree[b--]);
		a /= 2;
		b /= 2;
	}
	return s;
}

//actualizacion
void update(int k, int x) {
	k += n;
	tree[k] = max(tree[k],x);
	for (k /= 2; k >= 1; k /= 2) {
		tree[k] = max(tree[2*k],tree[2*k+1]);
	}
}
```
