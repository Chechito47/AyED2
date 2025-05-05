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