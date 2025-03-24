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

### ***MERGE SORT*** (Ordenacion por intercalacion)
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

