***
**Conceptos Clave**
- **Notacion de Grafo**       - `G = (V,E)`
- **Vecinos de un vertice** - `Γ(x) = { y ∈ V : xy ∈ E}`
- **Grado de un vertice**    - `d(x) = #(Γ(x))` 
- **Minimo y Maximo grado del grafo** - `δ = Min{d(x) : x ∈ V}` `Δ = Max{d(x) : x ∈ V}`
- **Grafos Regulares** - `Todos los vertices poseen el mismo grado`
- **Grafos Ciclicos** - `Cn` `n > 3 y x1x2, ... , x(n-1)xn ,xnx1 ` se foma un ciclo
- **Grafos completos** - `Kn`  `Todos los vertices estan unidos entre si`
- **Grafo y componentes conexas** - `Conexo si todos los V se alcanzan entre si`
- **Grafo Bipartito** - `Divido en 2 conjuntos el grafo` 
	- Cada vertice de un conjunto se une con uno del otro conjunto, pero no entre el mismo conjunto
- **Determinar componentes Conexas**
	- **DFS** - Se recorre desde un vertice hasta el nivel mas bajo y subo hasta poder bajar de nuevo
	- **BFS** - Se recorren los vecinos y luego voy al siguiente nivel
- **Coloreo** - `funcion C : V -> S (S conjunto de colores de cada vertice)`
- **Coloreo propio** - `C es propio si vw ∈ E ⇒ C(v) ≠ C(w)` (los vecinos no comparten color)
- **Numero cromatico χ(G)** - `χ(G) = min { k : ∃ un coloreo propio con k colores de G}`
- **K-color** - `χ(G) ≤ k` G ¿puede ser pintado con a lo sumo k colores? (es una pregunta a resolver)
- **Greedy coloreo** - `Recorro todos los vertices en un orden y asigno min color disponible`
- **Cotas Inferiores para χ(G)**
	- `1 ≤ χ(G) ≤ n`  (un color para cada vertice)
	- `χ(H) ≤ χ(G) ≤ Δ(G) + 1` (Es mayor al numero cromatico del subgrafo y menor al max grado + 1)
	- `χ(Kn) ≤ n` Grafo Kn 
	- `χ(C2n) ≤ 2` y `χ(C2n+1) ≤ 2`  Grafo ciciclico par y impar
	- `χ(G) ≤ 2 ` (G bipartito)
- **Como encontrar y probar χ(G)**
	1. Encuentro un coloreo de **k colores** propio expresado con funciones que asignan un color a cada vertice de manera que sea propio 
	2. **Debo probar que no hay un coloreo propio de k-1 colores**
		- O chequeo con cotas inferiores a ver si resulta que  `k ≤ χ(G)`
		- Sino aparece resolvemos por el absurdo asumiendo que tengo **k-1 colores disponibles** y coloreo hasta llegar a un absurdo
		- **Pero no es dar un solo coloreo que justo k-1 colores da no propio**
		- **Al absurdo que lleguemos debe aplicar a cualquier posible coloreo de k-1 colores**
- **Teorema de Brooks**
	- `χ(G) ≤ Δ(G) + 1` 
	- El teorema de Brooks baja la cota para grafos conexos que no sean ciclos impares ni completos
	- `Sea G conexo, G ≠ C2k+1, G ≠ Kn ⇒ χ(G) ≤ Δ(G)`
	- O sea existe un orden tal que greedy colorea todos los vertices
- **VIT**
	- Sea G y un coloreo propio de r colores (`{0,...,r-1}`) 
	- `y sea π: {0,1,...,r-1} → {0,1,...,r-1} una biyeccion`
	- `Sea V_i = { x ∈ V: C(x) = i}, i = 0,1,...,r-1` los vertices con el color i
	- Ordenamos los vertices en bloques de colores de menor a mayor:
	- `V_π(0),V_π(1),...,V_π(r-1)`
	- Entonces una vez ordenado Greedy colorea eso con **r colores o menos**

## Ejercicio 1 
![[Screenshot from 2025-02-10 09-32-45.png]]
![[Pasted image 20250210102455.png]]
![[Pasted image 20250210102507.png]]

## Ejercicio 2

![[Screenshot from 2025-02-10 10-32-37.png]]

**Caso r=2 M2**
![[Pasted image 20250210115846.png]]
- En este caso el grafo M2 es una K4 por ende `χ(M2) = 4`

**Caso r>2 & r par**
![[Pasted image 20250210120047.png]]
- Si vemos el subgrafo C2n+1 pues siempre hay un lado x1xr+1
- Por lo que siempre estara presente un este subgrafo C2n+1 los que nos dice que tiene una cota inferior / `3 ≤ χ(Mr)` para r par
- Generalizemos que se cumple esta cota inferior para el resto de r 
- ![[Pasted image 20250210120239.png]]
- Ahora demos el coloreo propio para 3 colores
``` ruby
Demos las restricciones primero

C(xi) != C(xi+1)
C(x0) != C(xr)
C(x0) != C(x2r)

ahora el coloreo

C(xi) = i mod 2 (1 ≤ i < r)
C(xi) = 2 (i=r+1, i=2r)
C(xi) = i + 1 mod 2 (r+1 < i < 2r )
```
- **# Por lo tanto** `χ(Mr)=3 para r par`

**Caso r impar **
![[Pasted image 20250210120451.png]]
- Resulta existir a diferencia del par un ciclo par C2n para todo M con r impar
- Generalizar esto es analogo al del par ademas es obvio que Existe el lado x1xr+1 siempre
- De esta forma sabemos que tiene una cota inferior /  `2 ≤ χ(Mr)`
- Demos el coloreo propio
```ruby
C(Xi) = i mod 2 (1 ≤ i ≤ 2r)
```
- De esta forma existe un coloreo propio con 2 colores y por la cota inferior tenemos que
- **#** `χ(Mr) = 2 para r impar`


## Ejercicio 3
![[Pasted image 20250210120818.png]]

## Ejercicio 4 
![[Pasted image 20250210120838.png]]