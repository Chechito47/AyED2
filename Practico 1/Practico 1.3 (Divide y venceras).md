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
a) (2^n) ⊏ (n log 2^n) ⊏ (2^n log n) ⊏ (n! log n)

b) (2^4 log n) ⊏ (n⁴ + 2 log n) ⊏ (n³ log n) ⊏ (4^n)

c) (n log n) ⊏ (log n^n) ⊏ (log n!)
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

a) n⁴ log n -> es el caso
```