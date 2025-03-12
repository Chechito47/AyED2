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
d) Incrementar las posiciones impares y dejar intactas las pares
```

> **proc** inc_impar (**in/out** a: array[1..n] **of nat**)  
	**for** i:=1 **to** n **do**  
		**if** (i mod 2 == 1) **then**  
			a[i] = a[i] + 1  
		**fi**  
	**od**  
**end proc**  


![ScreenShot](Imagenes%20practico%201.1/ej3.png)

```java
proc verordenado (in a: array[1..n] of nat, out o: bool)
	o := true

	if n>1 then    //Check for an array with two elements at least
		for i:=1 to n-1 do
			if (a[i] > a[i+1])
				o := False
				exit
			fi
		od
	fi
end proc

¿Que hace?
El algoritmo verifica si el arreglo "a" esta ordenado de menor a mayor

¿Como lo hace?
Primero verifica que al menos el arreglo tenga 2 elementos, caso contrario devuelve que esta ordenado.
Comparando dos elementos entre si, arrancando desde el primero y comparandolo con el siguiente. En caso de que alguna comparacion de como resultado que el elemento de la izq es mayor a de la de devuelve que el arreglo no esta ordenado y termina su ejecucion.
```


![ScreenShot](Imagenes%20practico%201.1/ej4.png)

```java
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
```java
a)
t:=0
for i:=1 to n do
	for j:=1 to n² do
		for k:=1 to n³ do
			t:= t + 1
		od
	od
od

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



b)
t:=0
for i:=1 to n do
	for j:=1 to i do
		for k:=j to j+3 do
			t:= t + 1
		od
	od
od

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

```pascal
proc p (in/out a: array[1..n] of T)
	var x: nat
	for i:=n downto 2 do
		x:= f(a,i)
		swap(a, i, x)
	od
end proc

fun f (a: array[1..n] of T, i: nat) ret x: nat
	x:= 1
	for j:=2 to i do
		if a[j] > a[x] then x:=j fi
	od
end fun

¿Que hacen?
A simple vista parece el algoritmo de ordenacion por seleccion (selection sort) el cual ordena los elementos de un arreglo de mayor a menor

¿Como lo hace?
Toma los elementos de a dos comenzando desde el final del arreglo, llama a la funcion "f" la cual los compara y si el que esta a la derecha es mayor que el de la izquierda guarda en x el valor del que estaba en la derecha para luego con la funcion swap intercambiarlos de lugar. Hace eso hasta que queden dos elementos.

Cambio de nombres:
proc selection_sort_biggest (in/out a: array[1..n] of T)
	var max: nat
	for i:=n downto 2 do
		max:= find_max(a,i)
		swap(a, i, x)
	od
end proc

fun find_max (a: array[1..n] of T, i: nat) ret max: nat
	max:= 1
	for j:=2 to i do
		if a[j] > a[max] then max:=j fi
	od
end fun
```



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

```java
a)
t:=1
do t < n
	t:= t * 2
od

Valor n     Resultado
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


b)
t:=n
do t > 0
	t:= t div 2
od

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


c)
for i:=1 to n do
	t:=i
	do t > 0
		t:= t div 2
	od
od

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
ops = (2*n)+(2*n-1)+- Cantidad de potencias de 2 que se pasaron con el n actual arrancando con -2


d)
for i:=1 to n do
	t:=i
	do t > 0
		t:= t - 2
	od
od

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
ops(nat)= nose
```


![ScreenShot](Imagenes%20practico%201.1/ej9.png)

```pascal
proc verordenado (in a: array[1..n] of nat, out o: bool)
	o := true

	if n>1 then    //Check for an array with two elements at least
		for i:=1 to n-1 do
			if (a[i] > a[i+1])
				o := False
				exit
			fi
		od
	fi
end proc

ops(verord)=ops(if n>1 then (for i:=1 to n-1 do (if (a[i] > a[i+1]) fi) od) fi)
ops(verord)=ops(if n>1 then (for i:=1 to n-1) fi)
ops(verord)=ops(if n>1 then (Σ 1 to n-1) fi)
ops(verord)=ops(Σ 1 to n-1)
ops(verord)=n-1 (aprox)

Notar que los dos if no los cuento porque no sirven, ya que estas comparaciones dependen directamente del n, por ende el resultado no es del todo exacto pero si de ese orden
```


![ScreenShot](Imagenes%20practico%201.1/ej10.png)

```pascal
proc q (in/out a: array[1..n] of T)
	for i:=n-1 downto 1 do
		r(a,i)
	od
end proc

proc r (in/out a: array[1..n] of T, in i: nat)
	var j: nat
	j:=i
	do j<n ^ a[j]>a[j+1] -> swap(a, j+1, j)
							j:=j+1
	od
end proc


¿Que hacen?
Ordena los elementos de un arreglo de menor a mayor. Es una especie de insertion sort

¿Como lo hace?
Comienza desde el anteultimo valor, llama a la funcion r la cual compara el valor dos valores. Si el de la izquierda es mayor a de la derecha hace swap.

Ejemplo:
5, 4, 7
4, 5, 7

Cambio de nombres:
proc insertion_sort_downto (in/out a: array[1..n] of T)
	for i:=n-1 downto 1 do
		r(a,i)
	od
end proc

proc insert_up (in/out a: array[1..n] of T, in i: nat)
	var j: nat
	j:=i
	do j<n ^ a[j]>a[j+1] -> swap(a, j+1, j)
							j:=j+1
	od
end proc
```