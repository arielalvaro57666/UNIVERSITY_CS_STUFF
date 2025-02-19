**Matching** 
- Dado un grafo G
- Un Matching es un subgrafo M de G
- y todos los vertices tienen solo un lado (d(x) = 1). O sea todos son de grado = 1
- Por ejemplo un trabajador se le puede asignar una sola tarea

**Matching Maximal**
- Queremos encontrar un matching que sea maximal  
- O sea la mayor cantidad de emparejamientos posibles 1 a 1
- $|E(M)| ≥ |E(M')|$ $∀ \ M'$ mathing en $G$
- **En nuestro caso trabajaremos sobre un grafo bipartito** $G=(X \ U \ Y, E)$
	- 2 conjuntos **X e Y**

**Matching Perfecto** 
- Un Matching es perfecto si los V(M) = V(G) (|X| = |Y|)

**Matching Completo**
- M es matching completo de X a Y si tiene todos los vertices de X
- M es matching completo de Y a X si tiene todos los vertices de Y

**Conjunto de vecinos de un subconjunto de vertices**
- Grafo G y subconjunto de vertices S => $Γ(S)$ 
- Los vecinos hacia adelante de los vertices de S
- S en nustros ejercicios sera el corte de no llegar a t de los vertices de X

**Teorema de Hall (Provee forma de certificar que no existe matching perfecto)**
- Sea G un grafo bipartito y para todo subcojunto S de un conjunto (X o Y)
- $∃$ Matching completo de X a Y <=> $|S| ≤ |Γ(S)|\ ∀ \ S ⊆ X$ 
- O sea debe tener almenos tantos vecinos como el tamaño de S
- O sea si de X tomo {A,B,C,D} => debe tener almenos tantos vecinos como vertices de S ya que si hubieran menos entonces el emparejamiento no seria unico
- $S = \ todas\ las \ filas \ marcadas$ 
- $Γ(S) = todas \ las \ columnas \ marcadas$

**Teorema del matrimonio**
- Dice que todo grafo bipartito regular iene un matching perfecto
## Algoritmo Matching Maximal

**Pasos**
1. Dado un grafo bipartito $G=(X \ U \ Y, E)$. **Construimos una Network** $N=N(G)$ 
	- Esa netowrk tendra los vertices de G mas s,t
	- Esta network tendra los lados que ya iban de los vertices de X a Y
	- y  lados desde s a todos los de X (forward)
	- y lados desde t a todos los de Y  (forward)
	- **Todos los lados tienen capacidad = 1**
2. Correr Dinic sobre esa N hasta el primer flujo bloqueante
3. Continuar con Edmonds Karp hasta que no se pueda llegar a t
4. El matching maximal se compone de los vertices de X e Y que tienen un lado con flujo = 1 

**Ejemplo**

- Sea G y transformamos G en una N(G)
![[Pasted image 20250216131340.png]]![[Pasted image 20250216131403.png]]

- Veamos la N como una matriz de adyacencia pero solo teniendo en cuenta los vertices que van desde un conjunto a otro
![[Pasted image 20250216131511.png]]

5. Correr Dinic hasta el primer flujo bloqueante
	- Sabemos que todos los lados tienen capacidad maxima de 1
	- Y es facil viendo el grafo de la network que la network auxiliar es el N
	- ya que todos hacen un progreso hacia t
	- pero debido que los lados sx y yt se saturaran solo habra 4 caminos en este caso

	- sA1t:1, sB3t:1 , sC5t:1, sD4t: 1 (listo flujo bloqueante alcanzado)
	- Entonces por el momento los lados que van al matching son:
	- $A1:\cancel{1} 0$, $B3:\cancel{1} 0$, $C5:\cancel{1} 0$  $D4:\cancel{1} 0$
	- Ademas sA, sB, sC, sD, 1t, 3t, 5t y 4t estan saturados

6. Continuar con Edmonds Karp

BFS -> [s, E(s), 3(E), 4(E),  B(3⁻), D(4⁻), 5(D), C(5⁻), -] 
Como no llegamos a t el matching no se puede extender
El matching maximal son los lados mencionados anteriormente unicamente
![[Pasted image 20250216135850.png]]

## Matching maximal con matching inicial
- Veamos ahora de encontrar el matching maximal
- Pero con un matching inicial ( a ojo)
![[Pasted image 20250217214059.png]]
![[Pasted image 20250217214159.png]]
![[Pasted image 20250217214659.png]]
Esto sugiere que  Ai, Bii y Diii tienen flujo = 1
Sigamos como si estuvieramos con edmonds karp
1° [s, C(s), I(C), II(C), A(I⁻), B(II⁻), III(A), IV(A), D(III⁻), t(IV)]
=> sC $\overleftarrow{IA}$ IV t
entonces teniamos $AI: 0, BII:0, DIII:0$ 
Veamos ahora  $CI:0, AI:1, AIV:0$

=> AIV, BII, CI, DIII como lados para el matching maximal
## Matching maximal con matching inicial (matriz)
$$
\begin{matrix}
 & I & II & III & IV \\
A & 1 & 0 & 1 & 1\\
B & 0 & 1 & 0 & 1 \\
C & 1 & 1 & 0 & 0  \\
D & 0 & 1 & 1 & 0\\
\end{matrix}
$$
Ahora vamos a hacer lo mismo pero con el metodo 
- Armemos un matching inicial y marcamos con s la filas sin matcehar
- Corremos BFS(s) 
- E iremos etiquetando pero el orden de BFS que tendremos en cuenta
$$
\begin{matrix}
 & I & II & III & IV \\
A & \fbox{1} & 0 & 1 & 1 &\\
B & 0 & \fbox{1} & 0 & 1 &\\
C & 1 & 1 & 0 & 0  &s\\
D & 0 & 1 & \fbox{1} &0 &\\
  &   &   &          &
\end{matrix}
$$
1. Entonces ahora corramos BFS y etiquetemos hasta encontrar una columna etiquetada libre
BFS -> s, C(s), I(C), II(C), A(I⁻), B(II⁻), III(A), IV(A), t(IV)

$$
\begin{matrix}
 & I & II & III & IV \\
A & \fbox{1} & 0 & 1 & 1 &I\\
B & 0 & \fbox{1} & 0 & 1 &II\\
C & 1 & 1 & 0 & 0  &s\\
D & 0 & 1 & \fbox{1} &0 &\\
  & C  &C   & A         &A
\end{matrix}
$$
- Veamos abajo fuimos etiquetando resulta que una columna etiquetada no tiene 1 marcados
- Esta columna etiquetada con A representa $\overrightarrow{AIV}$ por lo tanto estamos mandando flujo a traves de el y por ende pertenece al matching
- Ademas A fue añadido por I (backward) por ende ese lado deja de tener el 1 marcado (solo por decir, no hace falta pensarlo solo marcamos el 1 como dijimos)
$$
\begin{matrix}
 & I & II & III & IV \\
A & 1 & 0 & 1 & \fbox{1} &I\\
B & 0 & \fbox{1} & 0 & 1 &II\\
C & 1 & 1 & 0 & 0  &s\\
D & 0 & 1 & \fbox{1} &0 &\\
  & C  &C   & A         &A
\end{matrix}
$$
- Ademas la columna $I$ "quedo libre" veamos su etiqueta (I fue añadido por C)
- por ende tiene flujo, lo marcamos ya que pertenece al matching
- $$
\begin{matrix}
 & I & II & III & IV \\
A & 1 & 0 & 1 & \fbox{1} &I\\
B & 0 & \fbox{1} & 0 & 1 &II\\
C & \fbox{1} & 1 & 0 & 0  &s\\
D & 0 & 1 & \fbox{1} &0 &\\
  & C  &C   & A         &A
\end{matrix}
$$En este caso solo se requirio una cola BFS para conseguir este matching

**Resumen**
1. Construir matching incial a vista
2. Si etiquetamos una columna sin 1 marcados terminamos
	- O sea le agregamos el 1 correspondiente y liberamos el otro 1
## Matching con pesos

**Explicacion**
- Tenemos un grafo bipartito con conjuntos X e Y , Tendremos siempre $|X| = |Y|$ (para mayor facilidad)
- **Matriz de pesos** 
	- $A_x,_y$   es el costo de asignar un trabajo (y) al trabajador (x)
	- Filas = X
	- Columnas = Y

**Objetivo**
- Entre todos los matchings perfectos posibles queremos hallar el mas optimo
- Optimizaciones que nos pueden pedir:
- Los pesos son costos
	- Minimizar el mayor costo presente en el matching (El mayor costo presente)
	- Minimizar el costo total en el matching (la suma de los costos)
- Los pesos son ganancias
	- Maximizar la mayor ganancia
	- Maximizar la ganancia total

## Minimizar el mayor costo (algoritmo)

**Pasos**
1. Tomar el conjunto de numeros presentes a modo de array y tomar el indice del medio como umbral m
	- m = "maximo costo de esa diagonal"
2. Vemos si existe matching perfecto cuyo costo mayor sea < m => para esto Armamos la matriz A' que nos ayudara a visualizar a los que son menores o iguales que nuestro umbral
$$
A'_{i,j} =
\begin{cases}
1 & A_{i,j} \leq m \\
0 & A_{i,j} > m
\end{cases}
$$
- Los costos que sean menor o igual a m le asignamos 1
- Los costos que sean mayor o iguales a m le asignamos 0
1. Con esa matriz marcamos un matching inicial (si no es que conseguimos uno perfecto a vista) y corremos edmonds karp para ir obteniendo el **matching perfecto** 
	1. Si no tiene matching perfecto hay que tomar un umbral mas alto, o sea la mitad superior entre el indice del m testeado y el ultimo de la lista low = m high = len - 1
	2. Si encontramos matching perfecto bajamos al umbral y tomamos la mitad menor m, o sea entre low=0 y high = m
2. Repetimos desde 1 hasta no encontrar $m < m'$

No olvidar que estamos buscando un matching
- **No debe a ver mas que un emparejamiento por cada x e y, deben tener grado 1**
- En la matriz es facil ver que no se deberia compartir columnas para la seleccion de los lados que perteneceran al matching




**Ejemplo**
$$
\begin{matrix}
 & I & II & III & IV & V & VI & VII & VIII \\
A & 5 & 9 & 9 & 8 & 7 & 10 & 4 & 15 \\
B & 8 & 10 & 9 & 10 & 6 & 7 & 1 & 15 \\
C & 8 & 8 & 9 & 8 & 7 & 7 & 5 & 1 \\
D & 9 & 1 & 1 & 5 & 9 & 8 & 4 & 4 \\
E & 1 & 1 & 4 & 1 & 4 & 5 & 6 & 7 \\
F & 8 & 5 & 9 & 8 & 6 & 10 & 8 & 10 \\
G & 5 & 7 & 5 & 4 & 1 & 1 & 7 & 7 \\
H & 9 & 1 & 9 & 10 & 4 & 9 & 9 & 9 \\
\end{matrix}
$$

1° Numeros que aparecen
[1,4,5,6,7,8,9,10,15] son 9 numeros y la mitad de la lista es el indice 4 
=> 7 es nuestro umbral
- Damos el siguiente matching inicial
$$
\begin{matrix}
 & I & II & III & IV & V & VI & VII & VIII \\
A & \fbox{1} & 0 & 0 & 0 & 1 & 0 & 1 & 0 &\\
B & 0 & 0 & 0 & 0 & \fbox{1} & 1 & 1 & 0 &\\
C & 0 & 0 & 0 & 0 & 1 & \fbox{1} & 1 & 1 &\\
D & 0 & \fbox{1} & 1 & 1 & 0 & 0 & 1 & 1 &\\
E & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 1 & 1 &\\
F & 0 & 1 & 0 & 0 & 1 & 0 & 0 & 0 &\\
G & 1 & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 1 &\\
H & 0 & 1 & 0 & 0 & 1 & 0 & 0 & 0 &

\end{matrix}
$$
- Ejecutar BFS para matching perfecto
- 1° $s, F(s), H(s), II(F), V(F), D(II⁻), B(V), III(D), IV(D), VII(D), VIII(D), VI(B), E(III), G(IV), t(VII)$
	- => $s F\overleftarrow{IID}\ VII\ t$

$$
\begin{matrix}
 & I & II & III & IV & V & VI & VII & VIII \\
A & \fbox{1} & 0 & 0 & 0 & 1 & 0 & 1 & 0 &\\
B & 0 & 0 & 0 & 0 & \fbox{1} & 1 & 1 & 0 &\\
C & 0 & 0 & 0 & 0 & 1 & \fbox{1} & 1 & 1 &\\
D & 0 & 1 & 1 & 1 & 0 & 0 & \fbox{1} & 1 &\\
E & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 1 & 1 &\\
F & 0 & \fbox{1} & 0 & 0 & 1 & 0 & 0 & 0 &\\
G & 1 & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 1 &\\
H & 0 & 1 & 0 & 0 & 1 & 0 & 0 & 0 &

\end{matrix}
$$
- 2° 
- $s, H(s), II(H),  F(II), B(V), VI(B), VII(B), C(VI), D(VII), VIII(C) t(VIII)$
- $s H\ \overleftarrow{VB}\ \overleftarrow{VIC}\ VIII\ t$
$$
\begin{matrix}
 & I & II & III & IV & V & VI & VII & VIII \\
A & \fbox{1} & 0 & 0 & 0 & 1 & 0 & 1 & 0 &AI: 5\\
B & 0 & 0 & 0 & 0 & 1 & \fbox{1} & 1 & 0 &BVII:7\\
C & 0 & 0 & 0 & 0 & 1 & 1 & 1 & \fbox{1} &CVIII:1\\
D & 0 & 1 & 1 & 1 & 0 & 0 & \fbox{1} & 1 &DVII:4\\
E & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 1 & 1 &EIII:4\\
F & 0 & \fbox{1} & 0 & 0 & 1 & 0 & 0 & 0 &FII:5\\
G & 1 & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 1 &GIV:4\\
H & 0 & 1 & 0 & 0 & \fbox{1} & 0 & 0 & 0 &HV:3

\end{matrix}
$$
- Como vemos llegamos a un matching perfecto donde el mayor costo es 7
- Bajemos el umbral
- Entonces veamos la lista [1,4,5,6,7,8,9,10,15] => [1,4,|5|,6,7] m = 5 
- Armemos A' y demos el matching inicial, resulto ser matching perfecto el inicial
$$
\begin{matrix}
 & I & II & III & IV & V & VI & VII & VIII \\
A & \fbox{1} & 0 & 0 & 0 & 0 & 0 & 1 & 0 & AI:5\\
B & 0 & 0 & 0 & 0 & 0 & 0 & \fbox{1} & 0 & BVII:1\\
C & 0 & 0 & 0 & 0 & 0 & 0 & 1 & \fbox{1} & CVIII:1\\
D & 0 & 1 & 1 & \fbox{1} & 0 & 0 & 1 & 1 & DIV: 5\\
E & 1 & 1 & \fbox{1} & 1 & 1 & 1 & 0 & 0 & EIII:4\\
F & 0 & \fbox{1} & 0 & 0 & 0 & 0 & 0 & 0 & FII:5\\
G & 1 & 0 & 1 & 1 & 1 & \fbox{1} & 0 & 0 & GVI:1\\
H & 0 & 1 & 0 & 0 & \fbox{1} & 0 & 0 & 0 & HV:3\\
\end{matrix}
$$
- Debemos bajar el umbral [1,|4|,5] a 4
- Armemos A'
$$
\begin{matrix}
 & I & II & III & IV & V & VI & VII & VIII \\
A & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 &\\
B & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 &\\
C & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 &\\
D & 0 & 1 & 1 & 0 & 0 & 0 & 1 & 1 &\\
E & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 &\\
F & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 &\\
G & 0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 &\\
H & 0 & 1 & 0 & 0 & 1 & 0 & 0 & 0 &

\end{matrix}
$$
- F tiene la fila en 0 (no hay lados hacia adelante) 
- Como dice teorema de Hall
- Como para umbral = 4  no hay matching
- => El matching de umbral = 5 es la solucion logrando minimzar el mayor costo a 5

## Algoritmo Hungaro (Minimizar suma de los costos)

**Explicacion**
- Dada una matriz de costos queremos hallar un matching que minimice la suma de costos


![[Pasted image 20250218160401.png]]
- Esto representa la suma de los costo $C_{i,j}$ para todas las asignaciones que forman parte del matching

**Observacion 1**
- Si todos los costos $C_{i,j}$ son positivos y existe un matching M tal que para toda asignacion $ij ∈E(M)$ 
- => $C_{i,j} = 0$ entonces M es un matching de suma minima

**Observacion 2**
- Si tomamos una matriz de costos C y restamos una constante a toda una fila o columna
- Obtenemos una matriz $\tilde{C}$ => Un matching M minimiza la suma de costos para C <=> tambien la minimiza para $\tilde{C}$  

**Pasos**
1. Restar a cada fila su minimo (esto introduce al menos un 0)
2. Restar cada columna su minimo 
3. Buscar un matching de Ceros $M_0$ 
4. Si encontramos $M_0$ fin del algoritmo Sino recurrir al teorema de Hall
5. Certificamos que no hay matching perfecto de 0s con el teorema de hall
6. Modificamos la matriz 
	- La idea es agregar mas ceros en forma compatible con la matriz original usando la **Observacion 2** es decir restamos constantes adecuadas en filas o columnas
	- El S con |S| > |Γ(S)|=> No hay suficientes 0s en las filas de S
	

**Ejemplo**
$$
\begin{matrix}
 & I & II & III & IV & V \\
A & 7 & 6 & 5 & 10 & 7 \\
B & 2 & 7 & 4 & 2 & 10 \\
C & 4 & 5 & 5 & 15 & 11 \\
D & 10 & 12 & 1 & 10 & 2 \\
E & 12 & 15 & 13 & 9 & 10 \\
\end{matrix}
$$
1. Resto primero a cada fila su minimo y luego cada columna su minimo
$$
\begin{matrix}
 & I & II & III & IV & V \\
A & 2 & 0 & 0 & 5 & 1 \\
B & 0 & 4 & 2 & 0 & 7 \\
C & 0 & 0 & 1 & 11 & 6 \\
D & 9 & 10 & 0 & 9 & 0 \\
E & 3 & 5 & 4 & 0 & 0 \\
\end{matrix}
$$
1. Busco Matching de 0s
$$
\begin{matrix}
 & I & II & III & IV & V \\
A & 2 & \fbox{0} & 0 & 5 & 1 \\
B & 0 & 4 & 2 & \fbox{0} & 7 \\
C & \fbox{0} & 0 & 1 & 11 & 6 \\
D & 9 & 10 & \fbox{0} & 9 & 0 \\
E & 3 & 5 & 4 & 0 & \fbox{0} \\
\end{matrix}
$$
Como lo obtuvimos este matching minimiza la suma


**Ejemplo donde no encuentro matching de 0s**
- Supongamso que esta el matriz luego de restar los minimos a las filas y columnas
$$
\begin{matrix}
 & I & II & III & IV \\
A & \infty & 5 & 9 & 7 & \\
B & 9 & 5 & 5 & 8 &  \\
C & 7 & 5 & 9 & 5 &  \\
D & \infty & 5 & 4 & 7 &  \\
\end{matrix}
$$

1. Resto el minimo a las filas y columnas
$$
\begin{matrix}
 & I & II & III & IV \\
A & \infty & 0 & 4 & 2 & \\
B & 2 & 0 & 0 & 3 &  \\
C & 0 & 0 & 4 & 0 &  \\
D & \infty & 1 & 0 & 3 &  \\
\end{matrix}
$$
1. Doy una matching inicial de 0s
$$
\begin{matrix}
 & I & II & III & IV \\
A & \infty & \fbox{0} & 4 & 2 & \\
B & 2 & 0 & \fbox{0} & 3 &  \\
C & \fbox{0} & 0 & 4 & 0 &  \\
D & \infty & 1 & 0 & 3 &  \\
\end{matrix}
$$
- Intentamos extenderlo
BFS -> $s, D(s), III(D), B(III), II(B), A(II) TERMINA$
A demas S={A,B,D} da Γ(S) = {II, III}
No da matching perfecto ahora en el algoritmo hungaro para acelerar el proceso trazamos lineas que puedan trazar la mayor cantidad de 0s
![[Pasted image 20250218182530.png]]
- Si nos fijamos 3 lineas < n = 4 => debemos hacer aparecer nuevos 0s
- 