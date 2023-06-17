# Projecte_NEO4J
Segon projecte de l'assignatura de BDnR

## Participants
1. Manuel Arnau Fernández, NIU: 1597487
2. Sofia Di Capua Martin Mas, NIU: 1603685
3. Gerard Lahuerta Martin, NIU: 1601350
4. Ona Sánchez Núñez, NIU: 1601181

## Introducció

En aquest projecte es treballarà, a partir d'un subconjunt de dades de padrons, la càrrega
de dades i consultes en una base de dades de grafs (Neo4J) i es practicarà l'ús
d'algorismes d'analítica de grafs.

L'objectiu d'aquest projecte és afermar els conceptes ensenyats a classe mitjançant una aplicació diversa i realista d'una base de dades.

## Pràctica Neo4J
### Explicació de l'entrega

El projecte està format per 3 apartats realitzats entre els diversos membres de l'equip, on s'han dividit les tasques a mesura que es progressava amb el treball.

El primer apartat consisteix a importar les dades en la base de dades mitjançant un script en cypher. Aquest carrega totes les dades, genera els nodes, relacions i característiques. Cal tenir en compte que hem considerat fer servir constrains i indexos allà on era necessari, que no es dupliquin dades, que estiguin en el tipus correcte i que no es carreguin files null (Id de municipi, llar o individu=null).

La segona part del projecte és la implementació de 13 consultes proposades, sobre la base de dades generada.

L'última part del treball consisteix a analitzar les dades del graf per entendre millor l'estructura de les dades. En aquesta part ens endinsarem en les eines que ofereix Neo4J.

### Materials produïts a l'entrega

Els materials produïts en aquest projecte, i que es poden observar en aquest github, són:

DOCUMENTS:
- .txt: Document de text on estan totes les queries de l'exercici 2.
- .txt: Script de cypher que permet l'extracció de les dades i la generació de la base de dades.
- Projecte_Neo4J.pdf: Informe que conté tot el treball fet.
