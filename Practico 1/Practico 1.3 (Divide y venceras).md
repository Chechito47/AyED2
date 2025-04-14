![ScreenShot](Imagenes%20practico%201.3/ej1.png)

```pascal
a)
Primero veo si es DyV
Vemos que hay un caso en que se llama a si mismo con un menor dato de entrada por lo tanto es DyV

a = numero de llamadas recursivas
b = cantidad de veces que divido mi problema
k = orden del polinomio

Definimos una funcion recursiva r(n) que calculara la cantidad de asignaciones a la variable t que ocurren al ejecutar el procedimiento f1 con el dato de entrada n

Notemos tambien que si n<=1 no hago nada => t(0), t(1) = 0

t(n) = | 0 si  n <=1
	   | Σ(1 to 8) r(n/2) + Σ(1 to n³) 1 caso contrario

Que es lo mismo que

t(n) = | 0 si n<=1
	   | 8*r(n/2) + n³ caso contrario

Entonces, analizo:
a = 8
b = 2
g(n) = n³ => k = 3

Entonces, analizemos la forma de a comparada a b^k
a=8, 2³=8 => a = b^k
Entonces
r(n) es del orden n³ log n



b)
Nuevamente tenemos dos casos (aunque para el primer for la guarda no ex explicita)

Si n <= 0 no hago nada
si n>0 si

Veamos que pasa cuando si hago algo:
Hay dos for, el primero no es DyV porque no es recursivo por lo tanto lo puedo analizar de forma normal:

ops(b) = (Σ i=1 to n(Σ j=1 to i(t:=1)))
	   = (Σ i=1 to n(Σ j=1 to i(1)))
	   = (Σ i=1 to n(i(1)))
	   = (n(i(1)))
	   = n*(n+1)/2
	   = n²+n/2
	   = n²

Veamos el otro for: este si se es recursivo y por lo tanto es DyV

t(n) = | 0 si n <= 0
	   | Σ 1 to 4 f2(n/2) + n² si n > 0 (Donde el n² viene del for anterior)

Entonces

t(n) = | 0 si n <= 0
	   | 4 * f2(n/2) + n² si n > 0

Analizo la complejidad:
a = 4
b = 2
g(n) = n² => k = 2

Entonces 4 = 2² por lo tanto la complejidad del algortimo es de orden:
n² log n
```

![ScreenShot](Imagenes%20practico%201.3/ej2.png)

```pascal
a)
Cima: valor k en el intervalo 1..n tq a[1..k] esta ordenado crecientemente y a[k..n] esta ordenado decrecientemente
Por lo tanto:
a=[1, 2, 3, 4, 3, 2, 1] tiene a 4 como cima
a=[1, 2, 3, 4, 1] tiene a 4 como cima
a=[1, 2, 3, 4, 4] no tiene cima
a=[1, 4, 2, 3] no tiene cima
a=[1, 4, 3, 2] tiene 4 como cima
a=[1, 4, 4] no tiene cima

Entonces, necesito chequear primero que el arreglo sea creciente desde 1 hasta k.
Pero tambien debo ver que esa creciente no termine en N o en 1, si no en medio siendo este medio el k.
Luego tambien debo chequear que sea decreciente desde k hasta N

Si se cumplen esas condiciones puedo afirmar que k es una cima
```

<pre><code>
<b>fun</b> tiene_cima(<b>in</b> a:<b>array [1..N]</b> of <b>T</b>, <b>ret</b> hay_cima: <b>bool</b>)
	<b>var</b> i: <b>nat</b>
	i := 1
	hay_cima := false
	<b>while</b> i &lt; n ^ a[i] &lt; a[i+1] <b>do</b>
		i := i + 1
	<b>od</b>
	
	<b>if</b> i != 1 ^ i != n <b>then</b>
		<b>while</b> i &lt; n ^ a[i] > a[i+1] <b>do</b>
			i := i + 1
		<b>od</b>
		
		<b>if</b> i != n <b>then</b>
			hay_cima := false
		<b>else</b>
			hay_cima := true
		<b>fi</b>
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Supongamos a=[1, 2, 3, 4, 1] la cual tiene cima 4:
hay_cima=false, i=1, n=5
while 1 < 5 && 1 < 2 si => i=2
	  2 < 5 && 2 < 3 si => i=3
	  3 < 5 && 3 < 4 si => i=4
	  4 < 5 && 4 < 1 no

 if 4!=1 && 4 !=5 si
	 while 4 < 5 && 4 > 1 si => i=5

	if 5!5 no => hay_cima=true
Funciono


Supongamos a=[1]
hay_cima=false, i=1, n=1
while 1 < 1 no
if 1!=1 && 1!=1 no
	hay_cima=false


Supongamos a[3, 4, 5]
hay_cima=false, i=1, n=3
while 1 < 3 && 3 < 3 si => i=2
	  2 < 3 && 4 < 5 si => i=3
	  3 < 3 no

if 3!=1 && 3!=3 no
	hay_cima=false


Supongamos a[5, 4, 3]
hay_cima=false, i=1, n=3
while 1 < 3 && 5 < 4 no

if 1!=1 && 1!=3 no
hay_cima=false


Supongamos a[3, 4, 2]
hay_cima=false, i=1, n=3
while 1 < 3 && 3 < 4 si => i=2
	  2 < 3 && 4 < 2 no

if 2!=1 && 2!=3 si
	while 2 < 3 && 4 > 2 => i=3
		  3 < 3 no
	if 3!=3 no => hay_cima=true


Supongamos a[3, 4, 2, 5]
hay_cima=false, i=1, n=4
while 1 < 4 && 3 < 4 si => i=2
	  2 < 4 && 4 < 2 no

if 2!=1 && 2!=4 si
	while 2 < 4 && 4 > 2 si => i=3
		  3 < 4 && 2 > 5 no
	if 3!=4 si => hay_cima=false


Supongamos a[1, 4, 4]
hay_cima=fals, i=1, n=3
while 1 < 3 && 1 < 4 si => i=2
	  2 < 3 && 4 < 4 no

if 2!=1 && 2!=3 si
	while 2 < 3 && 4 > 4 no
	if 2!=3 si => hay_cima=false
```

```pascal
b) La idea de la busqueda secuencial es recorrer todo el arreglo (el cual esta sin ordenar) comparando 1 a 1 los elementos con el valor que buscamos. Si lo encontramos devolvemos su indice, caso contrario 0
Notar que asumimos que el arreglo cumple las condiciones y tiene una cima
```

<pre><code>
<b>fun</b> encontrar_cima_secuencial(<b>in</b> a:<b>array[1..N]</b> of <b>T</b>, <b>ret</b> cima: <b>nat</b>)
	cima := a[1]
	<b>if</b> n &gt;= 2 <b>then</b>
		<b>for</b> i:=2 <b>to</b> n <b>do</b>
			<b>if</b> a[i] &gt; a[a-1] <b>then</b>
				cima := a[i]
			<b>fi</b>
		<b>od</b>
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Supongamos a[1, 2, 3, 4, 1]
cima=1, n=5
if 5>=2 si
	for i=2 to 5
		if 2 > 1 si => cima=2
		i=3 to 5
		if 3 > 2 si => cima=3
		i=4 to 5
		if 4 > 3 si => cima=4
		i=5 to 5
		if 1 > 5 no
cima=4


Supongamos a[3, 4, 2]
cima=3, n=3
if 3>=2 si
	for i=2 to 3
		if 4 > 3 si => cima=4
		i=3 to 3
		if 2 > 4 no
cima=4
```

```pascal
c) Lo mismo que el item b) pero usando busqueda binaria:
{Pre: 1 ≤ lft ≤ n+1 ∧ 0 ≤ rgt ≤ n ∧ a ordenado}
fun binary_search_rec (a: array[1..n] of T, x:T, lft, rgt : nat) ret i:nat
	var mid: nat
	if lft > rgt → i = 0
		lft ≤ rgt → mid:= (lft+rgt) ÷ 2
		if  x < a[mid] → i:= binary_search_rec(a, x, lft, mid-1)
			x = a[mid] → i:= mid
			x > a[mid] → i:= binary_search_rec(a, x, mid+1,rgt)
		fi
	fi
end fun
{Post: (i = 0 ⇒ x no está en a[lft,rgt]) ∧ (i ̸= 0 ⇒ x = a[i])}

Hay un caso base cuando lft < rgt el cual devuelve que no existe.
Notar que vamos a tener que tener que realizar algunos chequeos para determinar en que parte de la lista esta la cima
```

<pre><code>
<b>fun</b> tiene_cima_binaria(<b>in</b> a:<b>array [1..N]</b> of <b>T</b>, <b>in</b> lft, rgt: <b>nat</b>, <b>ret</b> cima: <b>nat</b>)
	<b>var</b> mid: <b>nat</b>
	<b>if</b> lft &gt; rgt <b>then</b>
		cima := 0
	<b>else</b>
		mid := (lft + rgt)/2
		<b>if</b> a[mid-1] &lt; a[mid] <b>^</b> a[mid] &gt; a[mid+1] <b>then</b>
				cima := a[mid]
		<b>else if</b> a[mid-1] &gt; a[mid] <b>^</b> a[mid-1] &gt; a[mid+1] <b>then</b>
			tiene_cima_binaria(a, lft, mid-1)
		<b>else if</b> a[mid+1] &gt; a[mid] <b>^</b> a[mid+1] &gt; a[mid-1] <b>then</b>
			tiene_cima_binaria(a, mid+1, rgt)
		<b>fi fi fi</b>
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Supongamos a[1, 2, 3, 4, 1]
lft=1, rgt=5
if 1 > 5 no
	else
	mid = 3
	if 2 < 3 && 3 > 4 no
	if 2 > 3 && 2 > 4 no
	if 4 > 3 && 4 > 2 si => tiene_cima_binaria(a, 4, 5)

lft=4, rgt=5
if 4 > 5 no
	else
	mid=4
	if 3 < 4 && 4 > 1 si => cima=a[4]=4


Supongamos a[3, 4, 2]
lft=1, rgt=3
if 1 > 3 no
	else
	mid=2
	if 3 < 4 && 4 > 2 si => cima=a[2]=4
```

```pascal
d)fun tiene_cima
Vemos que no es DyV ya que no es recursiva. Analicemos las funciones iterativas:
Hay dos while i < n por lo tanto es de orden n

fun tiene_cima_secuencial
Vemos que tampoco es DyV. Analicemos las funciones iterativas:
for i=2 to n => (n-1) => es de orden n

fun tiene_cima_binaria
Veamos que esta si es DyV porque hay dos casos en que se invoca a si misma con un orden menor
Analicemos las funciones y los casos:

t(n) = | 0 si lft > rgt
	   | ?

No tengo ninguna operacion iterativa
Vemos que dividimos en 3 la lista, cuando la cima esta en el lado izq, cuando esta en lado derecho y cuando efectivamente encontramos la cima

a=0, b=3, k=0 => 0 = 3⁰ = 0 => n⁰ log n
```

![ScreenShot](Imagenes%20practico%201.3/ej3.png)

```pascal
Primero vemos que es de tipo DyV porque se llama a si misma con un orden menor.
¿Que hace? Retorna el elemento minimo del arreglo
¿Como lo hace? Hace uso de dos variables las cuales serviran como extremos, si son iguales => trivial, el minimo es el unico elemento
Caso contrario hace uso de un mid y busca el minimo entre las dos mitades resultantes de dividir el arreglo
Analicemos las operaciones:

if (i=k) then m=a[i]
Por lo tanto vemos que es irrelevante

else
	j=(i+k)/2
	Irrelevante tambien

	m=min(minimo(a, i, j), minimo(a, j+1, k))
	Esta es relevante
	a=2
	b=2
	k=0
	=> 2 > 2⁰=> 2 > 1 => orden n^(log₂2)
```

![ScreenShot](Imagenes%20practico%201.3/ej4.png)
```pascal
a) (n log 2^n) ⊏ (2^n) ⊏ (2^n log n) ⊏ (n! log n)

b) (2^4 log n) ⊏ (n⁴ + 2 log n) ⊏ (n³ log n) ⊏ (4^n)

c) (log n!) ⊏ (log n^n) ⊏ (n log n)
```

![ScreenShot](Imagenes%20practico%201.3/ej5.png)

```pascal
Vemos que es DyV porque se invoca a si mismo con un orden menor.
Vemos que toma dos constantes: K la cual nos indica la cantidad de divisiones que haremos. L indica en cuantas partes hacemos esas divisiones sobre el arreglo
¿Que hace? Algo relacionado a O grande
¿Como lo hace? Si el arreglo tiene un elemento o menos no hace nada. Caso contrario primero itera desde el primer elemento hasta el K dividiendo en la cantidad de veces indicada por L
Luego itera desde 1 hasta n⁴ haciendo la operacion O grande

Analicemos las operaciones:
if n<=1 then skip
Irrelevante

for i=1 to K do f(n div L)
a=K
b=L
k=?


for i=1 to n⁴ do operacion_de_O(1)
No es recursiva, la puedo analizar normalmente
=> orden n⁴

Entonces tenemos:
t(n) | 0 si n<1
	 | K*t(n/L) + n⁴
=> a=K, b=L, k=4

=> K=L⁴

Entonces:

a) n⁴ log n -> es el caso a=b^k
Propongo K=1, L=1 => 1=1⁴
Luego n^k log n => n⁴ log n

b) n⁴ -> es el caso a < b^k
Propongo K=0, L=1 => 0 < 1⁴
Luego n^k => n⁴

c) n⁵ -> es el caso a < b^k
Propongo K=1, L=1.5 => 1 < 1.5⁴ => 1 < 5
Luego n^k => n⁵
```

![ScreenShot](Imagenes%20practico%201.3/ej6_a.png)
![ScreenShot](Imagenes%20practico%201.3/ej6_b.png)

```pascal
a) n² + 2 log n => 2 log n + n^2
Entonces tenemos una funcion recursiva la cual a=b^k y tenemos otra funcion la cual no es recursiva cuyo orden es n²
Entonoces notemos que el lenguaje no sabe multiplicaciones ni logaritmos por lo tanto no sabe lo que es un n² o un log n.
No puedo hacer for i=1 to n²
Por lo que debo hacer primero una funcion de orden n² y luego otra de orden 2 log n (funciones distintas al mismo nivel porque hay un +)
Notemos que para el recursivo queremos que a=b^k entonces n^k log n = 2¹ log n por lo tanto debe ser tal que Σ1 to 2 recursivo => a=2, b=?, k=?
```

<pre><code>
<b>proc</b> 6_a(<b>in</b> n: <b>nat</b>)
	<b>var</b> k: <b>int</b>
	k := -10
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		<b>for</b> j := 1 <b>to</b> n <b>do</b>
			k := k +1
		<b>od</b>
	<b>od</b>

	<b>for</b> i := 1 <b>to</b> 2 <b>do</b>
		6_a(k div 2)
	<b>od</b>
<b>end proc</b>
</code></pre>

```pascal
El que hace no importa, analicemos las operaciones:
Primero tenemos 2 for, uno dentro del otro:
Ops(a) = Σ 1 to n (Σ 1 to n)
	   = Σ 1 to n (n)
	   = n * n
	   = n²

Luego analizamos el otro for, este es recursivo:
for i=1 to 2 do
	6_a(k div 2)

Entonces
t(n) = 2*(n/2) + n²
=> a=2, b=2, k=1
2 = 2¹ => 2=2 => 2¹ log n

Por lo tanto junto las dos cosas y queda
2 log n + n²



b) n² log n
Nuevamente estamos en el caso a=b^k => n^k log n
Especificamente queremos que n^k log n con k=2 => n² log n
Por lo tanto puedo hacer algo similar al caso anterior con la diferencia de que Σ 1 to n
```

<pre><code>
<b>fun</b> 6_b(<b>in</b> n:<b>nat</b>), <b>ret</b> res:<b>nat</b>
	<b>if</b> n &lt;= 1 <b>then</b>
		res := 1
	<b>else</b>
		<b>var</b> k: <b>int</b>
		k := -10
		<b>for</b> n <b>to</b> (n+3) <b>do</b> 
			<b>for</b> i := 1 <b>to</b> n <b>do</b>
				k := k + i
				<b>for</b> j := 1 <b>to</b> n <b>do</b>
					6_b(k div 2)
					k := k + 1
				<b>od</b>
			<b>od</b>
		<b>od</b>
	res := k
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Nuevamente no nos importa que hace.
Tenemos 2 casos:
t(n) | 1 si n<=1
	 | a *(n/b) + g(n)
Donde
a=4 (por el for n to n+3), b=2(por el div 2), k=2 (por que hay 2 for que iteran hasta n)

Entonces 4 = 2² => n^k log n => n² log n




c) 3^n
Entonces queremos que a<b^k
Especificamente que Σ 1 to 3
Entonces, partamos de la base del ej 6b
```

<pre><code>
<b>fun</b> 6_b(<b>in</b> n:<b>nat</b>), <b>ret</b> res:<b>nat</b>
	<b>if</b> n &lt;= 1 <b>then</b>
		res := 1
	<b>else</b>
		<b>var</b> k: <b>int</b>
		k := -10
		<b>for</b> 1 <b>to</b> 3 <b>do</b> 
			k := k + 1
			<b>for</b> n <b>to</b> n^n <b>do</b>
				6_b(k div 2)
			<b>od</b>
		<b>od</b>
	res := k
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Obviemos lo que hace, tenemos 2 casos:
t(n) = | 1 si n<=1
	   | a*(n/b) + g(n)
Donde
a=3 (porque Σ1 to 3), b=2 (por el div 2), k=n (porque no hay nada)
=> 3 < 2^n => n^k => 3^n (si al menos n>=2 tiene este orden)
```

![ScreenShot](Imagenes%20practico%201.3/ej7.png)

```pascal
El orden ciclico toma un arreglo y un natural i el cual indica en que parte se corta el arreglo. Si el arreglo esta ordenado desde a[i] hasta a[n] y luego desde a[1] hasta a[i-1] decimos que es ciclico
En el ejemplo [5, 6, 7, 8, 9, 1, 2, 3, 4] con i=6 vemos que desde a[i] (o sea a[1]) hasta a[n] esta ordenado (o sea 1, 2, 3, 4) Luego vemos desde a[1] hasta a[i-1] (o sea 5, 6, 7, 8, 9). Si tambien esta ordenado entonces es ciclico

Otro ejemplo seria [3,4,5,6,1,2] con i=5 ya que [1, 2] esta ordenado y luego [3, 4, 5, 6] tambien.
Por lo tanto podemos afirmar que i es el menor elemento del arrego y de la secuencia

Como la consigna luego pide buscar el minimo elemento (i) asumo que para el a) no es necesario y que se pasa como argumento:
```

<pre><code>
<b>fun</b> orden_ciclico(<b>in</b> a:<b>array[1..N]</b> of <b>T</b>, <b>in</b> i:<b>nat</b>), <b>ret</b> res:<b>bool</b>
	<b>var</b> j:<b>nat</b>
	j := i
	res := true
	<b>while</b> i &lt; n <b>do</b>
		<b>if</b> a[i] &gt;= a[i+1] <b>then</b>
			res := false
		<b>fi</b>
		i := i + 1
	<b>od</b>

	<b>if</b> res != false <b>then</b>
		<b>for</b> k:=1 <b>to</b> j-2 <b>do</b>
			<b>if</b> a[k] &gt;= a[k+1] <b>then</b>
				res := false
			<b>fi</b>
		<b>od</b>
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Veamos con el ejemplo de antes: a[3,4,5,6,1,2], i=5
j=5, res=true
5 < 6 si =>
	a[5] >= a[6] no
	i=6
6 < 6 no

res != false si
	1 <= 3 si
		a[1] >= a[2] no
	2 <= 3 si
		a[2] >= a[3] no
	3 <= 3 si
		a[3] >= a[4] no
	4 <= 3 no

res=true



b) Ahora nos pide que hagamos un algoritmo que busque el menor elemento (indice i) en un arreglo que nos pasan que ya es ciclico. Tenemos que usar busqueda secuencial, o sea posicion a posicion comparando con en el elemento que buscamos (o sea el menor) y devolvemos en indice en caso que lo encontremos
Entonces la idea es basicamente encontrar el menor elemento y listo, puedo tomar el primero como menor y utilizarlo secuencialmente para comparar.
```

<pre><code>
<b>fun</b> devuelvo_menor(<b>in</b> a:<b>array[1..N]</b> of <b>T</b>), <b>ret</b> res:<b>nat</b>
	<b>var</b> min, pos:<b>int</b>
	min := a[1]
	
	<b>if</b> n &lt; 2 <b>then</b>
		pos := 1
	<b>else</b>
		<b>for</b> i:=2 <b>to</b> n <b>do</b>
			<b>if</b> a[i] &lt; min <b>then</b>
				min := a[i]
				pos := i
			<b>fi</b>
		<b>od</b>
	<b>fi</b>
	res := pos
<b>end fun</b>
</code></pre>

```pascal
a[3,4,5,6,1,2]
min=3
if 6 < 2 no
else
	1 < 6 si
		4 < 3 no
	2 < 6 si
		5 < 3 no
	3 < 6 si
		6 < 3 no
	4 < 6 si
		1 < 3 si => min=1, pos=1
	5 < 6 si
		2 < 1 no
	
	pos = res = 1



c) Nos pide hacer lo mismo pero con busqueda binaria
```

<pre><code>
<b>proc</b> inicializo_min(<b>in</b> a:<b>array[1..N]</b> of <b>T</b>, <b>out</b> min:<b>int</b>)
	min := a[1]
	devuelvo_menor_binaria(a, min, 1, N)
<b>end proc</b>



<b>fun</b> devuelvo_menor_binaria(<b>in</b> a:<b>array[1..N]</b> of <b>T</b>, <b>in</b> min:<b>int</b>, <b>in</b> lft, rgt:<b>nat</b>), <b>ret</b> res:<b>nat</b>
	lft := 1
	rgt := n
	<b>if</b> lft &lt;= rgt <b>then</b>
		res := lft
	<b>else</b>
		mid := (lft + rgt) / 2
			<b>if</b> a[mid] &lt; min <b>then</b>
				min := a[mid]
				devuelvo_menor_binaria(a, min, lft, mid-1)
			<b>else</b> a[mid] &gt; min <b>then</b>
				devuelvo_menor_binaria(a, min, mid+1 rgt)
			<b>else</b>
				res := mid
			<b>fi</b>
	<b>fi</b>
<b>end fun</b>
</code></pre>

```pascal
Veamos con a[3,4,5,6,1,2]
inicializo_min:
	min = a[1] = 3
	devuelvo_menor_binaria(a, 3, 1, 6)

 devuelvo_menor_binaria:
 lft=1, rgt=6
 if 1 <= 6 no
else
	mid = (1+6)/2=3
	if 5 < 3 no
	if 5 > 3 si
		devuelvo_menor(a, 3, 4, 6)

lft=4, rgt=6
if 4 >= 6 no
else
	mid = (4+6)/2=5
	if 1 < 3 si
		min = 1
		devuelvo_menor_binaria(a, 1, 4, 4)

lft=4, rgt=4
if 4 >= 4 si
	res = min = 5
=> a[5] = 1 que es el min




Veamos con [5, 6, 7, 8, 9, 1, 2, 3, 4] que debe dar min=6
inicializo_min:
min:=a[1]=5
devuelvo_menor_binaria(a, 5, 1, 9)

lft=1, rgt=9
if 1>=9 no
else
	mid = (1+9)/2=5
	if 9 < 5 no
	if 9 > 5 si
		devuelvo_menor_binaria(a, 5, 6, 9)

lft=6, rgt=9
if 6 >= 9 no
else
	mid = (6+9)/2=7
	if 2 < 5 si
		min=2
		devuelvo_menor_binaria(a, 2, 6, 6)

lft=6, rgt=6
if 6 >= 6 si
	res = 6 => a[6]=1



d)Nos pide calcular el orden de complejidad:
Para orden_ciclico vemos que no es DyV porque no se llama a si misma.
Por lo tanto lo puedo hacer de la manera normal:

ops(7.a) = Σi to n (Σ1 to i-2)
		 = Σi to n (i-2)
		 = i-1
Entonces tiene orden n (n-1 donde el -1 es despreciable)



devuelvo_menor:
Vemos que tampoco es DyV, calculo normalmente:
ops(7.b) = Σ2 to n
		 = n-1
Entonces tiene orden n



devuelvo_menor_binaria:
Vemos que si es DyV:
t(n) = | lft si lft <= rgt
	   | a*t(n/b) + g(n)
a=1 (nro de llamadas recursivas)
b=3 (por la cantidad de casos en que divido)
k=0 (no hay ningun orden)

=> 1 =3⁰ => n^k log n => n⁰ log n => log n
```

![ScreenShot](Imagenes%20practico%201.3/ej8.png)

```pascal
Es DyV porque se llama a si misma con un orden menor

t(n) = | skip si n<=1
	   | a *t(n/b) + g(n) 

a=
```