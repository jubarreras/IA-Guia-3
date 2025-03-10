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

Ahora, generamos una lista de 50 entidades estatales y les asignamos un peso político aleatorio entre 1 y 100:

```python
# Generamos 50 entidades con pesos aleatorios
entidades = [f"Entidad {i+1}" for i in range(50)]
pesos = np.random.randint(1, 101, size=50)

# Mostramos algunas entidades con sus pesos
lista_entidades = dict(zip(entidades, pesos))
print("Lista de entidades con pesos:", lista_entidades)
```

El objetivo es distribuir las entidades entre los partidos de manera que el poder total asignado a cada partido sea proporcional a su número de curules. Usaremos un algoritmo genético para optimizar esta distribución.

**Implementación del Algoritmo Genético:**
**Representación del cromosoma:** Un cromosoma es una lista de 50 valores, donde cada valor indica a qué partido se asigna la entidad correspondiente.

**Función de aptitud (fitness):** Mide qué tan bien se ajusta la distribución de poder a la proporción de curules.

**Operadores genéticos:** Cruzamiento, mutación y selección.

```python
import random

# Parámetros del algoritmo genético
TAMANO_POBLACION = 100
GENERACIONES = 200
PROBABILIDAD_CRUZAMIENTO = 0.8
PROBABILIDAD_MUTACION = 0.1

# Datos iniciales
partidos = ['Partido A', 'Partido B', 'Partido C', 'Partido D', 'Partido E']
curules = {'Partido A': 10, 'Partido B': 15, 'Partido C': 12, 'Partido D': 8, 'Partido E': 5}
entidades = lista_entidades  # Usamos la lista generada anteriormente

# Función de aptitud
def calcular_aptitud(cromosoma):
    poder_partidos = {partido: 0 for partido in partidos}
    for i, partido in enumerate(cromosoma):
        poder_partidos[partido] += pesos[i]
    # Calculamos la diferencia entre el poder asignado y el deseado
    diferencia = 0
    for partido in partidos:
        poder_deseado = (curules[partido] / 50) * sum(pesos)
        diferencia += abs(poder_partidos[partido] - poder_deseado)
    return -diferencia  # Minimizamos la diferencia

# Generar población inicial
def generar_poblacion_inicial():
    return [random.choices(partidos, k=50) for _ in range(TAMANO_POBLACION)]

# Cruzamiento
def cruzar(padre1, padre2):
    punto = random.randint(1, 49)
    return padre1[:punto] + padre2[punto:], padre2[:punto] + padre1[punto:]

# Mutación
def mutar(cromosoma):
    for i in range(len(cromosoma)):
        if random.random() < PROBABILIDAD_MUTACION:
            cromosoma[i] = random.choice(partidos)
    return cromosoma

# Selección por torneo
def seleccionar(poblacion):
    return max(random.sample(poblacion, 3), key=calcular_aptitud)

# Algoritmo genético
def algoritmo_genetico():
    poblacion = generar_poblacion_inicial()
    for generacion in range(GENERACIONES):
        nueva_poblacion = []
        for _ in range(TAMANO_POBLACION // 2):
            padre1 = seleccionar(poblacion)
            padre2 = seleccionar(poblacion)
            if random.random() < PROBABILIDAD_CRUZAMIENTO:
                hijo1, hijo2 = cruzar(padre1, padre2)
            else:
                hijo1, hijo2 = padre1, padre2
            nueva_poblacion.append(mutar(hijo1))
            nueva_poblacion.append(mutar(hijo2))
        poblacion = nueva_poblacion
    mejor_cromosoma = max(poblacion, key=calcular_aptitud)
    return mejor_cromosoma

# Ejecutar el algoritmo
mejor_distribucion = algoritmo_genetico()
print("Mejor distribución:", mejor_distribucion)

poder_final = {partido: 0 for partido in partidos}
for i, partido in enumerate(mejor_distribucion):
    poder_final[partido] += pesos[i]
print("Poder final por partido:", poder_final)
```
**Ejemplo de salida**

```python
Poder final por partido: {'Partido A': 1200, 'Partido B': 1800, 'Partido C': 1500, 'Partido D': 900, 'Partido E': 600}
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
Se importa la librería numpy para trabajar con matrices y operaciones numéricas eficientes, y se incializan los parametros con los que se quiere trabajar.
```python
import numpy as np

# Parámetros del problema
capacidad_plantas = [3, 6, 5, 4]
demanda_ciudades = [4, 3, 5, 3]
costos_transporte = np.array([
    [1, 4, 3, 6],
    [4, 1, 4, 5],
    [3, 4, 1, 4],
    [6, 5, 4, 1]
])
costos_generacion = [680, 720, 660, 750]  # $/GW

# Configuración AG
POBLACION = 100
GENERACIONES = 200
PROB_CRUCE = 0.8
PROB_MUTACION = 0.05
```Las funciones del código implementan un Algoritmo Genético (AG) para resolver un problema de optimización. La función crear_individuo genera soluciones candidatas (matrices 4x4) que cumplen con las restricciones de capacidad y demanda. La función calcular_aptitud evalúa la calidad de cada individuo, penalizando soluciones que violen las restricciones y calculando el costo total de transporte y generación. La función seleccion elige individuos para la reproducción basándose en su aptitud, mientras que cruce combina dos individuos para crear descendencia intercambiando filas de la matriz. La función mutar introduce cambios aleatorios en una celda de la matriz para mantener la diversidad genética. El algoritmo principal inicializa una población, itera a través de generaciones, evalúa la aptitud de los individuos, selecciona los mejores, y aplica cruce y mutación para crear una nueva población. Al final, imprime la mejor solución encontrada y su costo total, optimizando la distribución de energía desde plantas a ciudades.
```python
def crear_individuo():
    # Generar matriz 4x4 que cumpla restricciones de capacidad
    individuo = np.zeros((4,4))
    for i in range(4):
        total = 0
        for j in range(4):
            max_posible = min(capacidad_plantas[i] - total, demanda_ciudades[j])
            if max_posible <= 0:
                individuo[i,j] = 0
                continue
            individuo[i,j] = np.random.uniform(0, max_posible)
            total += individuo[i,j]
    return individuo

def calcular_aptitud(individuo):
    # Verificar restricciones
    penalizacion = 0
    # Verificar capacidad plantas
    for i in range(4):
        suma = individuo[i,:].sum()
        if suma > capacidad_plantas[i]:
            penalizacion += 1e6 * (suma - capacidad_plantas[i])

    # Verificar demanda ciudades
    for j in range(4):
        suma = individuo[:,j].sum()
        if suma < demanda_ciudades[j]:
            penalizacion += 1e6 * (demanda_ciudades[j] - suma)

    # Calcular costos
    costo_transporte = (individuo * costos_transporte).sum()
    costo_generacion = sum([individuo[i,:].sum() * costos_generacion[i] for i in range(4)])

    return -(costo_transporte + costo_generacion + penalizacion)  # Minimizar

def seleccion(poblacion, aptitudes):
    # Selección por ruleta
    probabilidades = aptitudes - aptitudes.min()
    if probabilidades.sum() == 0:
        return poblacion[np.random.choice(len(poblacion))]
    probabilidades /= probabilidades.sum()
    return poblacion[np.random.choice(len(poblacion), p=probabilidades)]

def cruce(padre1, padre2):
    # Cruce por intercambio de filas
    if np.random.random() > PROB_CRUCE:
        return padre1.copy(), padre2.copy()

    punto = np.random.randint(1,3)
    hijo1 = np.vstack((padre1[:punto], padre2[punto:]))
    hijo2 = np.vstack((padre2[:punto], padre1[punto:]))
    return hijo1, hijo2

def mutar(individuo):
    # Mutación aleatoria en una celda
    if np.random.random() < PROB_MUTACION:
        i, j = np.random.randint(4), np.random.randint(4)
        max_val = min(capacidad_plantas[i], demanda_ciudades[j])
        individuo[i,j] = np.random.uniform(0, max_val)
    return individuo

# Algoritmo principal
poblacion = [crear_individuo() for _ in range(POBLACION)]
mejor_aptitud = -float('inf')

for gen in range(GENERACIONES):
    aptitudes = np.array([calcular_aptitud(ind) for ind in poblacion])

    # Registro del mejor
    mejor_idx = aptitudes.argmax()
    if aptitudes[mejor_idx] > mejor_aptitud:
        mejor_aptitud = aptitudes[mejor_idx]
        mejor_individuo = poblacion[mejor_idx].copy()

    # Nueva generación
    nueva_poblacion = []
    for _ in range(POBLACION//2):
        padre1 = seleccion(poblacion, aptitudes)
        padre2 = seleccion(poblacion, aptitudes)
        hijo1, hijo2 = cruce(padre1, padre2)
        nueva_poblacion.extend([mutar(hijo1), mutar(hijo2)])

    poblacion = nueva_poblacion

print("Mejor solución encontrada:")
print(mejor_individuo)
print(f"Costo total: ${-mejor_aptitud:.2f}")
```
El resultado para este calculo especifico será:
```python
Mejor solución encontrada:
[[1.08422944 0.1069619  0.89993074 0.59512829]
 [0.03929752 0.63827116 2.33141806 1.63678221]
 [0.23807249 1.59578442 2.52197908 0.6391114 ]
 [2.69092499 0.70160932 0.35614804 0.16700215]]
Costo total: $11464.54
```
### 4. Genere aleatoriamente una población de 50 palabras, que se escuche por el parlante del computador. Tomando como función de aptitud una palabra suya, usando AGs, con base en las palabras generadas aleatoriamente llegue a la palabra que usó como función de aptitud.
Este código implementa un **Algoritmo Genético (AG)** para encontrar una palabra objetivo (en este caso, 'luna') a partir de una población inicial de palabras aleatorias. La población se genera usando caracteres permitidos (letras minúsculas de 'a' a 'z'), y cada palabra se evalúa mediante una función de aptitud que mide cuántos caracteres coinciden con la palabra objetivo. En cada generación, se seleccionan los individuos más aptos (usando selección por ruleta), se aplica un cruce en un punto aleatorio para combinar palabras y se introduce una mutación aleatoria en algunas posiciones para mantener la diversidad. El proceso se repite durante un número máximo de generaciones, mostrando la mejor palabra encontrada en cada iteración. Si se encuentra la palabra objetivo exacta, se reproduce un sonido de éxito y el algoritmo termina; de lo contrario, se emite un sonido de avance y continúa la búsqueda.
Para este punto se utilizó la herramienta de MATLAB, ya que permite un manejo mas facil para solucionarlo, el codigo resultante es:

```matlab
% Palabra objetivo
palabra_objetivo = 'luna';

% Parámetros del algoritmo genético
tamano_poblacion = 50;
longitud_palabra = length(palabra_objetivo);
probabilidad_mutacion = 0.1;
generaciones = 100;

% Caracteres permitidos (letras minúsculas)
caracteres_permitidos = 'a':'z';

% Función para generar una palabra aleatoria
generar_palabra = @() caracteres_permitidos(randi(length(caracteres_permitidos), [1, longitud_palabra]));

% Generar población inicial
poblacion = cell(tamano_poblacion, 1);
for i = 1:tamano_poblacion
    poblacion{i} = generar_palabra();
end

% Función de aptitud
funcion_aptitud = @(palabra) sum(palabra == palabra_objetivo) / longitud_palabra;

for generacion = 1:generaciones
    % Calcular aptitud de cada individuo
    aptitudes = cellfun(funcion_aptitud, poblacion);
    
    % Seleccionar los mejores individuos (ruleta)
    probabilidades = aptitudes / sum(aptitudes);
    indices_seleccionados = randsample(1:tamano_poblacion, tamano_poblacion, true, probabilidades);
    nueva_poblacion = poblacion(indices_seleccionados);
    
    % Cruzamiento (punto de cruce aleatorio)
    for i = 1:2:tamano_poblacion
        punto_cruce = randi([1, longitud_palabra-1]);
        padre1 = nueva_poblacion{i};
        padre2 = nueva_poblacion{i+1};
        nueva_poblacion{i} = [padre1(1:punto_cruce), padre2(punto_cruce+1:end)];
        nueva_poblacion{i+1} = [padre2(1:punto_cruce), padre1(punto_cruce+1:end)];
    end
    
    % Mutación
    for i = 1:tamano_poblacion
        if rand < probabilidad_mutacion
            posicion_mutacion = randi(longitud_palabra);
            nueva_poblacion{i}(posicion_mutacion) = caracteres_permitidos(randi(length(caracteres_permitidos)));
        end
    end
    
    % Actualizar población
    poblacion = nueva_poblacion;
    
    % Mostrar la mejor palabra de la generación
    [mejor_aptitud, mejor_indice] = max(aptitudes);
    mejor_palabra = poblacion{mejor_indice};
    fprintf('Generación %d: %s (Aptitud: %.2f)\n', generacion, mejor_palabra, mejor_aptitud);
    
    % Reproducir la mejor palabra por el parlante
    if mejor_aptitud == 1
        disp('¡Palabra objetivo encontrada!');
        sound(sin(2*pi*(1:4000)/4000)); % Beep de éxito
        break;
    else
        sound(sin(2*pi*(1:2000)/2000)); % Beep de avance
    end
end
```
La salida que se tuvo con la palabra "hola" y "luna", respectivamente fue:
```
>> IAguia3
Generación 1: ddcc (Aptitud: 0.25)
Generación 2: ddkb (Aptitud: 0.50)
Generación 3: rbla (Aptitud: 0.75)
Generación 4: hola (Aptitud: 1.00)
¡Palabra objetivo encontrada!
>> IAguia3
Generación 1: wlxa (Aptitud: 0.25)
Generación 2: wlxa (Aptitud: 0.25)
Generación 3: wlxa (Aptitud: 0.25)
Generación 4: wlxa (Aptitud: 0.25)
Generación 5: wlez (Aptitud: 0.50)
Generación 6: wlna (Aptitud: 0.50)
Generación 7: wlng (Aptitud: 0.50)
Generación 8: wjna (Aptitud: 0.75)
Generación 9: llza (Aptitud: 0.75)
Generación 10: llna (Aptitud: 0.75)
Generación 11: lqna (Aptitud: 0.75)
Generación 12: wqwa (Aptitud: 0.75)
Generación 13: wlna (Aptitud: 0.75)
Generación 14: lmna (Aptitud: 0.75)
Generación 15: llna (Aptitud: 0.75)
Generación 16: wlxa (Aptitud: 0.75)
Generación 17: llfa (Aptitud: 0.75)
Generación 18: lnwa (Aptitud: 0.75)
Generación 19: wlna (Aptitud: 0.75)
Generación 20: lqna (Aptitud: 0.75)
Generación 21: lnna (Aptitud: 0.75)
Generación 22: lnna (Aptitud: 0.75)
Generación 23: lmna (Aptitud: 0.75)
Generación 24: lnna (Aptitud: 0.75)
Generación 25: lmna (Aptitud: 0.75)
Generación 26: lnna (Aptitud: 0.75)
Generación 27: llna (Aptitud: 0.75)
Generación 28: lmna (Aptitud: 0.75)
Generación 29: llna (Aptitud: 0.75)
Generación 30: lnna (Aptitud: 0.75)
Generación 31: luna (Aptitud: 1.00)
¡Palabra objetivo encontrada!
```
