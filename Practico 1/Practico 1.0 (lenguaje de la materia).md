
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



<pre><code>
<b>fun</b> factorial(n: nat) <b>ret</b> f: nat
	<b>if</b> n = 0
		<b>then</b> f := 1
	<b>else</b> f := n * factorial(n-1)
	<b>fi</b>
<b>end fun</b>

</code></pre>



**fun** factorial(n: nat) **ret** f: nat
	**if** n = 0
		**then** f := 1
	**else** f := n * factorial(n-1)
	**fi**
**end fun**




### Resaltado de sintaxis:
Si lo que deseas es **resaltar sintaxis** (por ejemplo, en GitHub, GitLab, o en tu generador de sitios que lo soporte), la forma correcta de hacerlo es especificar el lenguaje como te mencioné antes, pero fuera de la etiqueta `<code>`, así:

```markdown
```html
<pre><code>Este es un bloque de código con etiquetas HTML y negrita.</code></pre>
