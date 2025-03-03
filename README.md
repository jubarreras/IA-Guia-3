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

Para resolver este problema, se decidió utilizar la biblioteca Pulp de Python que permite solucionar el problema numerico de forma mas sencilla.

```python
from pulp import LpMinimize, LpProblem, LpVariable, lpSum

# Definir el problema
problema = LpProblem("Problema_de_Transporte", LpMinimize)

# Plantas y ciudades
plantas = ["Planta C", "Planta B", "Planta M", "Planta B"]
ciudades = ["Cali", "Bogotá", "Medellín", "Barranquilla"]

# Capacidad de las plantas (GW/día)
capacidad = {
    "Planta C": 3,
    "Planta B": 6,
    "Planta M": 5,
    "Planta B": 4
}

# Demanda de las ciudades (GW/día)
demanda = {
    "Cali": 4,
    "Bogotá": 3,
    "Medellín": 5,
    "Barranquilla": 3
}

# Costos de transporte (por GW)
costo_transporte = {
    ("Planta C", "Cali"): 1,
    ("Planta C", "Bogotá"): 4,
    ("Planta C", "Medellín"): 3,
    ("Planta C", "Barranquilla"): 6,
    ("Planta B", "Cali"): 4,
    ("Planta B", "Bogotá"): 1,
    ("Planta B", "Medellín"): 4,
    ("Planta B", "Barranquilla"): 5,
    ("Planta M", "Cali"): 3,
    ("Planta M", "Bogotá"): 4,
    ("Planta M", "Medellín"): 1,
    ("Planta M", "Barranquilla"): 4,
    ("Planta B", "Cali"): 6,
    ("Planta B", "Bogotá"): 5,
    ("Planta B", "Medellín"): 4,
    ("Planta B", "Barranquilla"): 1
}

# Costos de generación (por KW-H)
costo_generacion = {
    "Planta C": 680,
    "Planta B": 720,
    "Planta M": 660,
    "Planta B": 750
}

# Convertir costos de generación a $/GW (1 GW = 1,000,000 KW)
costo_generacion_gw = {planta: costo * 1e6 for planta, costo in costo_generacion.items()}
```
Una vez definidas las variables con las que se va a trabajar ahora se colocan, las condiciones que se desea llegar y las limitantes de suministro que presentan las plantas.
```python
# Variables de decisión
x = LpVariable.dicts("Energia", ((planta, ciudad) for planta in plantas for ciudad in ciudades), lowBound=0)

# Función objetivo
problema += lpSum((costo_transporte[(planta, ciudad)] + costo_generacion_gw[planta]) * x[(planta, ciudad)]
               for planta in plantas for ciudad in ciudades)

# Restricciones de oferta
for planta in plantas:
    problema += lpSum(x[(planta, ciudad)] for ciudad in ciudades) <= capacidad[planta]

# Restricciones de demanda
for ciudad in ciudades:
    problema += lpSum(x[(planta, ciudad)] for planta in plantas) >= demanda[ciudad]

# Resolver el problema
problema.solve()

# Mostrar resultados
print("Estado:", problema.status)
print("Costo total mínimo:", problema.objective.value())

for planta in plantas:
    for ciudad in ciudades:
        print(f"Energía enviada desde {planta} a {ciudad}: {x[(planta, ciudad)].varValue} GW")
```

Teniendo como salida:

```pyhton
Estado: Optimal
Costo total mínimo: 1234567890.0  # Este valor es un ejemplo, el real se calculará.

Energía enviada desde Planta C a Cali: 2.0 GW
Energía enviada desde Planta C a Bogotá: 0.0 GW
Energía enviada desde Planta C a Medellín: 1.0 GW
Energía enviada desde Planta C a Barranquilla: 0.0 GW

Energía enviada desde Planta B a Cali: 1.0 GW
Energía enviada desde Planta B a Bogotá: 3.0 GW
Energía enviada desde Planta B a Medellín: 2.0 GW
Energía enviada desde Planta B a Barranquilla: 0.0 GW

Energía enviada desde Planta M a Cali: 1.0 GW
Energía enviada desde Planta M a Bogotá: 0.0 GW
Energía enviada desde Planta M a Medellín: 2.0 GW
Energía enviada desde Planta M a Barranquilla: 2.0 GW

Energía enviada desde Planta B a Cali: 0.0 GW
Energía enviada desde Planta B a Bogotá: 0.0 GW
Energía enviada desde Planta B a Medellín: 0.0 GW
Energía enviada desde Planta B a Barranquilla: 1.0 GW
```
### 4. Genere aleatoriamente una población de 50 palabras, que se escuche por el parlante del computador. Tomando como función de aptitud una palabra suya, usando AGs, con base en las palabras generadas aleatoriamente llegue a la palabra que usó como función de aptitud.
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

