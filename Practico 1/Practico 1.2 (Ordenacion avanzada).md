![ScreenShot](Imagenes%20practico%201.2/ej1.png)

![ScreenShot](Imagenes%20practico%201.1/ej4.png)

<pre><code><span style="color:red;">a.1) [7, 1, 10, 3, 4, 9, 5]</span>
<i>Como tiene 7 elementos => grupo de 4 y otro de 3. Trabajo con el primer grupo</i> 
[<mark>7, 1, 10, 3</mark>, <mark>4, 9, 5</mark>]
<i>Vuelvo a dividir porque no esta ordenado</i>
[<mark>7, 1</mark>, <mark>10, 3</mark>, <mark>4, 9, 5</mark>]
<i>Divido de nuevo</i>
[<mark>7</mark>, <mark>1</mark>, <mark>10</mark>, <mark>3</mark>, <mark>4, 9, 5</mark>]
<i>Intercalo entre divisiones</i>
[<mark>1, 7</mark>, <mark>3, 10</mark>, <mark>4, 9, 5</mark>]
[<mark>1, 3, 7, 10</mark>, <mark>4, 9, 5</mark>]
<i>Sigo con la parte derecha</i>
[<mark>1, 3, 7, 10</mark>, <mark>4, 9</mark>, <mark>5</mark>]
[<mark>1, 3, 7, 10</mark>, <mark>4</mark>, <mark>9</mark>, <mark>5</mark>]
[<mark>1, 3, 7, 10</mark>, <mark>4, 9</mark>, <mark>5</mark>]
[<mark>1, 3, 7, 10</mark>, <mark>4, 5, 9</mark>]
<i>Intercalo finalmente</i>
[<mark>1, 3, 4, 5, 7, 9</mark>]


<span style="color:red;">a.2) [5, 4, 3, 2, 1]</span>
<i>Comienzo dividiendo</i>
[<mark>5, 4, 3</mark>, <mark>2, 1</mark>]
<i>Divido parte izquierda</i>
[<mark>5, 4</mark>, <mark>3</mark>, <mark>2, 1</mark>]
[<mark>5</mark>, <mark>4</mark>, <mark>3</mark>, <mark>2, 1</mark>]
<i>Intercalo parte izquierda</i>
[<mark>4, 5</mark>, <mark>3</mark>, <mark>2, 1</mark>]
[<mark>3, 4, 5</mark>, <mark>2, 1</mark>]
<i>Divido parte derecha</i>
[<mark>3, 4, 5</mark>, <mark>2</mark>, <mark>1</mark>]
<i>Intercalo parte derecha</i>
[<mark>3, 4, 5</mark>, <mark>1, 2</mark>]
<i>Intercalo ambas partes</i>
[<mark>1, 2, 3, 4, 5</mark>]

<span style="color:red;">a.3) [1, 2, 3, 4, 5]</span>
<i>Comienzo dividiendo</i>
[<mark>1, 2, 3</mark>, <mark>4, 5</mark>]
<i>Divido parte izquierda</i>
[<mark>1, 2</mark>, <mark>3</mark>, <mark>4, 5</mark>]
[<mark>1</mark>, <mark>2</mark>, <mark>3</mark>, <mark>4, 5</mark>]
<i>Intercalo parte izquierda</i>
[<mark>1, 2</mark>, <mark>3</mark>, <mark>4, 5</mark>]
[<mark>1, 2, 3</mark>, <mark>4, 5</mark>]
<i>Divido la parte derecha</i>
[<mark>1, 2, 3</mark>, <mark>4</mark>, <mark>5</mark>]
<i>Intercalo parte derecha</i>
[<mark>1, 2, 3</mark>, <mark>4, 5</mark>]
<i>Intercalo ambas partes</i>
[<mark>1, 2, 3, 4, 5</mark>]
</code></pre>

```pascal
b) Recordemos como es merge_sort_rec:
{Pre: n ≥ rgt ≥ lft > 0 ∧ a = A}
proc merge_sort_rec (in/out a: array[1..n] of T, in lft,rgt: nat)
	var mid: nat
	if rgt > lft → mid:= (rgt+lft) ÷ 2
		merge_sort_rec(a,lft,mid)
		{a[lft,mid] permutación ordenada de A[lft,mid]}
		merge_sort_rec(a,mid+1,rgt)
		{a[mid+1,rgt] permutación ordenada de A[mid+1,rgt]}
		merge(a,lft,mid,rgt)
		{a[lft,rgt] permutación ordenada de A[lft,rgt]}
	fi
end proc
{Post: a permutación de A ∧ a[lft,rgt] permutación ordenada de A[lft,rg]}

Tenemos que dar las llamadas a este procedimiento con los items del inciso a.1
a.1 = [7, 1, 10, 3, 4, 9, 5]
lft = 1
rgt = 7

(Voy haciendo la ejecucion por niveles, mientras mas tabs mas dentro estoy)
if(7 > 1) si => mid = (7+1)/2 = 4
merge_sort_rec(a, 1, 4)
	Dentro de esta llamada:
	lft=1, rgt=4
	if(4>1) si => mid=2
	merge_sort_rec(a, 1, 2) //Primer merge_sort_rec
		lft=1, rgt=2
		if(2>1) si => mid=1
		merge_sort_rec(a, 1, 2)
			lft=1, rgt=1
			if(1>1) no termino
		merge_sort_rec(a, 2, 2)
			lft=2, rgt=2
			if(2>2) no termino
		merge(a, 1, 1, 2)
		[1, 7][10, 3, 4, 9, 5] //ASI QUEDA
	merge_sort_rec(a, 3, 4) //Segundo merge_sort_rec
		lft=3, rgt=4
		if(4>3) si => mid=3
		merge_sort_rec(a, 3, 3)
			if(3>3) no termino
		merge_sort(a, 4, 4)
			if(4>4) no termino
		merge_(a, 3, 3, 4)
		[1, 7][3, 10][4, 9, 5] //ASI QUEDA
	merge(a, 1, 2, 4)
	[1, 3, 7, 10][4, 9, 5] //ASI VAMOS HASTA AHORA
merge_sort_rec(a, 5, 7)
	lft=5, rgt=7
	if(7>5) si => mid=5
	merge_sort_rec(a, 5, 5)
		if(5>5) no termino
	merge_sort_rec(a, 6, 7)
		lft=6, rgt=7
		if(7>6) si => mid=6
		merge_sort_rec(a, 6, 6)
			if(6>6) no termino
		merge_sort_rec(a, 7, 7)
			if(7>7) no termino
		merge(a, 6, 6, 7)
		[1, 3, 7, 10][4][5, 9] //No se porque quedan estos 2 y no los otros 2
	merge(a, 5, 5, 7)
	[1, 3, 7, 10][4, 5, 9]
merge(a, 1, 4, 7)
[1, 3, 4, 5, 7, 9, 10]
LISTO

Entonces las llamdas a merge_sort_rec son:
merge_sort_rec(a, 1, 4)
merge_sort_rec(a, 1, 2)
merge_sort_rec(a, 1, 2)
merge_sort_rec(a, 2, 2)
merge_sort_rec(a, 3, 4)
merge_sort_rec(a, 3, 3)
merge_sort_rec(a, 4, 4)
merge_sort_rec(a, 5, 7)
merge_sort_rec(a, 5, 5)
merge_sort_rec(a, 6, 7)
merge_sort_rec(a, 6, 6)
merge_sort_rec(a, 7, 7)
```

![ScreenShot](Imagenes%20practico%201.2/ej2.png)

```pascal
Recibe un array[1..2^n] y un natural.
Intercala el segmento a[1, 2^i] con a[2^i+1, 2*2^i]
Intercala el segmento a[2*2^i+1, 3*2^i] con a[3*2^i+1, 4*2^i]
etc...

Asumimos que cada uno de esos segmento esta ordenado.

Ejemplo: llamos al procedimiento con i=1.
array=[3, 7, 1, 6, 1, 5, 3, 4]
	   1  2  3  4  5  6  7  8
Hagamos la secuencia paso por paso
Intercalo a[1, 2] con a[3, 4] (o sea ordeno)
[1, 3, 6, 7, 1, 5, 3, 4]
Ahora intercalo a[5, 6] con a[7, 8]
[1, 3, 6, 7, 1, 3, 4, 5]

Ahora llamemos al procedimiento de nuevo pero con i=2
array=[1, 3, 6, 7, 1, 3, 4, 5]
Intercalo a[1, 4] con [5, 8] (O sea intercalo todo, xq el a[1, 4] es SEGMENTO, incluye a a[2] y a[3])
[1, 1, 3, 3, 4, 5, 6, 7]
Intercalo a[9, 12] con a[13, 16]
No puedo, mi arreglo no tan grande. Termino

Y ya tengo el arreglo ordenado. 
Ahora hagamos la implementacion
```

<pre><code>
<b>proc</b> intercalar_cada(<b>in/out</b> a:<b>array[1..2^n] of T</b>, <b>in</b> i:<b>nat</b>)
	<b>var</b> lft, rgt, size: <b>nat</b>
	lft := 1
	rgt := 2^i
	size := n
	mid := (lft + rgt) / 2
	<b>while</b> rgt &lt;= size <b>do</b>
		merge(a, lft, mid, rgt)
		lft = rgt + 1
		rgt = rgt + 2^i
		mid = (lft + rgt) / 2
	<b>od</b>
	lft := 1
	mid := (lft + rgt) / 2
	merge(a, lft, mid, rgt)
<b>end proc</b></code></pre>

```pascal
Notemos que lft y rgt estan entre [1, 2^i] y luego entre [2^i+1, 2*2^i].
Luego entre [2*2^i+1, 3*2^i] y luego entre [3*2^i+1, 4*2^i]

Por ejemplo, llamemos al procedimiento con i=1
array=[3, 7, 1, 6, 1, 5, 3, 4]
lft=1, rgt=2, size=8, mid=1
2 <= size si => merge (a, 1, 1, 2) => [3, 7][1, 6, 1, 5, 3, 4]
			   lft=3
			   rgt=4
			   mid=3
4 <= size si => merge (a, 3, 3, 4) => [3, 7][1, 6][1, 5, 3, 4]
			   lft=5
			   rgt = 6
			   mid=5
6 <= size si => merge (a, 5, 5, 6) => [3, 7][1, 6][1, 5][3, 4]
			   lft=7
			   rgt=8
			   mid=7
8 <= size si => merge(a, 7, 7, 8) => [3, 7][1, 6][1, 5][3, 4]

Ahora termine dentro del ciclo, salgo
lft=1, rgt=rgt=8, mid=4
merge(a, 1, 4, 8) => [1, 1, 3, 3, 4, 5, 6, 7] y LISTO


b)
```

<pre><code>
<b>proc</b> merge_sort_cada (<b>in/out</b>a: <b>array</b>[1..N] of <b>T</b>, <b>in</b> lft, rgt: <b>nat</b>)
<b>var</b> i: <b>nat</b>
i := 0
<b>do</b> rgt &gt; lft && i &lt;= n<b>then</b>
	intercalar_cada(a, i)
	i := i +1
<b>fi</b>
<b>end proc</b></code></proc>

![ScreenShot](Imagenes%20practico%201.2/ej3.png)
![ScreenShot](Imagenes%20practico%201.1/ej4.png)

```pascal
a.1) [7, 1, 10, 3, 4, 9, 5]
Ordenemos haciendo los llamados a la funcion quick_sort_rec

proc quick_sort(in/out a: array[1..n] of T)
	quick_sort_rec(a, 1, n)
end proc

proc quick_sort_rec(in/out a: array[1..n] of T, in lft, rgt: nat)
	var ppiv: nat
	if rgt > lft then
		partition (a, lft, rgt, ppiv)
		quick_sort_rec(a, lft, ppiv-1)
		quick_sort_rec(a, ppiv+1, rgt)
	fi
end proc

proc partition (in/out a: array[1..n] of T, in lft, rgt: nat, out ppiv: nat)
	var i, j: nat
	ppiv := lft
	i := lft + 1
	j := rgt
	do i <= j
		if a[i] <= a[ppiv] then i++
		   a[j] >= a[ppiv] then j--
		   a[i] > a[ppiv] ^ a[j] < a[ppiv] then swap(a, i, j)
		fi
	od
	swap(a, ppiv, j)
	ppiv := j
end proc

[7, 1, 10, 3, 4, 9, 5]
EJECUTEMOS LINEA POR LINEA
lft=1, rgt=7
quick_sort_rec(a, 1, 7)
	if (1 < 7) true =>
		partition(a, 1, 7, ppiv)
			ppiv=1, i=2, j=7
			do 2 <= 7 si
				if a[2] <= a[1] true i++
			do 3 <= 7 si
				if a[3] <= a[1] false
				if a[7] >= a[1] false
				swap(a, 3, 7)
				[7, 1, 5, 3, 4, 9, 10]
			do 3 <= 7 si
				if a[3] <= a[1] true i++
			do 4 <= 7 si
				if a[4] <= a[1] true i++
			do 5 <=7 si
				if a[5] <= a[1] true i++
			do 6 <= 7 si
				if a[6] <= a[1] false
				if a[7] >= a[1] true j--
			do 6 <= 6 si
				if a[6] <= a[1] false
				if a[6] >= a[1] true j--
			do 6 <= 5 no Termino ciclo
			swap(a, 1, 5)
			[4, 1, 5, 3, 7, 9, 10] DONDE EL 7 YA ESTA EN SU POSICION FINAL
			ppiv = 5
		quick_sort_rec(a, 1, 5-1)
			lft=1, rgt=4
			if 4 > 1 si
				partition(a, 1, 4, ppiv)
					ppiv=1, i=2, j=4
					do 2 <= 4 si
						if a[2] <=a[1] true i++
					do 3 <= 4 si
						if a[3] <= a[1] false
						if a[4] >= a[1] false
						swap(a, 3, 4)
						[4, 1, 3, 5, 7, 9, 10]
					do 3 <= 4 si
						if a[3] <= a[1] true i++
					do 4 <= 4 si
						if a[4] <= a[1] false
						if a[4] <= a[1] true j--
					do 4 <= 3 no SALGO DEL CICLO
					swap(a, 1, 3)
					[3, 1, 4, 5, 7, 9, 10] EL 4 Y 7 ESTAN EN SUS POS FINALES
					ppiv = 3
				quick_sort_rec(a, 1, 3-1)
					if 2 > 1 si
					partition(a, 1, 2, ppiv)
						ppiv=1, i=2, j=2
						do 2<=2 si
							if a[2] <= a[1] true i++
						do 3 <= 2 no SALFO DEL CICLO
						swap(a, 1, 2)
						[1, 3, 4, 5, 7, 9, 10] EL 1, 4 Y 7 EN POS FINALES
						ppiv = 2
					quick_sort_rec(a, 1, 2-1)
						if 1 > 1 no ENTONCES NO HAGO NADA
					quick_sort_rec(a, 2+1, 1)
						if 1 > 3 no ENTONCES NO HAGO NADA
				quick_sort_rec(a, 3+1, 4)
					if 4 > 4 no ENTONCES NO HAGO NADA
		quick_sort_rec(a, 5+1, 7) XQ USO EL PPIV DEL MISMO NIVEL DE IDENTACION
			if 7 > 6 si
				partition(a, 6, 7, ppiv)
					ppiv=6, i=7, j=7
					do 7 >= 7 si
						a[7] <= a[6] false
						a[7] >= a[7] true j--
					do 7 >= 6 no SALGO DEL CICLO
					swap(a, 6, 6)
					[1, 3, 4, 5, 7, 9, 10] NO CAMBIO NADA
					ppiv=6
				quick_sort_rec(a, 6, 6-1)
					if 5 > 6 no ENTONCES NO HAGO NADA
				quick_sort_rec(a, 6+1, 6)
					if 6 > 7 no ENTONCES NO HAGO NADA
LISTO TERMINE, LISTA ORDENADA: [1, 3, 4, 5, 7, 9, 10]


Entonces la secuencia de llamadas al procedimiento quick_sort_rec es la siguiente:
[7, 1, 10, 3, 4, 9, 5]
quick_sort_rec(a, 1, 7)
	partition(a, 1, 7, ppiv)
		[4, 1, 5, 3, 7, 9, 10]
	quick_sort_rec(a, 1, 4)
		partition(a, 1, 4, ppiv)
			[3, 1, 4, 5, 7, 9, 10]
		quick_sort_rec(a, 1, 2)
			partition(a, 1, 2, ppiv)
				[1, 3, 4, 5, 7, 9, 10]
			quick_sort_rec(a, 1, 1) NO HACE NADA
			quick_sort_rec(a, 3, 1) NO HACE NADA
		quick_sort_rec(a, 4, 4) NO HACE NADA
	quick_sort_rec(a, 6, 7)
		partition(a, 6, 7, ppiv)
			[1, 3, 4, 5, 7, 9, 10]
		quick_sort_rec(a, 6, 5) NO HACE NADA
		quick_sort_rec(a, 7, 6) NO HACE NADA
TERMINE
```




<pre><code><span style="background-color:red;">a.1) [7, 1, 10, 3, 4, 9, 5]</span>
<i>Elijo el pivot(7)</i>
[<u>7</u>, 1, 10, 3, 4, 9, 5]
<i>Comparo: 1&lt;7 si, 10&lt;7 no, 5&gt;7 no</i>
<i>Hago swap entre 5 y 10</i>
[<u>7</u>, 1, 5, 3, 4, 9, 10]
<i>Sigo comparando: 1&lt;7 si, 5&lt;7 si, 3&lt;7 si, 4&lt;7 si, 9&lt;7 no, 10&gt;7 si, 9&gt;7 si</i>
<i>No hago swap, solo ubico el pivot</i>
[4, 1, 5, 3, <mark>7</mark>, 9 10]

<i>Elijo el nuevo pivot(4)</i>
[<u>4</u>, 1, 5, 3, <mark>7</mark>, 9, 10]
<i>Comparo: 1&lt;4 si, 5&lt;4 no, 3&gt;4 no</i>
<i>Hago swap entre 5 y 3</i>
[<u>4</u>, 1, 3, 5, <mark>7</mark>, 9, 10]
<i>Sigo comparando: 1&lt;4 si, 3&lt;4 si, 5&lt;4 no, 5&gt;4 si</i>
<i>No hago swap, solo ubico el pivot</i>
[3, 1, <mark>4</mark>, 5, <mark>7</mark>, 9, 10]

<I>Elijo el nuevo pivot(3)</i>
[<u>3</u>, 1, <mark>4</mark>, 5, <mark>7</mark>, 9, 10]
<i>Comparo: 1&lt;3 si</i>
<i>No hago swap, solo ubico el pivot</i>
[1, <mark>3, 4</mark>, 5, <mark>7</mark>, 9, 10]

<i>Elijo el nuevo pivot(1)</i>
[<u>1</u>, <mark>3, 4</mark>, 5, <mark>7</mark>, 9, 10]
<i>Lista de un solo elemento, trivial</i>
[<mark>1, 3, 4</mark>, 5, <mark>7</mark>, 9, 10]

<i>Elijo el nuevo pivot(5)</i>
[<mark>1, 3, 4</mark>, <u>5</u>, <mark>7</mark>, 9, 10]
<i>Lista de un solo elemento, trivial</i>
[<mark>1, 3, 4, 5, 7</mark>, 9, 10]

<i>Elijo el nuevo pivot(9)</i>
[<mark>1, 3, 4, 5, 7</mark>, <u>9</u>, 10]
<i>Comparo: 10&lt;9 no, 10&gt;9 si</i>
<i>No hago swap, solo ubico el pivot</i>
[<mark>1, 3, 4, 5, 7, 9</mark>, 10]

<i>Elijo el nuevo pivot(10)</i>
[<mark>1, 3, 4, 5, 7, 9</mark>, <u>10</u>]
<i>Lista de un solo elemento, trivial</i>
[<mark>1, 3, 4, 5, 7, 9, 10</mark>] <b>ARREGLO ORDENADO</b>



<span style="background-color:red;">a.2) [5, 4, 3, 2, 1]</span>
<i>Elijo el pivot(5)</i>
[<u>5</u>, <b>4</b>, <b>3</b>, 2, 1]
<i>Comparo: 4&lt;5 si, 3&lt;5 si, 2&lt;5 si, 1&lt;5 si</i>
<i>No hago swap, solo posiciono el pivot</i>
[1, 4, 3, 2, <mark>5</mark>]

<i>Elijo el nuevo pivot(1), el 5 ya esta en su posicion final</i>
[<u>1</u>, 4, 3, 2, <mark>5</mark>]
<i>Comparo: 4&lt;1 no, 2&gt;1 si, 3&gt;1 si, 4&gt;1 si</i>
<i>No hago swap, solo posiciono el pivot</i>
[<mark>1</mark>, 4, 3, 2, <mark>5</mark>]

<i>Elijo el nuevo pivot (4)</i>
[<mark>1</mark>, <u>4</u>, 3, 2, <mark>5</mark>]
<i>Comparo: 3&lt;4 si, 2&lt;4 si</i>
<i>No hago swap, solo posiciono el pivot</i>
[<mark>1</mark>, 2, 3, <mark>4, 5</mark>]

<i>Elijo el nuevo pivot(2)</i>
[<mark>1</mark>, <u>2</u>, 3, <mark>4, 5</mark>]
<i>Comparo: 3&lt;2 no</i>
<i>No hago swap, posiciono pivot</i>
[<mark>1, 2</mark>, 3, <mark>4, 5</mark>]

<i>Elijo el nuevo pivot(3)</i>
[<mark>1, 2</mark>, <u>3</u>, <mark>4, 5</mark>]
<i>No puedo comparar nada</i>
<i>Lista de un elemento, trivial</i>
[<mark>1, 2, 3, 4, 5</mark>] <b>ARREGLO ORDENADO</b>



<span style="background-color:red;">a.3) [1, 2, 3, 4, 5]</span>
<i>Elijo el pivot(1)</i>
[<u>1</u>, 2, 3, 4, 5]
<i>Comparo: 2&lt;1 no, 5&gt;1 si, 4&gt;1 si, 3&gt;1 si, 2&gt;1 si</i>
<i>No hago swap, solo ubico el pivot</i>
[<mark>1</mark>, 2, 3, 4, 5]

<i>Elijo el nuevo pivot(2)</i>
[<mark>1</mark>, <u>2</u>, 3, 4, 5]
<i>Comparo: 3&lt;2 no, 5&gt;2 si, 4&gt;2 si, 3&gt;2 si</i>
<i>No hago swap, solo ubico el pivot</i>
[<mark>1, 2</mark>, 3, 4, 5]

<i>Elijo el nuevo pivot(3)</i>
[<mark>1, 2</mark>, <u>3</u>, 4, 5]
<i>Comparo: 4&lt;3 no, 5&gt;3 si, 4&gt;3 si</i>
<i>No hago swap, solo ubico el pivot</i>
[<mark>1, 2, 3</mark>, 4, 5]

<i>Elijo el nuevo pivot(4)</i>
[<mark>1, 2, 3</mark>, <u>4</u>, 5]
<i>Comparo: 5&lt;4 no, 5&gt;4 si</i>
<i>No hago swap, solo ubico el pivot</i>
[<mark>1, 2, 3, 4</mark>, 5]

<i>Elijo el nuevo pivot(5)</i>
[<mark>1, 2, 3, 4</mark>, <u>5</u>]
<i>Lista de un elemento, trivial</i>
[<mark>1, 2, 3, 4, 5</mark>] <b>ARREGLO ORDENADO</b></code></pre>


![ScreenShot](Imagenes%20practico%201.2/ej4.png)

```pascal
Entonces, debemos dar el elemento que tenga el valor medio entre el primero, ultimo y el de en medio
ej: 4, 20, 10 => ppiv=10 xq 4 < 10 < 20
a[lft]=4, a[mid]=20, a[rgt]=10 => a[lft] < a[rgt] < a[mid] => ppiv=rgt

Si fuese: a[lft]=20, a[mid]=4, a[rgt]=10 => a[mid] < a[rgt] < a[lft] => ppiv=rgt

Entonces es necesario poder decir cual sera el elemento de en medio, para eso podemos usar la formula del medio del merge sort: mid=(izq+der)/2 que redondea para abajo.

Por lo tanto tengo 6 alternativas:
a[lft] < a[mid] < a[rgt] => ppiv=mid
a[rgt] < a[mid] < a[lft] => ppiv=mid

a[mid] < a[lft] < a[rgt] => ppiv=lft
a[rgt] < a[lft] < a[lft] => ppiv=lft

a[mid] < a[rgt] < a[lft] => ppiv=rgt
a[lft] < a[rgt] < a[mid] => ppiv=rgt
```

<pre><code>
<b>proc</b> partition (<b>in/out</b> a: <b>array[1..n] of T</b>, <b>in</b> lft, rgt: <b>nat</b>,
				<b>out</b> ppiv: <b>nat</b>)
	<b>var</b> mid, i, j: <b>nat</b>
	i := lft + 1
	j := rgt
	mid := (lft + rgt)/2
	<b>if</b> a[lft] &lt;= a[mid] ^ a[mid] &lt;= a[rgt] <b>then</b> ppiv := mid
	   a[rgt] &lt;= a[mid] ^ a[mid] &lt;= a[lft] <b>then</b> ppiv := mid
	   a[mid] &lt;= a[lft] ^ a[lft] &lt;= a[rgt] <b>then</b> ppiv := lft
	   a[rgt] &lt;= a[lft] ^ a[lft] &lt;= a[mid] <b>then</b> ppiv := lft
	   a[mid] &lt;= a[rgt] ^ a[rgt] &lt;= a[lft] <b>then</b> ppiv := rgt
	   a[lft] &lt;= a[rgt] ^ a[rgt] &lt;= a[mid] <b>then</b> ppiv := rgt
	<b>fi</b>
	<b>do</b> i &lt;= j
		<b>if</b> a[i] &lt;= a[ppiv] <b>then</b> i++
		   a[j] &gt;= a[ppiv] <b>then</b> j--
		   a[i] &gt; a[ppiv] ^ a[j] &lt; a[ppiv] <b>then</b> swap(a, i, j)
		<b>fi</b>
	<b>od</b>
	swap(a, ppiv, j)
	ppiv := j
<b>end proc</b>
</code></pre>

```pascal
Aunque *CREO* que tambien podriamos hacer uso de una funcion para devolver ppiv:
```

<pre><code>
<b>proc</b> partition (<b>in/out</b> a: <b>array[1..n] of T</b>, <b>in</b> lft, rgt: <b>nat</b>,
				<b>in/out</b> ppiv: <b>nat</b>)
	<b>var</b> mid, i, j: <b>nat</b>
	i := lft + 1
	j := rgt
	mid := (lft + rgt)/2
	ppiv := buscar_ppiv(a, lft, rgt, mid)
	<b>do</b> i &lt;= j
		<b>if</b> a[i] &lt;= a[ppiv] <b>then</b> i++
		   a[j] &gt;= a[ppiv] <b>then</b> j--
		   a[i] &gt; a[ppiv] ^ a[j] &lt; a[ppiv] <b>then</b> swap(a, i, j)
		<b>fi</b>
	<b>od</b>
	swap(a, ppiv, j)
	ppiv := j
<b>end proc</b>


<b>fun</b> buscar_ppiv(<b>in/out</b>a: <b>array[1..N] of T</b>, <b>in</b> lft, rgt, mid: <b>nat</b>,
				<b>ret</b> ppiv:<b>nat</b>)
	<b>if</b> a[lft] &lt;= a[mid] ^ a[mid] &lt;= a[rgt] <b>then</b> ppiv := mid
	   a[rgt] &lt;= a[mid] ^ a[mid] &lt;= a[lft] <b>then</b> ppiv := mid
	   a[mid] &lt;= a[lft] ^ a[lft] &lt;= a[rgt] <b>then</b> ppiv := lft
	   a[rgt] &lt;= a[lft] ^ a[lft] &lt;= a[mid] <b>then</b> ppiv := lft
	   a[mid] &lt;= a[rgt] ^ a[rgt] &lt;= a[lft] <b>then</b> ppiv := rgt
	   a[lft] &lt;= a[rgt] ^ a[rgt] &lt;= a[mid] <b>then</b> ppiv := rgt
	<b>fi</b>
<b>end fun</b>
</code></pre>

![ScreenShot](Imagenes%20practico%201.2/ej5.png)

```pascal
k es a la sumo n. Devuelve el elemento a[k] si estuviera ordenado.

Es un algoritmo conocido como quick select, es una version modificada del quick sort que sirve para encontrar el k-esimo elemento mas pequeño de un arreglo sin ordenarlo completamente (aunque se podria dar el caso que se termine ordenando indirectamente)
La idea es usar el partition, ya que elije un pivote y reordena el arreglo tal que a la izquierda queden los menores del pivot y a la derecha los mayores o iguales dejando al pivot en su posicion final.
Entonces si el pivot termina siendo el k listo. Si no:
	Si es menor buscamos en la parte derecha del arreglo
	Si es mayor buscamos en la parte izquierda del arreglo
```

<pre><code>
<b>fun</b> quick_select(<b>in</b> a: <b>array[1..N] of T</b>, k: <b>nat</b>, <b>ret</b> res: <b>int</b>)
	<b>var</b> lft, rgt, ppiv: <b>nat</b>
	lft := 1
	rgt := n
	ppiv := partition(a, lft, rgt, ppiv)
	<b>do</b> ppiv != k
		<b>if</b> ppiv &lt; k <b>then</b> lft := ppiv + 1
    	   ppiv > k <b>then</b> rgt := ppiv -1
		<b>fi</b>
	ppiv := partition(a, lft, rgt, ppiv)
	<b>od</b>
	res := a[k]
<b>end fun</b>
</code></pre>

```pascal
Supongamos a=[4, 1, 3, 2], k=1
lft=1, rgt=4
partition(a, 1, 4, ppiv)
	ppiv=1, i=2, rgt=4
	Elijo el pivot(4)
	Comparo: 1 < 4 si => i=3
			 3 < 4 si => i=4
			 2 < 4 si => i=5
			 (i<=j no se cumple)
	Ubico el pivot
	[2, 1, 3, 4], ppiv=4
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=3

lft=1, rgt=3
partition(a, 1, 3, ppiv=4)
	ppiv=1, i=2, rgt=3
	Elijo el pivot(2)
	Comparo: 1 < 2 si => i=3
			 3 < 2 no
			 (recorri toda la sublista)
	Ubico el pivot
	[1, 2, 3, 4], ppiv =3
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=2

lft=1, rgt=2
partition(a, 1, 2, ppiv)
	ppiv=1, i=2, j=2
	Elijo el pivot(1)
	Comparo: 2 < 1 no
			 2 > 1 si
			 (recorri toda la sublista)
	Ubico el pivot
	[1, 2, 3, 4], ppiv=2
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=1

lft=1, rgt=1
partition(a, 1, 1, ppiv)
	ppiv=1, i=2, j=1
	No puedo hacer comparaciones, solo ubico el pivot (i<=j no se cumple)
	[1, 2, 3, 4], ppiv=1
ppiv=k => LISTO, ENCONTRE EL K QUE BUSCABA




Otro ejemplo, a=[3, 5, 6, 1, 2], k=2
lft=1, rgt=5
partition(a, 1, 5, ppiv)
	ppiv=1, i=2, rgt=5
	Elijo el pivot(3)
	Comparo: 5 < 3 no
			 2 > 3 no
	Entonces hago swap(a, 5, 2)
	[3, 2, 6, 1, 5]
	Comparo: 2 < 3 si => i=3
			 6 < 3 no
			 5 > 3 si => j=4
			 1 > 3 no
	Entonces hago swap(a, 6, 1)
	[3, 2, 1, 6, 5]
	Comparo: 2 < 3 si => i=3
			 1 < 3 si => i=4
			 6 < 3 no
			 6 > 3 si => j=3
			 (i<=j no se cumple)
	Ubico el pivot
	[1, 2, 3, 6, 5], ppiv=3
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=2

lft=1, rgt=2
partition(a, 1, 2, ppiv)
	ppiv=1, i=2, j=2
	Elijo el pivot(1)
	Comparo: 2 < 1 no
			 2 > 1 si => j=1
			 (i<=j no se cumple)
	Ubico el pivot
	[1, 2, 3, 6, 5], ppiv=1
ppiv!=k => ppiv < k => lft=ppiv+1 => lft=2

lft=2, rgt=2
partition(a, 2, 2, ppiv)
	ppiv=2, i=3, j=2
	(i<=j no se cumple)
	No puedo comparar, ubico el pivot
	[1, 2, 3, 6, 5], ppiv=2
ppiv=k LISTO, ENCONTRE EL K QUE BUSCABA




Parece que es normal que a la segunda llamada de partition se encuentre el a[k] bien posicionado a pesar que ppiv!=k.  Veamos un ejemplo donde eso no sucede:

Supongamos a=[9, 8, 7, 6, 5, 4, 3, 2, 1], k=3
lft=1, rgt=9
partition(a, 1, 9, ppiv)
	ppiv=1, i=2, j=9
	Elijo el pivot(9)
	Comparo: 8 < 9 si => i=3
			 7 < 9 si => i=4
			 6 < 9 si => i=5
			 5 < 9 si => i=6
			 4 < 9 si => i=7
			 3 < 9 si => i=8
			 2 < 9 si => i=9
			 1 < 9 si => i=10
			 (i<=j falso)
	Ubico el pivot
	[1, 8, 7, 6, 5, 4, 3, 2, 9], ppiv=9
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=8

lft=1, rgt=8
partition(a, 1, 8, ppiv)
	ppiv=1, i=2, j=8
	Elijo el pivot(1)
	Comparo: 8 < 1 no
			 2 > 1 si => j=7
			 3 > 1 si => j=6
			 4 > 1 si => j=5
			 5 > 1 si => j=4
			 6 > 1 si => j=3
			 7 > 1 si => j=2
			 8 > 1 si => j=1
			 (i<=j false)
	Ubico el pivot
	[1, 8, 7, 6, 5, 4, 3, 2, 9], ppiv=1
ppiv!=k => ppiv < k => lft=ppiv+1 => lft=2

lft=2, rgt=8
partition(a, 2, 8, ppiv)
	ppiv=2, i=3, j=8
	Elijo el pivot(8)
	Comparo: 7 < 8 si => i=4
			 6 < 8 si => i=5
			 5 < 8 si => i=6
			 4 < 8 si => i=7
			 3 < 8 si => i=8
			 2 < 8 si => i=9
			 (i<=j false)
	Ubico el pivot
	[1, 2, 7, 6, 5, 4, 3, 8, 9], ppiv=8
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=7

lft=2, rgt=7
partition(a, 2, 7, ppiv)
	ppiv=2, i=3, j=7
	Elijo el pivot(2)
	Comparo: 7 < 2 no
			 3 > 2 si => j=6
			 4 > 2 si => j=5
			 5 > 2 si => j=4
			 6 > 2 si => j=3
			 7 > 2 si => j=2
			 (i<=j false)
	Ubico el pivot
	[1, 2, 7, 6, 5, 4, 3, 8, 9], ppiv=2
ppiv!=k => ppiv < k => lft=ppiv+1 => lft=3

lft=3, rgt=7
partition(a, 3, 7, ppiv)
	ppiv=3, i=4, j=7
	Elijo el pivot(7)
	Comparo: 6 < 7 si => i=5
			 5 < 7 si => i=6
			 4 < 7 si => i=7
			 3 < 7 si => i=8
			 (i<=j false)
	Ubico el pivot
	[1, 2, 3, 6, 5, 4, 7, 8, 9], ppiv=7
ppiv!=k => ppiv > k => rgt=ppiv-1 => rgt=6

lft=3, rgt=6
partition(a, 3, 6, ppiv)
	ppiv=3, i=4, j=6
	Elijo el pivot(3)
	Comparo: 6 < 3 no
			 4 > 3 si => j=5
			 5 > 3 si => j=4
			 6 > 3 si => j=3
			 (i<=j false)
	Ubico el pivot
	[1, 2, 3, 6, 5, 4, 7, 8, 9], ppiv=3
ppiv=k LISTO, ENCONTRE EL K QUE BUSCABA

Por lo tanto vemos que la implementacion funciona correctamente
```

![ScreenShot](Imagenes%20practico%201.2/ej6.png)

```pascal
Queremos que partition separe en 3 segmentos:
	menores que ppiv
	iguales que ppiv
	mayores que ppiv
Luego debemos devolver pivot_lft y pivot_rgt
lt -> limite izquierdo en el que iteramos
gt -> limite derecho en el que iteramos
```

<pre><code>
<b>proc</b> partition3 (<b>in/out</b> a: <b>array[1..n] of T</b>, <b>in</b> lft, rgt: <b>nat</b>,
				<b>in/out</b> ppiv_lft, ppiv_rgt: <b>nat</b>)
	<b>var</b> lt, i, gt: <b>nat</b>
	lt := lft
	i := lft + 1
	gt := rgt
	<b>do</b> i &lt;= gt
		<b>if</b> a[i] &lt; a[lft] <b>then</b>
				swap(a, lt, i)
				lt := lt + 1
				i := i +1
		   a[i] &gt; a[lft] <b>then</b>
				swap(a, i, gt)
				gt := gt -1
		   a[i] == a[lft] <b>then</b> i := i + 1
		<b>fi</b>
	<b>od</b>
	ppiv_lft := lt
	ppiv_rgt := gt
<b>end proc</b></code></pre>

```pascal
Supongamos que tenemos la lista a[2, 2, 5, 7]
lt=1, i=2, gt=4, a[lft]=2
do 2 <= 4
	if 2 < 2 no
	   2 > 2 no
	   2 == 2 si => i=3
do 3 <= 4
	if 5 < 2 no
	   5 > 2 si => swap(a, 3, 4) => [2, 2, 7, 5], gt=3
do 3 <= 3
	if 7 < 2 no
	   7 > 2 si => swap(a, 3, 3) => [2, 2, 7, 5], gt=2
do 3 <= 2 false

ppiv_lft = 1
ppiv_rgt=2



Veamos con otra lista: a[2, 1, 4, 4, 3, 5]
lt=1, i=2, gt=6, a[lft]=2
do 2 <= 6
	1 < 2 si => swap(a, 1, 2)=>[1, 2, 4, 4, 3, 5], lt=2, i=3
do 3 <= 6
	4 < 2 no
	4 > 2 si => swap(a, 3, 6)=>[1, 2, 5, 4, 3, 4], gt=5
do 3 <= 5
	5 < 2 no
	5 > 2 si => swap(a, 3, 5)=>[1, 2, 3, 4, 5, 4], gt=4
do 3 <= 4
	3 < 2 no
	3 > 2 si => swap(a, 3, 4) =>[1, 2, 4, 3, 5, 4], gt=3
do 3 <= 3
	4 < 2 no
	4 > 2 si => swap(a, 3, 3) =>[1, 2, 4, 3, 5, 4], gt=2
do 3 <= 2 no

ppiv_lft=2
ppiv_rgt=2

Parece que funciona
Veamos con el mismo ejemplo pero con varios elementos iguales al pivot
a[2, 1, 2, 4, 4, 3, 2, 5]
lt=1, i=2, gt=8, a[lft]=2
do 2 <= 8
	1 < 2 si =>swap(a, 1, 2)=>[1, 2, 2, 4, 4, 3, 2, 5], lt=2, i=3
do 3 <= 8
	2 < 2 no
	2 > 2 no
	2 == 2 si => i=4
do 4 <= 8
	4 < 2 no
	4 > 2 si =>swap(a, 4, 8)=>[1, 2, 2, 5, 4, 3, 2, 4], gt=7
do 4 <= 7
	5 < 2 no
	5 > 2 si=>swap(a, 4, 7)=>[1, 2, 2, 2, 4, 3, 5, 4], gt=6
do 4 <= 6
	2 < 2 no
	2 > 2 no
	2 == 2 si => i=5
do 5 <= 6
	4 < 2 no
	4 > 2 si =>swap(a, 5, 6)=>[1, 2, 2, 2, 3, 4, 5, 4], gt=5
do 5 <= 5
	3 < 2 no
	3 > 2 si=>swap(a, 5, 5)=>[1, 2, 2, 2, 3, 4, 5, 4], gt=4
do 5 <= 4 no

ppiv_lft=2
ppiv_rgt=4

Por lo tanto, vemos que ppiv_lft nos indica el comienzo del segmento iguales al pivot y ppiv_rgt el fin del segmento igual al pivot

Entonces(llaves matematicas)
[lft, ppiv_lft) => menores al pivot
[ppiv_lft, ppiv_rgt] => iguales al pivot
(ppiv_rgt, rgt] => mayores al pivot

Luego modificamos quick_sort_rec:
```

<pre><code>
<b>proc</b> quick_sort_rec3(<b>in</b> a: <b>array[1..N] of T</b>, <b>in</b> lft, rgt: <b>nat</b>)
	<b>var</b> ppiv_lft, ppiv_rgt: <b>nat</b>
	<b>if</b> lft &lt; rgt <b>then</b>
		partition3(a, lft, rgt, ppiv_lft, ppiv_rgt)
		quick_sort_rec3(a, lft, ppiv_lft-1)
		quick_sort_rec3(a, ppiv_rgt+1, rgt)
	<b>fi</b>
<b>end proc</b>
</code></pre>
