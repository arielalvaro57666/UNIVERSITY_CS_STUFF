***
**Conceptos Clave**
- **Flujos** - **Red de transporte desde un productor a un consumidor**
	- Se representa con un grafo dirigido la red de transporte
	- Cada lado tiene un limite de carga
- **Objetivo** - **max flow -> transportar la mayor cantidad de bienes desde los productores a los consumidores**

- **Grafo dirigido** - `G = (V,E)` y `E ⊆ VxV` => $\overrightarrow{xy}$
	 - (lados son pares ordenados xy != yx) 

- **Vecinos de un vertice** 
	- **Vecinos forward** -  **Γ⁺(x) = {y ∈ V | $\overrightarrow{xy}$ ∈ E}**
	- **Vecinos backward** - **Γ⁻(x) = {y ∈ V | $\overrightarrow{yx}$ ∈ E}**

- **Network** - (V,E,C,s,t)
	- `(V,E)` Es el grafo dirigido
	- `C: E -> R⁺` - Funcion que denota la capacidad maxima de un lado
	- `s,t ∈ V` - s (nodo productor), t (nodo consumidor)

- **Flujo de una Network** - `f: E -> R⁺` 
	- **f denota el flujo actual que pasa por un lado** (sin pasar la capacidad dada por C)
	- f es flujo <=> 
	- `0 ≤ f(xy) ≤ c(xy) ∀ xy ∈ E` (“feasability”) (No excede su capacidad)
	- **∀ x ≠ s,t**
	$$\sum_{y \in \Gamma^+(x)} f(\overrightarrow{xy}) = \sum_{y \in \Gamma^-(x)} f(\overrightarrow{xy})
	$$
	- La suma de todo lo que sale de X = La suma de todo lo que entra por X

- **Operacion sobre conjuntos de V**-¿Cuanto se manda de un conjunto a otro conjunto de V?
	- Dado un grafo G, sean A,B ⊆ V
	- Entonces `φ` sera una funcion comodin para representar la sumatoria de los lados que empiezan en un conjunto A y llegan a un conjunto B

	- $\Phi(A, B) = \sum_{\substack{x \in A \\ y \in B \\ \overrightarrow{xy} \in E}} \phi(\overrightarrow{xy})$
	- esta **es la sumatoria** de aquellos lados que estan en A y llegan a B
	- Si x ∈ V podemos escribir
	- `φ(x,B) = φ({x},B)` (de x a B `φ(A,x) = φ(A,{x})` (del conjunto A a x)
	- **Flujo entrante y saliente de un nodo** 
		- $out_\phi (x) = \phi({x},V)$ 
		- $in_\phi (x) = \phi(V,{x})$ 
	- Esto es meramente para entender la funciona φ que es definida con funciones que vimos
	- verlo nomas para entender
- **Funciones con la definicion de φ que vamos a utilizar** ( * ) 
	- $f(A,B)$ la sumatoria de los flujo f de los lados que empiazan en A y van a B
	- $CAP(A,B)$ Capacidad de los lados que empiezan en A y van a B
	- $out_f (x) = f({x},V)$
	- $in_f (x) = f(V,x)$
- **Valor de un flujo** - Cantidad total de informacion que se mando desde s a t
	- $v(f) = \sum_{z \in \Gamma(s)} f(\overrightarrow{sz})$
	- $v(f) = \text{out}_f(s) = \text{in}_f(t)$
	- Es simplemente la suma de todos los flujos que salen de s o todo los que entran en t
- **Corte**
	- Sea N una network = (V,E,C,s,t) 
	- Un subcojunto S ⊂ V es un corte Si => s ∈ S y t ∉ S
	- Por ejemplo $S_1 = {s}$, $S_2=V-{t}$
	- **Capacidad de un corte**
		- La capacidad de un corte S es 
		- $CAP(S)=c(S, V-S) = c(S,S)$ (todo lo que puede salir de S hacia afuera)
		- ![[Pasted image 20250211111425.png]]
		- Veamos dos cortes de la network de arriba `{s}` y `{s,A}`
		- => $CAP({s}) = \overrightarrow{sA} = 10000$
		- => $CAP({s,A}) = \overrightarrow{AB} = 1$
	- **Corte minimal**
		- El corte minimal es el corte con la menor capacidad entre todos los cortes que hay
	- **Valor de flujo y corte**
	- $v(f) = f(S, V-S) = f(V - S, S)$
- **Flujo Maximal**
	- f es maximal si v(f) es el maximo entre todos los flujos encontrados
	- **Max flow, Min cut**
		1. Si f es un flujo y S es un corte => $v(f) ≤ CAP(S)$
		2. Si $v(f) = CAP(S)$ => **f es maximal y S minimal**
		3. Si f es maximal => ∃ un corte S con v(f) = CAP(S)
- **Camino aumentante** (Buscar un camino el cual aumente el flujo $sx_0....,x_r,t$)
	- En una network N un camino aumentate es una sucesion de vertices
	- $x_0 = s,x_1,x_2,...,x_r = t$ donde ∀ 0≤i≤r se cumple
		1. $\overrightarrow{x_i x_{i+1}} \in E \text{ y } f(\overrightarrow{x_i x_{i+1}}) < c(\overrightarrow{x_i x_{i+1}})$ (Ningun lado hacia adelante excede la capacidad)
		2. $\overrightarrow{x_{i+1} x_i} \in E \text{ y } f(\overrightarrow{x_{i+1} x_i}) > 0$ (Los lados hacia atras no deben estar vacios)
	- O sea buscamos un camino de s a t que se puede mandar (devolver para mandar en algun caso)
	- **Camino forward** - Podemos mandar flujo si no esta saturado
	- **Camino backward** - Podemos devolver flujo para mandarlo por otro lado
		- O sea restauramos la capacidad en ese lado y la mandamos por otro lado
	- **Flujo a partir del camino aumentante obtenido**
		- Una vez visto el flujo aumentante tenemos que ver cuanto flujo mandar
		- pues tenemos una serie de valores $\epsilon_i$ 
$$
\epsilon_i =
\begin{cases} 
    c(\overrightarrow{x_i x_{i+1}}) - f(\overrightarrow{x_i x_{i+1}}) & \text{Si } x_i x_{i+1} \text{ es un lado forward} \\
    f(\overrightarrow{x_{i+1} x_i}) & \text{Si } x_i x_{i+1} \text{ es un lado backward}
\end{cases}
$$
	-[!] Yo lo que quiero mandar en mi camino aumentate es claramente el minimo $\epsilon_i$ 
		- **O sea el cuello de botella del lado con menor capacidad para mandar** por el camino aumentante obtenido

**Como dibujar una network linda**
- A la hora de dibujar una network debemos dibujarla por niveles para que se vea ordenada y entendible
- Debemos dibujar los nodos por nivel primero
- El nivel de un nodo es la menor distancia (en lados) desde s al nodo 
- Para asignar un nivel a un nodo supongamos s esta en en nivel 0
- Y si la distancia de un nodo a s la menor distancia puede ser un solo lado entonces el nivel sera 1, si es 2 entonces 2 y asi.
## Algoritmo Ford-Fulkerson para max flow

**Pasos**
1. Si existe un camino aumentante de s a t otherwise saltar al paso 4
2. Hacemos la resta de capacidades lado forward y suma en el lado backward
3. Ir al paso 1
4. Terminar

**Ejemplo**

![[Pasted image 20250212111802.png]]
$\overrightarrow{sa}: 10$      $\overrightarrow{ab}: 4$     $\overrightarrow{bt}: 10$     $\overrightarrow{cd}: 9$     $\overrightarrow{db}: 6$         
$\overrightarrow{sc}: 10$      $\overrightarrow{ac}: 2$                                  $\overrightarrow{dt}: 10$   
	    $\overrightarrow{ad}: 8$ 

$sADt$ (8) -> $sCDt$ (2) -> $sC\overleftarrow{DA}Bt$ (4) -> $sADBt$ (2) -> $sCDBt$ (3) -> Termina
$\overrightarrow{sa}: \cancel{10} \cancel{2} 0$              $\overrightarrow{ab}: \cancel{4} 0$               $\overrightarrow{bt}: \cancel{10} \cancel{6} \cancel{4} 1$     $\overrightarrow{cd}: \cancel{9} \cancel{7} \cancel{3} 0$     $\overrightarrow{db}: \cancel{6} \cancel{4} 1$         
$\overrightarrow{sc}: \cancel{10} \cancel{8} \cancel{4} 1$           $\overrightarrow{ac}: 2$                                                                $\overrightarrow{dt}: \cancel{10} \cancel{2} 0$   
		        $\overrightarrow{ad}: \cancel{8} \cancel{0} \cancel{4} 2$ 

Entonces veamos el valor del flujo
$v(f) = f(\overrightarrow{sA}) + f(\overrightarrow{sC}) = 19$  (todo lo que sale de s)

## Algoritmo Enmonds Karp para max flow

Es correr ford-fulkerson con una pequeña modificacion para garantizar que termina

**Pasos**
1. Armamos la tabla de capacidades
2. Ejecutamos BFS y inicializamos la cola del BFS con el nodo s
3. Vemos los vecinos de x (desencolado en orden de la cola) (xz) y añadimos los vecinos a la cola si
	1. z no pertenece a la cola
	2. ∃ lado forward $\overrightarrow{xz}$ (se puede mandar flujo de x a z)
	3. ∃ lado backward $\overrightarrow{zx}$ ( se puede devolver flujo de x a z)
4. Si añadimos t a la cola termina BFS otherwise si no llega a ser agregado saltar a 7
5. Construimos el camino aumentante de s a t 
	- Para ello iremos construyendo en reversa desde t
	- y el orden es quien agrego a t, y asi hasta llegar a s
6. Luego buscamos el cuello de botella del flujo del camino para sumar y restar los flujos segun forward o backward
7. Terminar

**Resumido**
1. Llenar la cola BFS anotando el nodo que agrego ese nodo vertice(añadido por),...
2. Hacer reversa para encontrar camino aumentante
3. Sumar y Restar flujos
4. Repetir
5. Si finaliza calculamos el valor del flujo y chequeamos si es maximal (el corte minimal es la ultima cola que no llego a t)
	- Es maximal si la v(f) = CAP(S)
	- Para calcular esa CAP es todo lo que sale del corte hacia afuera o sea CAP(S,V-S)


**Ejemplo**

![[Pasted image 20250212122459.png]]
$$
\begin{aligned}
sA &\rightarrow \cancel{100}\cancel{20} \cancel{16} \cancel{6} 0 & AB &\rightarrow \cancel{150} \cancel{70} \cancel{66} \cancel{60} 50 & Bt &\rightarrow \cancel{80} 0  & Dt &\rightarrow \cancel{50} 0  & HI &\rightarrow \cancel{100} \cancel{90} 80 & KL &\rightarrow \cancel{100} \cancel{90} 80
\\
sC &\rightarrow \cancel{50} 0  & AC &\rightarrow \cancel{10} \cancel{0} 10  & CD &\rightarrow \cancel{50} \cancel{0} \cancel{6} 16  & EF &\rightarrow \cancel{100} \cancel{50} \cancel{40} 34  & Ht &\rightarrow \cancel{10} 0  & LP &\rightarrow \cancel{100} \cancel{90} 80
\\
sE &\rightarrow \cancel{50} 0  & BD &\rightarrow \cancel{100} \cancel{96} \cancel{90} 100  & CE &\rightarrow \cancel{20}\cancel{10}4  & Ft &\rightarrow \cancel{70} \cancel{20} \cancel{16} \cancel{6} 0 & ID &\rightarrow \cancel{100} \cancel{90} 80  & Pt &\rightarrow \cancel{100} \cancel{90} 80
\\
sG &\rightarrow \cancel{100} \cancel{90} \cancel{80} 70  & BJ &\rightarrow \cancel{100} \cancel{90} 80 & DF &\rightarrow \cancel{4} 0  & GH &\rightarrow \cancel{100} \cancel{90} \cancel{80} 70  & JK &\rightarrow \cancel{100} \cancel{90} 80
\end{aligned}
$$
1° BFS s $A(s), C(s), E(s), G(s), B(A), D(C), F(E), H(G), t(B)$ => Camino $sABt$: 80
2°BFS s, $A(s), C(s), E(s), G(s), B(A), D(C), F(E), H(G), J(B), t(D)$ -> Camino $sCDt$: 50
3°BFS s, $A(s), E(s), G(s), B(A), C(A), F(E), H(G), D(B), J(B), t(F)$ -> Camino $sEFt$: 50
4°BFS s, $A(s), G(s), B(A), C(A), H(G), D(B), J(B), E(C), t(H)$ -> Camino $sGHt$: 10
5°BFS s, $A(s), G(s), B(A), C(A), H(G), D(B), J(B), E(C), I(H), F(D), K(J), t(F)$-> $sABDFt$: 4
6°BFS s, $A(s), G(s), B(A), C(A), H(G), D(B), J(B), E(C), I(H), K(J), F(E), L(K), t(F)$ sACEFt:10
7°s,$A(s), G(s), B(A), H(G), D(B), J(B), I(H), C(D), K(J), E(C), L(K), F(E), P(L), t(F)$
- $sAB\overleftarrow{DC}EFt$: 6
8°s,$G(s),H(G),I(H), D(I), B(D), C(D), J(B), A(B), E(C), K(J), F(E), L(K), P(L), t(P)$
- $sGHI\overleftarrow{DB}JKLPt:10$ (10 porque el backward solo puede devolver 10)
9°s,$G(s),H(G),I(H), D(I), C(D), E(C), A(C), F(E), B(A), J(B), K(J), L(K), P(L), t(P)$ 
- $sGHI\overleftarrow{DCA}BJKLPt$: 10
10°s,$G(s), H(G), I(H), D(I), \overleftarrow{C(D)}, E(C), F(E), -(F)$ no llegamos a t **Termina**

**Valor del flujo ->** $v(f) = f(sA) + f(sC) + f(sE) + f(sG) = 100 + 50 + 50 + 30 = 230$

**¿Es maximal? ->** Corte minimal S = {s,G,H,I,D,C,E,F} V-S = {A,B,J,K,L,P}
- $CAP(S) = CAP(S, V-S) = f(sA) + f(Dt) + f(Ft) + f(Ht) = 100 + 50 + 70 + 10 = 230$
- Como $CAP(S) = v(f) => MAXIMAL$ 

## Algoritmo Dinic para max flow

**Idea**
- Dada una network N y un flujo f
- Construimos una network auxiliar $NA_f$
- Donde se encuentra un **flujo bloqueante g** 
- Con f y g construimos $f^*$

**Network Residual (NR)**
- Posee todos los vertices de N
- Por cada lado $\overrightarrow{xy} ∈ E(N)$ **se crean 0, 1 o 2 lados si:**
	- Si $f(\overrightarrow{xy}) < c(\overrightarrow{xy})$ en N => Se crea un lado $\overrightarrow{xy}$ en la NR con capacidad $c(\overrightarrow{xy}) - f(\overrightarrow{xy})$  
		- Lado forward es lo mismo en la NR
	- Si $f(\overrightarrow{xy}) > 0$ en N => Se crea un lado $\overrightarrow{yx}$ en la NR con capacidad $f(\overrightarrow{xy})$ 
		- Los lados backward de N seran forward en la NR 
- Simplemente por iteracion se crea una NR basada en N donde los lados seran
	- Los forward de N con cap max como la capacidad que le queda
	- Los bakcward de N que sean forward y con cap max el flujo actual del lado
	- Los saturados ni los añadimos
- **Pero mejor dicho es simplemente las capacidades residuales de lo que realmente se puede  mandar por tal lado**
- **Por lo que podemos simplemente seguir haciendo como haciamos con edmonds y acutalizar nuestros lados ya que no es necesario pensar en esto**
- **Leer esto de arriba es importante**

**Network Auxiliar (NA)**
- Es una **subnetwork de la NR** y es un grafo por niveles
- A partir de la NR se construye la NA manteniendo unicamente los lados que van a un nivel consecutivo
- cada nodo en la NA recibe un nivel(x) donde ese nivel es la cantidad minima de lados desde s a x (esto define los niveles)
- **Pasos para obtenerlo**
	- Corremos BFS desde s
	- Un lado es parte de la NA si se hace un progreso positivo hacia t
	- O sea un lado xy debe ir desde un nivel a el siguiente nivel -> $d(y) = d(x) + 1$
	- **d(x)** = numero de lados del camino mas corto entre s y x
	- O sea el nodo `y` esta a un lado mas de distancia significa que pertenece al siguiente nivel

**Flujo bloqueante**
- Un flujo bloqueante en una NA
- Es un flujo tal que todo camino dirigido (no aumentante) de s aa t
- tiene un lado xy con g(xy) = c(xy) (lado saturado)
- O sea todos los cambinos tienen almenos un lado saturado por ende no hay camino aumentante de s a t

**Algoritmo de Dinic pasos**
1. **Construir la NA corriendo BFS desde s para conseguir los niveles de la NA**
	- Recordar chequear que los lados forward y backward siempre esten un nivel mas en relacion al camino aumentante mas corto a ese nodo d(y) = d(x) + 1 nivel
	- O sea aca encolaremos/añadiremos los vertices donde su lado realmente haga un nivel mas que el que lo esta agregando
	- El nodo que ya este en la BFS no se reagrega **PERO** una que hare yo es repetirlos por quienes lo agregan para asi tener los lados de nivel + 1 mas a la vista
	- Porque sino luego de hacer el BFS debo decir a si el nodo t fue añadido por este pero otros tambien llegaban a nivel + 1 a t
	- Los nodos de la BFS son los que estan en la NA y se agregan todos los lados que hacen un nivel mas

2. **Hacer DFS sobre la NA desde s a t hasta encontrar un flujo bloqueante**
	- Sumar todos los cuellos de botella de todos los caminos aumentantes

3. **Repetir hasta que no pueda consturir una NA porque t no fue alcanzado**

**Ejemplo**
![[Pasted image 20250212122459.png]]
$$
\begin{aligned}
sA &\rightarrow \cancel{100} \cancel{20} \cancel{16} \cancel{6} 0 & AB &\rightarrow \cancel{150} \cancel{70} \cancel{66} 60  & Bt &\rightarrow \cancel{80} 0  & Dt &\rightarrow \cancel{50} 0  & HI &\rightarrow 100  & KL &\rightarrow 100 \\
sC &\rightarrow \cancel{50} 0  & AC &\rightarrow \cancel{10} 0 & CD &\rightarrow \cancel{50} \cancel{0} 6   & EF &\rightarrow \cancel{100} \cancel{50} \cancel{40 34  & Ht &\rightarrow \cancel{10} 0  & LP &\rightarrow 100 \\
sE &\rightarrow \cancel{50} 0  & BD &\rightarrow \cancel{100} 96  & CE &\rightarrow \cancel{20} \cancel{10} 4  & Ft &\rightarrow \cancel{70} \cancel{20} \cancel{16} \cancel{6} 0  & ID &\rightarrow 100  & Pt &\rightarrow 100 \\
sG &\rightarrow \cancel{100} 90  & BJ &\rightarrow 100  & DF &\rightarrow \cancel{4} 0  & GH &\rightarrow \cancel{100} 90  & JK &\rightarrow 100
\end{aligned}
$$
**Iteracion 1** (La NR es la misma N) (Construimos NA)
1. Armar NA
Cola BFS = [s,A(s),C(s),E(s),G(s), B(A), D(C), F(E), H(G), t(B),t(D),t(F),t(H) ]
En esta iteracion muestro la NA:
![[Pasted image 20250213122025.png]]
1.  Corremos DFS desde s sobre la NA para buscar los caminos aumentantes
sABt: 80, sCDt:50 ,sEFt:50, sGHt:10 (actualizamos las capacidades alla arriba)

**Iteracion 2**
bfs -> [s,  A(s), G(s), B(A), C(A), H(G),  D(B), J(B)   E(C),  I(H), F(D), K(J)  F(E), I(H), t(F)]
          1         1       2       2       2         3       3       3        3       4      4       4       4      5
Caminos ->  sACEFt:10 , sABDFt:4

**Iteracion 3**
bfs -> [s, A(s), G(s), B(A), H(G), D(B),  I(H), D(B), J(B), I(H),    C(D),    E(C), F(E), t(F)]
          1        1       2       2       3        3       3        3     3       4(<-)       5      5       6
Camino -> sAB$\overleftarrow{DC}$EFt:6

Luego continuamos hasta que bfs no llegue a t


## Algoritmo Wave para max flow

**Idea**
- Se elimina la filosofia de caminos aumentantes
- Sobre una NA mantener un **pre-flujo bloqueante** (mandar lo maximo posible)
- Que aun no es un flujo ya que no cumple la propiedad de la conservacion
- => Hay que trabajar hasta convertilo en un flujo

**Pasos**
1. Crear la misma NA como en Dinic (solo los lados que hacen un progreso de nivel + 1)
	- Si no llegue a t Saltar al paso 6
2. Para todos los vertices en orden BFS(s) 
	-  **Hacemos una ola forward** -> mandar todo lo que se pueda hacia adelante
	- **Si algun vertice tiene ->** $in_f (x) > out_f (x)$ (flujo entrante mayor al que sale)
	- => Decimos que **el vertice esta desbalanceado** (o sea debemos mandar para adelante)
	- => Debemos balancearlo en la misma ola forward
		- Hacemos que el vertice mande hacia adelante lo maximo posible (de manera forward) 
		- Si no se logro balancear **o sea le sigue quedando flujo luego saturar los caminos forward** => BLOQUEAR VERTICE
	- En otras palabras vamos mandando hacia adelante flujo por los lados forward de los vertices, (se puede ir mandando por niveles)
	- ya que s puede mandar a los primeros por ejemplo sx, sy, sz y luego como la es la NA, x y z deberan mandar flujo al siguiente nivel de esta forma balanceandose y dando flujo a los nodos por los camino forward
3. Para todos los verticesen orden BFS(s) **INVERTIDO**
	- **Hacemos una ola backward** 
	- revisando los vertices bloqueados (los no balanceados) 
	- Siendo asi los no balanceados que devuelvan flujo (la idea es al ultimo que le envio flujo)
4. repetir 3 y 4 suceisvamente hasta que todos los vertices esten balanceados
	- Los que fueron bloqueados siguen bloqueados y es responsabilidad de la ola backward de balancearlo (que este en 0) 
	- **Pero en la ola backward puede pasar que se vertices pierdan el balance**
	- Donde luego en la ola forward no pudo mandar todo para balancearse => bloqueo
	- y de nuevo... backward para balancear ese vertice que esta bloquedo
	- y asi hasta llegar a todos los vertices balanceados
5. Volver a 1 
6. Terminar



**Ejemplo** - Solo la 1ra iteracion bien detallada
$$
\begin{aligned}
sA &\rightarrow 8 & AF &\rightarrow 4 & BJ &\rightarrow 2 & DF &\rightarrow 4 & EH &\rightarrow 7 & GB &\rightarrow 4 & Kt &\rightarrow 3 \\
sC &\rightarrow 7 & AG &\rightarrow 3 & CG &\rightarrow 2 & DG &\rightarrow 2 & EK &\rightarrow 3 & Ht &\rightarrow 4 \\
sD &\rightarrow 10 & AI &\rightarrow 8 & CH &\rightarrow 3 & EF &\rightarrow 3 & Ft &\rightarrow 7 & It &\rightarrow 9 \\
sE &\rightarrow 15 & BI &\rightarrow 2 & CJ &\rightarrow 5 & EG &\rightarrow 6 & Gt &\rightarrow 6 & Jt &\rightarrow 15
\end{aligned}
$$
1° 
NA ->
- [s, A(s), C(s), D(s), E(s),  F(A), G(A), I(A),  H(C), J(C), K(E), t(F)]
     1        1        1      1        2       2       2       2        2      2      3

Mando desde s todo lo que pueda dejando a A,C,D,E Desbalanceados
$$
\begin{array}{c|cccc|cccccc|c}
s  & A  & C & D  & E& F & G & I & H & J & K & t \\
-40& 8  & 7 & 10 &15&   &   &   &   &   &   &   \\
   &    &   &   &   &   &   &   &   &   &   &   \\
   &    &   &   &   &   &   &   &   &   &   &   \\
   &    &   &   &   &   &   &   &   &   &   &   \\
   &    &   &   &   &   &   &   &   &   &   &   \\
   &    &   &   &   &   &   &   &   &   &   &   \\
\end{array}
$$
$$
\begin{aligned}
sA &\rightarrow \cancel{8} 0 & AF &\rightarrow 4 & BJ &\rightarrow 2 & DF &\rightarrow 4 & EH &\rightarrow 7 & GB &\rightarrow 4 & Kt &\rightarrow 3 \\
sC &\rightarrow \cancel{7} 0 & AG &\rightarrow 3 & CG &\rightarrow 2 & DG &\rightarrow 2 & EK &\rightarrow 3 & Ht &\rightarrow 4 \\
sD &\rightarrow \cancel{10} 0 & AI &\rightarrow 8 & CH &\rightarrow 3 & EF &\rightarrow 3 & Ft &\rightarrow 7 & It &\rightarrow 9 \\
sE &\rightarrow \cancel{15} 0 & BI &\rightarrow 2 & CJ &\rightarrow 5 & EG &\rightarrow 6 & Gt &\rightarrow 6 & Jt &\rightarrow 15
\end{aligned}
$$
Balanceemos solo A para entender
- Mandamos a F 4 
- Mandamos a G 3
- y a I lo que nos resta que es 1
- De esta forma vemos que esta balanceado pues fijate que entra 8 y sale 8  de A
$$
\begin{array}{c|cccc|cccccc|c}
s     & A     & C    & D     & E   & F    & G    & I    & H    & J    & K    & t \\
-40   &\cancel{8}& 7    & 10    &15   & 4    &3      &1      &      &      &      &   \\
      & \cancel{4}      &      &      &      &      &      &      &      &      &      &   \\
      &   \cancel{1}    &      &      &      &      &      &      &      &      &      &   \\
      &   0&      &      &      &      &      &      &      &      &      &   \\

\end{array}   
$$
$$
\begin{aligned}              
sA &\rightarrow \cancel{8} 0 & AF &\rightarrow \cancel{4} 0 & BJ &\rightarrow 2 & DF &\rightarrow 4 & EH &\rightarrow 7 & GB &\rightarrow 4 & Kt &\rightarrow 3 \\
sC &\rightarrow \cancel{7} 0 & AG &\rightarrow \cancel{3} 0 & CG &\rightarrow 2 & DG &\rightarrow 2 & EK &\rightarrow 3 & Ht &\rightarrow 4 \\
sD &\rightarrow \cancel{10} 0 & AI &\rightarrow \cancel{8} 7 & CH &\rightarrow 3 & EF &\rightarrow 3 & Ft &\rightarrow 7 & It &\rightarrow 9 \\
sE &\rightarrow \cancel{15} 0 & BI &\rightarrow 2 & CJ &\rightarrow 5 & EG &\rightarrow 6 & Gt &\rightarrow 6 & Jt &\rightarrow 15
\end{aligned}
$$

Balanceemos C,D y E
mandamos lo que podemos hacia adelante
- Notemos que D le quedo flujo aun por ende queda bloqueado ya que todos sus lados forward fueron saturados

$$
\begin{array}{c|cccc|cccccc|c}
s     & A     & C    & D     & E   & F    & G    & I    & H    & J    & K    & t \\
-40   &\cancel{8}& \cancel{7}    & \cancel{10}    &\cancel{15}   & \cancel{4}    &\cancel{3}      &1      & \cancel{3}     &  2    &      &   \\
      & \cancel{4}      & \cancel{5}     & \cancel{6}     &  \cancel{12}    &    \cancel{8}  &\cancel{5}      &      &   9   &      &      &   \\
      &   \cancel{1}    &\cancel{2}      &   \boxed{4}   & \cancel{6}  &  11    &  \cancel{7}    &      &      &      &      &   \\
      &   0&      0&      &  0    &      & 13     &      &      &      &      &   \\

\end{array}   
$$

$$
\begin{aligned}              
sA &\rightarrow \cancel{8} 0 & AF &\rightarrow \cancel{4} 0 & BJ &\rightarrow 2 & DF &\rightarrow \cancel{4} 0 & EH &\rightarrow \cancel{7} 1 & GB &\rightarrow 4 & Kt &\rightarrow 3 \\
sC &\rightarrow \cancel{7} 0 & AG &\rightarrow \cancel{3} 0 & CG &\rightarrow \cancel{2} 0 & DG &\rightarrow \cancel{2} 0& EK &\rightarrow 3 & Ht &\rightarrow 4 \\
sD &\rightarrow \cancel{10} 0 & AI &\rightarrow \cancel{8} 7 & CH &\rightarrow \cancel{3} 0 & EF &\rightarrow \cancel{3} 0 & Ft &\rightarrow 7 & It &\rightarrow 9 \\
sE &\rightarrow \cancel{15} 0 & BI &\rightarrow 2 & CJ &\rightarrow \cancel{5} 3 & EG &\rightarrow \cancel{6} 0 & Gt &\rightarrow 6 & Jt &\rightarrow 15
\end{aligned}
$$

Luego debemos continuar con la ola forward balanceando lo que debamos balancear
y como vemos simplemente es lo mismo **olas forward hacia adelante** 
- EN este caso se termino la ola forward con F, G y H con bloqueados con ese flujo
$$
\begin{array}{c|cccc|cccccc|c}
s     & A     & C    & D     & E   & F    & G    & I    & H    & J    & K    & t \\
-40   &\cancel{8}& \cancel{7}    & \cancel{10}    &\cancel{15}   & \cancel{4}    &\cancel{3}      &\cancel{1}      & \cancel{3}     &  \cancel{2}    &      &  \cancel{7} \\
      & \cancel{4}      & \cancel{5}     & \cancel{6}     &  \cancel{12}    &    \cancel{8}  &\cancel{5}      &    0  &   \cancel{9}   &   0   &      &  \cancel{13} \\
      &   \cancel{1}    &\cancel{2}      &   4   & \cancel{6}  &  \cancel{11}    &  \cancel{7}    &      &\boxed{5}      &      &      &  \cancel{14} \\
      &   0&      0&      &  0    &     \boxed{4} & \cancel{13}     &      &      &      &      &  \cancel{18} \\&   &      &      &      &      &  \boxed{7}    &      &      &      & & 20
       \\&   &      &      &      &      &      &      &      &      & 


\end{array}   
$$


$$
\begin{aligned}              
sA &\rightarrow \cancel{8} 0 & AF &\rightarrow \cancel{4} 0 & BJ &\rightarrow 2 & DF &\rightarrow \cancel{4} 0 & EH &\rightarrow \cancel{7} 1 & GB &\rightarrow 4 & Kt &\rightarrow 3 \\
sC &\rightarrow \cancel{7} 0 & AG &\rightarrow \cancel{3} 0 & CG &\rightarrow \cancel{2} 0 & DG &\rightarrow \cancel{2} 0& EK &\rightarrow 3 & Ht &\rightarrow \cancel{4} 0 \\
sD &\rightarrow \cancel{10} 0 & AI &\rightarrow \cancel{8} 7 & CH &\rightarrow \cancel{3} 0 & EF &\rightarrow \cancel{3} 0 & Ft &\rightarrow \cancel{7} 0 & It &\rightarrow \cancel{9} 8 \\
sE &\rightarrow \cancel{15} 0 & BI &\rightarrow 2 & CJ &\rightarrow \cancel{5} 3 & EG &\rightarrow \cancel{6} 0 & Gt &\rightarrow \cancel{6} 0 & Jt &\rightarrow \cancel{15} 13
\end{aligned}
$$

Ahora el algoritmo dice de recorrer los vertices en el orden BFS(s) inverso pero viendo solo los que hay que balancear (los que estan bloqueados)

- El primer es H, y H recibe de CH (3) y de EH (6) => DEVUELVO 5 A E
- y luego continuamos con G,F y D
- Notemos que los que vamos balanceando son los que tienen el cuadradito ya que estos se mantendran bloqueados tal como lo dice el algoritmo
- El unico que se desbalanceo fue E
![[Pasted image 20250214151646.png]]

$$
\begin{aligned}              
sA &\rightarrow \cancel{8} 0 & AF &\rightarrow \cancel{4} 0 & BJ &\rightarrow 2 & DF &\rightarrow \cancel{4} \cancel{0} 1& EH &\rightarrow \cancel{7} \cancel{1} 6 & GB &\rightarrow 4 & Kt &\rightarrow 3 \\
sC &\rightarrow \cancel{7} 0 & AG &\rightarrow \cancel{3} 0 & CG &\rightarrow \cancel{2} 0 & DG &\rightarrow \cancel{2} \cancel{0} 1& EK &\rightarrow 3 & Ht &\rightarrow \cancel{4} 0 \\
sD &\rightarrow \cancel{10} \cancel{0}6 & AI &\rightarrow \cancel{8} 7 & CH &\rightarrow \cancel{3} 0 & EF &\rightarrow \cancel{3} \cancel{0} 3& Ft &\rightarrow \cancel{7} 0 & It &\rightarrow \cancel{9} 8 \\
sE &\rightarrow \cancel{15} 0 & BI &\rightarrow 2 & CJ &\rightarrow \cancel{5} 3 & EG &\rightarrow \cancel{6} \cancel{0} 6 & Gt &\rightarrow \cancel{6} 0 & Jt &\rightarrow \cancel{15} 13
\end{aligned}

$$

Ahora que llegamos a la izquierda vuelva una OLA FORWARD de nuevo hacia la derecha
- E manda 3 a K
- K manda 3 a t
- en esta ola siga quedando E desbalanceado => BLOQUEADO
![[Pasted image 20250214152208.png]]
Se vuelve la ola de derecha a izquierda (ola backward)
- El unico desbalanceado es E => E devuelve 11 a s 
![[Pasted image 20250214152443.png]]
De esta forma dada una serie de olas forward y backward hasta llegar a todos los vertices balanceados se concluye la 1ra iteracion de Wave
- Se de