
EJ 1) Definición recursiva de la función factorial  
Encabezado: fun factorial(n: nat) ret f : nat

```pascal
Recordemos que:

CASO BASE: n = 0 => fac = 1
CASO RECURSIVO: n > 0 => fac = fac(n-1)
```

**fun** factorial(n: nat) **ret** f: nat
	**if** n = 0
		**then** f := 1
	**else** f := n * factorial(n-1)
	**fi**
**end fun**

<pre><code class="language-pascal">
<b>fun</b> factorial(n: nat) <b>ret</b> f: nat
	<b>if</b> n = 0
		<b>then</b> f := 1
	<b>else</b> f := n * factorial(n-1)
	<b>fi</b>
<b>end fun</b>

</code></pre>




El valor de **i** es importante en el siguiente código:

```pascal
var i: Integer;
begin
   i := 1;
end.
```


<pre><code class="language-pascal"><font color="#8064a2">var</font> <b>i</b>: Integer;
begin
   <b>i</b> := 1;
end.</code></pre>
