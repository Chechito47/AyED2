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

## Algoritmo de seleccion (selection sort)
Selecciono el **menor numero entre 1 y n** y lo comparo con el primero y ordeno, luego con el segundo mas pequeño, luego con el tercero mas pequeño y asi...

## Algortitmo de ordenacion por incercion ()
Es la forma en que ordenariamos las cartas en la mano.  
Comenzamos desde el segundo elemento, a este lo comparamos con el primero y lo ordenamos segun quien sea mas grande. Luego tomamos el tercer elemento y lo comparamos con los dos primeros (que ya estan ordenados) y lo ubicamos segun corresponda. Y asi hasta terminar con todos los elementos.  
*Ejemplo:* [5, 2, 9, 1, 5, 6]  
Tomo el segundo elemento y lo comparo con el primero
[5, **2**, 9, 1, 5, 6]
[2, 5, 9 ,1, 5, 6]
