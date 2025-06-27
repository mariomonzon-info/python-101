**Nivel Intermedio** de Python. 

Aquí entraremos en estructuras más potentes, técnicas de programación más sofisticadas y empezaremos a dar forma a habilidades cercanas al desarrollo profesional.

---

# 🧠 Módulo 1 – Estructuras de datos avanzadas y comprensión de listas

---

## 🎯 Objetivos del módulo

* Dominar estructuras como **conjuntos (`set`)**, **tuplas (`tuple`)** y **diccionarios (`dict`)**.
* Comprender su uso, ventajas y casos prácticos.
* Aprender y aplicar **comprensiones** (list, set, dict comprehensions).
* Trabajar con **enumeración**, **desempaquetado**, y **operaciones avanzadas** sobre colecciones.

---

## 🔹 Tuplas (`tuple`)

* Inmutables (no se pueden modificar).
* Más rápidas y ligeras que listas.
* Ideales para **datos fijos** o retornos múltiples.

📘 Ejemplo:

```python
coordenadas = (10, 20)
print(coordenadas[0])  # 10
```

### Desempaquetado de tuplas:

```python
x, y = coordenadas
```

---

## 🔹 Conjuntos (`set`)

* Colecciones **no ordenadas**, **sin elementos duplicados**.
* Muy útiles para comprobar existencia rápida, eliminar duplicados, etc.

📘 Crear un set:

```python
numeros = {1, 2, 3, 4, 2, 3}
print(numeros)  # {1, 2, 3, 4}
```

### Operaciones comunes:

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # Unión
print(a & b)  # Intersección
print(a - b)  # Diferencia
print(a ^ b)  # Diferencia simétrica
```

---

## 🔹 Diccionarios (`dict`)

* Almacenan **pares clave-valor**.
* Acceso muy rápido a través de la clave.

📘 Ejemplo:

```python
persona = {"nombre": "Lucía", "edad": 30}
print(persona["nombre"])  # Lucía
```

### Métodos útiles:

```python
persona.get("altura", "No disponible")
persona.keys()
persona.values()
persona.items()
```

---

## 🌀 Comprensiones (Comprehensions)

### 📄 List comprehension

```python
cuadrados = [x**2 for x in range(10)]
```

### 📄 Condicional en comprensión

```python
pares = [x for x in range(20) if x % 2 == 0]
```

---

### 📄 Set comprehension

```python
letras = {letra for letra in "banana"}
```

---

### 📄 Dict comprehension

```python
cuadrados = {x: x**2 for x in range(5)}
```

---

## 🧠 Enumeración y `zip`

### `enumerate()`

Permite iterar con índice:

```python
for i, valor in enumerate(["a", "b", "c"]):
    print(i, valor)
```

### `zip()`

Combina listas:

```python
nombres = ["Ana", "Luis"]
edades = [30, 25]

for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")
```

---

## ✅ Buenas prácticas

* Usa `tuples` cuando los datos no deban cambiar.
* Usa `sets` para eliminar duplicados y hacer comparaciones.
* Usa `dict` para estructurar datos clave-valor con significado.
* Aprovecha las comprensiones para escribir código más limpio y expresivo.

---

## ⚠️ Errores comunes

| Error                                      | Causa                                        |
| ------------------------------------------ | -------------------------------------------- |
| Mutar tuplas                               | No se puede, son inmutables                  |
| Acceder a claves inexistentes              | Usar `dict.get()` o `in` para evitar errores |
| Usar `[]` en lugar de `{}` para sets       | `{}` crea un dict vacío, no un set           |
| Usar comprensión cuando no mejora claridad | A veces es mejor un bucle normal             |

---

## 📝 Ejercicios prácticos

### Ejercicio 1

Dado un listado con nombres repetidos, crea una versión sin duplicados ordenada alfabéticamente.

---

### Ejercicio 2

Crea un diccionario que asocie cada letra de una palabra con su número de aparición.

Ejemplo: `"python"` → `{'p': 1, 'y': 1, 't': 1, 'h': 1, 'o': 1, 'n': 1}`

---

### Ejercicio 3

A partir de dos listas, crea un diccionario uniendo los elementos como clave y valor:

```python
nombres = ["Juan", "Marta"]
edades = [28, 35]
# {'Juan': 28, 'Marta': 35}
```

---

Perfecto, avanzamos con un módulo muy útil que te permitirá escribir código más flexible, limpio y potente:

---

# 🧠 Módulo 2 – Funciones avanzadas y programación funcional en Python

---

## 🎯 Objetivos del módulo

* Domprender cómo usar `*args` y `**kwargs` para funciones con número variable de argumentos.
* Aprender a usar funciones anónimas (`lambda`).
* Aplicar funciones de orden superior: `map()`, `filter()`, `reduce()`.
* Entender funciones como objetos de primera clase.
* Introducción a **closures** y funciones anidadas.

---

## 🧩 Funciones con argumentos variables

### `*args` (argumentos posicionales)

Permite pasar un número indeterminado de argumentos:

```python
def sumar(*numeros):
    return sum(numeros)

print(sumar(1, 2, 3, 4))  # 10
```

📌 `args` es una **tupla**.

---

### `**kwargs` (argumentos con clave)

Permite pasar argumentos nombrados arbitrarios:

```python
def mostrar_info(**datos):
    for clave, valor in datos.items():
        print(f"{clave}: {valor}")

mostrar_info(nombre="Ana", edad=30)
```

📌 `kwargs` es un **diccionario**.

---

## ⚡ Funciones anónimas (`lambda`)

Son funciones pequeñas y rápidas que se definen en una sola línea:

```python
doble = lambda x: x * 2
print(doble(4))  # 8
```

📌 Muy útiles como argumento en funciones como `map`, `filter`, `sorted`.

---

## 🔁 Funciones de orden superior

### `map()`

Aplica una función a cada elemento de un iterable:

```python
numeros = [1, 2, 3]
cuadrados = list(map(lambda x: x**2, numeros))  # [1, 4, 9]
```

---

### `filter()`

Filtra elementos según una condición:

```python
pares = list(filter(lambda x: x % 2 == 0, range(10)))  # [0, 2, 4, 6, 8]
```

---

### `reduce()` (requiere importar)

Aplica una función acumulativa:

```python
from functools import reduce

producto = reduce(lambda x, y: x * y, [1, 2, 3, 4])  # 24
```

---

## 🔄 Funciones como objetos

Las funciones se pueden **pasar como argumentos**, **devolver desde otras funciones** o **asignar a variables**:

```python
def saludar(nombre):
    return f"Hola {nombre}"

f = saludar
print(f("Mario"))  # Hola Mario
```

---

## 🧠 Funciones anidadas y closures

Una función puede definirse dentro de otra:

```python
def operacion(tipo):
    def suma(a, b):
        return a + b
    def resta(a, b):
        return a - b

    if tipo == "suma":
        return suma
    else:
        return resta

mi_funcion = operacion("suma")
print(mi_funcion(4, 2))  # 6
```

---

## ✅ Buenas prácticas

* Usa `*args` y `**kwargs` para funciones genéricas o decoradores.
* Prefiere funciones pequeñas, puras y reutilizables.
* Usa `lambda` solo para funciones simples y claras.
* Usa `map()` y `filter()` si mejoran la legibilidad, si no, prefiere comprensión de listas.

---

## ⚠️ Errores comunes

| Error                                 | Descripción                                       |
| ------------------------------------- | ------------------------------------------------- |
| Confundir `*args` y `**kwargs`        | `*` es para tuplas, `**` para diccionarios        |
| Lambda excesivamente complejas        | Pierden claridad, mejor definir una función       |
| No convertir `map` o `filter` a lista | Si no se convierte, se obtiene un objeto iterable |

---

## 📝 Ejercicios prácticos

### Ejercicio 1

Define una función `multiplicar_todos(*numeros)` que devuelva el producto de todos los valores pasados.

---

### Ejercicio 2

Filtra una lista de palabras, dejando solo las que tienen más de 5 letras usando `filter()` y `lambda`.

---

### Ejercicio 3

Crea una función `crear_saludo(nombre)` que devuelva otra función que, al llamarse, salude a esa persona.

---

### Ejercicio 4

Usa `map()` para convertir una lista de temperaturas en Celsius a Fahrenheit.
Fórmula: `°F = °C * 1.8 + 32`

---

Perfecto, seguimos avanzando en el Nivel Intermedio con herramientas muy potentes y profesionales:

---

# 🧠 Módulo 3 – Decoradores, Generadores e Iteradores Personalizados

---

## 🎯 Objetivos del módulo

* Comprender cómo funcionan los **decoradores** y para qué se utilizan.
* Crear y usar **generadores** con `yield` para flujos de datos eficientes.
* Implementar **iteradores personalizados** en clases.
* Usar estas herramientas para escribir código más limpio, reusable y eficiente.

---

## 🔄 Decoradores

Un **decorador** es una función que **modifica el comportamiento de otra función** sin alterar su código fuente.

### 📘 Sintaxis básica:

```python
def decorador(func):
    def nueva_funcion():
        print("Antes")
        func()
        print("Después")
    return nueva_funcion

@decorador
def saludar():
    print("Hola!")

saludar()
```

📌 Es equivalente a:

```python
saludar = decorador(saludar)
```

---

### 🔧 Decoradores con argumentos

```python
def decorador(func):
    def envoltura(*args, **kwargs):
        print("Ejecutando:", func.__name__)
        return func(*args, **kwargs)
    return envoltura
```

---

### 🎯 Decorador práctico: medir tiempo de ejecución

```python
import time

def medir_tiempo(func):
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fin = time.time()
        print(f"Tiempo: {fin - inicio:.4f} segundos")
        return resultado
    return wrapper

@medir_tiempo
def tarea_lenta():
    time.sleep(1)
    return "Hecho"

tarea_lenta()
```

---

## 🌀 Generadores

Un **generador** es una función que **devuelve un iterable** pero **no almacena todos los valores en memoria**. Ideal para grandes cantidades de datos.

📘 Usan la palabra clave `yield`:

```python
def cuenta():
    for i in range(5):
        yield i

for numero in cuenta():
    print(numero)
```

📌 Se pueden convertir a listas: `list(cuenta())`.

---

### 🎯 Ventaja: memoria eficiente

```python
# Usar un generador:
def cuadrados(n):
    for i in range(n):
        yield i * i
```

Este enfoque es **mucho más eficiente** que guardar todos los cuadrados en una lista.

---

## 🔁 Iteradores personalizados

Puedes crear clases que se comporten como iterables:

```python
class Contador:
    def __init__(self, limite):
        self.limite = limite
        self.actual = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.actual < self.limite:
            num = self.actual
            self.actual += 1
            return num
        else:
            raise StopIteration
```

📘 Uso:

```python
for numero in Contador(5):
    print(numero)
```

---

## ✅ Buenas prácticas

* Usa decoradores para añadir funcionalidades reutilizables (logs, validaciones, etc.).
* Usa generadores cuando no necesites todos los datos a la vez.
* Evita lógica compleja dentro de decoradores o `yield`.

---

## ⚠️ Errores comunes

| Error                                              | Causa                                       |
| -------------------------------------------------- | ------------------------------------------- |
| Olvidar retornar la función interna en decoradores | El decorador no funcionará correctamente    |
| Usar `return` en lugar de `yield` en generadores   | Termina la ejecución en la primera llamada  |
| No manejar `StopIteration` en iteradores manuales  | Fallos al recorrer iterables personalizados |

---

## 📝 Ejercicios prácticos

### Ejercicio 1

Crea un decorador que imprima el nombre y los argumentos de cualquier función que decore.

---

### Ejercicio 2

Escribe un generador que produzca los primeros `n` números de Fibonacci.

---

### Ejercicio 3

Crea una clase `RangoInverso` que itere desde un número hasta 0.

---

### Ejercicio 4

Crea un decorador `reintentar` que reintente ejecutar una función hasta 3 veces si lanza una excepción.

---

Perfecto, seguimos con un módulo clave para el desarrollo profesional y la creación de aplicaciones robustas:

---

# 🧠 Módulo 4 – Manejo de Excepciones y Testing en Python

---

## 🎯 Objetivos del módulo

* Aprender a identificar, lanzar y manejar **excepciones** en Python.
* Crear excepciones personalizadas.
* Escribir **tests unitarios** con `unittest`.
* Aplicar buenas prácticas para control de errores y validación.
* Entender la diferencia entre errores de sintaxis, lógicos y de ejecución.

---

## 🚨 ¿Qué es una excepción?

Una **excepción** es un evento que ocurre durante la ejecución y **interrumpe el flujo normal del programa**.

📘 Ejemplos comunes:

```python
print(5 / 0)  # ZeroDivisionError
print(undefinido)  # NameError
```

---

## 🧯 Captura de excepciones

### Sintaxis básica:

```python
try:
    # Código que puede fallar
    resultado = 10 / 0
except ZeroDivisionError:
    print("¡No se puede dividir entre cero!")
```

---

### Varios `except`:

```python
try:
    numero = int(input("Introduce un número: "))
except ValueError:
    print("Entrada no válida")
except KeyboardInterrupt:
    print("Interrupción del usuario")
```

---

### `else` y `finally`

```python
try:
    n = int(input("Número: "))
except ValueError:
    print("No es número")
else:
    print(f"El número es {n}")
finally:
    print("Fin del bloque")
```

📌 `finally` se ejecuta **siempre**.

---

## 🧨 Lanzar errores personalizados

```python
def dividir(a, b):
    if b == 0:
        raise ValueError("No se puede dividir por cero")
    return a / b
```

---

## 🚧 Crear excepciones propias

```python
class ErrorPersonalizado(Exception):
    pass

raise ErrorPersonalizado("Algo salió mal")
```

---

## 🧪 Testing con `unittest`

### Ejemplo básico:

```python
import unittest

def suma(a, b):
    return a + b

class TestSuma(unittest.TestCase):
    def test_suma_positivos(self):
        self.assertEqual(suma(2, 3), 5)

    def test_suma_negativos(self):
        self.assertEqual(suma(-1, -1), -2)

if __name__ == "__main__":
    unittest.main()
```

📌 Guarda este código como `test_suma.py` y ejecútalo desde terminal.

---

## 🧪 Métodos comunes en `unittest`

| Método                | Comprueba que...           |
| --------------------- | -------------------------- |
| `assertEqual(a, b)`   | `a == b`                   |
| `assertTrue(x)`       | `x es True`                |
| `assertFalse(x)`      | `x es False`               |
| `assertRaises(error)` | Se lanza un error esperado |

---

### 📌 Ejemplo con `assertRaises`

```python
def dividir(a, b):
    if b == 0:
        raise ZeroDivisionError
    return a / b

class TestDivision(unittest.TestCase):
    def test_division_por_cero(self):
        with self.assertRaises(ZeroDivisionError):
            dividir(10, 0)
```

---

## ✅ Buenas prácticas

* Maneja solo las excepciones que esperas.
* No uses excepciones como control de flujo habitual.
* Crea excepciones personalizadas para errores de dominio (negocio).
* Prueba funciones críticas con casos normales y extremos.
* Automatiza los tests con `pytest` o `unittest` en CI/CD.

---

## ⚠️ Errores comunes

| Error                                        | Causa                                      |
| -------------------------------------------- | ------------------------------------------ |
| Capturar todas las excepciones con `except:` | Oculta errores reales que deberías conocer |
| Olvidar cerrar archivos/red                  | No usar `finally` o `with open()`          |
| No probar errores esperados                  | Falsa sensación de seguridad en tus tests  |

---

## 📝 Ejercicios prácticos

### Ejercicio 1

Crea una función `leer_numero()` que pida un número al usuario y vuelva a intentarlo si falla (usa `try/except`).

---

### Ejercicio 2

Crea una función `raiz(n)` que devuelva la raíz cuadrada de un número, pero lance un error si es negativo.

---

### Ejercicio 3

Crea una clase `Calculadora` con métodos de suma, resta, multiplicación y división. Escribe tests para cada método.

---

### Ejercicio 4

Crea tu propia excepción `ErrorEdadInvalida` y úsala en una función que reciba una edad y la valide entre 0 y 120.

---

Excelente, vamos ahora con herramientas del **módulo estándar de Python** que te permitirán escribir código más expresivo, potente y profesional:

---

# 🧠 Módulo 5 – Colecciones avanzadas y utilidades del módulo estándar

---

## 🎯 Objetivos del módulo

* Usar estructuras avanzadas de `collections`: `namedtuple`, `Counter`, `defaultdict`, `deque`.
* Utilizar `itertools` para crear combinaciones, permutaciones y ciclos infinitos.
* Aplicar `functools` para memorizar resultados (`lru_cache`) y crear decoradores.
* Trabajar con fechas y tiempos usando `datetime`.

---

## 📦 `collections` – Estructuras especializadas

### 🔸 `namedtuple` – tuplas con nombre

```python
from collections import namedtuple

Persona = namedtuple("Persona", ["nombre", "edad"])
p = Persona("Ana", 30)

print(p.nombre)  # Ana
```

---

### 🔸 `Counter` – contador de elementos

```python
from collections import Counter

letras = Counter("banana")
print(letras)  # Counter({'a': 3, 'n': 2, 'b': 1})
```

---

### 🔸 `defaultdict` – diccionario con valores por defecto

```python
from collections import defaultdict

d = defaultdict(int)
d["a"] += 1
print(d)  # defaultdict(<class 'int'>, {'a': 1})
```

📌 Ideal para contar o agrupar sin inicializar.

---

### 🔸 `deque` – cola doblemente enlazada (rápida en ambos extremos)

```python
from collections import deque

cola = deque()
cola.append("a")
cola.appendleft("b")
print(cola)  # deque(['b', 'a'])
cola.pop()
cola.popleft()
```

---

## 🔁 `itertools` – iteradores eficientes

```python
from itertools import product, permutations, combinations, cycle, count, islice
```

### 🔹 `product()`

```python
from itertools import product
print(list(product([1, 2], ['a', 'b'])))
# [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]
```

---

### 🔹 `permutations()` y `combinations()`

```python
from itertools import permutations, combinations

print(list(permutations([1, 2, 3], 2)))  # Orden importa
print(list(combinations([1, 2, 3], 2)))  # Orden NO importa
```

---

### 🔹 `cycle()` y `count()`

```python
from itertools import cycle, count, islice

ciclo = cycle("ABC")
print(list(islice(ciclo, 6)))  # ['A', 'B', 'C', 'A', 'B', 'C']

for i in count(10, 2):  # Empieza en 10, paso 2
    print(i)
    if i >= 20:
        break
```

---

## 🛠️ `functools` – herramientas funcionales

### 🔸 `lru_cache` – cacheo automático de resultados

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print(fib(100))  # instantáneo gracias al cacheo
```

---

### 🔸 `partial` – funciones con argumentos predefinidos

```python
from functools import partial

def potencia(base, exponente):
    return base ** exponente

cuadrado = partial(potencia, exponente=2)
print(cuadrado(5))  # 25
```

---

## 🕓 `datetime` – trabajar con fechas y horas

```python
from datetime import datetime, timedelta

ahora = datetime.now()
mañana = ahora + timedelta(days=1)
ayer = ahora - timedelta(days=1)

print(ahora.strftime("%d/%m/%Y %H:%M"))
```

📌 Formatos útiles:

* `%d/%m/%Y`: 27/06/2025
* `%H:%M`: 14:35
* `%A`: nombre del día

---

## ✅ Buenas prácticas

* Usa `Counter` o `defaultdict` para contar ocurrencias o agrupar datos.
* Usa `itertools` para evitar bucles anidados costosos.
* Usa `lru_cache` para mejorar el rendimiento de funciones recursivas.
* Prefiere `datetime` frente a trabajar manualmente con strings de fechas.

---

## ⚠️ Errores comunes

| Error                                             | Motivo                                            |
| ------------------------------------------------- | ------------------------------------------------- |
| Usar diccionarios normales para contar            | Es más verboso que usar `Counter` o `defaultdict` |
| Usar bucles complejos sin `itertools`             | Menor legibilidad y eficiencia                    |
| No convertir `datetime` a string con `strftime()` | Resultado ilegible o confuso                      |

---

## 📝 Ejercicios prácticos

### Ejercicio 1

Dada una lista de palabras, usa `Counter` para encontrar la palabra más común.

---

### Ejercicio 2

Simula una cola FIFO con `deque` que recibe 5 elementos y elimina el más antiguo si supera el límite.

---

### Ejercicio 3

Crea todas las combinaciones posibles de una lista de colores usando `product()`.

---

### Ejercicio 4

Crea una función `fib(n)` decorada con `@lru_cache`, y mide el tiempo de ejecución comparado con una versión sin caché.

---

### Ejercicio 5

Calcula qué día será dentro de 100 días usando `datetime` y `timedelta`.

---

Perfecto. Cerraremos el **Nivel Intermedio** con un proyecto integrador que pondrá a prueba lo aprendido hasta ahora:

---

# 🧪 Módulo 6 – Proyecto práctico: Gestor de Biblioteca (Consola + Colecciones + Tests)

---

## 🎯 Objetivo del proyecto

Construir una aplicación de consola para gestionar una pequeña biblioteca.
Debe permitir:

* Añadir libros (con título, autor, ISBN, año).
* Buscar libros por autor o título.
* Marcar libros como prestados/devueltos.
* Guardar los datos en disco.
* Cargar datos desde archivo.
* Contar autores más comunes.
* Validar datos de entrada.
* Incluir **tests unitarios** para funciones clave.

---

## 🧱 Herramientas y conceptos aplicados

* Clases y objetos.
* Excepciones y validaciones.
* Colecciones (`Counter`, `defaultdict`).
* Archivos (`json`).
* Decoradores (para logging).
* Tests (`unittest`).

---

## 🧩 Estructura del programa

```bash
biblioteca/
├── main.py
├── libro.py
├── gestor.py
├── utils.py
├── datos.json
└── test_biblioteca.py
```

---

## 📄 `libro.py`

```python
class Libro:
    def __init__(self, titulo, autor, isbn, anio):
        self.titulo = titulo
        self.autor = autor
        self.isbn = isbn
        self.anio = anio
        self.prestado = False

    def marcar_prestado(self):
        self.prestado = True

    def marcar_devuelto(self):
        self.prestado = False

    def to_dict(self):
        return {
            "titulo": self.titulo,
            "autor": self.autor,
            "isbn": self.isbn,
            "anio": self.anio,
            "prestado": self.prestado,
        }

    @staticmethod
    def desde_dict(d):
        l = Libro(d["titulo"], d["autor"], d["isbn"], d["anio"])
        l.prestado = d["prestado"]
        return l
```

---

## 📄 `gestor.py`

```python
import json
from collections import Counter
from libro import Libro

class GestorBiblioteca:
    def __init__(self, archivo="datos.json"):
        self.archivo = archivo
        self.libros = []
        self.cargar()

    def agregar_libro(self, libro):
        if any(l.isbn == libro.isbn for l in self.libros):
            raise ValueError("ISBN duplicado")
        self.libros.append(libro)

    def buscar_por_autor(self, autor):
        return [l for l in self.libros if autor.lower() in l.autor.lower()]

    def buscar_por_titulo(self, titulo):
        return [l for l in self.libros if titulo.lower() in l.titulo.lower()]

    def marcar_prestado(self, isbn):
        libro = self._buscar_por_isbn(isbn)
        if libro: libro.marcar_prestado()

    def marcar_devuelto(self, isbn):
        libro = self._buscar_por_isbn(isbn)
        if libro: libro.marcar_devuelto()

    def _buscar_por_isbn(self, isbn):
        for l in self.libros:
            if l.isbn == isbn:
                return l
        raise ValueError("ISBN no encontrado")

    def autores_mas_comunes(self):
        return Counter([l.autor for l in self.libros]).most_common(3)

    def guardar(self):
        with open(self.archivo, "w", encoding="utf-8") as f:
            json.dump([l.to_dict() for l in self.libros], f, indent=4)

    def cargar(self):
        try:
            with open(self.archivo, "r", encoding="utf-8") as f:
                self.libros = [Libro.desde_dict(d) for d in json.load(f)]
        except FileNotFoundError:
            self.libros = []
```

---

## 📄 `main.py`

```python
from gestor import GestorBiblioteca
from libro import Libro

def mostrar_menu():
    print("\n1. Agregar libro")
    print("2. Buscar por autor")
    print("3. Buscar por título")
    print("4. Prestar libro")
    print("5. Devolver libro")
    print("6. Autores frecuentes")
    print("7. Guardar y salir")

def main():
    gestor = GestorBiblioteca()

    while True:
        mostrar_menu()
        opcion = input("Opción: ")

        try:
            if opcion == "1":
                t = input("Título: ")
                a = input("Autor: ")
                i = input("ISBN: ")
                y = int(input("Año: "))
                gestor.agregar_libro(Libro(t, a, i, y))
            elif opcion == "2":
                resultado = gestor.buscar_por_autor(input("Autor: "))
                for l in resultado:
                    print(f"{l.titulo} ({l.anio})")
            elif opcion == "3":
                resultado = gestor.buscar_por_titulo(input("Título: "))
                for l in resultado:
                    print(f"{l.titulo} ({l.anio})")
            elif opcion == "4":
                gestor.marcar_prestado(input("ISBN: "))
            elif opcion == "5":
                gestor.marcar_devuelto(input("ISBN: "))
            elif opcion == "6":
                print(gestor.autores_mas_comunes())
            elif opcion == "7":
                gestor.guardar()
                print("Datos guardados.")
                break
            else:
                print("Opción inválida.")
        except Exception as e:
            print(f"Error: {e}")

if __name__ == "__main__":
    main()
```

---

## 🧪 `test_biblioteca.py`

```python
import unittest
from gestor import GestorBiblioteca
from libro import Libro

class TestBiblioteca(unittest.TestCase):
    def setUp(self):
        self.gestor = GestorBiblioteca("test.json")
        self.gestor.libros = []

    def test_agregar_libro(self):
        l = Libro("Libro A", "Autor", "123", 2020)
        self.gestor.agregar_libro(l)
        self.assertEqual(len(self.gestor.libros), 1)

    def test_isbn_duplicado(self):
        l = Libro("Libro A", "Autor", "123", 2020)
        self.gestor.agregar_libro(l)
        with self.assertRaises(ValueError):
            self.gestor.agregar_libro(l)

    def test_buscar_por_autor(self):
        self.gestor.agregar_libro(Libro("X", "Carlos Ruiz", "1", 2000))
        self.assertTrue(self.gestor.buscar_por_autor("Carlos"))

if __name__ == "__main__":
    unittest.main()
```

---

## ✅ ¿Qué se ha puesto en práctica?

* Modularización de código.
* Manejo de excepciones y validación.
* Trabajo con estructuras avanzadas (`Counter`).
* Almacenamiento persistente (JSON).
* Decoradores y `__main__`.
* Tests unitarios funcionales.

---


