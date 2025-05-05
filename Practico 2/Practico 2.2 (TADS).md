![ScreenShot](Imagenes%20practico%202.2/ej1.png)

```pascal
Nos pide realizar la implementacion de los TADS que usaremos en este practico. Tenemos esto declarado:

implement List of T where

type Node of T = tuple
                    elem: T
                    next: pointer to (Node of T)
                 end tuple

type List of T = pointer to (Node of T)


Los TADS son los siguientes:

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

```pascal
	fun empty() ret l: list of T
	{- crea una lista vacia -}

Entonces como solo queremos una lisa vacia indicamos que l sea null.
No es necesario pedir memoria porque no es un puntero
```
<pre><code><b>fun</b> empty() <b>ret</b> l: List <b>of</b> T
	l := null
<b>end fun</b>
</code></pre>



```pascal
	proc addl(in e: T, in/out l: List of T)
	{- agrega el elemento al comienzo de la lista -}

Primero debemos declarar un puntero, luego como es puntero debemos pedir memoria. Luego indicamos que la posicion actual del puntero sea el elemento que queremos agregar y el siguiente la lista l(o sea el primero de la lista l). Por ultimo declaramos que l=p haciendo que todos eso se guarde en l.
Notar que no podemos liberar p, si lo hiceramos tendriamos l apuntando a null
```
<pre><code>
<b>proc</b> addl(<b>in</b> e: T, <b>in/out</b> l : List <b>of</b> T)
	<b>var</b> p: pointer <b>to</b> (Node <b>of</b> T)
	alloc(p)
	p -> elem := e
	p -> next := l
	l := p
<b>end proc</b>
</code></pre>



```pascal
	proc destroy(in/out l: List of T)
	{- libera memoria en caso que sea necesario -}

Como para liberar memoria hay que usar punteros, declarmos un puntero p. Luego revisamos que no sea vacio (o sea que no sea null). Si tiene algo entonces apuntamos hacia el primer elemento de ese algo con p e indicamos que ese algo ahora apunte a su segundo elemento. Y luego liberamos la memoria que ocupaba el primer elemento.
Esto se repite hasta liberar la memoria de todos los elementos de la lista
```
<pre><code><b>proc</b> destroy_list(<b>in/out</b> List <b>of</b> T)
	<b>var</b> p: pointer to (Node <b>of</b> T)
	<b>while</b> !(is_empty(l)) <b>do</b>
		p := l
		l := l->next
		free(p)
	<b>od</b>
<b>end proc</b></code></pre>



```pascal
	fun is_empty(l: List of T) ret b: bool
	{- devuelve True si l es vacia -}

Como b es de tipo bool, retornamos true si l es null, caso contrario false
```
<pre><code><b>fun</b> is_empty(l: List <b>of</b> T) <b>ret</b> b:<b>bool</b>
	b := (l == null)
<b>end fun</b></code></pre>



```pascal
	fun head(l: List of T) ret e: T
	{- devuelve el primer elemento de la lista l -}
	{- PRE: not is_empty() -}

Guardamos en e el elem de la lista (o sea primer elemento)
```
<pre><code><b>fun</b> head(l: List <b>of</b> T) <b>ret</b> e: T
	e := l->elem
<b>end fun</b></code></pre>



```pascal
	proc tail(in/out l: List of T)
	{- elimina el primer elemento de la lista l -}
	{- PRE: not is_empty() -}

Como nos pide eliminar un elemento usamos punteros. Declaramos el puntero p y apuntamos a l con ese puntero. Luego indicamos que l sea el siguiente (nos saltamos el elem) y por ultimo liberamos la memoria del p(o sea el primer elemento).

Notar que no podemos simplemente hacer l = l->next porque primero perderiamos la referencia al nodo original y segundo no sabriamos a que hacerle free

Seria algo asi lo que hacemos:
1) p := l
   p ──> [A]──> [B]──> [C]
   l ──> [A]──> [B]──> [C]

2) l := l->next
   p ──> [A]──> [B]──> [C]
   l ──> [B]──> [C]

3) free(p)
   p ──> (memoria liberada)
   l ──> [B]──> [C]──> null
```
<pre><code><b>proc</b> tail(l: List <b>of</b> T)
	<b>var</b> p: Pointer <b>to</b> (Node <b>of</b> T)
	p := l
	l := l->next
	free(p)
<b>end proc</b></code></pre>



```pascal
	proc addr(in/out l: List of T, in e: T)
	{- agrega el elemento e al final de la lista -}

Este es un poco mas complejo que los demas. Nuevamente hay que trabajar con punteros. En este caso usamos 2. Uno va a ser auxiliar y otro para guardar la lista, por eso solo pedimos memoria para uno de ellos.
Indicamos que el primero elemento del puntero auxiliar q sea el elemento que queremos agregar al final y que el siguiente sea nulo. O sea tenemos un puntero que apunta a una lista con un solo elemento
Nos fijamos que la lista l a la cual le queremos agregar el elemento no sea vacia. En caso que si sea vacia solamente agregamos el elemento haciendo l = q (o sea que la lista deje de ser vacia y sea la lista con un solo elemento).
En caso que no sea vacia hacemos que el puntero p que no habiamos apunte a l, luego mientras el elemento que sigue no sea el ultimo vamos avanzando uno por uno hasta llegar al ultimo. Cuando lleguemos simplemente indicamos que el siguiente sea el elemento que queremos agregar (Como p apunta a l, el agregar el elemento usando el puntero p afecta a la lista l)
```
<pre><code><b>proc</b> addr(<b>in/out</b> l: List <b>of</b> T, <b>in</b> e: T)
	<b>var</b> p, q: Pointer <b>to</b> (Node <b>of</b> T)
	alloc(q)
	q -> elem := e
	q -> next := null
	<b>if</b> (<b>not</b> is_empty(l)) <b>then</b>
		p := l
		<b>do</b> (p -> next != null)
			p := p -> next
		<b>od</b>
		p -> next := q
	<b>else</b>
		l := q
	<b>fi</b>
<b>end proc</b></code></pre>



```pascal
	fun length(l: List of T) ret n: nat
	{- devuelve la cantidad de elementos de la lista l -}

Declaramos un puntero, hacemos que el puntero apunte a la lsta l y la cantidad de elementos sea 0. Si la lista es vacio retorna 0.
Si no es vacia agregamos 1 y avanzamos un elemento y volvemos a chequear si ese elemento es el ultimo
```
<pre><code><b>fun</b> length(l: List <b>of</b> T) <b>ret</b> n: nat
	<b>var</b> p: Pointer <b>to</b> (Node <b>of</b> T)
	n := 0
	p := l
	<b>do</b> (p != null)
		n := n + 1
		p := p -> next
	<b>od</b>
<b>end fun</b></code></pre>



```pascal
	proc concat(in/out l: List of T, in IO: List of T)
	{- agrega al final de l todos los elementos de IO en el mismo orden}

Declaramos un puntero p y hacemos que apunte a la lista que queremos agregar. Ahora mientras esa lista tenga algun elemento usamos addr para ir agregandolos uno por uno hasta que no queden mas
```
<pre><code><b>proc</b> concat(<b>in/out</b> l: List <b>of</b> T, <b>in</b> IO: List <b>of</b> T)
	<b>var</b> p: Pointer <b>to</b> (Node <b>of</b> T)
	p := IO
	<b>while</b> IO != null <b>do</b>
		addr(l, p -> elem)
		p := p -> elem
	<b>od</b>
<b>end proc</b></code></pre>



```pascal
	fun index(l: List of T, n: nat) ret e: T
	{- devuelve el n-esimo elemento de la lista -}
	{- PRE: length(l) < n -}

Declaramos una variable i para iterar y un puntero p que apunte a l. Luego mientras el i todabia no llegue a la posicion n que buscamos avanzamos al siguiente elemento de la lista y sumamos 1 al i
Cuando ya lleguemos a la posicion n-1 devolvemos directamente en e el elem de p
(Es hasta n-1 porque el ultimo caso es n-1 y como hacemos p:=p->next ya estariamos apuntando al elemento n)
```
<pre><code><b>fun</b> index(l: List <b>of</b> T, n: nat) <b>ret</b> e: T
	<b>var</b> i: nat
	<b>var</b> p: Pointer <b>to</b> (Node <b>of</b> T)
	i := 0
	p := l
	<b>while</b> i &lt; n <b>do</b>
		p := p -> next
		i := i + 1
	<b>od</b>
	e := p -> elem
<b>end fun</b></code></pre>



```pascal
	proc take(in/out l: List of T, in n: nat)
	{- deja en l solo los primeros n elementos eliminando el resto -}

Declaramos una variable i para iterar y un puntero p que apunta a l. Si el n que ingresamos es 0 entonces borramos toda la lista porque no nos quedamos con ningun elemento.
Si es mayor a 0 entonces iteramos hasta llegar al n o hasta llegar al ultimo elemento de la lista (lo que pase primero). Luego eliminamos el elemento que sigue y declaramos que el elemento que sigue sea null para asi indicar que la lista termina aca en la posicion n
Notar que el while es hasta n, si fuera hasta n-1 el caso con n=2 fallaria
```
<pre><code><b>proc</b> take(<b>in/out</b> l: List <b>of</b> T, <b>in</b> n: nat)
	<b>var</b> p: Pointer <b>to</b> (Node <b>of</b> T)
	<b>var</b> i: nat
	p := l
	i := 1
	<b>if</b> n &gt; 1 <b>then</b>
		<b>while</b> i &lt; n && p->next != null <b>do</b>
			p := p -> next
			i := i + 1
		<b>od</b>
		destroy(p -> next)
		p -> next := null
	<b>else</b>
		destroy(p->next)
		p->next := null
	<b>fi</b>
<b>end proc</b></code></pre>



```pascal
	proc drop(in/out l: List of T, in n:nat)
	{- elimina los primeros n elementos de la lista l -}

Declaramos una variable i para iterar y un puntero p. Mientras el i sea menor que el numero indicado de elemento a borrar y la lista no sea vacia hacemos que el puntero p apunte a l(primer elemento), luego que l apunte al siguiente elemento y hacemos free(p) para eliminar el primer elemento y aumentamos la variable de iteracion en 1. Asi hasta que llegamos al n deseado o hasta que la lista sea vacia (lo que suceda primero)
```
<pre><code><b>proc</b> drop(<b>in/out</b> l: List <b>of</b> T, <b>int</b> n: nat)
	<b>var</b> i: nat
	<b>var</b> p: Pointer <b>of</b> (Node <b>of</b> T)
	i := 0
	<b>while</b> i &lt; n && <b>not</b> is_empty(l) <b>do</b>
		p := l
		l := l -> next
		free(p)
		i := i + 1
	<b>od</b>
<b>end proc</b></code></pre>



```pascal
	fun copy_list(l1: List of T) ret l2: List of T
	{- copia todos los elementos de la lista l1 en la nueva lista l2 -}

Declaro un puntero p que apunte a la l1 y que la l2 sea vacia. Luego mientras p tenga elementos los agrego 1 por 1 usando addr y avanzo hacia el siguiente elemento con p := p->next
```
<pre><code><b>fun</b> copy_list(l1: List <b>of</b> T) <b>ret</b> l2: List <b>of</b> T
	<b>var</b> p: Pointer <b>to</b> (Node <b>of</b> T)
	l2 := empty()
	p := l1
	<b>while</b> p != null <b>do</b>
		addr(l2, p->elem)
		p := p->next
	<b>od</b>
<b>end fun</b></code></pre>


![ScreenShot](Imagenes%20practico%202.2/ej2.png)

```pascal
Tenemos que haces lo mismo que antes pero en lugar de usar listas usamos arrays de tamaño N

implement List of T where

type Node of T = tuple
                    size: nat
                    elems: array[1..N] of T
                 end tuple

type List of T = pointer to (Node of T)

O sea los punteros ahora pasan a ser tuplas
```

```pascal
	fun empty() ret l: list of T
	{- crea una lista vacia -}

Declaramos que el tamaño sea 0
```
<pre><code><b>fun</b> empty() <b>ret</b> l: List <b>of</b> T
	l.size := 0
<b>end fun</b>
</code></pre>



```pascal
	proc addl(in e: T, in/out l: List of T)
	{- agrega el elemento al comienzo de la lista -}

Itero desde n hasta 1, haciendo que el elemento que sigue sea el actual (el primero siempre se guarda en una posicion que no hay nada que sera n+1)
Luego indico que el primer elemento sera el que quiero agregar y aumento el tamaño del arreglo en 1
```
<pre><code>
<b>proc</b> addl(<b>in</b> e: T, <b>in/out</b> l : List <b>of</b> T)
	<b>for</b> i := l.size <b>downto</b> 1 <b>do</b>
		l.elems[i+1] = l.elems[i]
	<b>od</b>
	l.elems[1] := e
	l.size := l.size + 1
<b>end proc</b>
</code></pre>



```pascal
	proc destroy(in/out l: List of T)
	{- libera memoria en caso que sea necesario -}

Como no hay punteros no hay memoria, entonces simplemente indico que el tamaño sea 0
```
<pre><code><b>proc</b> destroy_list(<b>in/out</b> List <b>of</b> T)
	l.size := 0
<b>end proc</b></code></pre>



```pascal
	fun is_empty(l: List of T) ret b: bool
	{- devuelve True si l es vacia -}

Como b es de tipo bool, retornamos true si l.size es 0, caso contrario false
```
<pre><code><b>fun</b> is_empty(l: List <b>of</b> T) <b>ret</b> b:<b>bool</b>
	b := (l.size == 0l)
<b>end fun</b></code></pre>



```pascal
	fun head(l: List of T) ret e: T
	{- devuelve el primer elemento de la lista l -}
	{- PRE: not is_empty() -}

Guardamos en e el elems[1] del arreglo
```
<pre><code><b>fun</b> head(l: List <b>of</b> T) <b>ret</b> e: T
	e := l->elems[1]
<b>end fun</b></code></pre>



```pascal
	proc tail(in/out l: List of T)
	{- elimina el primer elemento de la lista l -}
	{- PRE: not is_empty() -}
Simplemente hacemos que el primero pase a ser el segundo sobreescribiendolo y le restamos 1 al size
```
<pre><code><b>proc</b> tail(l: List <b>of</b> T)
	<b>var</b> i: nat
	<b>for</b> i := 1 <b>to</b> l.size-1 <b>do</b>
		l.elems[i] := l.elems[i+1]
	<b>od</b>
	l.size := l.size-1
<b>end proc</b></code></pre>



```pascal
	proc addr(in/out l: List of T, in e: T)
	{- agrega el elemento e al final de la lista -}

Como quiero agregar un elemento al final directamente lo agrego en la posicion l.size+1 y aumento el size
```
<pre><code><b>proc</b> addr(<b>in/out</b> l: List <b>of</b> T, <b>in</b> e: T)
	l.elems[l.size + 1] := e
	l.size := l.size + 1
<b>end proc</b></code></pre>



```pascal
	fun length(l: List of T) ret n: nat
	{- devuelve la cantidad de elementos de la lista l -}

Uso size
```
<pre><code><b>fun</b> length(l: List <b>of</b> T) <b>ret</b> n: nat
	n := l.size
<b>end fun</b></code></pre>



```pascal
	proc concat(in/out l: List of T, in IO: List of T)
	{- agrega al final de l todos los elementos de IO en el mismo orden}

Iteramos desde 1 hasta el tamaño de IO agregando los elementos de IO en la posicion del l+i. Luego cambiamos el size del l a l.size + IO.size
```
<pre><code><b>proc</b> concat(<b>in/out</b> l: List <b>of</b> T, <b>in</b> IO: List <b>of</b> T)
	<b>for</b> i := 1 <b>to</b> IO.size <b>do</b>
		l.elems[l.size + 1] := IO.elems[i]
	<b>od</b>
	l.size := l.size + IO.size
<b>end proc</b></code></pre>



```pascal
	fun index(l: List of T, n: nat) ret e: T
	{- devuelve el n-esimo elemento de la lista -}
	{- PRE: length(l) < n -}

Devuelvo el elemento n que es el que me pide
```
<pre><code><b>fun</b> index(l: List <b>of</b> T, n: nat) <b>ret</b> e: T
	e := l.elems[n]
<b>end fun</b></code></pre>



```pascal
	proc take(in/out l: List of T, in n: nat)
	{- deja en l solo los primeros n elementos eliminando el resto -}

Declaro el size como el n de entrada haciendo que los que le siguen no existan mas
```
<pre><code><b>proc</b> take(<b>in/out</b> l: List <b>of</b> T, <b>in</b> n: nat)
	l.size := n
<b>end proc</b></code></pre>



```pascal
	proc drop(in/out l: List of T, in n:nat)
	{- elimina los primeros n elementos de la lista l -}

Itero desde 1 hasta el size-n (asumo que n<=l.size) y para cada elemento en la posicion i lo reemplazo por el elemento que va en la posicion n+i (o sea al primer elemento lo reemplazo por el n+1 elemento)
```
<pre><code><b>proc</b> drop(<b>in/out</b> l: List <b>of</b> T, <b>int</b> n: nat)
	<b>for</b> i := 1 <b>to</b> l.size-n <b>do</b>
		l.elems[i] := l.elems[i+n]
	<b>od</b>
	l.size := l.size - n
<b>end proc</b></code></pre>



```pascal
	fun copy_list(l1: List of T) ret l2: List of T
	{- copia todos los elementos de la lista l1 en la nueva lista l2 -}

Hago que el size de l2 sea igual al de l1 y itero desde 1 hasta el size de l1 guardando en el l2 los elementos de l1
```
<pre><code><b>fun</b> copy_list(l1: List <b>of</b> T) <b>ret</b> l2: List <b>of</b> T
	l2.size := l1.size
	<b>for</b> i := 1 <b>to</b> l1.size <b>do</b>
		l2.elems[i] := l1.elems[i]
	<b>od</b>
<b>end fun</b></code></pre>


![ScreenShot](Imagenes%20practico%202.2/ej3.png)

```pascal
Nos dice que las operaciones copy, take y drop nos seran utiles. Entonces nos podemos hacer la idea que tenemos que copiar la lista original, luego tomar los n-1 elementos de la lista original y dropear los n-1 de la copia para ahi agregar el elemento. Luego podriamos hacer un cocat para pegar el resto de la lista copia a la original y listo
```

<pre><code><b>proc</b> add_at(<b>in/out</b> l: List <b>of</b> T, <b>in</b> n: nat, <b>in</b> e: T)
	<b>var</b> l_aux: List <b>of</b> T
	l_aux := copy_list(l)
	take(l, n-1)
	drop(l_aux, n-1)
	addl(l_aux, e)
	concat(l, l_aux)
	destroy_list(l_aux)
<b>end proc</b></code></pre>

![ScreenShot](Imagenes%20practico%202.2/ej4.png)

```pascal
Primero me pide dar la especificacion, osea decir si es fun o proc, que argumentos toma y que decir resumidamente que hace. 
Me pide que tenga los siguientes constructores (constructors):
1- Iniciar el partido(iniciar el tanteador)
2- Registrar tanto del equipo A(sumar tanto)
3- Registrar tanto del equipo B

Tambien me operaciones (operations) para lo siguiente:
1- Comprobar si el marcador es 0
2- Comprobar si el equipo A marco algun tanto
3- Comprobar si el equipo B marco algun tanto
4- Retornar true si el equipo A va ganando
5- Retornar true si el equipo B va ganando
6- Retornar true si los equipos estan empatados
7- Anotarle n cantidad de tantos a un equipo
8- Restarle n cantidad de tanto a un equipo (hasta min=0)

Aunque el destroy no esta especificado seria el siguiente:
1- Liberar tanteador cuando el partido termine(liberar memoria)

Entonces:


a)
spec Tablero where
constructors
	fun inicializo_tanteador() ret t: Tablero
	{- Inicializo el tablero -}

	proc sumar_tanto_A(in/out t: Tablero)
	{- incremento el contador de puntos para el equipo A en el tablero}
	
	proc sumar_tanto_B(in/out t: Tablero)
	{- incremento el contador de puntos para el equipo B en el tablero}

destroy
	proc destroy_tablero(in/out t: Tablero)
	{- libero la memoria del tablero -}

operations
	fun tanteador_0(t: Tablero) ret b: bool
	{- determina si ningun equipo marco un tanto -}

	fun tanteador_A(t: Tablero) ret b: bool
	{- determina si el equipo A ha marcado aunque sea un tanto -}

	fun tanteador_B(t: Tablero) ret b: bool
	{- determina si el equipo B ha marcado aunque sea un tanto -}

	fun va_ganando_A(t: Tablero) ret b: bool
	{- determina si el equipo A va ganando -}

	fun va_ganando_B(t: Tablero) ret b: bool
	{- determina si el equipo B va ganando -}

	fun va_empate(t: Tablero) ret b: bool
	{- determina si los equipos estan empatados -}

	proc sumar_n(in/out t: Tablero, in n: nat, in c: char)
	{- le sumo n tantos al equipo c -}

	proc restar_n(in/out t: Tablero, in n: nat, in c: char)
	{- le resto n tantos al equipo c -}
```
 
```pascal
b) Me pide realizar la implentacion del TAD tablero usando una tupla con dos contadores, uno para los tantos de cada equipo. 
Antes, recordemos que para contadores tenemos los siguientes constructores y operaciones:

spec Counter where

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

<pre><code><b>implement</b> Tablero <b>where</b>

	<b>type</b> tanteador = <b>tuple</b>
							tantos_A: Counter
							tantos_B: Counter
						   <b>end tuple</b>

<b>type</b> Tablero = tanteador</code></pre>

```pascal
	fun inicializo_tanteador() ret t: Tablero
	{- Inicializo el tablero -}

Como ya tengo un constructor que inicializa lo uso
```
<pre><code><b>fun</b> inicializo_tanteador() <b>ret</b> t: Tablero
	t.tantos_A := init()
	t.tantos_B := init()
<b>end fun</b></code></pre>

```pascal
	proc sumar_tanto_A(in/out t: Tablero)
	{- incremento el contador de puntos para el equipo A en el tablero}

Uso el incr
```
<pre><code><b>proc</b> sumar_tanto_A(<b>in/out</b> t: Tablero)
	incr(t.tantos_A)
<b>end proc</b></code></pre>

```pascal
	proc sumar_tanto_B(in/out t: Tablero)
	{- incremento el contador de puntos para el equipo B en el tablero}

Uso el incr
```
<pre><code><b>proc</b> sumar_tanto_B(<b>in/out</b> t: Tablero)
	incr(t.tantos_B)
<b>end proc</b></code></pre>

```pascal
	proc destroy_tablero(in/out t: Tablero)
	{- libero la memoria del tablero -}

Uso el destroy que ya tengo (notar que destruye un Counter el que tengo)
```

<pre><code><b>proc</b> destroy_tablero(<b>in/out</b> t: Tablero)
	destroy(t.tantos_A)
	destroy(t.tantos_B)
<b>end proc</b></code></pre>

```pascal
	fun tanteador_0(t: Tablero) ret b: bool
	{- determina si ningun equipo marco un tanto -}
Uso is_init que devuelve true si el contador es el inicial(o sea si es 0)
```

<pre><code><b>fun</b> tanteador_0(t: Tablero) <b>ret</b> b: bool
	b := is:init(t.tantos_A) && is_init(t.tantos_B)
<b>end fun</b></pre></code>


```pascal
	fun tanteador_A(t: Tablero) ret b: bool
	{- determina si el equipo A ha marcado aunque sea un tanto -}
Uso nuevamente is_init pero con not
```

<pre><code><b>fun</b> tanteador_A(t: Tablero) <b>ret</b> b: bool
	b := <b>not</b> is_init(t.tantos_A)
<b>end fun</b></code></pre>


```pascal
	fun tanteador_B(t: Tablero) ret b: bool
	{- determina si el equipo B ha marcado aunque sea un tanto -}
Uso nuevamente is_init pero con not
```

<pre><code><b>fun</b> tanteador_B(t: Tablero) <b>ret</b> b: bool
	b := <b>not</b> is_init(t.tantos_B)
<b>end fun</b></code></pre>

```pascal
	fun va_ganando_A(t: Tablero) ret b: bool
	{- determina si el equipo A va ganando -}

Directamente uso el t.tantos_A
```

<pre><code><b>fun</b> va_gananado_A(t: Tablero) <b>ret</b> b: bool
	b := t.tantos_A &gt; t.tantos_B
<b>end fun</b></code></pre>


```pascal
	fun va_ganando_B(t: Tablero) ret b: bool
	{- determina si el equipo B va ganando -}

Directamente uso el t.tantos_B
```

<pre><code><b>fun</b> va_gananado_B(t: Tablero) <b>ret</b> b: bool
	b := t.tantos_B &gt; t.tantos_A
<b>end fun</b></code></pre>


```pascal
	fun va_empate(t: Tablero) ret b: bool
	{- determina si los equipos estan empatados -}

Directamenr uso t.tantos
```

<pre><code><b>fun</b> va_empate(t: Tablero) <b>ret</b> b: bool
	b := t.tantos_A == t.tantos_B
<b>end fun</b></code></pre>


```pascal
	proc sumar_n(in/out t: Tablero, in n: nat, in c: char)
	{- le sumo n tantos al equipo c -}

Leo la letra y en caso que sea de algun equipo le hago incr n veces
```

<pre><code><b>proc</b> sumar_n(<b>in/out</b> t: Tablero, <b>in</b> n: nat, <b>in</b> c: char)
	<b>if</b> c == 'A' <b>or</b> 'a' <b>then</b>
		<b>for</b> i := 1 <b>to</b> n <b>do</b>
			incr(t.tantos_A)
		<b>od</b>
	<b>else if</b> c == 'B' <b>or</b> 'b' <b>then</b>
		<b>for</b> i := 1 <b>to</b> n <b>do</b>
			incr(t.tantos_B)
		<b>od</b>
	<b>fi</b>
<b>end proc</b></code></pre>


```pascal
	proc restar_n(in/out t: Tablero, in n: nat, in c: char)
	{- le resto n tantos al equipo c -}

IDEM que el anterior pero uso decr si not is_init(t.tantos) (o sea si no es 0)
```

<pre><code><b>proc</b> restar_n(<b>in/out</b> t: Tablero <b>in</b> n: nat, <b>in</b> c: char)
	<b>if</b> c == 'A' <b>or</b> 'a' <b>then</b>
		<b>for</b> i := 1 <b>to</b> n <b>do</b>
			<b>if</b> <b>not</b> is_init(t.tantos_A) <b>then</b>
				decr(t.tantos_A)
			<b>fi</b>
		<b>od</b>
	<b>else if</b> c == 'B' <b>or</b> 'b' <b>then</b>
		<b>for</b> i := 1 <b>to</b> n <b>do</b>
			<b>if</b> <b>not</b> is_init(t.tantos_B) <b>then</b>
				decr(t.tantos_B)
			<b>fi</b>
		<b>od</b>
	<b>fi</b>
<b>end proc</b></code></pre>













```pascal
c) Ahora nos pide hacer lo mismo que en el item B pero en lugar de usar contadores usar naturales
```

<pre><code><b>implement</b> Tablero <b>where</b>

<b>type</b> tantedor = <b>tuple</b>
						t.tantos_A: nat
						t.tantos_B: nat
					<b>end tuple</b>

<b>type</b> Tablero = tuple</code></pre>

```pascal
	fun inicializo_tanteador() ret t: Tablero
	{- Inicializo el tablero -}

Los inicializo en 0
```
<pre><code><b>fun</b> inicializo_tanteador() <b>ret</b> t: Tablero
	t.tantos_A := 0
	t.tantos_B := 0
<b>end fun</b></code></pre>

```pascal
	proc sumar_tanto_A(in/out t: Tablero)
	{- incremento el contador de puntos para el equipo A en el tablero}

Sumo 1
```
<pre><code><b>proc</b> sumar_tanto_A(<b>in/out</b> t: Tablero)
	t.tantos_A := t.tantos_A + 1
<b>end proc</b></code></pre>

```pascal
	proc sumar_tanto_B(in/out t: Tablero)
	{- incremento el contador de puntos para el equipo B en el tablero}

Sumo 1
```
<pre><code><b>proc</b> sumar_tanto_B(<b>in/out</b> t: Tablero)
	t.tantos_B := t.tantos_B + 1
<b>end proc</b></code></pre>

```pascal
	proc destroy_tablero(in/out t: Tablero)
	{- libero la memoria del tablero -}

Los reseteo a 0
```

<pre><code><b>proc</b> destroy_tablero(<b>in/out</b> t: Tablero)
	t.tantos_A := 0
	t.tantos_B := 0
<b>end proc</b></code></pre>

```pascal
	fun tanteador_0(t: Tablero) ret b: bool
	{- determina si ningun equipo marco un tanto -}

Verifico si son los 2 == 0
```

<pre><code><b>fun</b> tanteador_0(t: Tablero) <b>ret</b> b: bool
	b := (t.tantos_A == 0 && t.tantos_B == 0)
<b>end fun</b></pre></code>


```pascal
	fun tanteador_A(t: Tablero) ret b: bool
	{- determina si el equipo A ha marcado aunque sea un tanto -}

Verifico si t.tantos_A > 0
```

<pre><code><b>fun</b> tanteador_A(t: Tablero) <b>ret</b> b: bool
	b := (t.tantos_A &gt; 0)
<b>end fun</b></code></pre>


```pascal
	fun tanteador_B(t: Tablero) ret b: bool
	{- determina si el equipo B ha marcado aunque sea un tanto -}

Verifico si t.tantos_B > 0
```

<pre><code><b>fun</b> tanteador_B(t: Tablero) <b>ret</b> b: bool
	b := (t.tantos_B &gt; 0)
<b>end fun</b></code></pre>

```pascal
	fun va_ganando_A(t: Tablero) ret b: bool
	{- determina si el equipo A va ganando -}

Queda igual que antes
```

<pre><code><b>fun</b> va_gananado_A(t: Tablero) <b>ret</b> b: bool
	b := t.tantos_A &gt; t.tantos_B
<b>end fun</b></code></pre>


```pascal
	fun va_ganando_B(t: Tablero) ret b: bool
	{- determina si el equipo B va ganando -}

Queda igual que antes
```

<pre><code><b>fun</b> va_gananado_B(t: Tablero) <b>ret</b> b: bool
	b := t.tantos_B &gt; t.tantos_A
<b>end fun</b></code></pre>


```pascal
	fun va_empate(t: Tablero) ret b: bool
	{- determina si los equipos estan empatados -}

Queda igual que antes
```

<pre><code><b>fun</b> va_empate(t: Tablero) <b>ret</b> b: bool
	b := t.tantos_A == t.tantos_B
<b>end fun</b></code></pre>


```pascal
	proc sumar_n(in/out t: Tablero, in n: nat, in c: char)
	{- le sumo n tantos al equipo c -}

Leo la letra y en caso que sea de algun equipo le sumo n
```

<pre><code><b>proc</b> sumar_n(<b>in/out</b> t: Tablero, <b>in</b> n: nat, <b>in</b> c: char)
	<b>if</b> c == 'A' <b>or</b> 'a' <b>then</b>
		t.tantos_A := t.tantos_A + n
	<b>else if</b> c == 'B' <b>or</b> 'b' <b>then</b>
		t.tantos_B := t.tantos_B + n
	<b>fi</b>
<b>end proc</b></code></pre>


```pascal
	proc restar_n(in/out t: Tablero, in n: nat, in c: char)
	{- le resto n tantos al equipo c -}

IDEM que el anterior pero si t.tantos <= n directamente hago que valga 0 
```

<pre><code><b>proc</b> restar_n(<b>in/out</b> t: Tablero <b>in</b> n: nat, <b>in</b> c: char)
	<b>if</b> c == 'A' <b>or</b> 'a' <b>then</b>
		<b>if</b> t.tantos_A &lt;= n <b>then</b>
			t.tantos_A := 0
		<b>else</b>
			t.tantos_A := t.tantos_A - n
		<b>fi</b>
	<b>else if</b> c == 'B' <b>or</b> 'b' <b>then</b>
		<b>if</b> t.tantos_B &lt;= n <b>then</b>
			t.tantos_B := 0
		<b>else</b>
			t.tantos_B := t.tantos_B - n
		<b>fi</b>
	<b>fi</b>
<b>end proc</b></code></pre>




![ScreenShot](Imagenes%20practico%202.2/ej5.png)

```pascal
Nos pide especificar el TAD Conjunto finito de elementos de tipo T.

Los constructores son:
1- Conjunto vacio (crear porque es constructor)
2- Agregar un elemento al conjunto

Las operaciones son:
1- Chequear si un elemento e pertenece al conjunto c
2- Chequear si el conjunto es vacio
3- Unir un conjunto con otro (no dice como)
4- Insersectar un conjunto con otro
5- Obtener la diferencia entre dos conjuntos

Las 3 ultimas son procedimientos que toman dos conjuntos y modifican el primero de ellos




a)
spec set of T where

constructors
	fun empty_set() ret s: set of T
	{- Crea un conjunto vacio -}

	proc add_element_set(in/out s: set of T, in e: T)
	{- Agrega el elemento e al conjunto -}
	{- PRE e no pertenece a s -}

destroy
	proc destroy_set (in/out s: set of T)
	{- Libera memoria en caso que sea necesario. -}

operations
	fun is_element_set(s: set of T, e: T) ret b: bool
	{- Chequea si el elemento pertence al conjunto -}

	fun is_empty_set(s: set of T) ret b: bool
	{- Chequea si el conjunto es vacio -}

	proc merge_set(in/out s1: set of T, in s2: set of T)
	{- Uno el conjunto1 con el conjunto2 en s1-}

	proc intersect(in/out s1: set of T, in s2: set of T)
	{- Guardamos en s1 la interseccion de los dos conjuntos -}

	proc diff(in/out s1: set of T, in s2: set of T)
	{- Guardamos en s1 la diferencia entre los dos conjuntos -}
end spec

Para el destroy uso el de siempre
```


![ScreenShot](Imagenes%20practico%202.2/ej6.png)

```pascal
a) Ahora nos pide implementar el TAD del ejercicio 5 con una lista de tipo T(o sea el type set of t sea = list of t) donde el constructor para agregar elementos al conjunto se implementa con el addl de listas (o sea nos esta diciendo que lo usemos)
```

<pre><code><b>implement</b> set <b>of</b> T <b>where</b>

<b>type</b> set <b>of</b> T = List <b>of</b> T</code></pre>


```pascal
	fun empty_set() ret s: set of T
	{- Crea un conjunto vacio -}

Como ahora tenemos tipo lista puedo usar esas operaciones, entonces uso empty de TAD lista
```

<pre><code><b>fun</b> empty_set() <b>ret</b> s: set <b>of</b> T
	s := empty()
<b>end fun</b></code></pre>


```pascal
	proc add_element_set(in/out s: set of T, in e: T)
	{- Agrega el elemento e al conjunto -}
	{- PRE e no pertenece a s -}
Entonces debo usar addl, como no dice en que lugar y nos indica que hay que suar addl asumo que esta bien que sea en la primer posicion
```

<pre><code><b>proc</b> add_element_set(<b>in/out</b> s: set <b>of</b> T, <b>in</b> e: T)
	addl(e, s)
<b>end proc</b></code></pre>


```pascal
	proc destroy_set (in/out s: set of T)
	{- Libera memoria en caso que sea necesario. -}

Uso el destroy que ya tengo
```

<pre><code><b>proc</b> destroy_set(<b>in/out</b> s: set <b>of</b> T)
	destroy(s)
<b>end proc</b></code></pre>



```pascal
	fun is_element_set(s: set of T, e: T) ret b: bool
	{- Chequea si el elemento pertence al conjunto -}
Puedo usar el index iterando desde 1 hasta length(s) realizando la comprobacion si el elemento es igual al e
```

<pre><code><b>fun</b> is_element_set(s: set <b>of</b> T, e: T) <b>ret</b> b: bool
	<b>var</b> n: T
	<b>var</b> i: nat
	i := 1
	b := false
	<b>while</b> i &lt; n  && <b>not</b> b <b>do</b>
		<b>if</b> index(s, i) == e <b>then</b>
			b := true
		<b>fi</b>
	<b>od</b>
<b>end fun</b></code></pre>

```pascal
	fun is_empty_set(s: set of T) ret b: bool
	{- Chequea si el conjunto es vacio -}

Puedo usar el is_empty que ya tengo de listas o directemente decir que si el largo es 0 es vacio
```

<pre><code><b>fun</b> is_empty_set(s: set <b>of</b> T) <b>ret</b> b: bool
	b := (length(s) == 0)
<b>end fun</b></code></pre>


```pascal
	proc merge_set(in/out s1: set of T, in s2: set of T)
	{- Uno el conjunto1 con el conjunto2 en s1-}

Como no se pueden repetir los elementos lo que puedo hacer es comparar todos los elementos de la lista la lista que quiero agregar (usando index). Si ya esta lo omit, si no esta lo agrego usando addr

Basicamente lo que hago es usar un bool para decir si ese elemento existe, luego n1 es el largo de la lista original, n2 de la que quiero agregar. Itero primero sobre la lista que quiero agregar y luego sobre la lista original.
Hago lo siguiente: me fijo si el primer elemento de la lista que quiero agregar ya esta en alguna de todas las posiciones de la lista original. Si ya esta no hago nada, pero si no esta lo agrego al final y listo (no tengo que aumentar el tamaño de la lista porque eso solo me serviria en caso de que meta una lista con dos o mas elementos iguales a la lista original pero eso suponemos que no se puede hacer)
Luego repito esto con todos los elementos que quiero agregar.
```

<pre><code><b>proc</b> merge_set(<b>in/out</b> s1: set <b>of</b> T, <b>in</b> s2: set <b>of</b> T)
	<b>var</b> exists: bool
	<b>var</b> n1: nat
	<b>var</b> n2: nat
	n1 := length(s1)
	n2 := length(s2)
	<b>for</b> i := 1 <b>to</b> n2 <b>do</b>
		exists: false
		<b>for</b> j := 1 <b>to</b> n1 <b>do</b>
			<b>if</b> index(s1, i) == index(s2, j) <b>then</b>
				exists := true
			<b>fi</b>
		<b>od</b>
		<b>if</b> <b>not</b> exists <b>then</b>
			addr(s1, i)
		<b>fi</b>
	<b>od</b>
<b>end proc</b></code></pre>


```pascal
	proc intersect(in/out s1: set of T, in s2: set of T)
	{- Guardamos en s1 la interseccion de los dos conjuntos -}

Ahora no queremos agregar elementos, sino que queremos ir guardando en una de las listas los elementos que tenggan en comun entre ambas.
Se me ocurre hacer una lista vacia e ir guardando ahi los elementos que sean iguales para al final retornar volcar esa lista sobre s1 para devolverla
```

<pre><code><b>proc</b> intersect(<b>in/out</b> s1: set <b>of</b> T, <b>in</b> s2: set <b>of</b> T)
	<b>var</b> s_temp: set of T
	<b>var</b> n1: nat
	<b>var</b> n2: nat
	s_temp := empty_set()
	n1 := length(s1)
	n2 := length(s2)
	<b>for</b> i := 1 <b>to</b> n1 <b>do</b>
		<b>for</b> j := 1 <b>to</b> n2 <b>do</b>
			<b>if</b> index(s1, i) == index(s2, j) <b>then</b>
				addl(index(s_temp, i))
			<b>fi</b>
		<b>od</b>
	<b>od</b>
	s1 := empty_set()
	s1 := copy_list(s_temp)
<b>end proc</b></code></pre>



```pascal
	proc diff(in/out s1: set of T, in s2: set of T)
	{- Guardamos en s1 la diferencia entre los dos conjuntos -}

Es como el anterior pero en lugar de guardar lo que tienen en comun guardamos lo que es diferente
```

<pre><code><b>proc</b> diff(<b>in/out</b> s1: set <b>of</b> T, <b>in</b> s2: set <b>of</b> T)
	<b>var</b> s_temp: set of T
	<b>var</b> n1: nat
	<b>var</b> n2: nat
	s_temp := empty_set()
	n1 := length(s1)
	n2 := length(s2)
	<b>for</b> i := 1 <b>to</b> n1 <b>do</b>
		<b>for</b> j := 1 <b>to</b> n2 <b>do</b>
			<b>if</b> index(s1, i) != index(s2, j) <b>then</b>
				addl(index(s_temp, i))
			<b>fi</b>
		<b>od</b>
	<b>od</b>
	s1 := empty_set()
	s1 := copy_list(s_temp)
<b>end proc</b></code></pre>











```pascal
b) Ahora tenemos que hacer lo mismo pero de manera tal que no haya elementos repetidos (se ve que antes si podia haber, mala mia) y que la lista este siempre ordenada de manera creciente (o sea un INVARIANTE)
```

<pre><code><b>implement</b> set <b>of</b> T <b>where</b>

<b>type</b> set <b>of</b> T = List <b>of</b> T</code></pre>


```pascal
	fun empty_set() ret s: set of T
	{- Crea un conjunto vacio -}

Igual que antes
```

<pre><code><b>fun</b> empty_set() <b>ret</b> s: set <b>of</b> T
	s := empty()
<b>end fun</b></code></pre>


```pascal
	proc add_element_set(in/out s: set of T, in e: T)
	{- Agrega el elemento e al conjunto -}
	{- PRE e no pertenece a s -}

Ahora debemos asegurarnos de meter el elemento en el orden correcto para que la lista siga siendo creciente. Asumo que ya la lista viene ordenada y no la tengo que ordenar

Entonces tengo el primer caso en que sea o no lista vacia. Si es lista vacia lo meto directamente. Si no, me tengo que ir fijando el primer elemento (iterando) (o sea uso head) hasta que head > e y entonces lo meto en esa posicion.
Para meterlo en esa posicion deberia usar add_at que lo mete en la posicion n y desplaza los que le siguen

Pero notemos que head solo toma la lista, por lo que es necesario crear una lista auxiliar e ir eleminando los elementos de esa lista para usar head
```

<pre><code><b>proc</b> add_element_set(<b>in/out</b> s: set <b>of</b> T, <b>in</b> e: T)
	<b>var</b> s_temp: set <b>of</b> T
	<b>var</b> i: nat
	i := 1
	s_temp := copy_list(s)
	<b>while</b> <b>not</b> is_empty_set(s_temp) && head(s_temp) &lt; e <b>do</b>
		tail(s_temp)
		i := i + 1
	<b>od</b>
	<b>if</b> is_empty_set(s_temp) <b>or</b> head(s_temp) &gt; e <b>then</b>
		add_at(s, i, e)
	<b>fi</b>
	destroy_set(s_temp)
<b>end proc</b></code></pre>



```pascal
	proc destroy_set (in/out s: set of T)
	{- Libera memoria en caso que sea necesario. -}

Queda igual
```

<pre><code><b>proc</b> destroy_set(<b>in/out</b> s: set <b>of</b> T)
	destroy(s)
<b>end proc</b></code></pre>



```pascal
	fun is_element_set(s: set of T, e: T) ret b: bool
	{- Chequea si el elemento pertence al conjunto -}

Queda igual
```

<pre><code><b>fun</b> is_element_set(s: set <b>of</b> T, e: T) <b>ret</b> b: bool
	<b>var</b> n: T
	<b>var</b> i: nat
	i := 1
	b := false
	<b>while</b> i &lt; n  && <b>not</b> b <b>do</b>
		<b>if</b> index(s, i) == e <b>then</b>
			b := true
		<b>fi</b>
	<b>od</b>
<b>end fun</b></code></pre>

```pascal
	fun is_empty_set(s: set of T) ret b: bool
	{- Chequea si el conjunto es vacio -}

Queda igual
```

<pre><code><b>fun</b> is_empty_set(s: set <b>of</b> T) <b>ret</b> b: bool
	b := (length(s) == 0)
<b>end fun</b></code></pre>


```pascal
	proc merge_set(in/out s1: set of T, in s2: set of T)
	{- Uno el conjunto1 con el conjunto2 en s1-}

Similar al merge_set anterior pero ahora debo revisar que los elementos que vaya poniendo esten en orden creciente, para eso en el que caso que no exista verifico en que lugar tengo que agregar el elemento nuevo haciendo uso del for k:=1
```

<pre><code><b>proc</b> merge_set(<b>in/out</b> s1: set <b>of</b> T, <b>in</b> s2: set <b>of</b> T)
	<b>var</b> exists: bool
	<b>for</b> i := 1 <b>to</b> length(s2) <b>do</b>
		exists: false
		<b>for</b> j := 1 <b>to</b> length(s1) <b>do</b>
			<b>if</b> index(s1, i) == index(s2, j) <b>then</b>
				exists := true
			<b>fi</b>
		<b>od</b>
		<b>if</b> <b>not</b> exists <b>then</b>
			<b>for</b> k := 1 <b>to</b> length(s1) <b>do</b>
				<b>if</b> index(s1, i) &lt; index(s2, k) <b>then</b>
					add_at(s2, k, index(s2, k))
				<b>fi</b>
		<b>fi</b>
	<b>od</b>
<b>end proc</b></code></pre>


```pascal
	proc intersect(in/out s1: set of T, in s2: set of T)
	{- Guardamos en s1 la interseccion de los dos conjuntos -}

TEngo que hacer que se guarden en orden. Es similar a lo de antes, nada mas que ahora en lugar de que sean interseccion no los guardo directamnete.
Primero me fijo si la lista es vacia (para el primer elemento siempre lo sera) entonces lo meto por izquierda. En los casos en que la lista s_temp (donde los guardo) no sea vacia uso la misma logica que use antes para encontrar la posicion en la que debe ir y lo coloco en dicha posicion usando add_at
```

<pre><code><b>proc</b> intersect(<b>in/out</b> s1: set <b>of</b> T, <b>in</b> s2: set <b>of</b> T)
	<b>var</b> s_temp: set of T
	<b>var</b> n1: nat
	<b>var</b> n2: nat
	<b>var</b> exists: bool
	s_temp := empty_set()
	n1 := length(s1)
	n2 := length(s2)
	<b>for</b> i := 1 <b>to</b> n1 <b>do</b>
		exists := false
		<b>for</b> j := 1 <b>to</b> n2 <b>do</b>
			<b>if</b> index(s1, i) == index(s2, j) <b>then</b>
				exists := true
			<b>fi</b>
		<b>od</b>
		<b>if</b> exists && <b>not</b> is_empty(s_temp) <b>then</b>
			<b>for</b> k := 1 <b>to</b> length(s_temp) <b>do</b>
				<b>if</b> index(s1, i) &lt; index(s_temp_, k) <b>then</b>
					add_at(s_temp, k, index(s_temp, k))
				<b>fi</b>
			<b>od</b>
		<b>else if</b> exists && is_empty(s_temp) <b>then</b>
			addl(s_temp, index(s_temp, i))
		<b>fi</b>
	<b>od</b>
	s1 := empty_set()
	s1 := copy_list(s_temp)
<b>end proc</b></code></pre>



```pascal
	proc diff(in/out s1: set of T, in s2: set of T)
	{- Guardamos en s1 la diferencia entre los dos conjuntos -}

IDEM que el anterior, cambio el igual por un distinto
```

<pre><code><b>proc</b> intersect(<b>in/out</b> s1: set <b>of</b> T, <b>in</b> s2: set <b>of</b> T)
	<b>var</b> s_temp: set of T
	<b>var</b> n1: nat
	<b>var</b> n2: nat
	<b>var</b> exists: bool
	s_temp := empty_set()
	n1 := length(s1)
	n2 := length(s2)
	<b>for</b> i := 1 <b>to</b> n1 <b>do</b>
		exists := false
		<b>for</b> j := 1 <b>to</b> n2 <b>do</b>
			<b>if</b> index(s1, i) != index(s2, j) <b>then</b>
				exists := true
			<b>fi</b>
		<b>od</b>
		<b>if</b> exists && <b>not</b> is_empty(s_temp) <b>then</b>
			<b>for</b> k := 1 <b>to</b> length(s_temp) <b>do</b>
				<b>if</b> index(s1, i) &lt; index(s_temp_, k) <b>then</b>
					add_at(s_temp, k, index(s_temp, k))
				<b>fi</b>
			<b>od</b>
		<b>else if</b> exists && is_empty(s_temp) <b>then</b>
			addl(s_temp, index(s_temp, i))
		<b>fi</b>
	<b>od</b>
	s1 := empty_set()
	s1 := copy_list(s_temp)
<b>end proc</b></code></pre>