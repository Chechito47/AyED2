
```pascal
1) Definición recursiva de la función factorial  
Encabezado: fun factorial(n: nat) ret f : nat

Recordemos que:

CASO BASE: n = 0 => fac = 1
CASO RECURSIVO: n > 0 => fac = fac(n-1)
```

<pre><code class="language-pascal">
<b>fun</b> factorial(n: nat) <b>ret</b> f: nat
	<b>if</b> n = 0
		<b>then</b> f := 1
	<b>else</b> f := n * factorial(n-1)
	<b>fi</b>
<b>end fun</b>

</code></pre>

```pascal
1) Definición iterativa de la función factorial
Encabezado: fun factorial(n: nat) ret f : nat

Ahora como debe ser iterativa debemos hacer uso de un ciclo de repeticion como do, for o while
```

<pre><code class="language-pascal">
<b>fun</b> factorial(n: nat) <b>ret</b> f: nat
	<b>var</b> aux: int
	aux := 1
	f := 1
	<b>while</b> (aux &lt;= n) do 
		f := f * aux
		aux := aux + 1
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
Veamos como funciona con n=3:
aux=1, f=1
	1<=3 si => f=1*1=1
aux=2

	2<=3 si => f=1*2=2
aux=3

	3<=3 si => f=2*3=6
aux=4

	4<=3 no => end fun

Entonces funciona bien.

Aunque notemos que tambien se puede hacer un con for:
```

<pre><code class="language-pascal">
<b>fun</b> factorial(n: nat) <b>ret</b> f:nat
	f := 1
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		f := f * i
		<b>od</b>
	<b>end fun</b>
	</code></pre>
```pascal
3) Procedimiento para inicializar un arreglo en cero
Encabezado: proc init_array(out a: array[N..M] of int)
```

<pre><code class="langugage-pascal">
<b>proc</b> init_array(<b>out</b> a: array[N..M] <b>of</b> int)
<b>for</b> i := N <b>to</b> M <b>do</b>
	a[i] := 0
<b>od</b>
<b>end proc</b>
</code></pre>

```pascal
Veamos un ejemplo, array=[1, 2, 3] (notemos que tiene que estar ordenado y empezar en 1)
(a0 indica la posicion 0)

i=a0
1<=3 si => a[0] = 0
2<=3 si => a[1] = 0
3<=3 si => a[2] = 0
```


```pascal
4) Procedimiento para incrementar en 1 los valores de un arreglo
Encabezado: proc init_array(in/out a: array[N..M] of int)

Similar al anterior, cambio el =0 para sumar 1
```

<pre><code class="langugage-pascal">
<b>proc</b> init_array(<b>out</b> a: array[N..M] <b>of</b> int)
<b>for</b> i := N <b>to</b> M <b>do</b>
	a[i] := a[i] + 1
<b>od</b>
<b>end proc</b>
</code></pre>

```pascal
5) Función para encontrar el mínimo elemento de un arreglo
Encabezado: fun min(a: array[1..N] of int) ret m : int
Hacer dos versiones: una con un while y otra con un for.
```

<pre><code class="language-pascal">
<b>fun</b> min(a: array [1..N] <b>of</b> int) <b>ret</b> m: int
	<b>var</b> aux: int
	aux := infinito
	<b>for</b> i := 1 to N <b>do</b>
		<b>if</b> a[i] &lt; aux <b>then</b>
			aux := a[i]
		<b>fi</b>
	<b>od</b>	
	m := aux
<b>end fun</b>
</code></pre>

```pascal
Veamos con array=[2, 1, 4]
aux=inf
2<inf si => aux=2
1<2 si => aux=1
4<1 no
termina el ciclo

m=1
```