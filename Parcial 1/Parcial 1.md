![ScreenShot](Imagenes%20parcial%201/ej1.png)

```pascal
1) a) Nos dice insertion sort, es el compara uno a uno con el los elmentos ya ordenados:

[3, 17, 2, 10, 5, 8, 3]
Tomo el 17, esta ordenado, no hago cambios
[3, 17, 2, 10, 5, 8, 3]
Tomo el 2 => 2<17 si => swap
[3, 2, 17, 10, 5, 8, 3]
Tomo el 2 => 2<3 si => swap
[2, 3, 17, 10, 5, 8, 3]
Tomo el 10 => 10<17 si => swap
[2, 3, 10, 17, 5, 8, 3]
Tomo el 10 => 10<3 no => no hago nada
[2, 3, 10, 17, 5, 8, 3]
Tomo el 5 => 5 < 10 si => swap
[2, 3, 10, 5, 17, 8, 3]
Tomo el 5 => 5 < 10 si => hago swap
[2, 3, 5, 10, 17, 8, 3]
Tomo el 5 => 5<3 no => no hago nada
[2, 3, 5, 10, 17, 8, 3]
Tomo el 8 => 8 < 17 si => swap
[2, 3, 5, 10, 8, 17, 3]
Tomo el 8 => 8 < 10 si => swap
[2, 3, 5, 8, 10, 17, 3]
Tomo el 8 => 8 < 5 no => no hago anda
[2, 3, 5, 8, 10, 17, 3]
Tomo el 3 => 3<17 y 3<10 y 3<8 y 3<5 => hago swaps
[2, 3, 3, 5, 10, 8, 17]



b) Me pide ubicar un elemento en un arreglo de manera tal que los menores queden antes que el elemento y que devuelva cuantos menores hay

proc p (in/out a: array[1..N] of int, in e: int, out k: nat)
	var menores := 1
	for i := 1 to n do
		if (a[i] < e)
			menores++
			swap(a, i, menores)
		fi
	od
k := menores


[2, 4, 1] e=3
menores=0
for 1 to 3
	2 < 3 si
	menores=1
	swap(a, 1, 1) -> no cambia
for 2 to 3
	4 < 3 no
for 3 to 3
	1 < 3 si
	menores = 2
	swap(a, 3, 2)
	[2, 1, 4]

k=2
```


![ScreenShot](Imagenes%20parcial%201/ej2.png)

```pascal
mientras la lista no sea vacia, hace que b apunte al elementelemento que le sigue a esa lista. Si la lista b no sea viacia hace que la siguiente posicion de a sea la siguiente de b

Termina haciendo que a sea la siguiente 




```

![ScreenShot](Imagenes%20parcial%201/ej3.png)
