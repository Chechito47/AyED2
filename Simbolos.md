<ctrl shift u 03a3> -> Σ


```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```


> :warning: **Warning:** Do not push the big red button.






```pascal
Recordemos, si voy a usar la variable en el lado izquierdo debe ser de tipo out:
x = 2
Ya que le uso para como valor de salida para otra variable o valor

Pero si lo voy a usar del lado derecho debe ser de tipo in:
y = x
Ya que la uso como valor de entrada para otra variable


a) Inicializar cada componente del arreglo con el valor 0
```
.
<pre><code class="language-pascal">
<b>proc</b> init0 (<b>out</b> a: <b>array</b>[1..N ] <b>of nat</b>)
	<b>for</b> i := 1 <b>to</b> N <b>do</b>
		a[i] := 0
	<b>od</b>
<b>end proc</b>
</code></pre>
