
# Cheatsheet Definitiva de Python: De Cero a Senior

Esta guía está diseñada para ser un recurso de referencia y un camino de aprendizaje progresivo. Cubre desde los conceptos más básicos hasta las herramientas y técnicas que definen a un desarrollador Python senior.

## 1\. Fundamentos de Python

La base de todo programa. Dominar estos conceptos es crucial antes de avanzar.

### Breve Explicación

  - **Variables**: Nombres que apuntan a valores en memoria.
  - **Tipos Primitivos**: Los bloques de construcción básicos de datos: `str` (texto), `int` (enteros), `float` (decimales), `bool` (verdadero/falso).
  - **Operadores**: Símbolos para realizar operaciones: aritméticos (`+`, `-`, `*`, `/`, `%`), de comparación (`==`, `!=`, `<`, `>`), lógicos (`and`, `or`, `not`).
  - **Entrada/Salida**: `print()` para mostrar información, `input()` para recibir datos del usuario.

### Ejemplos Prácticos

```python
# Variables y Tipos
nombre_curso = "Python Profesional"  # str
version = 3.12                       # float
num_estudiantes = 25                 # int
es_online = True                     # bool

# Operadores
suma = 10 + 5
es_mayor = suma > 20  # False

# Entrada y Salida
nombre_usuario = input("Introduce tu nombre: ")
print(f"Hola, {nombre_usuario}! Bienvenido al curso de {nombre_curso}.")
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa nombres de variables descriptivos y en `snake_case` (ej: `nombre_de_usuario`).
  - ❗ **Error común**: Confundir el operador de asignación (`=`) con el de comparación (`==`).
  - ❗ **Error común**: Intentar concatenar tipos diferentes sin conversión explícita (ej: `"Edad: " + 25`). Usa f-strings o `str()`.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Asignación múltiple para intercambiar variables o inicializar varias a la vez.
    ```python
    x, y = 5, 10
    x, y = y, x  # Intercambio elegante
    print(x, y)  # Output: 10 5
    ```

### Uso Real

Los fundamentos se usan en cada línea de código Python. Son la base para la lógica de negocio, manipulación de datos y algoritmos.

-----

## 2\. Estructuras de Control

Permiten que tu programa tome decisiones y repita acciones.

### Breve Explicación

  - **Condicionales (`if`/`elif`/`else`)**: Ejecutan bloques de código si se cumple una condición.
  - **Bucles (`for`, `while`)**: Iteran sobre secuencias (`for`) o se repiten mientras una condición sea verdadera (`while`).
  - **Control de Flujo (`break`, `continue`, `pass`)**:
      - `break`: Sale del bucle actual.
      - `continue`: Salta a la siguiente iteración del bucle.
      - `pass`: Es una operación nula, un marcador de posición.

### Ejemplos Prácticos

```python
# Condicionales
edad = 18
if edad < 18:
    print("Menor de edad")
elif edad == 18:
    print("Justo en el límite")
else:
    print("Mayor de edad")

# Bucle For
frameworks = ["Django", "Flask", "FastAPI"]
for i, fw in enumerate(frameworks):
    print(f"Índice {i}: {fw}")

# Bucle While con break y continue
contador = 0
while contador < 10:
    contador += 1
    if contador % 2 == 0:
        continue  # Salta los números pares
    print(f"Número impar: {contador}")
    if contador == 7:
        break # Termina el bucle si llega a 7
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Evita anidar `if`s profundamente. Usa funciones o rediseña la lógica.
  - ❗ **Error común**: Bucles infinitos en `while` si la condición de salida nunca se alcanza. Asegúrate de que la variable de control se actualice.
  - ✅ **Buenas prácticas**: Usa `for` sobre `while` cuando iteres sobre un iterable conocido. Es más seguro y legible.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa el "operador ternario" para condicionales en una sola línea.
    ```python
    resultado = "Aprobado" if nota >= 5 else "Suspendido"
    ```
  - ⚡ **Tip**: El bucle `for` puede tener una cláusula `else` que se ejecuta si el bucle termina sin un `break`.
    ```python
    for n in [2, 3, 5, 7]:
        if n == 4:
            print("Encontrado")
            break
    else:
        print("No encontrado") # Se ejecutará
    ```

### Uso Real

Validación de datos de entrada, procesamiento de colecciones, implementación de lógica de negocio (ej: "si el usuario es admin, mostrar panel").

-----

## 3\. Estructuras de Datos

Colecciones eficientes para almacenar y organizar datos.

### Breve Explicación

  - **Listas (`list`)**: Colecciones ordenadas y *mutables*.
  - **Tuplas (`tuple`)**: Colecciones ordenadas e *inmutables*.
  - **Diccionarios (`dict`)**: Pares clave-valor, sin orden (aunque en Python 3.7+ mantienen el orden de inserción).
  - **Conjuntos (`set`)**: Colecciones desordenadas de elementos únicos.
  - **Comprensiones**: Sintaxis concisa para crear listas, dicts y sets.

### Ejemplos Prácticos

```python
# Listas (mutables)
numeros = [1, 2, 3, 4, 5]
numeros.append(6)
primer_elemento = numeros[0]
sub_lista = numeros[1:4]  # Slicing: [2, 3, 4]

# Tuplas (inmutables)
coordenadas = (10.0, 20.5)
# coordenadas[0] = 5.0  # Esto daría un TypeError

# Diccionarios
configuracion = {"host": "localhost", "port": 8080, "user": "admin"}
print(configuracion["host"])  # Acceso por clave
configuracion["timeout"] = 30 # Añadir nuevo par

# Sets (elementos únicos)
tags = {"python", "dev", "code", "python"}
print(tags)  # Output: {'code', 'dev', 'python'}

# Comprensión de listas
cuadrados = [x**2 for x in range(10) if x % 2 == 0]
# [0, 4, 16, 36, 64]
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa tuplas para datos que no deben cambiar (ej: coordenadas, constantes). Usa listas para datos que esperas modificar.
  - ❗ **Error común**: Modificar una lista mientras se itera sobre ella puede causar comportamientos inesperados. Itera sobre una copia: `for item in mi_lista[:]`.
  - ✅ **Buenas prácticas**: Usa `dict.get(key, default_value)` para evitar `KeyError` al acceder a claves que podrían no existir.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa la desestructuración (unpacking) para asignar elementos de listas/tuplas a variables.
    ```python
    nombre, extension = "documento.txt".split('.')
    ```
  - ⚡ **Tip**: La comprensión de diccionarios es muy potente.
    ```python
    cuadrados_dict = {x: x**2 for x in range(5)}
    # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
    ```

### Uso Real

  - **Listas**: Almacenar resultados de una consulta a base de datos.
  - **Diccionarios**: Representar objetos JSON, configuraciones, pasar datos de forma estructurada.
  - **Sets**: Eliminar duplicados de una lista, realizar operaciones matemáticas de conjuntos (unión, intersección).

-----

## 4\. Funciones

Bloques de código reutilizables que realizan una tarea específica.

### Breve Explicación

  - **Definición (`def`)**: Crea una función.
  - **Argumentos**: Valores que se pasan a la función. Pueden ser posicionales o por nombre (keyword).
  - `*args` y `**kwargs`: Sintaxis para aceptar un número variable de argumentos posicionales (`*args`) y de palabra clave (`**kwargs`).
  - **Funciones Anidadas**: Funciones definidas dentro de otras funciones.
  - **Lambda**: Funciones anónimas y de una sola línea.

### Ejemplos Prácticos

```python
# Función básica con argumentos y valor de retorno
def saludar(nombre, saludo="Hola"):
    return f"{saludo}, {nombre}!"

print(saludar("Ana"))
print(saludar("Juan", saludo="Buenos días"))

# Función con *args y **kwargs
def procesar_datos(*args, **kwargs):
    print("Argumentos posicionales (tupla):", args)
    print("Argumentos de palabra clave (dict):", kwargs)

procesar_datos(1, 2, 3, user="root", active=True)

# Función Lambda
sumar = lambda a, b: a + b
print(sumar(5, 3)) # Output: 8

# Uso de lambda en una función de orden superior
numeros = [1, -2, 3, -4]
numeros_positivos = list(filter(lambda x: x > 0, numeros))
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Las funciones deben tener una única responsabilidad (Principio de Responsabilidad Única).
  - ❗ **Error común**: Usar un tipo mutable (como una lista o dict) como valor de argumento por defecto. Se comparte entre todas las llamadas a la función.
    ```python
    # ¡MAL!
    def agregar_item(item, mi_lista=[]):
        mi_lista.append(item)
        return mi_lista

    # Forma correcta
    def agregar_item_correcto(item, mi_lista=None):
        if mi_lista is None:
            mi_lista = []
        mi_lista.append(item)
        return mi_lista
    ```

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa la desestructuración de `**kwargs` para pasar diccionarios a funciones.
    ```python
    db_config = {"host": "db.server.com", "port": 5432}
    # conectar(**db_config) en lugar de conectar(host="...", port="...")
    ```
  - ⚡ **Tip**: Las funciones son objetos de primera clase. Puedes pasarlas como argumentos, devolverlas desde otras funciones y asignarlas a variables.

### Uso Real

Abstracción de lógica, reutilización de código, creación de APIs y librerías. El pilar de un código modular y mantenible.

-----

## 5\. Manejo de Errores y Excepciones

Gestionar errores de forma robusta para crear aplicaciones fiables.

### Breve Explicación

  - `try`/`except`: El bloque `try` contiene código que podría fallar. El bloque `except` captura y maneja la excepción (error).
  - `else`: Se ejecuta si el bloque `try` *no* lanza una excepción.
  - `finally`: Se ejecuta *siempre*, haya o no una excepción. Ideal para liberar recursos.
  - `raise`: Lanza una excepción manualmente.

### Ejemplos Prácticos

```python
def dividir(a, b):
    try:
        resultado = a / b
    except ZeroDivisionError:
        print("Error: No se puede dividir por cero.")
        return None
    except TypeError:
        print("Error: Ambos operandos deben ser números.")
        return None
    else:
        print("División realizada con éxito.")
        return resultado
    finally:
        print("Ejecutando el bloque finally (limpieza).")

dividir(10, 2)
dividir(10, 0)
dividir("a", 2)
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Sé específico al capturar excepciones (`except ValueError:` en lugar de `except:`). Capturar `Exception` de forma genérica puede ocultar bugs.
  - ❗ **Error común**: Usar el manejo de excepciones para control de flujo normal. Usa `if`/`else` si la condición no es excepcional.
  - ✅ **Buenas prácticas**: Registra el error completo (traceback) en un sistema de logging en lugar de solo imprimir un mensaje.
  - ✅ **Buenas prácticas**: Considera crear tus propias excepciones personalizadas heredando de `Exception` para representar errores específicos de tu dominio.
    ```python
    class APIError(Exception):
        pass

    # raise APIError("Fallo al contactar el servicio externo")
    ```

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Puedes capturar múltiples excepciones en una sola línea.
    ```python
    try:
        # ...
    except (ValueError, TypeError) as e:
        print(f"Ocurrió un error de valor o tipo: {e}")
    ```

### Uso Real

Validar entradas del usuario, gestionar fallos de red, manejar errores al leer archivos o interactuar con bases de datos. Esencial para cualquier aplicación en producción.

-----

## 6\. Programación Orientada a Objetos (OOP)

Modela entidades del mundo real con sus propiedades (atributos) y comportamientos (métodos).

### Breve Explicación

  - **Clase (`class`)**: Plantilla para crear objetos.
  - **Objeto**: Instancia de una clase.
  - **`__init__`**: Método constructor, se llama al crear un objeto. `self` representa la instancia.
  - **Herencia**: Una clase (hija) puede heredar atributos y métodos de otra (padre).
  - **Encapsulamiento**: Ocultar la complejidad interna. En Python se logra por convención con `_` (protegido) y `__` (privado, con *name mangling*).
  - `@staticmethod` y `@classmethod`: Métodos que no dependen de una instancia. `@classmethod` recibe la clase como primer argumento (`cls`).

### Ejemplos Prácticos

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
        self._kilometraje = 0  # Atributo protegido

    def __str__(self):
        # Representación como string para el usuario
        return f"{self.marca} {self.modelo} ({self._kilometraje} km)"

    def avanzar(self, km):
        if km > 0:
            self._kilometraje += km

# Herencia
class Coche(Vehiculo):
    def __init__(self, marca, modelo, num_puertas):
        super().__init__(marca, modelo) # Llama al constructor del padre
        self.num_puertas = num_puertas

    def tocar_bocina(self):
        print("¡Pip, pip!")

    @classmethod
    def crear_coche_de_carreras(cls, modelo):
        # Factory method usando un classmethod
        return cls("Ferrari", modelo, 2)

mi_coche = Coche("Seat", "Ibiza", 5)
mi_coche.avanzar(100)
print(mi_coche)
mi_coche.tocar_bocina()

coche_carreras = Coche.crear_coche_de_carreras("F1")
print(coche_carreras)
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa la composición sobre la herencia cuando sea posible. Es más flexible.
  - ❗ **Error común**: Olvidar `self` como primer argumento de los métodos de instancia.
  - ✅ **Buenas prácticas**: Usa `super().__init__(...)` para asegurarte de que la inicialización de la clase padre se ejecute en la herencia.
  - ✅ **Buenas prácticas**: Define `__str__` (para usuarios) y `__repr__` (para desarrolladores, debe ser inequívoca) en tus clases.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa `dataclasses` (Python 3.7+) para crear clases que principalmente almacenan datos, reduciendo el código repetitivo (`__init__`, `__repr__`, etc.).
    ```python
    from dataclasses import dataclass

    @dataclass
    class Punto:
        x: float
        y: float
    ```

### Uso Real

Modelar cualquier entidad compleja: usuarios, productos, conexiones a bases de datos. Fundamental en frameworks como Django y en el diseño de software a gran escala.

-----

*Esta guía continuará en las siguientes secciones para cubrir todos los temas solicitados.*

-----

## 7\. Módulos y Paquetes

Organiza tu código en archivos y directorios para hacerlo mantenible y reutilizable.

### Breve Explicación

  - **Módulo**: Un archivo `.py` que contiene código Python.
  - **Paquete**: Un directorio que contiene módulos y un archivo `__init__.py` (aunque en Python 3.3+ es opcional, sigue siendo una buena práctica).
  - **`import`**: Trae un módulo (o un objeto de un módulo) al espacio de nombres actual.
      - `import modulo`: Importa el módulo completo.
      - `from paquete import modulo`: Importa un módulo específico de un paquete.
      - `from modulo import funcion`: Importa un objeto específico de un módulo.

### Ejemplos Prácticos

```
mi_proyecto/
├── main.py
├── mi_paquete/
│   ├── __init__.py
│   ├── utils.py
│   └── models.py
```

```python
# mi_paquete/utils.py
def formatear_texto(texto):
    return texto.strip().capitalize()

# mi_paquete/models.py
class Usuario:
    def __init__(self, nombre):
        self.nombre = nombre

# mi_paquete/__init__.py
# Puede estar vacío o puede exponer elementos del paquete
from .utils import formatear_texto
from .models import Usuario

print("Paquete 'mi_paquete' inicializado")

# main.py
# from mi_paquete.utils import formatear_texto
# from mi_paquete.models import Usuario

# O, si se definieron en __init__.py:
from mi_paquete import formatear_texto, Usuario

nombre_formateado = formatear_texto("  hola mundo  ")
usuario = Usuario(nombre_formateado)
print(usuario.nombre) # Output: Hola mundo
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa importaciones absolutas (`from mi_paquete import utils`) en lugar de relativas (`from . import utils`) siempre que sea posible para evitar ambigüedad.
  - ❗ **Error común**: Importaciones circulares, donde el módulo A importa B y el módulo B importa A. Esto suele indicar un mal diseño que necesita refactorización.
  - ✅ **Buenas prácticas**: Evita `from modulo import *`. Contamina el espacio de nombres y hace difícil saber de dónde viene cada función.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa alias para acortar nombres largos o evitar colisiones.
    ```python
    import numpy as np
    import pandas as pd
    from mi_paquete.very_long_module_name import a_function as short_name
    ```

### Uso Real

Cualquier proyecto que tenga más de un archivo. Es la base de la arquitectura de software en Python.

-----

## 8\. Manejo de Archivos

Interactúa con el sistema de archivos para leer y escribir datos.

### Breve Explicación

  - **`open()`**: Función integrada para abrir un archivo.
  - **Context Manager (`with`)**: La forma preferida de trabajar con archivos. Garantiza que el archivo se cierre automáticamente, incluso si ocurren errores.
  - **Modos**: `'r'` (lectura, por defecto), `'w'` (escritura, sobreescribe), `'a'` (añadir), `'b'` (binario, ej: `'rb'`).
  - **JSON y CSV**: Formatos comunes para el intercambio de datos. La librería estándar de Python tiene módulos para manejarlos.

### Ejemplos Prácticos

```python
import json
import csv

# Escribir y leer un archivo de texto
try:
    with open("saludo.txt", "w", encoding="utf-8") as f:
        f.write("Hola, mundo!\n")
        f.write("Adiós, mundo.\n")

    with open("saludo.txt", "r", encoding="utf-8") as f:
        for linea in f:
            print(linea.strip())
except IOError as e:
    print(f"Error de E/S: {e}")


# Trabajar con JSON
datos = {"id": 123, "nombre": "Producto A", "tags": ["A", "B"]}
with open("datos.json", "w") as f:
    json.dump(datos, f, indent=4)

with open("datos.json", "r") as f:
    datos_leidos = json.load(f)
    print(datos_leidos["nombre"])

# Trabajar con CSV
with open("usuarios.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["id", "nombre", "email"])
    writer.writerow([1, "Ana", "ana@example.com"])
    writer.writerow([2, "Luis", "luis@example.com"])
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa siempre `with open(...)`. Garantiza el cierre del archivo.
  - ❗ **Error común**: Olvidar especificar la codificación (`encoding="utf-8"`). Puede causar errores en diferentes sistemas operativos o con caracteres no ASCII.
  - ✅ **Buenas prácticas**: Usa la librería `pathlib` para manejar rutas de archivos de forma orientada a objetos y multi-plataforma.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Para leer todas las líneas de un archivo en una lista:
    ```python
    with open("archivo.txt", "r") as f:
        lineas = f.read().splitlines()
    ```
  - ⚡ **Tip**: Usa `pathlib` para una manipulación de rutas más limpia.
    ```python
    from pathlib import Path
    ruta = Path("mi_directorio") / "sub_directorio" / "archivo.txt"
    ruta.parent.mkdir(parents=True, exist_ok=True) # Crea directorios padres
    ruta.write_text("Contenido del archivo")
    ```

### Uso Real

Leer ficheros de configuración, procesar logs, guardar y cargar el estado de una aplicación, generar reportes en CSV.

-----

## 9\. Decoradores y Generadores

Conceptos avanzados de programación funcional que permiten un código más expresivo y eficiente.

### Breve Explicación

  - **Decorador**: Una función que toma otra función como argumento, le añade alguna funcionalidad y devuelve otra función, sin modificar el código de la función original. Es "azúcar sintáctico" para funciones de orden superior.
  - **Generador**: Una función que utiliza la palabra clave `yield` para devolver un iterador. Los generadores producen valores uno a uno ("lazy evaluation"), lo que es muy eficiente en memoria para grandes secuencias de datos.

### Ejemplos Prácticos

```python
# Decorador simple para medir el tiempo de ejecución
import time

def cronometro(func):
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fin = time.time()
        print(f"'{func.__name__}' tardó {fin - inicio:.4f} segundos.")
        return resultado
    return wrapper

@cronometro
def tarea_larga(n):
    """Una función que simula una tarea costosa."""
    time.sleep(n)
    return "Tarea completada"

print(tarea_larga(1))


# Generador para la secuencia de Fibonacci
def fibonacci_generator(limite):
    a, b = 0, 1
    while a < limite:
        yield a
        a, b = b, a + b

# El generador no calcula todos los números a la vez
fib_gen = fibonacci_generator(100)
for num in fib_gen:
    print(num, end=" ") # 0 1 1 2 3 5 8 13 21 34 55 89
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa `functools.wraps` en tus decoradores para preservar los metadatos de la función original (`__name__`, `__doc__`).
  - ❗ **Error común**: Confundir una función generadora con una normal. Si contiene `yield`, devuelve un objeto generador, no se ejecuta hasta que se itera sobre él.
  - ✅ **Buenas prácticas**: Usa generadores en lugar de listas para procesar grandes volúmenes de datos (ej. leer un archivo de gigabytes línea por línea) para no agotar la memoria.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Las expresiones generadoras son como las comprensiones de listas, pero con paréntesis. Crean un generador sobre la marcha.
    ```python
    suma_cuadrados = sum(x**2 for x in range(1_000_000)) # Eficiente en memoria
    ```
  - ⚡ **Tip**: El módulo `itertools` contiene funciones de alto rendimiento que operan sobre iteradores y generadores (`chain`, `islice`, `cycle`).

### Uso Real

  - **Decoradores**: Logging, caching, autenticación y autorización (muy común en frameworks web como Flask y FastAPI), gestión de transacciones de bases de datos.
  - **Generadores**: Pipelines de procesamiento de datos, lectura de grandes ficheros, generación de secuencias infinitas.

-----

## 10\. Librerías Estándar Clave

Python viene "con las pilas incluidas". Conocer estas librerías te ahorra tiempo y esfuerzo.

### Breve Explicación

  - `datetime`: Para trabajar con fechas y horas.
  - `os` y `pathlib`: Interacción con el sistema operativo y manipulación de rutas. `pathlib` es la opción moderna.
  - `sys`: Acceso a parámetros y funciones específicas del sistema (intérprete).
  - `collections`: Estructuras de datos especializadas (`Counter`, `defaultdict`, `deque`).
  - `typing`: Soporte para anotaciones de tipo.

### Ejemplos Prácticos

```python
import datetime as dt
from pathlib import Path
from collections import Counter, defaultdict
import sys

# datetime
ahora = dt.datetime.now(dt.timezone.utc)
print(f"Ahora (UTC): {ahora.isoformat()}")
mañana = ahora + dt.timedelta(days=1)

# pathlib
ruta_actual = Path.cwd() # Current Working Directory
ruta_archivo = ruta_actual / "mi_app" / "settings.ini"
print(f"Ruta construida: {ruta_archivo}")
print(f"¿Existe? {ruta_archivo.exists()}")

# collections.Counter
votos = ["rojo", "azul", "verde", "rojo", "azul", "rojo"]
conteo_votos = Counter(votos)
print(conteo_votos) # Counter({'rojo': 3, 'azul': 2, 'verde': 1})
print(conteo_votos.most_common(1)) # [('rojo', 3)]

# collections.defaultdict
d = defaultdict(int) # Si una clave no existe, su valor por defecto será int() -> 0
d["a"] += 1
print(d["a"]) # 1
print(d["b"]) # 0

# sys
print(f"Argumentos de línea de comandos: {sys.argv}")
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa siempre fechas y horas con zona horaria (`aware`) para evitar ambigüedades, especialmente en aplicaciones de servidor. El estándar es UTC.
  - ❗ **Error común**: Concatenar rutas con `+` y strings. Esto no es portable entre sistemas operativos. Usa `pathlib` o `os.path.join`.
  - ✅ **Buenas prácticas**: Revisa la documentación de `collections`. `deque` es mucho más rápido que `list` para operaciones `pop` y `append` en ambos extremos.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa `datetime.fromisoformat()` para parsear fechas en formato ISO 8601 de forma robusta.
  - ⚡ **Tip**: `defaultdict(list)` es muy útil para agrupar elementos.
    ```python
    agrupado = defaultdict(list)
    for k, v in [('a', 1), ('b', 2), ('a', 3)]:
        agrupado[k].append(v)
    # defaultdict(<class 'list'>, {'a': [1, 3], 'b': [2]})
    ```

### Uso Real

Casi cualquier aplicación real necesita una o más de estas librerías para tareas comunes como manejar fechas, rutas, configuraciones o procesar datos de forma eficiente.

-----

## 11\. Tipado y Anotaciones Modernas

Añade información de tipos a tu código para mejorar la legibilidad, mantenibilidad y permitir la detección de errores estáticos.

### Breve Explicación

  - **Type Hints (Anotaciones de tipo)**: Sintaxis (introducida en PEP 484) para indicar los tipos esperados de variables, argumentos de funciones y valores de retorno.
  - `typing`: Módulo que provee tipos más complejos (`List`, `Dict`, `Union`, `Optional`, `TypedDict`).
  - `mypy`: Una herramienta externa (comprobador de tipos estático) que analiza el código con anotaciones de tipo y reporta errores de inconsistencia.

### Ejemplos Prácticos

```python
from typing import List, Dict, Optional, Union, TypedDict

# Anotaciones en variables y funciones
nombre: str = "Guido"
PI: float = 3.14159

def saludar(nombre: str) -> str:
    return f"Hola, {nombre}"

# Tipos complejos
def procesar_ids(ids: List[int]) -> None:
    for id_usuario in ids:
        print(id_usuario)

# Optional indica que un valor puede ser el tipo o None
def buscar_usuario(id_usuario: int) -> Optional[Dict[str, Union[int, str]]]:
    if id_usuario == 1:
        return {"id": 1, "nombre": "Admin"}
    return None # Válido porque el retorno es Optional

# TypedDict para diccionarios con una estructura fija
class UsuarioDict(TypedDict):
    id: int
    nombre: str
    email: Optional[str]

def procesar_usuario(usuario: UsuarioDict) -> None:
    print(f"Procesando a {usuario['nombre']}")

admin_user: UsuarioDict = {"id": 1, "nombre": "Admin", "email": None}
procesar_usuario(admin_user)
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Integra `mypy` en tu flujo de CI/CD (Integración Continua) para detectar errores de tipo automáticamente.
  - ❗ **Error común**: Creer que las anotaciones de tipo afectan el rendimiento o el comportamiento en tiempo de ejecución. Python las ignora; son solo para herramientas estáticas y para la legibilidad.
  - ✅ **Buenas prácticas**: Sé gradual. Puedes empezar a añadir tipos a las partes más críticas de tu código y expandir desde ahí.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: A partir de Python 3.10, puedes usar la sintaxis nativa `|` para `Union` y tipos genéricos en minúsculas.
    ```python
    # Python 3.10+
    def buscar_usuario(id_usuario: int) -> dict[str, int | str] | None:
        # ...
        pass
    ```
  - ⚡ **Tip**: Usa `TypeAlias` (Python 3.10+) para crear alias de tipos complejos y reutilizables.
    ```python
    from typing import TypeAlias

    IDUsuario: TypeAlias = int
    ResultadoAPI: TypeAlias = dict[str, any]
    ```

### Uso Real

Fundamental en proyectos grandes y de equipo. Mejora la colaboración, reduce bugs, y facilita el autocompletado y análisis en los IDEs. Frameworks modernos como FastAPI lo usan intensivamente.

-----

## 12\. Testing en Python

Escribe pruebas automatizadas para asegurar que tu código funciona como se espera y prevenir regresiones.

### Breve Explicación

  - **`unittest`**: Framework de testing incluido en la librería estándar. Inspirado en xUnit de Java.
  - **`pytest`**: El framework de testing de facto en la comunidad Python. Menos repetitivo, más potente y con un gran ecosistema de plugins.
  - **Mocking**: Reemplazar partes de tu sistema (ej: una llamada a una API externa, una consulta a base de datos) con objetos falsos para aislar el código que estás probando.

### Ejemplos Prácticos

Supongamos que tenemos una función en `calculadora.py`:

```python
# calculadora.py
def sumar(a, b):
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        raise TypeError("Ambos operandos deben ser números")
    return a + b
```

Ahora, una prueba con `pytest`:

```python
# test_calculadora.py
# No se necesita importación especial, pytest descubre las funciones `test_*`
import pytest
from calculadora import sumar # Importamos la función a probar

def test_sumar_positivos():
    assert sumar(2, 3) == 5

def test_sumar_negativos():
    assert sumar(-1, -1) == -2

def test_sumar_con_cero():
    assert sumar(5, 0) == 5

# Probar que una excepción se lanza correctamente
def test_sumar_lanza_excepcion_con_tipos_invalidos():
    with pytest.raises(TypeError):
        sumar("a", 2)
```

Para ejecutarlo, simplemente corre `pytest` en la terminal.

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Las pruebas deben ser rápidas, independientes y repetibles (FIRST).
  - ❗ **Error común**: Escribir pruebas que dependen de servicios externos (bases de datos, APIs). Usa mocks para aislar tus pruebas.
  - ✅ **Buenas prácticas**: Estructura tus pruebas siguiendo la estructura de tu código. Si tienes `mi_app/modulo.py`, crea `tests/test_modulo.py`.
  - ✅ **Buenas prácticas**: Usa `pytest.fixture` para crear y limpiar recursos que usan varias pruebas (ej. una conexión a una base de datos de prueba).

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa el plugin `pytest-cov` para medir la cobertura de tus pruebas (qué porcentaje de tu código está siendo probado).
  - ⚡ **Tip**: `unittest.mock.patch` es un decorador o context manager muy potente para mockear objetos temporalmente.
    ```python
    from unittest.mock import patch

    def test_mi_funcion_con_mock():
        with patch('modulo.api_externa.llamar') as mock_llamar:
            mock_llamar.return_value = {"status": "ok"}
            # ... tu código que llama a la función ...
            mock_llamar.assert_called_once() # Verifica que fue llamada
    ```

### Uso Real

Indispensable en el desarrollo profesional. Garantiza la calidad del software, facilita la refactorización segura y es un pilar de las metodologías ágiles y DevOps.

-----

## 13\. Buenas Prácticas y Código Limpio

Escribe código que no solo funcione, sino que sea fácil de leer, entender y mantener por otros (y por tu yo futuro).

### Breve Explicación

  - **PEP 8**: La guía de estilo oficial para el código Python. Define convenciones para nombramiento, formato, indentación, etc.
  - **Convenciones Idiomáticas**: Escribir código "Pythonic", que se siente natural en el lenguaje (ej: usar comprensiones, `with`, `enumerate`).
  - **Docstrings**: Cadenas de documentación para módulos, clases y funciones que explican su propósito. Herramientas como Sphinx las pueden convertir en documentación web.
  - **Modularidad**: Separar el código en componentes lógicos (funciones, clases, módulos) con responsabilidades bien definidas.

### Ejemplos Prácticos

```python
# Código NO idiomático y difícil de leer
def f(l):
    temp=[]
    for i in range(len(l)):
        if l[i]%2==0:
            temp.append(l[i]*2)
    return temp

# Código LIMPIO, Pythonic y bien documentado
def duplicar_pares(numeros: List[int]) -> List[int]:
    """
    Toma una lista de enteros y devuelve una nueva lista
    conteniendo solo los números pares, duplicados.

    Args:
        numeros: Una lista de enteros.

    Returns:
        Una nueva lista con los pares duplicados.
    """
    # Usa una comprensión de listas: más conciso y eficiente
    return [n * 2 for n in numeros if n % 2 == 0]

# Uso de herramientas de linting y formateo
# Herramientas como `black`, `flake8` e `isort` automatizan el formato y
# la detección de problemas de estilo.
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: Usa un formateador automático como `black` y un linter como `flake8` o `ruff`. Elimina las discusiones sobre estilo.
  - ❗ **Error común**: Escribir funciones muy largas o clases que hacen demasiadas cosas. Aplica el Principio de Responsabilidad Única.
  - ✅ **Buenas prácticas**: Los comentarios explican el *porqué* (la intención), el código explica el *cómo*. Un buen código a menudo no necesita comentarios.
  - ✅ **Buenas prácticas**: Escribe docstrings en un formato estándar como Google, NumPy o reStructuredText.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: "Pide perdón, no permiso" (EAFP - Easier to Ask for Forgiveness than Permission). A menudo es más Pythonic usar `try/except` que comprobar condiciones previamente.
    ```python
    # No tan Pythonic (LBYL - Look Before You Leap)
    if 'key' in my_dict:
        print(my_dict['key'])

    # Pythonic (EAFP)
    try:
        print(my_dict['key'])
    except KeyError:
        pass # O manejar el error
    ```

### Uso Real

La diferencia fundamental entre un programador junior y uno senior. El código limpio reduce el coste de mantenimiento, facilita la incorporación de nuevos desarrolladores y disminuye la probabilidad de bugs.

-----

## 14\. Productividad y Trucos Avanzados

Técnicas que te hacen más eficiente y te permiten entender Python a un nivel más profundo.

### Breve Explicación

  - **One-liners**: Expresiones concisas para realizar tareas complejas en una sola línea. Deben usarse con juicio para no sacrificar la legibilidad.
  - **Introspección**: La capacidad de un programa para examinar el tipo o las propiedades de un objeto en tiempo de ejecución.
  - **Profiling**: Medir el rendimiento de tu código (tiempo de ejecución, uso de memoria) para encontrar cuellos de botella.
  - **Debugging Avanzado**: Usar herramientas más allá de `print()` para depurar.

### Ejemplos Prácticos

```python
# One-liners (usar con cuidado)
# Invertir un diccionario
mi_dict = {'a': 1, 'b': 2}
dict_invertido = {v: k for k, v in mi_dict.items()}

# Introspección
print(dir(mi_dict)) # Muestra todos los atributos y métodos
print(isinstance(mi_dict, dict)) # True
if hasattr(mi_dict, 'keys'):
    print("El objeto tiene un método 'keys'")

# Profiling básico
import cProfile

def computacion_intensiva():
    return sum(i**2 for i in range(100000))

# cProfile.run('computacion_intensiva()')

# Debugging avanzado con `breakpoint()` (Python 3.7+)
def mi_funcion(a, b):
    resultado = a + b
    # El código se detendrá aquí y te dará una consola (PDB)
    breakpoint()
    return resultado

# mi_funcion(2, "tres") # Provocaría un error
```

### Buenas Prácticas y Errores Comunes

  - ✅ **Buenas prácticas**: El código legible es mejor que el código inteligente. No sacrifiques la claridad por un one-liner oscuro.
  - ❗ **Error común**: Optimización prematura. No intentes optimizar el código hasta que hayas medido (`profiled`) y sepas dónde están los cuellos de botella reales.
  - ✅ **Buenas prácticas**: Aprende a usar el depurador de tu IDE (VS Code, PyCharm) o PDB. Es mucho más potente que usar `print()`.

### Tips y Trucos Pythonic

  - ⚡ **Tip**: Usa `zip` para iterar sobre múltiples secuencias a la vez.
    ```python
    nombres = ["Alice", "Bob"]
    edades = [30, 25]
    for nombre, edad in zip(nombres, edades):
        print(f"{nombre} tiene {edad} años.")
    ```
  - ⚡ **Tip**: Usa el operador "morsa" `:=` (Python 3.8+) para asignar un valor a una variable como parte de una expresión.
    ```python
    # Útil para evitar llamadas duplicadas en bucles o comprensiones
    # if (n := len(a)) > 10:
    #     print(f"La lista es demasiado larga ({n} elementos)")
    ```

### Uso Real

  - **Profiling**: Optimizar el rendimiento de APIs, algoritmos de data science o cualquier código crítico.
  - **Introspección**: Construir frameworks, librerías y herramientas que necesitan adaptarse a diferentes tipos de objetos.
  - **Debugging**: Resolver bugs complejos en cualquier tipo de aplicación.

-----

## 15\. Errores Comunes y Cómo Evitarlos

Lecciones aprendidas de la experiencia para evitar las trampas más frecuentes.

### Breve Explicación

Esta sección recopila errores típicos que confunden tanto a principiantes como a desarrolladores intermedios, junto con las soluciones correctas.

### Casos Reales y Consejos Senior

1.  **Argumentos Mutables por Defecto** (ya mencionado, pero crucial)

      - ❗ **El problema**: `def func(a, b=[])` crea *una sola lista* cuando se define la función. Todas las llamadas sin `b` modificarán esa misma lista.
      - ✅ **La solución**: `def func(a, b=None): if b is None: b = []`.

2.  **Copias de Listas (Shallow vs. Deep)**

      - ❗ **El problema**: `nueva_lista = vieja_lista` no crea una copia. Ambas variables apuntan a la *misma* lista. `nueva_lista = vieja_lista[:]` o `vieja_lista.copy()` crea una copia superficial (*shallow*). Si la lista contiene objetos mutables (como otras listas), esos objetos internos *no* se copian.
      - ✅ **La solución**: Para una copia completa e independiente, usa `import copy; nueva_lista = copy.deepcopy(vieja_lista)`.

3.  **Comparación con `is` vs. `==`**

      - ❗ **El problema**: `is` comprueba si dos variables apuntan al *mismo objeto en memoria* (identidad). `==` comprueba si los valores de los objetos son equivalentes. Para enteros pequeños y algunas cadenas, Python puede reutilizar objetos, haciendo que `is` funcione por casualidad, pero no es fiable.
      - ✅ **La solución**: Usa `is` solo para comparar con singletons como `None`, `True` y `False`. Para todo lo demás (números, strings, listas), usa `==`.

4.  **Cierre Léxico en Bucles (Closures)**

      - ❗ **El problema**: Al crear funciones (ej. `lambda`) dentro de un bucle, estas no "capturan" el valor de la variable del bucle en cada iteración. Capturan la variable misma, que tendrá el último valor del bucle.
      - ✅ **La solución**: Usa un argumento por defecto en la `lambda` para forzar la evaluación del valor en ese momento: `lambda i=i: i * 2`.

<!-- end list -->

```python
# Ejemplo del problema del closure
funcs = []
for i in range(5):
    funcs.append(lambda: print(i)) # Todas las lambdas imprimirán 4

for f in funcs:
    f() # Imprime 4, 4, 4, 4, 4

# Solución
funcs_solucion = []
for i in range(5):
    funcs_solucion.append(lambda i=i: print(i))

for f in funcs_solucion:
    f() # Imprime 0, 1, 2, 3, 4
```

-----

## 16\. Herramientas del Ecosistema Python

El software que todo desarrollador Python profesional usa para gestionar proyectos y dependencias.

### Breve Explicación

  - **`pip`**: El instalador de paquetes para Python. Instala librerías desde el Python Package Index (PyPI).
  - **Entornos Virtuales (`venv`, `virtualenv`)**: Crean directorios aislados con una instalación de Python y sus propias librerías. Esencial para evitar conflictos de dependencias entre proyectos.
  - **`requirements.txt`**: Un archivo de texto que lista las dependencias de un proyecto, permitiendo que otros las instalen fácilmente con `pip install -r requirements.txt`.
  - **Herramientas Modernas (`Poetry`, `pipenv`)**: Herramientas que combinan la gestión de dependencias y entornos virtuales. `Poetry` también ayuda a construir y publicar paquetes.
  - **`pipx`**: Para instalar y ejecutar herramientas de línea de comandos de Python en entornos aislados, sin contaminar tu Python global.

### Ejemplos Prácticos de Flujo de Trabajo

```bash
# 1. Crear un entorno virtual para un nuevo proyecto
python3 -m venv .venv  # .venv es un nombre común para el directorio del entorno

# 2. Activar el entorno virtual
# En macOS / Linux
source .venv/bin/activate
# En Windows
# .venv\Scripts\activate

# (El prompt de tu terminal cambiará para indicar que estás en el entorno)

# 3. Instalar librerías
pip install requests
pip install "fastapi[all]"

# 4. Guardar las dependencias en requirements.txt
pip freeze > requirements.txt

# --- Otro desarrollador recibe el proyecto ---
# 1 & 2. Crear y activar su propio entorno
# 3. Instalar dependencias desde el archivo
# pip install -r requirements.txt
```

### Buenas Prácticas y Consejos Senior

  - ✅ **Buenas prácticas**: **Siempre** usa un entorno virtual para cada proyecto. Sin excepciones.
  - ✅ **Buenas prácticas**: Fija las versiones de tus dependencias (`requests==2.28.1`) en `requirements.txt` para garantizar compilaciones reproducibles. `pip freeze` lo hace automáticamente.
  - ✅ **Buenas prácticas**: Considera usar `Poetry` para proyectos nuevos. Gestiona un archivo `pyproject.toml` que es el estándar moderno, resuelve dependencias de forma más robusta y simplifica la publicación de paquetes.
  - ✅ **Buenas prácticas**: Usa `pipx` para instalar herramientas globales como `black`, `pytest`, o `poetry` en sí mismo.
    ```bash
    pipx install black
    pipx install poetry
    ```

### Uso Real

Este flujo de trabajo es el estándar de la industria para el desarrollo en Python, desde pequeños scripts hasta aplicaciones web masivas. Garantiza que los proyectos sean portables, reproducibles y que las dependencias estén gestionadas de forma limpia.
