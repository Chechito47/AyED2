Ordenación por selección es del orden de n2 .
Ordenación por inserción es del orden de n2 (peor caso y
caso medio).
Ordenación por intercalación es del orden de n log2 n.
Ordenación rápida es del orden de n log2 n (caso medio).
Búsqueda lineal es del orden de n.
Búsqueda binaria es del orden de log2 n.
## Algortimos, ¿Que hacen?¿Como lo hacen?
***Algortimo***: es como una receta, **serie de pasos para resolver un problema**. No tiene relacion con el codigo.
### ¿Que hace?
Al hablar de que hace un algortimo nos referimos unicamente a **que hace y nada mas, sin decir nada del codigo ni de su implementacion**. Por ejemplo los algoritmos de ordenacion podriamos decir que:  
	*Reciben un arreglo de elementos y devuelven una permutacion del original ordenada de menor a mayor.*
### ¿Como lo hace?
Nuevamente **no hay que decir nada del codigo**, simplemente **decir su implementacion en lenguaje que todas las personas puedan entender**. Por ejemplo para el algoritmo de ordenacion por seleccion podrias decir que:
	*Selecciona el menor elemento del arreglo y lo compara con que esta ubicado primero y lo ubica segun corresponda, luego repite esto con el segundo mas pequeño y asi hasta terminar.*
## Procedimientos y funciones
### Procedimientos
Es algo que podemos utilizar para hacer varias cosas, incluso no serian necesarias las funciones pero existen para mayor simpleza.  
Estas se caracterizan por **no devolver nada**, o sea **no tener ret**. También **es necesario que aclarar de tipo son los argumentos que recibe**.
#### Ejemplo
<pre><code><b>proc</b> p (<b>in</b> a: <b>int</b>, <b>out</b> b: <b>int</b>)
</code></pre>
### Funciones
Existen por mayor simpleza. Estas se caracterizan por **siempre tener devolver algo, o sea tienen siempre un ret**. Ademas **no es necesario indicar de tipo son los argumentos que recibe pero** hay que notar que **estos no los puede modificar** (o sea no los puede usar como out)
#### Ejemplo
<pre><code><b>fun</b> f(a:<b>int</b>) <b>ret</b> b: <b>int</b>
</code></pre>
## In/Out
Que algo sea in significa que es una variable que vamos a usar como entrada, o sea que estara posicionada del lado derecho de una asignacion porque la usaremos para darle valor a otras variables. 

Si es solamente de tipo out estara posicionado del lado izquierdo de las asignaciones porque sera utilizado para dar salida.

SI es de tipo in/out puede estar posicionada en cualquier lado

```pascal
J tipo in
x = j

J tipo out
j = x

J tipo in/out
x = j
j = y
```

## ***SELECTION SORT*** (Ordenacion por seleccion)
Funciona creando una lista ordenada usando los elementos de la lista no ordenada.  
Selecciono el **numero mas pequeño de la lista** no ordenada (*entre 1 y n*) y lo coloco en la **primera posicion** de la lista ordenada. Luego **busco el menor elemento en la lista no ordenada y lo coloco primero en la sublista de los no ordenados ampliando asi la lista de los ordenados y redujendo la de los no ordenados.** Repito esto hasta que no queden elementos. (*basicamente tomo el menor de todos, lo pongo primero y luego tomo el menor de los restantes y lo pongo segundo, luego el menor y lo pongo tercero y asi...*)
### Ejemplo
[6, 4, 2, 3, 1]  
Busco el menor de la lista y lo coloco primero (1)  
[6, 4, 2, 3, **1**]  
[1, 6, 4, 2, 3]  
Considerando la sublista no ordenada [6, 4, 2, 3] busco el menor (2) y lo coloco primero  
[1, 6, 4, **2**, 3]  
[1, 2, 6, 4, 3]  
Considerando la sublista no ordenada [6, 4, 3] busco el menor (3) y lo coloco primero  
[1, 2, 6, 4, **3**]  
[1, 2, 3, 6, 4]  
Considerando la sublista no ordenada [6, 4] busco el menor (4) y lo coloco primero  
[1, 2, 3, 6, **4**]  
[1, 2, 3, 4, 6] **LISTA YA ORDENADA**

### Complejidad
**Mejor caso**: O(n²) => El algoritmo siempre realiza la misma cantidad de comparaciones (no intercambios)  
**Peor caso**: O(n²) => La lista esta en orden inverso  
Realiza pocos intercambios pero al tener un gran numero de comparaciones hace que no sea eficiente.

## ***INSERTION SORT*** (Ordenacion por insercion)
Es la forma en que ordenariamos las cartas en la mano.  
Comenzamos desde el **segundo elemento**, a este lo **comparamos con el primero** y lo **ordenamos** segun quien sea mas grande. Luego tomamos el tercer elemento y lo comparamos con los dos primeros (que ya estan ordenados) y lo ubicamos segun corresponda. Y asi hasta terminar con todos los elementos.  
### Ejemplo
[5, 2, 9, 1, 5, 6]  
Tomo el segundo elemento (2) y lo comparo con el primero (5)  
[5, **2**, 9, 1, 5, 6]  
[2, 5, 9 ,1, 5, 6]  
Tomo el siguiente (9) y lo comparo con los anteriores  
[2, 5, **9**, 1, 5, 6]  
[2, 5, 9, 1, 5, 6]  
Tomo el siguiente (1) y lo comparo con los anteriores  
[2, 5, 9, **1**, 5, 6]  
[1, 2, 5, 9, 5, 6]  
Tomo el siguiente (5) y comparo con los anteriores  
[1, 2, 5, 9, **5**, 6]  
[1, 2, 5, 5, 9, 6]  
Tomo el siguiente (6) y comparo con los anteriores  
[1, 2, 5, 5, 9, **6**]  
[1, 2, 5, 5, 6, 9]  **LISTA YA ORDENADA**

### Complejidad
**Mejor caso**: O(n) => Se da cuando la lista ya esta ordenada  
**Peor caso**: O(n²) => Se da cuando la lista esta totalmente al revez (ordenada de mayor a menor)  
Es eficiente para listas pequeñas o parcialmente ordenadas pero no es adecuado para listas largas.

## ***MERGE SORT*** (Ordenacion por intercalacion)
Es del tipo **divide y venceras**.  
**Divide los elementos en listas mas pequeñas** y las **ordena recursivamente** antes de **intercalarlas** entre si. *Divide el conjunto de elementos en dos mitades, esas mitades se ordenan recursivamente (o sea se dividen en mas mitades) y finalmente se intercalan las mitades ya ordenadas hasta formar un solo conjunto de elementos ordenado*.
### Ejemplo
[38, 27, 43, 3, 9, 82, 10]  
Divido en dos mitades  
[38, 27, 43, 3] y [9, 82, 10]  
Comienzo con la parte izquierda, divido recursivamente en mitades mas pequeñas  
[38, 27], [43, 3] y [9, 82, 10]  
[38], [27], [43], [3] y [9, 82, 10]  
Intercalo los elementos (de la forma en que los dividi)  
[27, 38], [3, 43] y [9, 82, 10]  
[3, 27, 38, 43] y [9, 82, 10]  
Divido recursivamente la parte derecha  
[3, 27, 38, 43], [9, 82] y [10]  
[3, 27, 38, 43], [9], [82] y [10]  
Intercalo los elementos de la parte derecha  
[3, 27, 38, 43], [9, 82] y [10]  
[3, 27, 38, 43], [9, 10, 82]  
Intercalo los elementos de ambas partes  
[3, 9, 10, 27, 38, 43, 82]  
### Complejidad
**Mejor caso**: O(log (n)) => Cuando la lista esta ordenada o casi ordenada  
**Peor caso**: O(log(n))   => Cuando la lista esta totalmente desordenada  
**Siempre realiza la misma cantidad de divisiones de lista e intercalaciones** sin importar el orden de la misma, por lo que **tiene complejidad especial** (si la lista crece el tiempo que tarda no crece mucho) por lo que es **optimo para listas grandes**. Pero hay que resaltar que necesita espacio para almacenar las listas divididas por lo que no es el optimo en cuanto a memoria.

## ***QUICK SORT*** (Ordenacion rapida)
Es otro del tipo divide y venceras.  
Lo primero que hace es **elegir un pivot** el cual puede ser el primer, ultimo o un elemento aleatorio de la lista. Luego **reorganiza la lista** para que todos los elementos **menores que el pivot queden a la izquierda** y todos **los mayores iguales a la derecha** (lo hace con la funcion ***partition***) **repitiendo esto de manera recursiva** hasta tener listas de 1 solo elemento las cuales son triviales.

### Ejemplo
[3, 9, 2, 1, 7, 3, 5]  
Elijo al primer elemento (3) como pivot  
[*3*, 9, 2, 1, 7, 3, 5]  
Divido la lista segun quienes son mas pequeños y mas grandes que el pivot  
[*3*, 2, 1] y [9, 7, 3, 5]  
Ordeno el lado izquierdo realizando permutaciones  
[2, 1, 3] y [9, 7, 3, 5]  
El 3 ya esta ordenado, elijo (2) como mi nuevo pivot  
[*2*, 1, 3] y [9, 7, 3, 5]  
Otra vez ordeno realizando permutaciones  
[1, 2, 3] y [9, 7, 3, 5]  
Elijo (1) como el nuevo pivot  
[*1*, 2, 3] y [9, 7, 3, 5]  
Como es lista de un solo elemento es trivial  
[1, 2, 3] y [9, 7, 3, 5]  
Comienzo con el lado derecho, elijo (9) como mi pivot  
[1, 2, 3] y [*9*, 7, 3, 5]  
Ubico el pivot realizando permutaciones  
[1, 2, 3] y [5, 7, 3, 9]  
Ahora como el pivot esta ubicado elijo uno nuevo (5)  
[1, 2, 3] y [*5*, 7, 3, 9]  
Ubico el pivot con permutaciones  
[1, 2, 3] y [3, 5, 7, 9]  
Ahora elijo (3) como pivot pero al ser lista de un solo elemento es trivial  
[1, 2, 3] y [3, 5, 7, 9]  
IDEM con pivot (7)  
[1, 2, 3] y [3, 5, 7, 9]  **LISTA YA ORDENADA**  

Tambien se puede hacer de la siguiente manera (usando partition):  
[*3*, 9, 2, 1, 7, 3, 5]  
Comparo: 9<=2 no, 5>=3 si, 3>=3 si, 7>=3 si, 1>=3 no => swap(a, 9, 1)  
[*3*, 1, 2, 9, 7, 3, 5]  
Sigo comparando: 1<=3 si, 2<=3 si, 9<=3 no, 9>=3 si => solo ubico el pivot  
[1, 2, ==3==, 9, 7, 3, 5]  
Elijo el nuevo pivot(1)  
Comparo: 2<=1 no => solo ubico el pivot  
[==1==, 2, ==3==, 9, 7, 3, 5]  
Elijo el nuevo pivot(2)  
Comparo: lista trivial de un solo elemento => solo ubico el pivot  
[==1, 2, 3==, 9, 7, 3, 5]  
Elijo el nuevo pivot(9)  
Comparo: 7<=9 si, 3<=9 si, 5<=9 si => solo ubico el pivot  
[==1, 2, 3==, 5, 7, 3, ==9==]  
Elijo el nuevo pivot(5)  
Comparo: 7<=5 no, 9>=5 si, 3>=5 no => swap(a, 7, 3)  
[==1, 2, 3==, 3, 7, 5, ==9==]  
Elijo el nuevo pivot(3)  
Comparo: 7<=3 no, 5>=3 si, 7>=3 => solo ubico el pivot  
[==1, 2, 3, 3==, 7, 5, ==9==]  
Elijo el nuevo pivot(7)  
Comparo: 5<=7 si => solo ubico el pivot  
[==1, 2, 3, 3==, 5, ==7, 9==]  
Elijo el nuevo pivot(5)  
Lista de un solo elemento, trivial  
[==1, 2 3, 3, 5, 7, 9==]  

### Funcion partition
Es la **encargada de acomodar a los elementos menores del pivot** en la parte izquierda **y los mayores iguales** en la derecha para luego **colocar al pivot en la posicion que le corresponde**.  
Va mirando al elemento mas a la izquierda (sin contar al pivot) y al de mas a la derecha, mientras encuentre elementos que esten bien ubicados (o sea si de la izquierda se encuentra un elemento menor que el pivot) avanza sin hacer nada, si no no avanza del lado que hay un elemento mal ubicado y si lo hace del otro hasta encontrar un elemento mal ubicado y hace una paermutacion.
### Ejemplo (siguiendo con el anterior)
Veamos que hizo la funcion partition en el ejemplo anterior:    
```pascal
[3, 9, 2, 1, 7, 3, 5]  
lft=1, rgt=7  
partition(a, lft, rgt, ppiv)  
	ppiv=1, i=2, j=7
	do 2<=7  
		if 9<=3 no
		if 5>=3 si => j=6
		   3>=3 si => j=5
		   7>=3 si => j=4
		   1>=3 no => swap(a, 9, 1)
		[3, 1, 2, 9, 7, 3, 5]
	do 2<=4
		if 1<=3 si => i=3
		   2<=3 si => i=4
		   9<=3 no
		   9>=3 si => j=3
		   (i<=j false)
	Ubico el pivot
	swap(a, ppiv, j) => swap(a, 1, 3)
	[2, 1, 3, 9, 7, 3, 5]
	ppiv=3
```

## Comparar los ordenes de los algoritmos
Escribimos **f(n) ⊏ g(n)** para indicar que **g(n) crece mas rapido que f(n)**. Por ejemplo:  
- (n log₂n) ⊏ (n²)
- (log₂n) ⊏ (n)  
*Es como si fuera un <*  

Escribimos **f(n) ≈ g(n)** para indicar que **g(n) y f(n) crecen al mismo ritmo**. Por ejemplo:  
- n²/2 - n/2 ≈ n²
- 45n² ≈ n²

Notemos que **no nos interesan las constantes multiplicativas**, por ejemplo:  
- 4n² ≈ n²
- 1000 log n ≈ log n

**Tampoco nos interesan los terminos menos significativos que crecen mas lento**:
- n² + n ≈ n²
- n³ + n² log₂n ≈ n³
- log n + 3500 ≈ log n  
*En cierta forma es similar a la notacion O grande y o chiquita*

**Sea f (n) > 0** para “casi todo n ∈ N”. Entonces:
- g(n) ⊏ h(n) ⇐⇒ f(n)g(n) ⊏ f(n)h(n)
- g(n) ≈ h(n) ⇐⇒ f(n)g(n) ≈ f(n)h(n)

**Sea lim h(n) = ∞** *cuando n→∞*. Entonces:
- f(n) ⊏ g(n) ⇒ f (h(n)) ⊏ g(h(n))
- f(n) ≈ g(n) ⇒ f (h(n)) ≈ g(h(n))

*Ejemplo de jerarquia:*
1 ⊏ log₂n ≈ log₃n ⊏ n⁰·⁰⁰¹ ⊏ n¹·⁵ ⊏ n² ⊏ n⁵ ⊏ n¹⁰⁰ ⊏ (1.01)^n ⊏ 2^n ⊏ 100^n ⊏ 10000^n ⊏ n! ⊏ n^n

**Regla del límite**. Sean f (n) y g(n) tales que:
- **lim** *n→+∞* **f(n)** = **lim** *n→+∞* **g(n)** ***= ∞*** y
- **lim** *n→+∞* **f(n)/g(n) existe**
Entonces:
1. Si **lim** *n→+∞* **f(n)/g(n) = 0** entonces **f(n) ⊏ g(n)**
2. Si **lim** *n→+∞* **f(n)/g(n) = +∞** entonces **g(n) ⊏ f(n)**
3. Caso contrario **f(n) ≈ g(n)** *(el limite es un nro real positivo)*

