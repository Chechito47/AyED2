
![ScreenShot](Imagenes%20practico%201.1/ej1.png)

```pascal
Recordemos, si voy a usar la variable en el lado izquierdo debe ser de tipo out:
x = 2
Ya que le uso para como valor de salida para otra variable o valor

Pero si lo voy a usar del lado derecho debe ser de tipo in:
y = x
Ya que la uso como valor de entrada para otra variable


a) Inicializar cada componente del arreglo con el valor 0
```

<pre><code class="language-pascal">
<b>proc</b> init0 (<b>out</b> a: <b>array</b>[1..N ] <b>of nat</b>)
	<b>for</b> i := 1 <b>to</b> N <b>do</b>
		a[i] := 0
	<b>od</b>
<b>end proc</b>
</code></pre>


```pascal
b) Inicializar el arreglo con los primeros n numeros naturales positivos
```

<pre><code class="language-pascal">
<b>proc</b> init_nat (<b>out</b> a: <b>array</b>[1..N] <b>of nat</b>)
	<b>var</b> j: nat
	j := 1
	<b>for</b> i <b>to</b> N <b>do</b>
		a[i] := j
		j := j + 1
	<b>od</b>
<b>end proc</b>
</code></pre>


```pascal
c) Inicializar el arreglo con los primeros n numeros naturales impares
```

<pre><code class="language-pascal">
<b>proc</b> init_impar (<b>out</b> a: <b>array</b>[1..N] <b>of nat</b>)
	<b>var</b> j: nat
	j := 1
	<b>for</b> i <b>to</b> N <b>do</b>
		a[i] := j
		j := j + 2
	<b>od</b>
<b>end proc</b>
</code></pre>

```pascal
d) Incrementar las posiciones impares y dejar intactas las pares
```

<pre><code class="language-pascal">
<b>proc</b> inc_impar (<b>in/out</b>: a[1..N] <b>of nat</b>)
	<b>var</b> i: nat
	i := 1
	<b>while</b> i &lt;= N <b>do</b>
		a[i] = a[i] + 1
		i := i + 2
	<b>od</b>
<b>end proc</b>
</code></pre>


![ScreenShot](Imagenes%20practico%201.1/ej2.png)
```pascal
Las anteriores ya las hice con for
d) Incrementar las posiciones impares y dejar intactas las pares
```

<pre><code class="language-pascal">
<b>proc</b> inc_impar(<b>in/out</b> a: <b>array</b>[1..N] <b>of nat</b>)
	<b>for</b> i := 1 <b>to</b> N <b>do</b>
		<b>if</b> (i mod 2 == 1) <b>then</b>
			a[i] = a[i] + 1
		<b>fi</b>
	<b>od</b>
</code></pre> 

![ScreenShot](Imagenes%20practico%201.1/ej3.png)

<pre><code class="language-pascal">
<b>proc</b> determinar_ordenado(<b>in</b> a:<b>array</b>[1..N] <b>of nat</b>, <b>out</b> res: bool)
	res := true
	<b>if</b> n &gt; 1 <b>then</b>
		<b>for</b> i := 1 <b>to</b> N-1 <b>do</b>
			<b>if</b> a[i] > a[i+1]
				res := false
				<b>exit</b>
			<b>fi</b>
		<b>od</b>
	<b>fi</b>
<b>end proc</b>
</code></pre>

```pascal
¿Que hace?
El algoritmo verifica si el arreglo "a" esta ordenado de menor a mayor

¿Como lo hace?
Primero verifica que al menos el arreglo tenga 2 elementos, caso contrario devuelve que esta ordenado.
Comparando dos elementos entre si, arrancando desde el primero y comparandolo con el siguiente. En caso de que alguna comparacion de como resultado que el elemento de la izq es mayor a de la de devuelve que el arreglo no esta ordenado y termina su ejecucion.
```


![ScreenShot](Imagenes%20practico%201.1/ej4.png)

```pascal
Recordemos que el de seleccion toma el menor elemento entre 1 y n y lo compara con el primero y si es mas pequeño lo pone primero, sino segundo

a)
[7, 1, 10, 3, 4, 9, 5]
[7, *1*, 10, 3, 4, 9, 5]
[1, 7, 10, 3, 4, 9, 5]
[1, 7, 10, *3*, 4, 9, 5]
[1, 3. 7, 10, 4, 9, 5]
[1, 3. 7, 10, *4*, 9, 5]
[1, 3. 4, 7, 10, 9, 5]
[1, 3. 4, 7, 10, 9, *5*]
[1, 3. 4, 5, 7, 10, 9]
[1, 3. 4, 5, *7*, 10, 9]
[1, 3. 4, 5, 7, 10, 9]
[1, 3. 4, 5, 7, 10, *9*]
[1, 3. 4, 5, 7, 9, 10]


b)
[5, 4, 3, 2, 1]
[5, 4, 3, 2, *1*]
[1, 5, 4, 3, 2]
[1, 5, 4, 3, *2*]
[1, 2, 5, 4, 3]
[1, 2, 5, 4, *3*]
[1, 2, 3, 5, 4]
[1, 2, 3, 5, *4*]
[1, 2, 3, 4, 5]

c)
[1, 2, 3, 4 ,5]
[*1*, 2, 3, 4 ,5]
[1, 2, 3, 4 ,5]
[1, *2*, 3, 4 ,5]
[1, 2, 3, 4 ,5]
[1, 2, *3*, 4 ,5]
[1, 2, 3, *4* ,5]
[1, 2, 3, 4 ,5]
```


![ScreenShot](Imagenes%20practico%201.1/ej5.png)

![ScreenShot](Imagenes%20practico%201.1/ecuaciones.png)

<pre><code>
t := 0
<b>for</b> i := 1 <b>to</b> n <b>do</b>
	<b>for</b> j := 1 <b>to</b> n² <b>do</b>
		<b>for</b> k := 1 <b>to</b> n³ <b>do</b>
			t := t + 1
		<b>od</b>
	<b>od</b>
<b>od</b>
</code></pre>

```pascal
a)
A simple vista podemos notar que el algoritmo hace n⁶ asignaciones:
ops(A) = ops(t:=0) + ops(for i:=1 to n do (for j:=1 to n² do (for 
		 k:=1 to n³ do t:=t+1 od) od) od)
ops(A) = 1 + ops(for i:=1 to n do (for j:=1 to n² do (for k:=1
		 to n³ do t:=t+1 od) od) od)
ops(A) = 1 + ops(for i:=1 to n do (for j:=1 to n² do (for k:=1
		 to n³ (1) od) od)
ops(A) = 1 + ops(for i:=1 to n do (for j:=1 to n² do (Σ 1 to n³)(1)) od) od)
ops(A) = 1 + ops(for i:=1 to n do (Σ 1 to n²) ((Σ 1 to n³)(1)) od)
ops(A) = 1 + ops((Σ 1 to n²) (Σ 1 to n²) ((Σ 1 to n³)(1))
ops(A) = 1 + ops(n(n²(n³*1)))
ops(A) = 1 + n*n²*n³
ops(A) = 1 + n⁶

Donde el + 1 es despreciable
```

<pre><code>
t := 0
<b>for</b> i := 1 <b>to</b> n <b>do</b>
	<b>for</b> j := 1 <b>to</b> i <b>do</b>
		<b>for</b> k := j <b>to</b> j + 3 <b>do</b>
			t := t + 1
		<b>od</b>
	<b>od</b>
<b>od</b>
</code></pre>

```pascal
b)
ops(B) = ops(t:=0) + ops(for i:=1 to n do (for j:=1 to i do (for k:=j to j+3
		 do t:=t+1)))
ops(B) = 1 + ops(for i:=1 to n do (for j:=1 to i do (for k:=j to j+3 do t:=t+1)))
ops(B) = 1 + ops(for i:=1 to n do (for j:=1 to i do (for k:=j to j+3 (1)od)od)od)
ops(B) = 1 + ops(for i:=1 to n do (for j:=1 to i do (Σ j to j+3 (1)od)od))
ops(B) = 1 + ops(for i:=1 to n do (Σ j to i do (Σ j to j+3 (1)od)od))
ops(B) = 1 + ops(Σ(i to n) do (Σ(j to i) do (Σ (j to j+3) (1)od)od))
ops(B) = 1 + ops(Σ(i to n) do (Σ(j to i) * 4)od))
ops(B) = 1 + ops(Σ(i to n) ((n*(n+1)/2) * 4)))
ops(B) = 1 + n * ((n*(n+1)/2) * 4))
ops(B) = 1 + n * 4n(n+1)/2
ops(B) = 1 + 4n²(n+1)/2
ops(B) = 1 + 2n²(n+1)
```


![ScreenShot](Imagenes%20practico%201.1/ej6.png)

<pre><code>
<b>proc</b> p (<b>in/out</b> a:<b>array</b>[1..n] <b>of T</b>)
	<b>var</b> x: <b>nat</b>
	<b>for</b> i := n <b>downto</b> 2 <b>do</b>
		x := f(a, i)
		swap(a, i, x)
	<b>od</b>
<b>end proc</b>


<b>fun</b> f(a:<b>array</b>[1..n] <b>of T</b>, i: <b>nat</b>) <b>ret</b> x: <b>nat</b>
	x := 1
	<b>for</b> j := 2 <b>to</b> i <b>do</b>
		<b>if</b> a[j] &gt; a[x] <b>then</b> x := j <b>fi</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
¿Que hacen?
A simple vista parece el algoritmo de ordenacion por seleccion (selection sort) el cual ordena los elementos de un arreglo de mayor a menor

¿Como lo hace?
Toma los elementos de a dos comenzando desde el final del arreglo, llama a la funcion "f" la cual los compara y si el que esta a la derecha es mayor que el de la izquierda guarda en x el valor del que estaba en la derecha para luego con la funcion swap intercambiarlos de lugar. Hace eso hasta que queden dos elementos.

Cambio de nombres:
```

<pre><code>
<b>proc</b> selection_sort_biggest (<b>in/out</b> a:<b>array</b>[1..n] <b>of T</b>)
	<b>var</b> max: <b>nat</b>
	<b>for</b> i := n <b>downto</b> 2 <b>do</b>
		max := find_max(a, i)
		swap(a, i, x)
	<b>od</b>
<b>end proc</b>


<b>fun</b> find_max(a:<b>array</b>[1..n] <b>of T</b>, i: <b>nat</b>) <b>ret</b> max: <b>nat</b>
	x := 1
	<b>for</b> j := 2 <b>to</b> i <b>do</b>
		<b>if</b> a[j] &gt; a[max] <b>then</b> max := j <b>fi</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

![ScreenShot](Imagenes%20practico%201.1/ej7.png)

```pascal
a)
[7, 1, 10, 3, 4, 9, 5]
[7, *1*, 10, 3, 4, 9, 5]
[1, 7, 10, 3, 4, 9, 5]
[1, 7, *10*, 3, 4, 9, 5]
[1, 7, 10, 3, 4, 9, 5]
[1, 7, 10, *3*, 4, 9, 5]
[1, 3, 7, 10, 4, 9, 5]
[1, 3, 7, 10, *4*, 9, 5]
[1, 3, 4, 7, 10, 9, 5]
[1, 3, 4, 7, 10, *9*, 5]
[1, 3, 4, 7, 9, 10, 5]
[1, 3, 4, 7, 9, 10, *5*]
[1, 3, 4, 5, 7, 9, 10]


b)
[5, 4, 3, 2, 1]
[5, *4*, 3, 2, 1]
[4, 5, 3, 2, 1]
[4, 5, *3*, 2, 1]
[3, 4, 5, 2, 1]
[3, 4, 5, *2*, 1]
[2, 3, 4, 5, 1]
[2, 3, 4, 5, *1*]
[1, 2, 3, 4, 5]


c)
[1, 2, 3, 4 ,5]
[1, *2*, 3, 4 ,5]
[1, 2, 3, 4 ,5]
[1, 2, *3*, 4 ,5]
[1, 2, 3, 4 ,5]
[1, 2, 3, *4* ,5]
[1, 2, 3, 4 ,5]
[1, 2, 3, 4 ,*5*]
[1, 2, 3, 4 ,5]
```


![ScreenShot](Imagenes%20practico%201.1/ej8.png)
<pre><code>
t := 1
<b>do</b> t &lt; n
	t := t * 2
<b>od</b>
</code></pre>

```pascal
a)
Notemos que de base tenemos una asignacion a t
Valor n Resultado
n=0  -> asig=1
n=1  -> asig=1
n=2  -> asig=2
n=3  -> asig=3
n=4  -> asig=3
n=5  -> asig=4
n=6  -> asig=4
n=7  -> asig=4
n=8  -> asig=4
n=9  -> asig=5
n=10 -> asig=5

Es similar al log2(x)
```

<pre><code>
t := n
<b>do</b> t &gt; 0
	t := t div 2
<b>od</b>
</code></pre>

```pascal
b)
Notar que nuevamente tengo una asignacion a t para todos los casos de base
Paro cuando 1 < t < 0
n=0  -> asig=1
n=1  -> asig=2
n=2  -> asig=3
n=3  -> asig=3
n=4  -> asig=4
n=5  -> asig=4
n=6  -> asig=4
n=7  -> asig=4
n=8  -> asig=5
n=9  -> asig=5
n=10 -> asig=5

Otra vez similar a log2(x)
```

<pre><code>
<b>for</b> i := 1 <b>to</b> n <b>do</b>
	t := i
	<b>do</b> t &gt; 0
		t := t div 2
	<b>od</b>
<b>od</b>
</code></pre>

```pascal
c)
n=2
1<=2 si => t=1; 1>0 si => t=1/2=0.5
2<=2 si => t=2; 2>0 si => t=2div2=1
				1>0 si => t=1/2=0.5
3<=2 no termino => 5 asignaciones a t

n=3
1<=3 si => t=1; 1>0 si => t=1/2=0.5
2<=3 si => t=2; 2>0 si => t=2/2=1
				1>0 si => t=1/2=0.5
3<=3 si => t=3; 3>0 si => t=3/2=1.5
				1.5>0 si => t=1.5/2=0.75
4<=3 no termino => 8

N=4
IDEM que anterior hasta:
4<=4 si => t=4; 4>0 si => t=4/2=2
				2>0 si => t=4/2=1
				1>0 si => t=1/2=0.5
5<=4 no termino => 8 + 4 = 12 asignaciones


No tengo asignacion base
n=0  -> asig=0
n=1  -> asig=2
n=2  -> asig=5
n=3  -> asig=8
n=4  -> asig=12
n=5  -> asig=16
n=6  -> asig=20
n=7  -> asig=24
n=8  -> asig=29
n=9  -> asig=34
n=10 -> asig=35

Por cada potencia de 2 incremento la cantidad de aignaciones en 1, arrancando en 2.
Entonces:
ops = (2*n)+(2*tn-1)+- Cantidad de potencias de 2 que se pasaron con el n actual arrancando con -2
```

<pre><code><b>for</b> i := 1 <b>to</b> n <b>do</b>
	t := i
	<b>do</b> t &gt; 0
		t := t - 2
	<b>od</b>
<b>od</b>
</code></pre>

```pascal
d)
n=1
1<=1 si => t=1; 1>0 si => t=1-2=-1
2<=1 no termino => 2 asignaciones

n=2
IDEM que n=1 con:
2<=2 si => t=2; 2>0 si => t=2-2=0
3<=3 no termino => 4 asignaciones

n=3
IDEM que n=2 con:
3<=3 si => t=3; 3>0 si => t=3-2=1
				1>0 si => t=1-2=-1
4<=3 no termino => 4 + 3= 7 asignaciones

n=4
IDEM que n=3 con:
4<=4 si => t=4; 4>0 si => t=4-2=2
				2>0 si => t=2-2=0
5<=4 no termino => 7 + 3= 10 asignaciones

n=5
IDEM que n=4 con:
5<=5 si => t=5; 5>0 si => t=5-2=3
				3>0 si => t=3-2=1
				1>0 si =>t=1-2=-1
6<=6 no termino => 10 + 4= 14 asignaciones

n=0  -> asig=0
n=1  -> asig=2
n=2  -> asig=4
n=3  -> asig=7
n=4  -> asig=10
n=5  -> asig=14
n=6  -> asig=18
n=7  -> asig=23
n=8  -> asig=28
n=9  -> asig=34
n=10 -> asig=40

Arranco sumando de a 2, luego por cada impar la cantidad de ops aumenta en 1
ops(nat)= res(n)= 2n + x
		  donde x=0 y aumenta en 1 cada vez que n se hace impar
		  x se empieza a sumar desde n=3
```


![ScreenShot](Imagenes%20practico%201.1/ej9.png)

<pre><code class="language-pascal">
<b>proc</b> determinar_ordenado(<b>in</b> a:<b>array</b>[1..N] <b>of nat</b>, <b>out</b> res: bool)
	res := true
	<b>if</b> n &gt; 1 <b>then</b>
		<b>for</b> i := 1 <b>to</b> N-1 <b>do</b>
			<b>if</b> a[i] > a[i+1]
				res := false
				<b>exit</b>
			<b>fi</b>
		<b>od</b>
	<b>fi</b>
<b>end proc</b>
</code></pre>

```pascal
ops(det_ord)=ops(if n>1 then (for i:=1 to N-1 do (if (a[i] > a[i+1]) fi) od) fi)
ops(det_ord)=ops(if n>1 then (for i:=1 to N-1) fi)
ops(det_ord)=ops(if n>1 then (Σ 1 to n-1) fi)
ops(det_ord)=ops(Σ 1 to n-1)
ops(det_ord)=n-1 (aprox)

Notar que los dos if no los cuento porque no sirven, ya que estas comparaciones dependen directamente del n, por ende el resultado no es del todo exacto pero si de ese orden
```


![ScreenShot](Imagenes%20practico%201.1/ej10.png)

<pre><code>
<b>proc</b> q(<b>in/out</b> a:<b>array</b>[1..n] <b>of T</b>)
	<b>for</b> i := n-1 <b>downto</b> 1 <b>do</b>
		r(a, i)
	<b>od</b>
<b>end proc</b>


<b>proc</b> r(<b>in/out</b> a:<b>array</b>[1..n] <b>of T</b>, <b>in</b> i: <b>nat</b>)
	<b>var</b> j: <b>nat</b>
	j := i
	<b>do</b> j &lt; n ^ a[j] &gt; a[j+1] -> swap(a, j+1, j)
								    j := j + 1
	<b>od</b>
<b>end proc</b></code></pre>

```pascal
¿Que hacen?
Ordena los elementos de un arreglo de menor a mayor. Es una especie de insertion sort

¿Como lo hace?
Comienza desde el anteultimo valor, llama a la funcion r la cual compara el valor dos valores. Si el de la izquierda es mayor a de la derecha hace swap.

Ejemplo:
5, 4, 7
4, 5, 7

Cambio de nombres:
```

<pre><code>
<b>proc</b> insertion_sort_downto(<b>in/out</b> a:<b>array</b>[1..n] <b>of T</b>)
	<b>for</b> i := n-1 <b>downto</b> 1 <b>do</b>
		insert_downto(a, i)
	<b>od</b>
<b>end proc</b>


<b>proc</b> insert_downto(<b>in/out</b> a:<b>array</b>[1..n] <b>of T</b>, <b>in</b> i: <b>nat</b>)
	<b>var</b> j: <b>nat</b>
	j := i
	<b>do</b> j &lt; n ^ a[j] &gt; a[j+1] -> swap(a, j+1, j)
								    j := j + 1
	<b>od</b>
<b>end proc</b></code></pre>
