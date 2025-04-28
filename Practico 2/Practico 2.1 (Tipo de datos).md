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

```