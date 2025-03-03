# IA-Guia-3

### 3.1. Suponga que usted es el jefe de gobierno y está interesado en que pasen los proyectos de su programa político. Sin embargo, en el congreso conformado por 5 partidos, no es fácil su tránsito, por lo que debe repartir el poder, conformado por ministerios y otras agencias del gobierno, con base en la representación de cada partido. Cada entidad estatal tiene un peso de poder, que es el que se debe distribuir. Suponga que hay 50 curules, distribuya aleatoriamente, con una distribución no informe entre los 5 partidos esas curules. Defina una lista de 50 entidades y asígneles aleatoriamente un peso político de 1 a 100 puntos. Cree una matriz de poder para repartir ese poder, usando AGs.

Para resolver este problema, primero debemos generar los datos iniciales: la distribución de curules entre los 5 partidos y la lista de 50 entidades estatales con sus respectivos pesos políticos. Luego, usaremos un algoritmo genético (AG) para distribuir el poder entre los partidos de manera proporcional a su representación en el congreso.

```python
import numpy as np

# Definimos los 5 partidos
partidos = ['Partido A', 'Partido B', 'Partido C', 'Partido D', 'Partido E']

# Distribuimos aleatoriamente las 50 curules
curules = np.random.multinomial(50, [0.2, 0.3, 0.25, 0.15, 0.1])

# Mostramos la distribución
distribucion_curules = dict(zip(partidos, curules))
print("Distribución de curules:", distribucion_curules)
```
### 2. Una empresa proveedora de energía eléctrica dispone de cuatro plantas de generación para satisfacer la demanda diaria de energía eléctrica en Cali, Bogotá, Medellín y Barranquilla. Cada una puede generar 3, 6, 5 y 4 GW al día respectivamente. Las necesidades de Cali, Bogotá, Medellín y Barranquilla son de 4, 3, 5 y 3 GW al día respectivamente. Los costos por el transporte de energía por cada GW entre plantas y ciudades se dan en la siguiente tabla:

| Planta | Cali | Bogotá | Medellín | Barranqui. |
|-----------|-----------|-----------|-------| ------ |
| Planta C    | 1  | 4   | 3 | 6 |
| Planta B   | 4 | 1 | 4 | 5 |
| Planta M    | 3 | 4 | 1 | 4 |
| Planta B    | 6 | 5 | 4 | 1 |

### Los costos del KW-H por generador se dan en la siguiente tabla:

| Generador  | $ KW - H |
|-----------|-----------|
| Planta C    | 680  | 
| Planta B   | 720 |
| Planta M    | 660 | 
| Planta B    | 750 | 

### Encontrar usando AGs el mejor despacho de energía minimizando los costos de transporte y generación.

### 4. Genere aleatoriamente una población de 50 palabras, que se escuche por el parlante del computador. Tomando como función de aptitud una palabra suya, usando AGs, con base en las palabras generadas aleatoriamente llegue a la palabra que usó como función de aptitud.

### 5. Tome un problema de los anteriores, u otro cualquiera, y utilice la librería Python de AGs Pygad y para solucionarlo.
