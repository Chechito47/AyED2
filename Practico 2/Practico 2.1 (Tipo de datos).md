![ScreenShot](Imagenes%20practico%202.1/ej1.png)

```pascal
a) Nos pide calcular el elemento minimo dada una matriz [1..n,1..m] o sea que tenemos una matriz de 2 elementos
```

<pre><code>
<b>fun</b> return_min(a: <b>array[1..N, 1..M]</b> of <b>int</b>) <b>ret</b> min:<b>int</b>
	min := a[1, 1]
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		<b>for</b> j := 1 <b>to</b> m <b>do</b>
			<b>if</b> a[i, j] &lt; min <b>then</b>
				min := a[i, j]
			<b>fi</b>
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
b) Ahora tenemos que devolver el elemento minimo de cada fila de a[1..n, 1..m] en a[1..n]
Como es por filas, a[i, j] solo nos interesa ir comparando por j, el i lo debemos dejar en un valor fijo mientras vamos comparando todos los j
Recordemos que a[1][1] es un entero como lo es a[1]
```

<pre><code>
<b>fun</b> devolver_array_min(a:<b>array[1..N, 1..M]</b> of <b>int</b>) <b>ret</b> min:<b>array[1..N]</b> of <b>int</b>
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		min[i] := a[i, 1]
		<b>for</b> j := 2 <b>to</b> m <b>do</b>
			<b>if</b> a[i, j] &lt; min[i] <b>then</b>
				min[i] := a[i, j]
			<b>fi</b>
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

![ScreenShot](Imagenes%20practico%202.1/ej2.png)

```pascal
Notemos que dice que: med[2014, febrero, 3, Pres] indica la presion atmosferica en esa fecha.

a) Como nos pide un solo valor hacemos una funcion para que devuelva dicho valor
```

<pre><code>
<b>fun</b> temp_min_hist(a:<b>array</b>[1980..2016, enero..diciembre, 1..28, Temp,
					Prec] <b>of Nat</b>), <b>ret</b> temp_min:<b>int</b>
	temp_min := a[1980, enero, 1, TempMin]
	<b>for</b> año := 1980 <b>to</b> 2016 <b>do</b>
		<b>for</b> mes := enero <b>to</b> diciembre <b>do</b>
			<b>for</b> dia := 1 <b>to</b> 28 <b>do</b>
				<b>if</b> a[año, mes, dia, TempMin] &lt; temp_min <b>then</b>
					temp_min := a[año, mes, dia, TempMin]
				<b>fi</b>
			<b>od</b>
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
b) Nuevamente podemos usar una funcion porque debemos devolver un arreglo de tamaño 2016-1980
```

<pre><code>
<b>fun</b> max_temp_year(a:<b>array</b>[1980..2016, enero..diciembre, 1..28, Temp..Proc] <b>of int</b>), <b>ret</b> res:<b>array</b>[year]:<b>of int</b>
	<b>var</b> max_year:<b>int</b>
	<b>for</b> year := 1980 <b>to</b> 2016 <b>do</b>
		max_year := a[year, enero, 1, TempMax]
		<b>for</b> month := enero <b>to</b> diciembre <b>do</b>
			<b>for</b> day := 1 <b>to</b> 28 <b>do</b>
				<b>if</b> a[year, month, day, TempMax] &gt; max_year <b>then</b>
					max_year := a[year, month, day, TempMax]
				<b>fi</b>
			<b>od</b>
		<b>od</b>
		res[year] := max_year
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
c) Ahora nos pide devolver un arreglo de tamaño 2016-1980 que devuelva el mes que hubo mas precipitaciones
Por lo tanto hay que calcular las precipitaciones mensuales y compararlas con las de los otros meses
```

<pre><code>
<b>fun</b> max_pre_month(a:<b>array</b>[1980..2016, enero..diciembre, 1..28, Temp..Proc]
					<b>of int</b>), <b>ret</b> res:<b>array</b>[1980..2016] <b>of int</b>
	<b>var</b> max_month:<b>mes</b>
<b>var</b> max_rain, month_rain:<b>nat</b>
	<b>for</b> year := 1980 <b>to</b> 2016 <b>do</b>
		max_month := enero
		max_rain := 0
		<b>for</b> month := enero <b>to</b> diciembre <b>do</b>
			month_rain := 0
			<b>for</b> day := 1 <b>to</b> 28 <b>do</b>
				month_rain := month_rain + a[year, month, day, Prec]
			<b>od</b>
			<b>if</b> month_rain &gt; max_rain <b>then</b>
				max_rain := month_rain
				max_month := month
			<b>fi</b>
		<b>od</b>
		res[year] := max_month
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
d) Ahora tengo que usar el arreglo que devuelve el c) para devolver el año con menor precipitaciones entre ellas
Uso lo mismo que antes, cambio el retorno, agrego algunas variables y las condiciones para verificar el menor año entre las de mayor precipitaciones
```

<pre><code>
<b>fun</b> max_pre_month(a:<b>array</b>[1980..2016, enero..diciembre, 1..28, Temp..Proc]
					<b>of int</b>), <b>ret</b> min_max_rain_year:<b>int</b>
	<b>var</b> max_month:<b>mes</b>
	<b>var</b> max_rain, month_rain, min_max_rain:<b>nat</b>
	<b>var</b> res:<b>array</b>[1980..2016] <b>of int</b>
	<b>for</b> year := 1980 <b>to</b> 2016 <b>do</b>
		max_month := enero
		max_rain := 0
		<b>for</b> month := enero <b>to</b> diciembre <b>do</b>
			month_rain := 0
			<b>for</b> day := 1 <b>to</b> 28 <b>do</b>
				month_rain := month_rain + a[year, month, day, Prec]
			<b>od</b>
			<b>if</b> month_rain &gt; max_rain <b>then</b>
				max_rain := month_rain
				max_month := month
			<b>fi</b>
		<b>od</b>
		res[year] := max_month
	<b>od</b>
	min_max_rain := res[1980]
	min_max_rain := 1980
	<b>for</b> year := 1981 <b>to</b> 2016 <b>do</b>
		<b>if</b> res[year] &lt; min_max_rain <b>then</b>
			min_max_rain := res[year]
			min_max_rain_year := year
		<b>fi</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
e)Dar el mismo resultado que en d) pero sin usar el codigo tal cual
Podemos omitir el res[year] y verificar directamente la condicion que queremos
Puedo eliminar max_month ya que no me sirve
```

<pre><code>
<b>fun</b> max_pre_month(a:<b>array</b>[1980..2016, enero..diciembre, 1..28, Temp..Proc]
					<b>of int</b>), <b>ret</b> min_max_rain_year:<b>nat</b>
	<b>var</b> max_rain, month_rain, min_max_rain:<b>nat</b>
	<b>for</b> year := 1980 <b>to</b> 2016 <b>do</b>
		max_rain := 0
		<b>for</b> month := enero <b>to</b> diciembre <b>do</b>
			month_rain := 0
			<b>for</b> day := 1 <b>to</b> 28 <b>do</b>
				month_rain := month_rain + a[year, month, day, Prec]
			<b>od</b>
			<b>if</b> (month_rain &gt; max_rain) <b>then</b>
				max_rain := month_rain
			<b>fi</b>
		<b>od</b>
		<b>if</b> max_rain &lt; min_max_rain <b>then</b>
			min_max_rain := max_rain
			min_max_rain_year := year
		<b>fi</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

![ScreenShot](Imagenes%20practico%202.1/ej3.png)

```pascal
Notemos que ahora no tenemos un tipo enumerado, sino que tenemos una tupla a la cual se accede con el .
Notemos que tenemos un arreglo
a)
```

<pre><code>
<b>proc</b> promedio_person(<b>in</b> a:<b>array</b>[1..N] <b>of</b> person)
	<b>var</b> age_prom, weight_prom:<b>nat</b>
		<b>for</b> i := 1 <b>to</b> n <b>do</b>
			age_prom := age_prom + a[i].age
			weight_prom := weight_prom + a[i].weight
		<b>od</b>
	age_prom := age_prom / n
	weight_prom := weight_prom / n
<b>end proc</b>
</code></pre>

```c
b) Ahora debemos ordenadro alfabeticamante el arreglo. Podemos usar cualquier algoritmo de ordenacion
Decido por insertion sort porque en el lab hay funciones similares a las que usa. Me baso en este codigo:

void insertionSort(int arr[], int n)
{
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;

        /* Move elements of arr[0..i-1], that are
           greater than key, to one position ahead
           of their current position */
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}

Notemos que hay algunas partes que se pueden reemplazar por funciones que conocemos como es_menor o swap
```

<pre><code>
<b>proc</b> order_alphabetically(<b>in/out</b> a:<b>array</b>[1..N] <b>of</b> person)
	<b>for</b> i := 2 <b>to</b> n <b>do</b>
		insert_alphabetically(a, i)
	<b>od</b>
<b>end proc</b>


<b>proc</b> insert_alphabetically(<b>in/out</b> a:<b>array</b>[1..N] <b>of</b> person, <b>in</b> i:<b>nat</b>)
	<b>var</b> j:<b>nat</b>
	j := i
	<b>while</b> (j &gt; i) <b>AND</b> es_menor(a[j].name, a[j+1].name) <b>do</b>
		swap(a, j-1, j)
		j := j -1
	<b>od</b>
<b>end proc</b>


<b>fun</b> es_menor(a1:<b>array</b>[1..N] <b>of char</b>, a2:<b>array</b>[1..M]
					<b>of char</b>), <b>ret</b> res:<b>bool</b>
	<b>if</b> a1[1] &lt; a2[1] <b>then</b>
		res := true
	<b>else if</b> a1[1] &gt; a2[1] <b>then</b>
		res := false
	<b>else</b>
		<b>for</b> i := 2 <b>to</b> n <b>do</b>
			<b>if</b> a1[i] &lt; a2[i] <b>then</b>
				res := true
			<b>else if</b> a1[i] &gt; a2[i] <b>then</b>
				res := false
			<b>fi</b>
		<b>od</b>
		<b>if</b> n &lt; m <b>then</b>
			res := true
		<b>fi</b>
	<b>fi</b>
<b>end fun</b>


<b>proc</b> swap(<b>in/out</b> a:<b>array</b>[1..N] <b>of</b> person, <b>in/out</b> j, j-1:<b>nat</b>)
	<b>var</b> temp:person
	tmp := a[j]
	a[j] := a[j-1]
	a[j-1] := tmp
<b>end proc</b>
</code></pre>


![ScreenShot](Imagenes%20practico%202.1/ej4.png)

```pascal
a) Nos pide intercambiar los valores referidos sin modificar p y q.
Estos son del tipo puntero, o sea que son *p y *q

Entonces, en tmp guardamos el valor al que apunta p, luego decimos que p apunte al valor que apunta q (ahora q vale p) y por ultimo decimos que q apunte al valor tmp (o sea al valor que apuntaba originalmente p)
```

<pre><code>
<b>proc</b> swap_ref(<b>in/out</b> p, q: <b>pointer to int</b>)
	<b>var</b> tmp:<b>int</b>
	tmp := *p
	*p := *q
	*q := tmp
<b>end proc</b>
</code></pre>

```pascal
b) Ahora nos pide intercambiar los valores de los punteros, por lo tanto si uso un aux de tipo puntero podria hacerlo
Entonces, p, q y tmp son en realidad *p, *q y *tmp

Comienzo apuntando tmp a donde apunta p, luego apunto p a donde apunta q y por ultimo apunto q a donde apuntaba p inicialmente

La diferencia con el anterior radica en que antes usamos un tmp del tipo entero al que directamente la asignamos un valor, ahora usamos uno del tipo pointer al que le decimos que apunte a una direccion especifica haciendo que toda la implementacion sea con punteros
```

<pre><code>
<b>proc</b> swap_val(<b>in/out</b> p, q: <b>pointer to int</b>)
	<b>var</b> tmp:<b>pointer to int</b>
	tmp := p
	p := q
	q := tmp
<b>end proc</b>
</code></pre>

```pascal
c) Tenemos r (pointer to int) que comienza tq r = p.
*p = 5, *q = -4, *r = *p = 5

Ejecutemos los dos casos:

swap_ref:
			    *r = *p
tmp = *p   =>   tmp = 5 (tenemos tmp vale 5, *p apunta a 5 y *q a -4)
*p = *q    =>   *p = -4 (tenemos tmp=5 y *p y *q apuntan a -4)
*q = tmp   =>   *q = 5  (ahora *q apunta a 5 y *p a -4)
		   =>   *r = -4 (xq *p ahora apunta a -4)



swap_val:
			r = p   (r=flecha de p)
tmp = p  => tmp = 5 (tenemos tmp guarda la flecha de p y p=5, q=-4)
p = q    => p = -4  (tmp=flecha de p, p= flecha de q (tmp=5, p=-4))
q = tmp  => q = 5   (q=flecha de tmp (flecha de p, o sea q=5))
		 => r = 5   (r no cambia)
```

![ScreenShot](Imagenes%20practico%202.1/ej5_a.png)
![ScreenShot](Imagenes%20practico%202.1/ej5_b.png)

```pascal
a) Entonces puedo verificar si a[k] < b[k]. En caso que no se cumpla es porque llegamos al ultimo elemento o porque a[k] >= b[k].
Cualquiera sea el caso, verificamos si a en la ultima posicion es menor que b en la ultima posicion. Si lo es, era porque llegamos al n y devuelve true
Si no lo es era porque a[k] >= b[k] y devuelve false
```
<pre><code>
<b>fun</b> lex_less(a, b:<b>array</b>[1..N] <b>of nat</b>), <b>ret</b> res:<b>bool</b>
	<b>var</b> k:<b>nat</b>
	<b>while</b> a[k] &lt; b[k] && k &lt;= n <b>do</b>
		k := k+1
	<b>od</b>
	res := a[k] &lt; b[k]
<b>end fun</b>
</code></pre>

```pascal
b) IDEM que anterior, pero ahora verificamos a[k] <= b[k]
```

<pre><code>
<b>fun</b> lex_less_or_equal(a, b:<b>array</b>[1..N] <b>of nat</b>), <b>ret</b> res:<b>bool</b>
	<b>var</b> k:<b>nat</b>
	<b>while</b> a[k] &lt;= b[k] && k &lt;= n <b>do</b>
		k := k+1
	<b>od</b>
	res := a[k] &lt; b[k]
<b>end fun</b>
</code></pre>

```pascal
c) Tenemos que recicibir los arreglos y en vez de devolver un bool devolvemos el tipo ord que corresponda segun la situacion. O sea nos basamos en el a) y tenemos que verificar si es igual o menor tambien
```

<pre><code>
<b>fun</b> lex_compare(a, b:<b>array</b>[1..N] <b>of nat</b>), <b>ret</b> res:<b>ord</b>
	<b>var</b> k:<b>nat</b>
	<b>while</b> a[k] &lt; b[k] && k &lt;= n <b>do</b>
		k := k+1
	<b>od</b>
	<b>if</b> a[k] &lt; b[k] <b>then</b>
		res := menor
	<b>else if</b> a[k] &gt; b[k] <b>then</b>
		res := mayor
	<b>else</b>
		res := igual
	<b>fi</b>
<b>end fun</b>
</code></pre>

![ScreenShot](Imagenes%20practico%202.1/ej6.png)

```pascal
Notemos que no necesariamente tienen el mismo tamaño
Tambien notemos que la suma es coordenada a coordenada, o sea a[i, j] + b[i, j]
Como nos pide devolver la suma de las matrices, (ojo que no pide devolver el la suma de todos con todos, solo la de las matrices en sus coordenadas)
```

<pre><code>
<b>fun</b> return_suma(a, b:<b>array</b>[1..N, 1..M] <b>of nat</b>), <b>ret</b> res:<b>array</b>[1..N, 1..M] <b>of nat</b>
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		<b>for</b> j := 1 <b>to</b> m <b>do</b>
			res[i, j] := a[i, j] + b[i, j]
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
Si quisiera devolver la suma total de todos con todos seria con una variable normal:
```

<pre><code>
<b>fun</b> return_suma(a, b:<b>array</b>[1..N, 1..M] <b>of nat</b>), <b>ret</b> res:<b>nat</b>
	res := 0
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		<b>for</b> j := 1 <b>to</b> m <b>do</b>
			res := res + (a[i, j] + b[i, j])
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

![ScreenShot](Imagenes%20practico%202.1/ej7.png)

```pascal
Hay que hacer algo similar al 6 inicializando en 0 porque usamos ese valor, caso contrario podria tener basura. En uno del 6 no fue necesario inicializarlo porque directamente le asignamos un valor
```

<pre><code>
<b>fun</b> return_producto(a, b:<b>array</b>[1..N, 1..M] <b>of nat</b>), <b>ret</b> res:<b>array</b>[1..N, 1..M] <b>of</b> nat
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		<b>for</b> j := 1 <b>to</b> m <b>do</b>
			res[i, j] := 0
			<b>for</b> k := 1 <b>to</b> M <b>do</b>
				res[i, j] := res[i, j] + (a[i, k] * b[k, j])
			<b>od</b>
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>

```pascal
Supongamos que N=M=2, entonces
a=[(1, 2), (3, 4)], b=[(5, 6), (7, 8)]
i=1 to 2
	j=1 to 2
		res[1,1]=0
		k=1 to 2
			res[1, 1] = 0 + (a[1, 1] * b[1, 1]) = 0+1*5=5
		k=2 to 2
			res[1, 1] = 5 + (a[1, 2] * b[2, 1]) = 5+2*7=19
			=> res[1,1] = 19
	j=2 to 2
		res[1, 2]=0
		k=1 to 2
			res[1,2]= 0 + (a[1, 1] * b[1, 2]) = 0+1*6=6
		k=2 to 2
			res[1,2]= 6 + (a[1, 2] * b[2, 2]) = 6+2*8=22
			=> res[1, 2] = 22
i=2 to 2
	j=1 to 2
		res[2,1]=0
		k=1 to 2
			res[2,1] = 0 + (a[2,1] * b[1, 1]) = 0+3*5=15
		k=2 to 2
			res[2,1] = 15 + (a[2,2] * b[2, 1]) = 15+4*7=43
			=> res[2,1] = 43
	j=2 to 2
		res[2,2]=0
		k=1 to 2
			res[2,2] = 0 + (a[2, 1] * b[1, 2]) = 0+3*6=18
		k=2 to 2
			res[2,2] = 6 + (a[2,2] * b[2, 2]) = 18+4*8=50
			=> res[2,2] = 50
LISTO

Entonces resulta res=[(19, 22), (43, 50)]


Ahora si quisiera guardar el resultado de todas las multiplicaiones en un solo registro podria hacer lo siguiente:
```

<pre><code>
<b>fun</b> return_producto2(a, b:<b>array</b>[1..N, 1..M] <b>of nat</b>), <b>ret</b> res:<b>nat</b>
	res := 0
	<b>for</b> i := 1 <b>to</b> n <b>do</b>
		<b>for</b> j := 1 <b>to</b> m <b>do</b>
			<b>for</b> k := 1 <b>to</b> M <b>do</b>
				res := res + (a[i, k] * b[k, j])
			<b>od</b>
		<b>od</b>
	<b>od</b>
<b>end fun</b>
</code></pre>
