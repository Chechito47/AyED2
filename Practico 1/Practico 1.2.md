![ScreenShot](Imagenes%20practico%201.2/ej1.png)

![ScreenShot](Imagenes%20practico%201.1/ej4.png)

```pascal
a.1) [7, 1, 10, 3, 4, 9, 5]

VOY A USAR == PARA SEPARAR LOS PAQUETITOS QUE DIVIDO

Entonces como tiene 7 elementos, hago la division haciendo mid=(n-1)/2 = 3
[==7, 1, 10, 3== , ==4, 9, 5==]
Vuelvo a dividir a la mitad porque no esta ordenada
[==7, 1==, ==10, 3==, ==4, 9, 5==]
Vuelvo a dividir a la mitad porque no esta ordenada
[==7==, ==1==, ==10==, ==3==, ==4, 9, 5==]

Comienzo a ordenar, tomo los paquetitos de a dos empezando de la izq y voy intercalando
[==1==, ==7==, ==10==,  ==3==, ==4, 9, 5==]
[==1, 7==, ==10==,  ==3==, ==4, 9, 5==]
[==1, 7==, ==3==,  ==10==, ==4, 9, 5==]
[==1, 7==, ==3, 10==, ==4, 9, 5==]
[==1, 3, 7, 10==, ==4, 9, 5==]

Ahora sigo con los paquetes de la derecha, divido a la mitad
[==1, 3, 7, 10==, ==4, 9==, ==5==]
Divido a la mitad
[==1, 3, 7, 10==, ==4==, ==9==, ==5==]
Intercalo
[==1, 3, 7, 10==, ==4, 9==, ==5==]
[==1, 3, 7, 10==, ==4, 5, 9==]

Intercalo las dos mitades
[==1, 3, 4, 5, 7, 9, 10==]



a.2) [5, 4, 3, 2, 1]
Divido a la mitad => 5/2=3
[==5, 4, 3==, ==2, 1==]
Divido a la mitad de nuevo
[==5==, ==4, 3==, ==2, 1==]
Divido a la mitad de nuevo
[==5==, ==4==, ==3==, ==2, 1==]

Intercalo
[==3, 4, 5==, ==2, 1==]
[==3, 4, 5==, ==1, 2==]

Intercalo las dos mitades
[==1, 2, 3, 4, 5==]



a.3) [1, 2, 3, 4, 5]
Divido a la mitad => 5/2=2+1=3
[==1, 2, 3==, ==4, 5==]
Divdo a la mitad de nuevo
[==1, 2==, ==3==, ==4, 5==]
Divido a la mitad otra vez mas
[==1==, ==2==, ==3==, ==4, 5==]

Intercalo
[==1, 2==, ==3==, ==4, 5==]
[==1, 2, 3==, ==4, 5==]

Intercalo a la otra parte
[==1, 2, 3, 4, 5==]



b) Recordemos como es merge_sort_rec:
{Pre: n ≥ rgt ≥ lft > 0 ∧ a = A}
proc merge_sort_rec (in/out a: array[1..n] of T, in lft,rgt: nat)
	var mid: nat
	if rgt > lft → mid:= (rgt+lft) ÷ 2
		merge_sort_rec(a,lft,mid)
		{a[lft,mid] permutación ordenada de A[lft,mid]}
		merge_sort_rec(a,mid+1,rgt)
		{a[mid+1,rgt] permutación ordenada de A[mid+1,rgt]}
		merge(a,lft,mid,rgt)
		{a[lft,rgt] permutación ordenada de A[lft,rgt]}
	fi
end proc
{Post: a permutación de A ∧ a[lft,rgt] permutación ordenada de A[lft,rg]}

Tenemos que dar las llamadas a este procedimiento con los items del inciso a.1

a.1=[7, 1, 10, 3, 4, 9, 5]
Veamos los datos qu tendriamos,
a.1=[7, 1, 10, 3, 4, 9, 5]
lft=1 (primero del paquete de la izq)
rgt=4 (ultimo del paquete de la izq)
=> mid= (1+4)/2 = 2.5=>2
=> el primer paso son los 3 merge_sort_rec, como es recursivo se ejecuta uno
hasta que no cumple mas con las condicionesy recien ahi se ejecuta el que sigue

Hago el primer merge_sort_rec con los primeros datos
{merge_sort_rec(a.1, 1, 4)}
[==7, 1, 10, 3==, 4, 9, 5]

Mis nuevos datos son estos, el rgt es el el ultimo elemento de hacer la mitad
lft=1, rgt=2 => mid=(1+2)/2=2 (redondeo para abajo)
{merge_sort_rec(a.1, 1, 2)} (hago el primer merge_sort_rec de nuevo)
[==7, 1==, ==10, 3==, 4, 9, 5]

lft=1, rgt=2 => mid=1
{merge_sort_rec(a.1, 1, 1), merge_sort_rec(a.1, 2, 2), merge(a.1, 1, 1, 2)}
```