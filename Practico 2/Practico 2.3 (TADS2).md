![ScreenShot](Imagenes%20practico%202.3/ej1.png)

```pascal
El TAD Pila es el siguiente:

spec Stack of T where

type Stack of T = List of T

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

```pascal
	fun empty_stack() ret s : Stack of T
	{- crea una pila vacía. -}

Como tengo tipo listas uso empty
```

<pre><code><b>fun</b> empty_stack() <b>ret</b> s: Stack <b>of</b> T
	s := empty()
<b>end fun</b></code></pre>



```pascal
	proc push (in e : T, in/out s : Stack of T)
	{- agrega el elemento e al tope de la pila s. -}

Uso addr
```

<pre><code><b>proc</b> push (<b>in</b> e: T, <b>in/out</b> s: Stack <b>of</b> T)
	addr(s, e)
<b>end proc</b></code></pre>




```pascal
	proc destroy_stack (in/out s: Stack of T)
	{- Elimino la memoria -}
```

<pre><code><b>proc</b> destroy_stack (<b>in/out</b> s: Stack <b>of</b> T)
	destroy(s)
<b>end proc</b></code></pre>


```pascal
	fun is_empty_stack(s : Stack of T) ret b : Bool
	{- Devuelve True si la pila es vacía -}

Directamente uso is_empty porque s es tipo lista
```

<pre><code><b>fun</b> is_empty_stack(s: Stack <b>of</b> T) <b>ret</b> b: Bool
	s := is_empty(s)
<b>end fun</b></code></pre>


```pascal
	fun top(s : Stack of T) ret e : T
	{- Devuelve el elemento que se encuentra en el tope de s. -}
	{- PRE: not is_empty_stack(s) -}

Con tope nos referimos al de mas arriba, o sea al ultimo. Puedo usar index para devolver el ultimo elemento
```

<pre><code><b>fun</b> top(s: Stack <b>of</b> T) <b>ret</b> e: T
	e := index(s, length(s))
<b>end fun</b></code></pre>


```pascal
	proc pop (in/out s : Stack of T)
	{- Elimina el elemento que se encuentra en el tope de s. -}
	{- PRE: not is_empty_stack(s) -}

Puedo hacer algo similar a lo anterior pero en luagr de usar index uso take (notar que este deja en la lista los primeros n elementos, por lo tanto seria take de n-1)
```

<pre><code><b>proc</b> pop(<b>in/out</b> s: Stack <b>of</b> T)
	take(length(s)-1)
<b>end proc</b></code></pre>



![ScreenShot](Imagenes%20practico%202.3/ej2.png)

```pascal
Ahora me pide que lo implemente pero en vez de con listas con punteros
```

```pascal
implement Stack of T where

type Stack of T = tuple
					elem: T
					next: pointer to (Node of T)
				end tuple

type Stack of T = pointer to (Node of T)

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

```pascal
	fun empty_stack() ret s : Stack of T
	{- crea una pila vacía. -}

Indico s como NULL
```

<pre><code><b>fun</b> empty_stack() <b>ret</b> s: Stack <b>of</b> T
	s := NULL
<b>end fun</b></code></pre>



```pascal
	proc push (in e : T, in/out s : Stack of T)
	{- agrega el elemento e al tope de la pila s. -}

Uso un puntero y guardo como primer elemento el que quiero agregar y en next guardo el resto que ya tenia
```

<pre><code><b>proc</b> push (<b>in</b> e: T, <b>in/out</b> s: Stack <b>of</b> T)
	<b>var</b> p: <b>pointer to</b> (Node <b>of</b> T)
	alloc(p)
	p->elem := e
	p->next := s
	s := p
<b>end proc</b></code></pre>




```pascal
	proc destroy_stack (in/out s: Stack of T)
	{- Elimino la memoria -}
```

<pre><code><b>proc</b> destroy_stack (<b>in/out</b> s: Stack <b>of</b> T)
	<b>while</b> s->next != NULL <b>do</b>
		destroy_stack(s->next)
		free(s)
	<b>od</b>
<b>end proc</b></code></pre>


```pascal
	fun is_empty_stack(s : Stack of T) ret b : Bool
	{- Devuelve True si la pila es vacía -}

Me fijo si s es NULL
```

<pre><code><b>fun</b> is_empty_stack(s: Stack <b>of</b> T) <b>ret</b> b: Bool
	b := (s == NULL)
<b>end fun</b></code></pre>


```pascal
	fun top(s : Stack of T) ret e : T
	{- Devuelve el elemento que se encuentra en el tope de s. -}
	{- PRE: not is_empty_stack(s) -}

Devuelvo el elem
```

<pre><code><b>fun</b> top(s: Stack <b>of</b> T) <b>ret</b> e: T
	e := s->elem
<b>end fun</b></code></pre>


```pascal
	proc pop (in/out s : Stack of T)
	{- Elimina el elemento que se encuentra en el tope de s. -}
	{- PRE: not is_empty_stack(s) -}

Directamete uso un puntero para apuntar a s, luego hago que s apunte al que le sigue y libero p
```

<pre><code><b>proc</b> pop(<b>in/out</b> s: Stack <b>of</b> T)
	<b>var</b> p: <b>pointer to</b> (Node <b>of</b> T)
	p := s
	s := s->next
	free(p)
<b>end proc</b></code></pre>


![ScreenShot](Imagenes%20practico%202.3/ej3.png)

```pascal
a) Ahora nos pide implementar el TAD Cola:

spec Queue of T where
constructors
	fun empty_queue() ret q : Queue of T
	{- crea una cola vacía. -}
	
	proc enqueue (in/out q : Queue of T, in e : T)
	{- agrega el elemento e al final de la cola q. -}

operations
	fun is_empty_queue(q : Queue of T) ret b : Bool
	{- Devuelve True si la cola es vacía -}
	
	fun first(q : Queue of T) ret e : T
	{- Devuelve el elemento que se encuentra al comienzo de q. -}
	{- PRE: not is_empty_queue(q) -}
	
	proc dequeue (in/out q : Queue of T)
	{- Elimina el elemento que se encuentra al comienzo de q. -}
	{- PRE: not is_empty_queue(q) -}
```

<pre><code><b>implement</b> Queue <b>of</b> T <b>where</b>

<b>type</b> Queue <b>of</b> T = <b>tuple</b>
									elems: array[0..N-1] <b>of</b> T
									size: nat
								<b>end tuple</b></code></pre>

```pascal
	fun empty_queue() ret q : Queue of T
	{- crea una cola vacía. -}

Si el size es 0 true
```

<pre><code><b>fun</b> empty_queue() <b>ret</b> q: Queue <b>of</b> T
	q.size = 0
<b>end fun</b></code></pre>



```pascal
	proc enqueue (in/out q : Queue of T, in e : T)
	{- agrega el elemento e al final de la cola q. -}

Aumento el size y agrego el elemento en esa posicion
```

<pre><code><b>proc</b> enqueue (<b>in</b> e: T, <b>in/out</b> q: Queue <b>of</b> T)
	q.size := q.size + 1
	q.elems[q.size] := e
<b>end proc</b></code></pre>




```pascal
	proc destroy_queue (in/out q: Queue of T)
	{- Elimino la memoria -}

Digo que el size sea 0
```

<pre><code><b>proc</b> destroy_queue (<b>in/out</b> q: Queue <b>of</b> T)
	q.size := 0
<b>end proc</b></code></pre>


```pascal
	fun is_empty_queue(q : Queue of T) ret b : Bool
	{- Devuelve True si la cola es vacía -}

Si el size es 0 entonces true
```

<pre><code><b>fun</b> is_empty_queue(q: Queue <b>of</b> T) <b>ret</b> b: Bool
	q := (q.size == 0)
<b>end fun</b></code></pre>


```pascal
	fun first(q : Queue of T) ret e : T
	{- Devuelve el elemento que se encuentra al comienzo de q. -}
	{- PRE: not is_empty_queue(q) -}


```

<pre><code><b>fun</b> first(q: Queue <b>of</b> T) <b>ret</b> e: T
	e := q.elems[0]
<b>end fun</b></code></pre>


```pascal
	proc dequeue (in/out q : Queue of T)
	{- Elimina el elemento que se encuentra al comienzo de q. -}
	{- PRE: not is_empty_queue(q) -}

Puedo hacer algo similar a lo anterior pero en luagr de usar index uso take (notar que este deja en la lista los primeros n elementos, por lo tanto seria take de n-1)
```

<pre><code><b>proc</b> dequeue(<b>in/out</b> q: Queue <b>of</b> T)
	<b>for</b> i := 1 <b>to</b> N-1 <b>do</b>
		q.elems[i-1] := q.elems[i]
	<b>od</b>
	q.size := q.size - 1
<b>end proc</b></code></pre>









```pascal
b) Entonces uso aritemtica modular
Agrego tambien un campo de inicio a la tupla:
```

<pre><code><b>implement</b> Queue <b>of</b> T <b>where</b>

<b>type</b> Queue <b>of</b> T = <b>tuple</b>
									elems: array[0..N-1] <b>of</b> T
									size: nat
									start: nat
								<b>end tuple</b></code></pre>

```pascal
	fun empty_queue() ret q : Queue of T
	{- crea una cola vacía. -}

Indico que el size y start sean 0
```

<pre><code><b>fun</b> empty_queue() <b>ret</b> q: Queue <b>of</b> T
	q.size := 0
	q.start := 0
<b>end fun</b></code></pre>



```pascal
	proc enqueue (in/out q : Queue of T, in e : T)
	{- agrega el elemento e al final de la cola q. -}

```

<pre><code><b>proc</b> enqueue (<b>in</b> e: T, <b>in/out</b> q: Queue <b>of</b> T)
	q.size := q.size + 1
	q.elems[(q.init + q.size) % N] := e
<b>end proc</b></code></pre>




```pascal
	proc destroy_queue (in/out q: Queue of T)
	{- Elimino la memoria -}

Digo que el size sea 0
```

<pre><code><b>proc</b> destroy_queue (<b>in/out</b> q: Queue <b>of</b> T)
	q.size := 0
	q.start := 0
<b>end proc</b></code></pre>


```pascal
	fun is_empty_queue(q : Queue of T) ret b : Bool
	{- Devuelve True si la cola es vacía -}

Si el size es igual al comienzo entonces es vacia (o sea si el tamaño es igual a la primera posicion o sea 0)
```

<pre><code><b>fun</b> is_empty_queue(q: Queue <b>of</b> T) <b>ret</b> b: Bool
	q := (q.start == q.size)
<b>end fun</b></code></pre>


```pascal
	fun first(q : Queue of T) ret e : T
	{- Devuelve el elemento que se encuentra al comienzo de q. -}
	{- PRE: not is_empty_queue(q) -}

Devuelvo el elemento q.start
```

<pre><code><b>fun</b> first(q: Queue <b>of</b> T) <b>ret</b> e: T
	e := q.elems[q.start]
<b>end fun</b></code></pre>


```pascal
	proc dequeue (in/out q : Queue of T)
	{- Elimina el elemento que se encuentra al comienzo de q. -}
	{- PRE: not is_empty_queue(q) -}


```

<pre><code><b>proc</b> dequeue(<b>in/out</b> q: Queue <b>of</b> T)
	q.start := (q.start + 1) % N
	q.size := q.size - 1
<b>end proc</b></code></pre>

==ARBOLES NO LO VEMOS==

