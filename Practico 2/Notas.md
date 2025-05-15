## Lista de operaciones y funciones:
### TAD de listas

```pascal
CONSTRUCTORS:
	fun empty() ret l: list of T
	{- crea una lista vacia -}
	
	proc addl(in e: T, in/out l: List of T)
	{- agrega el elemento al comienzo de la lista -}


DESTROY:
	proc destroy(in/out l: List of T)
	{- libera memoria en caso que sea necesario -}


OPERATIONS:
	fun is_empty(l: List of T) ret b: bool
	{- devuelve True si l es vacia -}

	fun head(l: List of T) ret e: T
	{- devuelve el primer elemento de la lista l -}
	{- PRE: not is_empty() -}

	proc tail(in/out l: List of T)
	{- elimina el primer elemento de la lista l -}
	{- PRE: not is_empty() -}

	proc addr(in/out l: List of T, in e: T)
	{- agrega el elemento e al final de la lista -}

	fun length(l: List of T) ret n: nat
	{- devuelve la cantidad de elementos de la lista l -}

	proc concat(in/out l: List of T, in IO: List of T)
	{- agrega al final de l todos los elementos de IO en el mismo orden}

	fun index(l: List of T, n: nat) ret e: T
	{- devuelve el n-esimo elemento de la lista -}
	{- PRE: length(l) < n -}

	proc take(in/out l: List of T, in n: nat)
	{- deja en l solo los primeros n elementos eliminando el resto -}

	proc drop(in/out l: List of T, in n:nat)
	{- elimina los primeros n elementos de la lista l -}

	fun copy_list(l1: List of T) ret l2: List of T
	{- copia todos los elementos de la lista l1 en la nueva lista l2 -}
```

### TAD contador

```pascal
constructors
	fun init() ret c : Counter
	{- crea un contador inicial. -}
	proc incr (in/out c : Counter)
	{- incrementa el contador c. -}

destroy
	proc destroy (in/out c : Counter)
	{- Libera memoria en caso que sea necesario. -}

operations
	fun is_init(c : Counter) ret b : Bool
	{- Devuelve True si el contador es inicial -}
	proc decr (in/out c : Counter)
	{- Decrementa el contador c. -}
	{- PRE: not is_init(c) -}
```



### TAD Pila
```pascal
spec Stack of T where

constructors
	fun empty_stack() ret s : Stack of T
	{- crea una pila vacía. -}
	
	proc push (in e : T, in/out s : Stack of T)
	{- agrega el elemento e al tope de la pila s. -}

operations
	fun is_empty_stack(s : Stack of T) ret b : Bool
	{- Devuelve True si la pila es vacía -}
	
	fun top(s : Stack of T) ret e : T
	{- Devuelve el elemento que se encuentra en el tope de s. -}
	{- PRE: not is_empty_stack(s) -}
	
	proc pop (in/out s : Stack of T)
	{- Elimina el elemento que se encuentra en el tope de s. -}
	{- PRE: not is_empty_stack(s) -}
end spec
```


## Arboles binarios
### Constructores
- Arbol vacio
- Nodo consistente de un elemento tipo T y dos arboles

Aclaraciones: 
1. Un nodo es un arbol no vacio que tiene raiz, subarbol derecho e izquierdo
2. A los subarboles tambien se los llama hijos(izq y der) (el nodo es su padre)
3. Una hoja es un nodo con los dos hijos vacios
4. Se usa la terminologia hijo, padre, nieto, abuelo, hermanos, ancestro, descendiente
5. Tambien se usa camino, altura, profundidad y nivel

### Niveles
Nivel 1 -> hay a lo sumo 1 nodo  
Nivel 2 -> hay a lo sumo 2 nodos  
Nivel 3 -> hay a lo sumo 4 nodos  
Nivel 4 -> hay a lo sumo 8 nodos  
Nivel i -> hay a lo sumo 2^(i−1) nodos  
Arbol de altura n hay a lo sumo:  
	2⁰ + 2¹ + . . . + 2^(n−1) = 2^n − 1 nodos  
En un árbol “balanceado” la altura es del orden del log₂ k donde k es el número de nodos  

### Camino
Lo podemos representar como la secuencia donde cada elemento nos indica si debo bajar a la izquierda o derecha

## TAD Arbol binario

```pascal
spec Tree of T where

constructors
	fun empty_tree() ret t : Tree of T
	{- crea una árbol vacío. -}
	
	fun node (tl : Tree of T, e : T, tr : Tree of T) ret t : Tree of T
	{- crea el nodo con elemento e y subárboes tl y tr. -}

operations
	fun is_empty_tree(t : Tree of T) ret b : Bool
	{- Devuelve True si el árbol es vacío -}

	fun root(t : Tree of T) ret e : T
	{- Devuelve el elemento que se encuentra en la raíz de t. -}
	{- PRE: not is_empty_tree(t) -}

	fun left(t : Tree of T) ret tl : Tree of T
	{- Devuelve el subárbol izquierdo de t. -}
	{- PRE: not is_empty_tree(t) -}

	fun right(t : Tree of T) ret tl : Tree of T
	{- Devuelve el subárbol derecho de t. -}
	{- PRE: not is_empty_tree(t) -}
	
	fun height(t : Tree of T) ret n : Nat
	{- Devuelve la distancia que hay entre la raíz de t y la hoja más profunda -}

	fun is_path(t : Tree of T, p : Path) ret b : Bool
	{- Devuelve True si p es un camino válido en t -}
	
	fun subtree_at(t : Tree of T, p : Path) ret t0 : Tree of T
	{- Devuelve el subárbol que se encuentra al recorrer el camino p en t . -}
	
	fun elem_at(t : Tree of T, p : Path) ret e : T
	{- Devuelve el elemento que se encuentra al recorrer el camino p en t . -}
	{- PRE: is_path(t,p) -}
```


## TAD Set
```pascal
spec Set of T where

constructors
	fun empty_set() ret s : Set of T
	{- Crea un conjunto vacío -}
	
	proc add(in/out s : Set of T, in e : T)
	{- Agrega el elemento e al conjunto s -}

destroy
	proc destroy_set(in/out s : Set of T)
	{- Libera memoria en caso que sea necesario. -}

operations
	fun cardinal(s : Set of T) ret n : nat
	{- Devuelve la cantidad de elementos que tiene s -}
	
	fun is_empty_set(s : Set of T) ret b : bool
	{- Devuelve True si s es vacío -}
	
	fun member(e : T, s : Set of T) ret b : bool
	{- Devuelve True si el elemento e pertenece al conjunto s -}
	
	proc elim(in/out s : Set of T, in e : T)
	{- Elimina el elemento e del conjunto s, en caso que esté -}
	
	proc union(in/out s : Set of T, in s0 : Set of T)
	{- Agrega a s todos los elementos de s0 -}
	
	proc inters(in/out s : Set of T, in s0 : Set of T)
	{- Elimina de s todos los elementos que NO pertenezcan a s0 -}
	
	proc diff(in/out s : Set of T, in s0 : Set of T)
	{- Elimina de s todos los elementos que pertenecen a s0 -}
	
	fun get(s : Set of T) ret e : T
	{- Obtiene algún elemento cualquiera del conjunto s -}
	{- PRE: not is_empty_set(s) -}
	
	fun copy_set(s1 : Set of T) ret s2 : Set of T
	{- Copia el conjunto s1 -}
```