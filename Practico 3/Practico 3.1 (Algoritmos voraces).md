![ScreenShot](Imagenes%20practico%203.1/ej1.png)

```pascal
Si modificamos el algoritmo de la mochila para sacarle la fragmentacion tendriamos:
```

<pre><code><b>type</b> Objeto = <b>tuple</b>
		id : Nat
		value: Float
		weight: Float
	<b>end tuple</b>

<b>type</b> Obj_Mochila = <b>tuple</b>
		obj : Objeto
		fract : Float
	<b>end tuple</b>


<b>fun</b> mochila(W: Float, C: Set <b>of</b> Objeto) <b>ret</b> L : List <b>of</b> Obj_Mochila
	var o_m : Obj_Mochila
	var resto : Float
	var C_aux : Set <b>of</b> Objeto
	L:= empty_list()
	C_aux:= set_copy(C)
	resto:= W
	<b>do</b> (resto > 0) →
		o_m.obj := select_obj(C_aux)
		<b>if</b> o_m.obj.weight &lt;= resto <b>then</b>
			resto := resto - o_m.obj.weight
		<b>else</b>
			resto := 0
		<b>fi</b>
		addl(L,o_m)
		elim(C_aux,o_m.obj)
	<b>od</b>
	set_destroy(C_aux)
<b>end fun</b></code></pre>
```pascal
Entonces ahora si no tenemos fragmentacion el criterio de seleccion vemos que no funciona porque si volvemos a usar el de mejor relacion peso/valor sin fragmentacion provoca que volvamos al caso en que pueda haber objetos de menor relacion peso/calidad que aprovechen mejor la capacidad de la mochila (por eso habiamos incluido fragmentacion en un principio).

Entonces si tuvieramos el caso: 
Mochila: W=10
Objetos: ([12, 6], [11, 5], [8, 4])

El criterio elige al objeto de valor 11, peso 5. Luego estamos forzados a elegir el de valor 8 y peso 4 porque es el unico que entra

Pero lo mas optimo hubiese sido elegir al [12, 6] y [8, 4] haciendo un valor totla de 20 frente a los 19 que hace.

Entonces claramente vemos que falla
```


![ScreenShot](Imagenes%20practico%203.1/ej2.png)

```pascal
Tenemos monedas infinitas.
Comenzamos con una moneda de 1 y las que siguen son AL MENOS el doble que la anterior.

Propongo:
monedas = [1, 3, 7, 16], cambio = 21

El criterio selecciona siempre la mayor moneda que sea menor o igual al resto hasta que este sea 0. Entonces elige las siguiente monedas:
	16          -> resto = 5
	16, 3       -> resto = 2
	16, 3, 1    -> resto = 1
	16, 3, 1, 1 -> resto = 0

Pero hubiese sido mas optimo elegir 3 monedas de 7 (porque utilizamos menos monedas)

Entonces vemos que es FALSO
```

![ScreenShot](Imagenes%20practico%203.1/criterios.png)
![ScreenShot](Imagenes%20practico%203.1/ej3.png)

```pascal
- Datos:
Autonomia: A
Desde l0 hasta ln pasando por l1 hasta ln-1 en orden
Sabemos la distancia di <= A entre la localidad li-1 y la localidad li (1<=i<=n)
En cada localidad hay una estacion de servicio

- Criterio de seleccion:
Paro a cargar combustible si la cantidad que tengo no me es suficiente para llegar a la siguiente localidad (distancia >= autonomia)

- Estructuras
Uso tuplas de dos elementos, una tupla para las localidades (como si fuera un gps) que indique un identificador o nombre (como podria serlo l1, l2 o lo que fuere) y la distancia que falta para llegar. Este habria que consultarlo cada vez que estamos en una localidad para saber si la distancia a la proxima localidad es mayor a la autonomia actual (A)

La otra tupla debe contener la lista de localidades en las que cargamos combustible y la cantidad cargadas de combustible que hicimos
```

<pre><code><b>type</b> Localidades = <b>tuple</b>
					id: string
					distancia: nat
				<b>end tuple</b>

<b>type</b> Paradas = <b>tuple</b>
					localidad: List <b>of</b> Localidades
					cant_paradas: nat
				<b>end tuple</b>

<b>fun</b> viaje(A: nat, L: List <b>of</b> Localidades) <b>ret</b> res: Paradas</code></pre>

```pascal
- Descripcion de la solucion:
Comienzo cargando en la primer localidad, luego voy hasta la localidad que la autonomia me permita y cargo cuando veo que con la autonomia que tengo no llego hasta la siguiente localidad (agrego esa localidad a la lista y voy contando cuantas  <b>notveces cargue combustible)

- Implementacion:
```

<pre><code><b>type</b> Localidades = <b>tuple</b>
					id: string
					distancia: nat
				<b>end tuple</b>

<b>type</b> Paradas = <b>tuple</b>
					localidad: List <b>of</b> Localidades
					cant_paradas: nat
				<b>end tuple</b>

<b>fun</b> viaje(A: nat, L: List <b>of</b> Localidades) <b>ret</b> res: Paradas
	<b>var</b> L_aux := List <b>of</b> Localidades
	<b>var</b> l: Localidades
	<b>var</b> combustible: nat

	L_aux := copy_list(L)
	res.localidad := empty_list()
	res.cant_paradas := 0
	combustible := 0

	{- Mientras haya localidades sin visitar veo si tengo que recargar en la siguiente localidad. Si tengo que hacerlo tengo tanque lleno (A), si no gasto la distancia hasta la prox localidad: Luego elimino de la lista las localidades que ya visite para no repetir -}
	<b>while not</b> is_empty_list(L_aux) <b>do</b>
		l := busco_localidad_recargar(L_aux, combustible)
		<b>if</b> l.distancia &lt; combustible <b>then</b>
			combustible := combustible - l.distancia
		<b>else</b>
			combustible := A
		<b>fi</b>
		addr(res.localidad, l)
		res.cant_paradas := cant_paradas + 1
		eliminar_localidades(L_aux, l)
	<b>od</b>
	destroy_list(L_aux)
<b>end fun</b>

{- Le paso como argumento una copia de la lista L. Busco una localidad usando head y me fijo el primer elemento. Si no cumple hago tail e itero hasta encontrarlo. Cuando encuentre donde recargar destruyo la lista -}
<b>fun</b> busco_localidad_recargar(L: List <b>of</b> Localidades, n: nat) <b>ret</b> res: Localidades
	<b>var</b> L_aux: List <b>of</b> Localidades
	<b>var</b> l: localidades
	<b>var</b> combustible: nat
	<b>var</b> localidad_encontrada: bool

	L_aux := copy_list(L)
	combustible := n
	localidad_encontrada := false

	{- Si tenemos suficiente combustible para llegar a la localidad siguiente no cargamos -}
	<b>while not</b> is_empty(L_aux) <b>^ not</b> localidad_encontrada <b>do</b>
		l := head(L_aux)
		<b>if</b> l.distancia &gt;= combustible <b>then</b>
			localidad_encontrada := true
			res := l
		<b>fi</b>
		tail(L_aux)
	<b>od</b>
	destroy_list(L_aux)
<b>end fun</b>

<b>proc</b> eliminar_localidades(<b>in/out</b> L: List <b>of</b> Localidades, <b>in</b> l: nat)
	<b>var</b> l_aux: Localidades
	<b>var</b> localidad_encontrada: bool

	localidad_encontrada := false
	<b>while not</b> localidad_encontrada <b>do</b>
		l_aux := head(L_aux)
		<b>if</b> l = l_aux <b>then</b>
			localidad_encontrada := true
		<b>fi</b>
		tail(L_aux)
	<b>od</b>
<b>end proc</b></code></pre>


![ScreenShot](Imagenes%20practico%203.1/criterios.png)
![ScreenShot](Imagenes%20practico%203.1/ej4.png)

```pascal
- Datos: n ballenas
Conocemos los tiempos s1, s1,..., sn que son capaces de sobrevivir
Llevar una ballena al mar toma tiempo t
Mientras la estan regresando el tiempo de la ballena que esta siendo rescatada se para

- Criterio de seleccion:
La ballena con menor tiempo de supervicencia

- Estructuras de datos:
Planteo a las ballenas como una tupla de dos elementos, uno un identificador para diferenciar una de otra (con el tiempo no basta, podrian tener dos ballenas el mismo tiempo), y el tiempo de supervivencia

Luego para la funcion principal tomo como argumento un conjunto de ballenas (porque no me importa el orden, llego y estan ya todas varadas) y el tiempo t el cual sera constante. Y devuelvo una lista(porque ahora si me importa el orden en que las salve) de ballenas que pude salvar
```

<pre><code><b>type</b> Ballena = <b>tuple</b>
					id: nat
					tiempo_sup: nat
				<b>end tuple</b>

<b>fun</b> (S: Set <b>of</b> Ballena, t: nat) <b>ret</b> res: List <b>of</b> Ballena</code></pre>

```pascal
Notar que luego nos hara falta una funcion para elegir que ballena salvar usando el criterio de seleccion y un procedimiento para descartar a las ballenas que no pude salvar el cual debera correr luego de que salve una ballena

- Descripcion de la solucion:
Salvo a la ballena con menor tiempo de supervivencia y la elimino del conjunto de ballenas agregandola a la de ballenas salvadas. Luego descarto a las ballenas que murieron mientras rescataba a la otra y repito hasta que no queden ballenas por ser rescatadas.

- Implementacion:
```

<pre><code><b>type</b> Ballena = <b>tuple</b>
					id: nat
					tiempo_sup: nat
				<b>end tuple</b>


{- Mientras todavia queden ballenas vivas elijo una segun mi criterio,
la agrego a la lista de las que salve y la elmino de la lista de las que 
quedan por salvar(vivas, la copia), actualizo el tiempo transcurrido y 
descarto las ballenas que murieron mientras salvaba a otra. Esto se 
repite en bucle hasta que no queden ballenas vivas. Cuando ya no 
queden elimino la lista aux -}
<b>fun</b> (S: Set <b>of</b> Ballena, t: nat) <b>ret</b> res: List <b>of</b> Ballena
	<b>var</b> S_aux: Set <b>of</b> Ballena
	<b>var</b> b: Ballena
	<b>var</b> tiempo_trans: nat

	S_aux := copy_set(S)
	res := empty_list()
	tiempo_trans := 0

	<b>while not</b> empty_set(S_aux) <b>do</b>
		b := elegir_ballena()
		addr(res, b)
		elim(S_aux, b)
		tiempo_trans := tiempo_trans + t
		descartar_ballenas_muertas(S_aux, tiempo_trans)
	<b>od</b>
	destroy_set(S_aux)
<b>end fun</b>


{- Elijo la ballena con menor tiempo de supervivencia. Tengo que usar la
funcion get que selecciona un elemento aleatorio de la lista y entonces
voy haciendo eso y guardando el de menor tiempo de supervivencia
(recordemos que aunque queden 1 segundo de vida las puedo salvar)
eliminando de la lista los elementos que ya verifique y hasta que la lista
sea vacia -}
<b>fun</b> elegir_ballena(S: Set <b>of</b> Ballena) <b>ret</b> res: Ballena
	<b>var</b> S_aux: Set <b>of</b> Ballena
	<b>var</b> b: Ballena
	<b>var</b> tiempo_aux: nat

	S_aux := copy_set(S)
	tiempo_aux := ∞

	<b>while not</b> is_empty_set(S_aux) <b>do</b>
		b := get(S_aux)
		<b>if</b> b.tiempo_sup &lt; tiempo_aux <b>then</b>
			tiempo_aux := b.tiempo_sup
			res := b
		<b>fi</b>
		elim(S_aux, b)
	<b>od</b>
	destroy_set(S_aux)
<b>end fun</b>


{- Paso como argumento la lista de las ballenas que todavia no salve o que 
murieron y el tiempo que transcurrio. Me fijo si las ballenas de la lista 
tienen un tiempo de vida menor al que hasta ahora transcurrio. Si es asi 
esa ballena ya esta muerta, entonces la descarto de la lista -}
<b>proc</b> descartar_ballenas_muertas(<b>in/out</b> S: Set <b>of</b> Ballena, <b>in</b> t: nat)
	<b>var</b> S_aux: Set <b>of</b> Ballena
	<b>var</b> b: Ballena

	S_aux := copy_set(S)

	<b>while not</b> empty_set(S_aux) <b>do</b>
		b := get(S_aux)
		<b>if</b> b.tiempo_sup &lt; t <b>then</b>
			elim(S, b)
		<b>fi</b>
		elim(S_aux, b)
	<b>od</b>
	destroy_set(S_aux)
<b>end proc</b></code></pre>


![ScreenShot](Imagenes%20practico%203.1/criterios.png)
![ScreenShot](Imagenes%20practico%203.1/ej5.png)
```pascal
- Datos:
Le quiero prestar el celular a la mayoria de amigos posibles
La cantidad de amigos es n
Los viajes de ellos se superponen
Cuando devuelven el celular lo hacen por la noche, recien al otro dia se puede volver a prestar
NOTEMOS QUE SE LO QUIERO PRESTAR UNA VEZ A CADA AMIGO EN EL CASO IDEAL, UNA VEZ QUE SE LO PRESTE A UN AMIGO LO DESCARTO DEL CONJUNTO AL QUE HAY QUE PRESTARSELO

- Criterio de seleccion:
Se lo presto al amigo con el viaje mas corto.
Lo ideal seria poder prestarlo a alguien cuyo viaje no se superponga con el otro y que en caso que si se superponga se lo presto al que su viaje dure menos pero es algo bastante complejo de programar

- Estructura de datos:
Planto una tupla de 3 elementos, uno para un identificador de amigo, otro para el dia de partida y otro para el de regreso.
Para la funcion principal tomo como argumento un conjunto de amigos(porque no tienen un orden, dicho "orden" vendria dado por las fechas de sus viajes) y devuelve una lista con los amigos a los que les preste el celular
```

<pre><code><b>type</b> Amigo = <b>tuple</b>
						id: nat
						fecha_salida: nat
						fecha_regreso: nat
					<b>end tuple</b>

<b>fun</b> (S: Set <b>of</b> Amigo) <b>ret</b> res: List <b>of</b> Amigo</code></pre>

```pascal
LA FORMA EN QUE PLANTEAMOS LA TUPLA HACE POSIBLE QUE UN AMIGO SOLO PUEDA HACER UN VIAJE

- Descripcion de la solucion:
Le presto el celular al primer amigo que haga un viaje. Luego cuando me lo devuelve verifico la fecha de regreso de los amigos que vayan a hacer un viaje y se lo presto al que mas pronto regrese. Repito hasta que le haya prestado el celular a todos o mis amigos no viajen mas.

- Implementacion:
```

<pre><code><b>type</b> Amigo = <b>tuple</b>
						id: nat
						fecha_salida: nat
						fecha_regreso: nat
					<b>end tuple</b>

{- Mientras haya amigos a los que todavia no les preste el celular, elijo 
al que tenga el viaje mas corto, lo agrego a la lista de amigos a los que
ya se lo preste y lo descarto de la lista de amigos a los que todavia no -}
<b>fun</b> (S: Set <b>of</b> Amigo) <b>ret</b> res: List <b>of</b> Amigo
	<b>var</b> S_aux: Set <b>of</b> Amigo
	<b>var</b> a: Amigo
	<b>var</b> dia_regreso: nat

	S_aux := copy_set(S)
	res := empty_list()

	<b>while not</b> empty_set(S_aux) <b>do</b>
		a := elegir_amigo(S_aux)
		dia_regreso := a.fecha_regreso
		addr(res, a)
		elim(S_aux, a)
		descartar_amigos(S_aux, dia_regreso)
	<b>od</b>
	destroy_set(S_aux)
<b>end fun</b>


{- Mientras haya amigos a los que todavia no les preste el celular elijo
al que vuelva mas rapido de su viaje. Uso get para ir tomando un amigo
aleatorio y comparo sus fechas de regreso (Notar que ya estan todas
dadas) y selecciono al que vuelve mas rapido, descartando con elim
a los que no sirven -}
<b>fun</b> elegir_amigos(S: Set <b>of</b> Amigo) <b>ret</b> res: Amigo
	<b>var</b> S_aux: Set <b>of</b> Amigo
	<b>var</b> a: Amigo
	<b>var</b> menor_dia_regreso: nat

	S_aux := copy_set(S)
	menor_dia_regreso := ∞

	<b>while not</b> empty_set(S_aux) <b>do</b>
		a := get(S_aux)
		<b>if</b> a.fecha_regreso &lt; menor_dia_regreso <b>then</b>
			menor_dia_regreso := a.fecha_regreso
			res := a
		<b>fi</b>
		elim(S_aux, a)
	<b>od</b>
	destroy_set(S_aux)
<b>end fun</b>


{- Si la fecha de partida de un amigo es menor o igual a la fecha en la
que me devuelven el celular entonces no se lo puedo prestar, queda
descartado -}
<b>proc</b> descartar_amigos(<b>in/out</b> S: Set <b>of</b> Amigo, <b>in</b> fecha_regreso: nat)
	<b>var</b> S_aux: Set <b>of</b> Amigo
	<b>var</b> a: Amigo

	S_aux := copy_set(S)

	<b>while not</b> empty_set(S_aux) <b>do</b>
		a := get(S_aux)
		<b>if</b> a.fecha_partida &lt;= fecha_regreso <b>then</b>
			elim(S, a)
		<b>fi</b>
		elim(S_aux, a)
	<b>od</b>
	destroy_set(S_aux)
<b>end proc</b></code></pre>
