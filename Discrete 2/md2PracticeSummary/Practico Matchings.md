
## Matching Maximal

**Ejemplo**

- Sea G y transformamos G en una N(G)
![[Pasted image 20250216131340.png]]![[Pasted image 20250216131403.png]]

- Veamos la N como una matriz de adyacencia pero solo teniendo en cuenta los vertices que van desde un conjunto a otro
![[Pasted image 20250216131511.png]]

1. Correr Dinic hasta el primer flujo bloqueante
	- Sabemos que todos los lados tienen capacidad maxima de 1
	- Y es facil viendo el grafo de la network que la network auxiliar es el N
	- ya que todos hacen un progreso hacia t
	- pero debido que los lados sx y yt se saturaran solo habra 4 caminos en este caso

	- sA1t:1, sB3t:1 , sC5t:1, sD4t: 1 (listo flujo bloqueante alcanzado)
	- Entonces por el momento los lados que van al matching son:
	- $A1:\cancel{1} 0$, $B3:\cancel{1} 0$, $C5:\cancel{1} 0$  $D4:\cancel{1} 0$
	- Ademas sA, sB, sC, sD, 1t, 3t, 5t y 4t estan saturados

2. Continuar con Edmonds Karp

BFS -> [s, E(s), 3(E), 4(E),  B(3⁻), D(4⁻), 5(D), C(5⁻), -] 
Como no llegamos a t el matching no se puede extender
El matching maximal son los lados mencionados anteriormente unicamente
![[Pasted image 20250216135850.png]]