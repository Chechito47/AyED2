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
	size := 2^n
	<b>while</b> rgt &lt;= size <b>do</b>
		merge(a, lft, )