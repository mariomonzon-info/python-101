Perfecto. Comenzaré con el diseño del plan de estudios para el **nivel básico**, estructurado para enseñar desde cero los fundamentos del lenguaje Python. Incluirá explicaciones paso a paso con ejemplos prácticos, ejercicios con soluciones, y buenas prácticas fundamentales.

Te avisaré en cuanto esté listo para tu revisión.


# Curso de Python: Nivel Básico

## Plan de Estudios (Nivel Básico)

* **Introducción a Python y al entorno de desarrollo:** ¿Qué es Python? Instalación, ejecución de scripts, y primer programa *Hola Mundo*.
* **Variables y tipos de datos básicos:** Números (`int`, `float`), cadenas de texto, booleanos; operadores básicos y conversión de tipos.
* **Estructuras de control:** Condicionales (`if`, `elif`, `else`) y bucles (`for`, `while`); ejemplos y usos comunes.
* **Estructuras de datos fundamentales:** Listas, tuplas, diccionarios y conjuntos; operaciones básicas (indexación, recorridos, métodos comunes).
* **Funciones:** Definición y uso de funciones, parámetros y valores de retorno; alcance de variables (*scope*).
* **Módulos y paquetes:** Cómo organizar el código en archivos y carpetas, uso de `import` para reutilizar código estándar y propio.
* **Entrada/Salida de archivos:** Lectura y escritura de archivos de texto usando `open`, y manejo básico de errores con `try`/`except`.
* **Introducción a la Programación Orientada a Objetos:** Clases y objetos simples, atributos y métodos básicos, constructor `__init__`.
* **Proyecto práctico simple:** Un pequeño programa real (por ejemplo, una lista de tareas o un juego de adivinanza) que combine los conceptos anteriores.

## 1. Introducción a Python

Python es un lenguaje de programación **interpretado**, **orientado a objetos** y de **alto nivel**, con *tipado dinámico*. Su sintaxis simple y legible facilita el aprendizaje y enfatiza la legibilidad del código. Python incluye estructuras de datos integradas y fomenta la modularidad mediante módulos y paquetes. Para empezar, instala Python (desde [python.org](https://www.python.org)) y un editor de código (como VS Code o PyCharm).

Como primer ejemplo, crea un archivo `hola_mundo.py` con el siguiente código y ejecútalo con `python hola_mundo.py` en la terminal:

```python
print("¡Hola, mundo!")
```

Esto mostrará en pantalla:

```
¡Hola, mundo!
```

Este programa demuestra la función `print`, que envía texto a la salida estándar. Es el equivalente a "Hello World" en Python.

## 2. Variables y Tipos de Datos

En Python, **las variables no requieren declaración de tipo** previa: se crean al asignarles un valor. Python es de *tipado dinámico*, así que la misma variable puede cambiar de tipo en tiempo de ejecución. Por ejemplo:

```python
contador = 10       # Entero (int)
mensaje = "¡Hola!"  # Cadena de texto (str)
pi = 3.1416         # Número de punto flotante (float)
activo = True       # Booleano (bool)
```

Las operaciones aritméticas básicas funcionan directamente: `+`, `-`, `*`, `/` y operador módulo `%`. Al mezclar `int` y `float`, el resultado es `float`. Por ejemplo:

```python
resultado = contador + pi
print(resultado)  # Imprime 13.1416
```

También existen otras estructuras de datos, que cubriremos más adelante (listas, diccionarios, etc.).

### Ejemplo paso a paso

1. Creamos variables y mostramos sus valores:

   ```python
   a = 5
   b = 2.5
   print(a, b)       # Salida: 5 2.5
   ```
2. Hacemos operaciones:

   ```python
   suma = a + b
   producto = a * b
   print("Suma:", suma)
   print("Producto:", producto)
   # Salida:
   # Suma: 7.5
   # Producto: 12.5
   ```
3. Comprobamos tipos con `type()`:

   ```python
   print(type(a), type(b), type(suma))
   # Salida:
   # <class 'int'> <class 'float'> <class 'float'>
   ```

   Observamos que `suma` es `float` porque uno de los operandos era `float`.

## 3. Estructuras de Control

Las estructuras de control dirigen el flujo del programa.

* **Condicionales (`if`):** Permiten ejecutar código sólo si se cumple una condición. Ejemplo:

  ```python
  edad = 20
  if edad >= 18:
      print("Eres adulto")
  else:
      print("Eres menor de edad")
  ```

  Si la condición `edad >= 18` es verdadera, imprime "Eres adulto". Nota la indentación de 4 espacios (convención de PEP8).

* **Bucles:**

  * `for`: recorre elementos de una secuencia (lista, rango, cadena, etc.). Ejemplo:

    ```python
    for i in range(1, 6):
        print(i, end=" ")
    # Salida: 1 2 3 4 5
    ```
  * `while`: repite mientras una condición sea verdadera. Ejemplo:

    ```python
    cuenta = 1
    while cuenta <= 5:
        print(cuenta, end=" ")
        cuenta += 1
    # Salida: 1 2 3 4 5
    ```

### Ejemplo paso a paso

Construimos un pequeño programa que recorre los números del 1 al 5 y muestra si son pares o impares:

```python
for i in range(1, 6):
    if i % 2 == 0:
        print(i, "es par")
    else:
        print(i, "es impar")
```

Salida:

```
1 es impar
2 es par
3 es impar
4 es par
5 es impar
```

Explicación: Usamos `% 2` para obtener el resto de dividir por 2 y determinamos paridad.

## 4. Estructuras de Datos Fundamentales

Python incluye varias estructuras de datos integradas:

* **Listas (`list`):** Colección ordenada y mutable. Se crean con corchetes `[]`.
  Ejemplo:

  ```python
  frutas = ["manzana", "plátano", "cereza"]
  print(frutas[0])      # 'manzana'
  frutas.append("uva")  # Agrega elemento
  ```
* **Tuplas (`tuple`):** Colección ordenada e inmutable. Se crean con paréntesis `()`.

  ```python
  coordenada = (10.0, 20.0)
  ```
* **Diccionarios (`dict`):** Colección de pares clave-valor. Llaves `{}`:

  ```python
  info = {"nombre": "Ana", "edad": 30}
  print(info["nombre"])  # 'Ana'
  info["edad"] = 31      # Actualiza valor
  ```
* **Conjuntos (`set`):** Colección desordenada de elementos únicos.

  ```python
  primos = {2, 3, 5, 7}
  primos.add(11)
  ```

### Ejemplo paso a paso

Trabajemos con listas y diccionarios:

```python
# Lista de compras
compras = ["leche", "pan", "huevos"]
compras.append("café")
for item in compras:
    print("-", item)
# Diccionario de usuario
usuario = {"usuario": "juan", "nivel": 1}
usuario["nivel"] += 1
print("Usuario:", usuario["usuario"], "- Nivel:", usuario["nivel"])
```

Salida:

```
- leche
- pan
- huevos
- café
Usuario: juan - Nivel: 2
```

Explicación: Agregamos elementos a la lista y recorremos con `for`. En el diccionario, accedemos y actualizamos un valor por su clave.

## 5. Funciones

Las funciones agrupan código reutilizable. Se definen con `def nombre(parámetros):`.
Ejemplo:

```python
def saludar(nombre):
    # Imprime un saludo con el nombre dado.
    print("¡Hola,", nombre + "!")
    
saludar("Ana")  # Llama a la función
# Salida: ¡Hola, Ana!
```

* Los parámetros son variables locales a la función.
* Se puede devolver un valor con `return`:

  ```python
  def sumar(a, b):
      return a + b

  resultado = sumar(3, 4) 
  print(resultado)  # 7
  ```
* Usar `return` sin valor es equivalente a `None`.

**Ámbito de variables (*scope*):** Las variables definidas dentro de una función no afectan a las variables fuera de ella. Por ejemplo:

```python
x = 10

def cambiar_valor():
    x = 5
    print("Dentro:", x)

cambiar_valor()   # Dentro: 5
print("Fuera:", x)  # Fuera: 10
```

Aquí, la `x` global no cambia.

## 6. Módulos y Paquetes

A medida que el código crece, conviene organizarlo en módulos (archivos `.py`). Para usar código de otros archivos o librerías, usamos `import`. Por ejemplo, el módulo estándar `math`:

```python
import math
print(math.sqrt(16))  # 4.0
```

También se pueden crear módulos propios:

```python
# archivo saludos.py
def saludar(nombre):
    print("¡Hola,", nombre + "!")

# en otro archivo principal
from saludos import saludar
saludar("Luis")  # ¡Hola, Luis!
```

La guía oficial de estilo (PEP 8) sugiere **nombres de funciones y variables en minúsculas** separados por guiones bajos, y **nombres de clases en CapWords**. Además, es buena práctica usar **docstrings** (comentarios de documentación) para describir la funcionalidad.

## 7. Entrada/Salida de Archivos

Para manejar archivos de texto, usamos `open`. Ejemplo de escritura:

```python
with open("datos.txt", "w") as f:
    f.write("Línea 1\n")
    f.write("Línea 2\n")
```

El uso de `with` asegura que el archivo se cierre automáticamente (manejo de contexto). Para leer:

```python
with open("datos.txt", "r") as f:
    contenido = f.read()
    print(contenido)
```

Si ocurre un error (archivo no existe, permisos, etc.), capturamos excepciones:

```python
try:
    with open("no_existe.txt") as f:
        datos = f.read()
except FileNotFoundError:
    print("Error: archivo no encontrado.")
```

## 8. Buenas Prácticas y Errores Comunes (Nivel Básico)

* **Sigue PEP 8:** Usa indentación de 4 espacios, nombres descriptivos y estilo consistente. Por ejemplo, funciones y variables en minúsculas con guiones bajos. Escribir comentarios claros es crucial: *“comentarios contradictorios son peores que no tener comentarios”* y siempre deben estar actualizados.
* **Documenta tu código:** Incluye docstrings en funciones, clases y módulos para explicar su propósito.
* **Evita errores comunes:** Olvidar los dos puntos (`:`) al final de `if`, `for`, etc., o la indentación incorrecta (mezclar tabuladores y espacios) provocan `IndentationError`. Confundir `=` (asignación) con `==` (comparación) en condicionales. También, al leer `input`, recuerda convertir tipos con `int()`, `float()`, etc., ya que devuelve cadena.
* **Validación de entradas:** Siempre verifica que los datos de entrada sean válidos antes de procesarlos para evitar errores en tiempo de ejecución.

## 9. Ejercicios Prácticos (Nivel Básico)

**Ejercicio 1:** Escribe una función `es_par(numero)` que reciba un entero y devuelva `True` si es par o `False` si es impar. Prueba la función con varios valores.

* *Solución:*

  ```python
  def es_par(numero):
      return numero % 2 == 0

  # Pruebas
  print(es_par(4))  # True
  print(es_par(7))  # False
  ```

**Ejercicio 2:** Dada una lista de números, escribe un programa que calcule la suma de todos sus elementos usando un bucle `for`.

* *Solución:*

  ```python
  numeros = [3, 5, 2, 8, 1]
  total = 0
  for n in numeros:
      total += n
  print("Suma:", total)  # Suma: 19
  ```

**Ejercicio 3:** Crea un diccionario con al menos 3 pares clave-valor (por ejemplo, nombres de personas y sus edades). Luego, agrega un nuevo par y muestra por pantalla todos los pares del diccionario.

* *Solución:*

  ```python
  personas = {"Ana": 25, "Luis": 30, "Marta": 22}
  personas["Carlos"] = 28
  for nombre, edad in personas.items():
      print(nombre, "tiene", edad, "años.")
  ```

## 10. Patrones de Diseño Comunes (Nivel Básico)

Aunque los patrones de diseño suelen estudiarse a niveles intermedio/avanzado, es útil conocer algunos básicos:

* **Singleton:** Asegura que una clase tenga una sola instancia global. En Python se puede lograr usando variables de módulo o modificando el comportamiento de `__new__`. Por ejemplo, el patrón Singleton garantiza que exista un único objeto de la clase.
* **Factory Method:** Patrón creacional que delega la creación de objetos a métodos específicos, evitando acoplar el código al constructor concreto de la clase. En Python, a menudo se implementa con funciones que devuelven diferentes tipos de objetos según el contexto.
* **Strategy (Estrategia):** Patrón de comportamiento que encapsula algoritmos en clases intercambiables. Permite cambiar el comportamiento de un objeto en tiempo de ejecución. Por ejemplo, usar diferentes funciones de ordenamiento según la situación.

Estos patrones, junto con las buenas prácticas mencionadas y el desarrollo activo a través de proyectos, refuerzan el aprendizaje y preparan al estudiante para contextos profesionales.

**Referencias:** Materiales educativos y documentación oficial (Python.org, PEP 8) han sido consultados para la elaboración de este plan de estudios, así como estudios sobre metodologías activas en la enseñanza de la programación.


¡Por supuesto! A continuación te presento una **versión extendida** del apartado **"Introducción a Python"** con más profundidad, ejemplos y contexto para autodidactas y principiantes.

---

## 📌 1. Introducción a Python (extendido)

### ¿Qué es Python?

**Python** es un lenguaje de programación de propósito general, interpretado, interactivo, de alto nivel y multiplataforma. Fue creado por **Guido van Rossum** y lanzado por primera vez en 1991.

Sus principales características lo hacen especialmente adecuado para principiantes:

* ✅ Sintaxis sencilla y legible.
* ✅ Tipado dinámico (no necesitas declarar el tipo de variable).
* ✅ Gran comunidad y abundantes recursos de aprendizaje.
* ✅ Enorme ecosistema de librerías para áreas como desarrollo web, ciencia de datos, inteligencia artificial, automatización, videojuegos, etc.
* ✅ Portabilidad: funciona en Windows, macOS y Linux.

### ¿Dónde se usa Python?

Python es ampliamente utilizado por empresas tecnológicas como Google, Netflix, Meta, Spotify, Dropbox, entre muchas otras. Algunos campos donde se destaca:

| Campo                      | Aplicaciones comunes                   |
| -------------------------- | -------------------------------------- |
| Desarrollo Web             | Django, Flask, FastAPI                 |
| Ciencia de Datos           | Pandas, NumPy, Matplotlib, Jupyter     |
| Inteligencia Artificial    | TensorFlow, PyTorch, Scikit-learn      |
| Automatización / Scripting | Scripts de sistema, bots, web scraping |
| Big Data                   | PySpark, Dask, Apache Beam             |
| Videojuegos                | Pygame                                 |

### 🧰 ¿Qué necesitas para empezar?

#### 1. Instalar Python

Visita [https://www.python.org/downloads](https://www.python.org/downloads) y descarga la versión recomendada para tu sistema operativo.

✅ Asegúrate de marcar la opción **"Add Python to PATH"** durante la instalación (en Windows).

Puedes comprobar que está instalado correctamente ejecutando en terminal o consola:

```bash
python --version
# o en algunos sistemas:
python3 --version
```

#### 2. Elegir un editor de código

Algunas opciones recomendadas:

* **Visual Studio Code (VS Code)** 🥇 *(liviano y con muchas extensiones útiles)*
* PyCharm (versión Community gratuita)
* Thonny (ideal para principiantes)
* Jupyter Notebook (especialmente útil en ciencia de datos)

### 🧪 Tu primer programa en Python

Abre tu editor, crea un archivo llamado `hola_mundo.py`, y escribe:

```python
print("¡Hola, mundo!")
```

Guarda y ejecuta desde la terminal con:

```bash
python hola_mundo.py
```

📌 **Salida esperada:**

```
¡Hola, mundo!
```

Esto hace uso de la función integrada `print()` para mostrar texto en la consola.

### ⚙️ ¿Cómo funciona un programa en Python?

Python no necesita compilarse antes de ejecutarse. Es **interpretado**, lo que significa que el intérprete lee y ejecuta el código línea por línea.

📄 Ejemplo:

```python
nombre = "Mario"
print("Hola,", nombre)
```

🔎 ¿Qué ocurre aquí?

1. Python asigna la cadena `"Mario"` a la variable `nombre`.
2. La función `print()` muestra el saludo, concatenando texto con el contenido de la variable.

**Resultado:**

```
Hola, Mario
```

---

### 🤓 Interacción con el usuario

Puedes pedir datos al usuario con `input()`:

```python
nombre = input("¿Cómo te llamas? ")
print("Encantado de conocerte,", nombre)
```

🟡 **Nota importante:**
La función `input()` siempre devuelve un **string**. Si necesitas trabajar con números, conviértelos con `int()` o `float()`:

```python
edad = int(input("¿Cuántos años tienes? "))
print("El próximo año tendrás", edad + 1)
```

---

### ⚠️ Errores comunes al comenzar

| Error              | Causa y solución                                    |
| ------------------ | --------------------------------------------------- |
| `SyntaxError`      | Error de sintaxis, como olvidar dos puntos `:`      |
| `IndentationError` | Mala indentación (mezcla de espacios y tabuladores) |
| `NameError`        | Usar una variable antes de declararla               |
| `TypeError`        | Operaciones inválidas entre tipos                   |
| `ValueError`       | Conversión inválida (por ejemplo, `int("hola")`)    |

👉 Consejo: **lee el mensaje de error completo y busca la línea que lo provoca.** Python suele darte pistas claras.

---

### 🔄 Ejecutar código en modo interactivo

Python también puede ejecutarse en **modo interactivo**, como una calculadora. Escribe `python` en la terminal y verás el prompt:

```python
>>> 2 + 3
5
>>> "Hola".upper()
'HOLA'
```

Puedes usar este modo para probar cosas rápidas, funciones o expresiones.

---

### 🧱 Estructura básica de un script en Python

Aunque un archivo puede contener solo líneas sueltas, es recomendable seguir una estructura clara:

```python
# Comentario: este es un saludo personalizado

def saludar(nombre):
    """Función que imprime un saludo."""
    print(f"Hola, {nombre}")

if __name__ == "__main__":
    # Punto de entrada del programa
    saludar("Mario")
```

🔹 `if __name__ == "__main__":` permite que el código solo se ejecute si el archivo se ejecuta directamente, y no cuando es importado como módulo. Esta práctica es muy habitual en proyectos reales.

---

### ✅ Buenas prácticas iniciales

* Usa nombres descriptivos: `nombre_cliente`, `total_factura`.
* Comenta solo lo necesario, evita obviedades.
* Indenta con 4 espacios (no uses tabuladores).
* Guarda tus archivos con extensión `.py`.
* Prueba, rompe, juega. ¡Aprende haciendo!

---

Perfecto, continuamos con el **Módulo 2: Variables y Tipos de Datos** dentro del nivel básico de Python. Este módulo se centra en cómo declarar, usar y manipular variables en Python, junto con sus tipos de datos fundamentales.

---

# 🧠 Módulo 2: Variables y Tipos de Datos en Python

---

## 🎯 Objetivos del módulo

* Comprender qué es una variable y cómo se declara en Python.
* Identificar y usar correctamente los tipos de datos básicos.
* Aprender a realizar operaciones con datos numéricos y cadenas.
* Aplicar conversión de tipos (casting) entre diferentes tipos de datos.
* Introducir buenas prácticas al nombrar y usar variables.

---

## 🧩 ¿Qué es una variable?

Una **variable** es un contenedor que almacena datos. En Python, no necesitas declarar su tipo: el intérprete lo infiere automáticamente al asignar el valor.

📌 **Sintaxis básica:**

```python
nombre_variable = valor
```

🔎 Ejemplos:

```python
edad = 25              # entero
nombre = "Lucía"       # cadena (str)
altura = 1.68          # flotante
es_estudiante = True   # booleano
```

📌 Las variables pueden cambiar de tipo dinámicamente:

```python
x = 10        # int
x = "texto"   # ahora es str
```

---

## 🧪 Tipos de datos fundamentales

### 🔢 Números (`int`, `float`)

* `int`: números enteros (positivos o negativos)
* `float`: números decimales (con punto)

```python
a = 10          # int
b = 3.14        # float
```

📘 **Operaciones básicas:**

| Operador | Significado      | Ejemplo  | Resultado |
| -------- | ---------------- | -------- | --------- |
| `+`      | Suma             | `2 + 3`  | `5`       |
| `-`      | Resta            | `5 - 2`  | `3`       |
| `*`      | Multiplicación   | `3 * 4`  | `12`      |
| `/`      | División (float) | `7 / 2`  | `3.5`     |
| `//`     | División entera  | `7 // 2` | `3`       |
| `%`      | Módulo (resto)   | `7 % 2`  | `1`       |
| `**`     | Potencia         | `2 ** 3` | `8`       |

### 🧵 Cadenas de texto (`str`)

Una **cadena** es una secuencia de caracteres entre comillas (`" "` o `' '`).

```python
mensaje = "Hola, mundo"
```

🔧 Operaciones comunes:

```python
print(mensaje.upper())     # "HOLA, MUNDO"
print(mensaje.lower())     # "hola, mundo"
print(len(mensaje))        # 11 (número de caracteres)
print(mensaje[0])          # "H" (indexación)
```

📌 Las cadenas son **inmutables**: no puedes modificar directamente un carácter.

---

### 🔁 Conversión de tipos (Casting)

```python
edad = "25"
edad = int(edad)   # convierte de str a int

pi = 3.1416
texto = str(pi)    # convierte float a str
```

⚠️ Si intentas convertir texto no numérico a `int`, obtendrás un error:

```python
int("hola")  # ValueError
```

---

### 🔘 Booleanos (`bool`)

Los booleanos representan **valores de verdad**:

```python
es_mayor = True
esta_activo = False
```

Se usan normalmente en condiciones (`if`, `while`, etc.).

📌 Cualquier valor puede evaluarse como verdadero o falso:

| Valor         | Evaluado como |
| ------------- | ------------- |
| `0`, `0.0`    | `False`       |
| `""`, `[]`    | `False`       |
| `None`        | `False`       |
| Todo lo demás | `True`        |

---

## 🛠️ Ejemplos prácticos

### Ejemplo 1: Calcular el área de un triángulo

```python
base = float(input("Introduce la base: "))
altura = float(input("Introduce la altura: "))
area = (base * altura) / 2
print("Área del triángulo:", area)
```

---

### Ejemplo 2: Concatenar texto

```python
nombre = input("Tu nombre: ")
saludo = "Hola, " + nombre + "!"
print(saludo)
```

---

### Ejemplo 3: Comprobar tipo de variable

```python
a = 7
b = 2.5
c = "Python"
print(type(a))   # <class 'int'>
print(type(b))   # <class 'float'>
print(type(c))   # <class 'str'>
```

---

## ✅ Buenas prácticas

* Usa nombres descriptivos: `nombre_usuario`, `total_factura`.
* Evita nombres genéricos como `x`, `a1`, `dato` (salvo en bucles o ejemplos).
* No uses caracteres especiales ni espacios en los nombres.
* Python es **sensible a mayúsculas/minúsculas**: `Edad` y `edad` son diferentes.

---

## ⚠️ Errores comunes

| Error                       | Descripción                                  |
| --------------------------- | -------------------------------------------- |
| `NameError`                 | Variable no definida                         |
| `TypeError`                 | Operaciones incompatibles (`"texto" + 5`)    |
| `ValueError`                | Conversión inválida (`int("Hola")`)          |
| Confundir `=` con `==`      | `=` asigna; `==` compara                     |
| Uso de comillas incorrectas | Mezclar comillas simples y dobles sin cerrar |

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Solicita al usuario su **nombre** y **edad**, y muestra un mensaje que diga:

> "Hola, \[nombre], el próximo año tendrás \[edad+1] años."

✅ **Pista:** Usa `input()`, `int()`, `str()`, `print()`.

---

### 📝 Ejercicio 2

Declara dos variables numéricas y muestra su suma, resta, producto y división en líneas separadas.

---

### 📝 Ejercicio 3

Dado el número de segundos ingresado por el usuario, convierte a horas, minutos y segundos. Muestra el resultado como:

> "Equivale a X horas, Y minutos y Z segundos."

---

Genial, seguimos avanzando. A continuación te presento el:

---

# 🧠 Módulo 3: Estructuras de Control en Python

---

## 🎯 Objetivos del módulo

* Comprender cómo controlar el flujo de ejecución de un programa con condicionales y bucles.
* Saber cuándo y cómo usar `if`, `elif`, `else`, `for` y `while`.
* Aplicar lógica básica para resolver problemas mediante estructuras de control.
* Introducir el uso de `break`, `continue` y `pass`.

---

## ⚙️ Condicionales (`if`, `elif`, `else`)

Las condicionales permiten que un bloque de código se ejecute **solo si** se cumple una condición.

📌 **Sintaxis básica:**

```python
if condición:
    # Bloque si la condición es verdadera
elif otra_condición:
    # Otro bloque si la primera falla
else:
    # Bloque si ninguna condición anterior se cumple
```

🔍 **Ejemplo práctico:**

```python
edad = int(input("Introduce tu edad: "))

if edad >= 18:
    print("Eres mayor de edad.")
elif edad >= 13:
    print("Eres adolescente.")
else:
    print("Eres un niño.")
```

📘 **Notas importantes:**

* La indentación (sangría) es **obligatoria** y normalmente es de **4 espacios**.
* Las condiciones se evalúan en orden. En cuanto se cumple una, las demás se ignoran.

---

### 🔁 Operadores de comparación

| Operador | Significado   | Ejemplo (`a = 5`, `b = 3`) |
| -------- | ------------- | -------------------------- |
| `==`     | Igualdad      | `a == b` → `False`         |
| `!=`     | Distinto      | `a != b` → `True`          |
| `>`      | Mayor que     | `a > b` → `True`           |
| `<`      | Menor que     | `a < b` → `False`          |
| `>=`     | Mayor o igual | `a >= 5` → `True`          |
| `<=`     | Menor o igual | `b <= 3` → `True`          |

---

## 🔃 Bucles (`for` y `while`)

Los bucles permiten repetir bloques de código mientras se cumpla una condición.

---

### 🔁 Bucle `for`

Recorre secuencias como listas, cadenas o rangos numéricos.

📌 **Sintaxis:**

```python
for variable in secuencia:
    # Bloque que se repite
```

🔍 **Ejemplo:**

```python
for i in range(1, 6):
    print(f"Iteración {i}")
```

📘 `range(inicio, fin)` genera una secuencia de números desde `inicio` hasta `fin-1`.

---

### 🔁 Bucle `while`

Repite el bloque **mientras** la condición sea verdadera.

📌 **Sintaxis:**

```python
while condición:
    # Bloque que se repite
```

🔍 **Ejemplo:**

```python
contador = 1
while contador <= 5:
    print("Cuenta:", contador)
    contador += 1
```

⚠️ Cuidado con los **bucles infinitos**. Asegúrate de que la condición pueda volverse falsa en algún momento.

---

## 🧩 Control de flujo adicional

### 🔸 `break`

Sale del bucle inmediatamente.

```python
for i in range(10):
    if i == 5:
        break
    print(i)
# Imprime: 0 1 2 3 4
```

### 🔸 `continue`

Salta a la siguiente iteración del bucle.

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
# Imprime: 0 1 3 4
```

### 🔸 `pass`

No hace nada. Es útil como marcador temporal.

```python
if True:
    pass  # futuro código aquí
```

---

## 🔄 Operadores lógicos

Se pueden combinar múltiples condiciones usando:

| Operador | Descripción | Ejemplo            |
| -------- | ----------- | ------------------ |
| `and`    | Y lógico    | `a > 0 and a < 10` |
| `or`     | O lógico    | `a < 0 or a > 100` |
| `not`    | Negación    | `not es_activo`    |

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Clasificación de notas

```python
nota = float(input("Introduce tu nota (0-10): "))

if nota >= 9:
    print("Sobresaliente")
elif nota >= 7:
    print("Notable")
elif nota >= 5:
    print("Aprobado")
else:
    print("Suspenso")
```

---

### Ejemplo 2: Mostrar números pares

```python
for i in range(1, 11):
    if i % 2 == 0:
        print(i)
```

---

### Ejemplo 3: Validar contraseña

```python
contraseña = "secreto"
intentos = 3

while intentos > 0:
    entrada = input("Introduce la contraseña: ")
    if entrada == contraseña:
        print("¡Acceso concedido!")
        break
    else:
        print("Incorrecto.")
        intentos -= 1
```

---

## ✅ Buenas prácticas

* Usa `for` cuando conozcas el número de iteraciones, y `while` cuando dependas de una condición.
* Asegúrate de **actualizar variables** dentro de bucles `while`.
* Evita `break` innecesarios: puede dificultar la lectura.
* Usa comentarios para indicar qué hace un bucle si no es obvio.

---

## ⚠️ Errores comunes

| Error                           | Causa                                  |
| ------------------------------- | -------------------------------------- |
| Bucle infinito                  | Condición de `while` siempre verdadera |
| Comparar con `=` en vez de `==` | Error lógico común                     |
| Indentación incorrecta          | Bloques `if` o `for` mal indentados    |
| `range()` mal definido          | Puede no ejecutar nada si está vacío   |

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Pide un número y muestra si es positivo, negativo o cero.

---

### 📝 Ejercicio 2

Muestra todos los múltiplos de 3 entre 1 y 100 usando un bucle `for`.

---

### 📝 Ejercicio 3

Pide al usuario una contraseña y permite 3 intentos para acertar. Si acierta, muestra "Acceso permitido", si no, "Acceso denegado".

---

Perfecto, seguimos con el siguiente módulo.

---

# 🧠 Módulo 4: Estructuras de Datos Fundamentales en Python

---

## 🎯 Objetivos del módulo

* Conocer y utilizar correctamente las estructuras de datos integradas en Python: **listas**, **tuplas**, **diccionarios** y **conjuntos**.
* Comprender cuándo utilizar cada estructura según el contexto.
* Aplicar métodos y operaciones útiles para manipular colecciones.
* Reforzar el pensamiento lógico al recorrer y transformar datos.

---

## 🧺 1. Listas (`list`)

Una **lista** es una colección ordenada, mutable (editable) y que permite elementos duplicados.

📌 **Definición:**

```python
frutas = ["manzana", "plátano", "cereza"]
```

📘 Las listas pueden contener cualquier tipo de dato (incluso mezclados):

```python
mezcla = [1, "hola", True, 3.14]
```

---

### 🔧 Operaciones comunes con listas

| Operación                       | Código                     | Resultado                              |
| ------------------------------- | -------------------------- | -------------------------------------- |
| Acceder por índice              | `frutas[0]`                | `"manzana"`                            |
| Modificar un elemento           | `frutas[1] = "pera"`       | Cambia `"plátano"` → `"pera"`          |
| Agregar al final                | `frutas.append("uva")`     | `["manzana", "pera", "cereza", "uva"]` |
| Insertar en posición específica | `frutas.insert(1, "kiwi")` | Inserta `"kiwi"` en la posición 1      |
| Eliminar por valor              | `frutas.remove("pera")`    | Elimina la primera aparición           |
| Eliminar por índice             | `del frutas[0]`            | Elimina el primer elemento             |
| Vaciar la lista                 | `frutas.clear()`           | Lista vacía                            |
| Longitud de la lista            | `len(frutas)`              | Número de elementos                    |

---

### 🔁 Recorrer listas

```python
for fruta in frutas:
    print(fruta)
```

📘 También puedes usar índices:

```python
for i in range(len(frutas)):
    print(i, frutas[i])
```

---

## 📦 2. Tuplas (`tuple`)

Una **tupla** es como una lista, pero **inmutable** (no se puede modificar). Se usa para agrupar datos que no deben cambiar.

📌 **Definición:**

```python
coordenada = (40.7128, -74.0060)
```

📘 También se puede crear sin paréntesis:

```python
tupla = "rojo", "verde", "azul"
```

---

### 🔧 Operaciones con tuplas

| Acción             | Código              |
| ------------------ | ------------------- |
| Acceder por índice | `coordenada[0]`     |
| Longitud           | `len(coordenada)`   |
| Empaquetar tupla   | `a, b = coordenada` |
| Convertir a lista  | `list(coordenada)`  |

🔒 **No puedes** hacer: `coordenada[0] = 100` → Esto lanza un `TypeError`.

---

## 🧠 3. Diccionarios (`dict`)

Un **diccionario** es una colección de pares clave–valor, sin orden garantizado (hasta Python 3.7), y mutable.

📌 **Definición:**

```python
persona = {
    "nombre": "Ana",
    "edad": 30,
    "ciudad": "Madrid"
}
```

---

### 🔧 Operaciones con diccionarios

| Acción            | Código                               | Resultado o efecto                  |
| ----------------- | ------------------------------------ | ----------------------------------- |
| Acceder por clave | `persona["nombre"]`                  | `"Ana"`                             |
| Modificar valor   | `persona["edad"] = 31`               | Cambia el valor asociado a `"edad"` |
| Agregar nuevo par | `persona["profesión"] = "Ingeniera"` | Añade nuevo par                     |
| Eliminar par      | `del persona["ciudad"]`              | Elimina clave `"ciudad"`            |
| Comprobar clave   | `"nombre" in persona`                | `True`                              |
| Obtener claves    | `persona.keys()`                     | `dict_keys([...])`                  |
| Obtener valores   | `persona.values()`                   | `dict_values([...])`                |
| Obtener pares     | `persona.items()`                    | `dict_items([...])`                 |

---

### 🔁 Recorrer diccionarios

```python
for clave, valor in persona.items():
    print(clave, ":", valor)
```

---

## 🔳 4. Conjuntos (`set`)

Un **conjunto** es una colección no ordenada, **sin elementos duplicados**. Ideal para operaciones de teoría de conjuntos.

📌 **Definición:**

```python
colores = {"rojo", "verde", "azul"}
```

🔍 **Automáticamente elimina duplicados:**

```python
set_ejemplo = {1, 2, 2, 3}
print(set_ejemplo)  # {1, 2, 3}
```

---

### 🔧 Operaciones con conjuntos

| Acción            | Código                    | Resultado                        |                        |
| ----------------- | ------------------------- | -------------------------------- | ---------------------- |
| Agregar elemento  | `colores.add("amarillo")` | Añade al conjunto                |                        |
| Eliminar elemento | `colores.remove("verde")` | Elimina si existe                |                        |
| Unión             | \`set1                    | set2\`                           | Combina sin duplicados |
| Intersección      | `set1 & set2`             | Elementos comunes                |                        |
| Diferencia        | `set1 - set2`             | Elementos en `set1` no en `set2` |                        |

---

## ⚠️ Diferencias clave entre estructuras

| Estructura  | Ordenada | Mutable | Claves | Duplicados | Acceso rápido |
| ----------- | -------- | ------- | ------ | ---------- | ------------- |
| Lista       | ✅        | ✅       | ❌      | ✅          | ✅             |
| Tupla       | ✅        | ❌       | ❌      | ✅          | ✅             |
| Diccionario | ✅ (3.7+) | ✅       | ✅      | ❌ (claves) | ✅             |
| Conjunto    | ❌        | ✅       | ❌      | ❌          | ❌             |

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Lista de tareas

```python
tareas = []
tareas.append("Estudiar Python")
tareas.append("Hacer ejercicio")

for tarea in tareas:
    print("-", tarea)
```

---

### Ejemplo 2: Diccionario de usuario

```python
usuario = {
    "nombre": "Lucía",
    "email": "lucia@example.com"
}
usuario["nivel"] = "Intermedio"

for k, v in usuario.items():
    print(f"{k}: {v}")
```

---

### Ejemplo 3: Conjuntos para eliminar duplicados

```python
numeros = [1, 2, 2, 3, 4, 4, 5]
sin_repetidos = set(numeros)
print(sin_repetidos)
```

---

## ✅ Buenas prácticas

* Usa listas cuando el orden importe y puedas necesitar modificar los elementos.
* Usa tuplas para valores constantes o coordenadas.
* Usa diccionarios cuando necesites asociar claves con valores.
* Usa conjuntos cuando necesites eliminar duplicados o hacer operaciones entre grupos.

---

## ⚠️ Errores comunes

| Error                  | Causa                                            |
| ---------------------- | ------------------------------------------------ |
| `IndexError`           | Acceder a índice fuera de rango en listas/tuplas |
| `KeyError`             | Usar clave inexistente en diccionario            |
| `TypeError`            | Intentar modificar una tupla                     |
| Mutabilidad accidental | Cambiar lista dentro de otra sin querer          |

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Crea una lista con los nombres de 5 frutas. Recorre la lista e imprime solo las frutas con más de 5 letras.

---

### 📝 Ejercicio 2

Crea un diccionario que represente un libro con título, autor y año. Muestra sus valores formateados.

---

### 📝 Ejercicio 3

Crea dos conjuntos con varios números. Muestra su unión, intersección y diferencia.

---

Perfecto, continuamos con uno de los pilares más importantes de la programación:

---

# 🧠 Módulo 5: Funciones en Python

---

## 🎯 Objetivos del módulo

* Comprender qué es una función y por qué es esencial en la programación.
* Aprender a declarar y utilizar funciones en Python.
* Usar parámetros, argumentos, valores por defecto y retorno de resultados.
* Aplicar buenas prácticas para escribir funciones legibles, reutilizables y modulares.
* Introducir funciones lambda y el concepto de ámbito (scope).

---

## 🧩 ¿Qué es una función?

Una **función** es un bloque de código reutilizable que ejecuta una tarea específica. Permite dividir programas en partes más organizadas y comprensibles.

📌 **Ventajas de usar funciones:**

* Reutilización del código
* Modularidad
* Legibilidad
* Mantenimiento más sencillo

---

## 🔧 Declaración de funciones

📘 **Sintaxis básica:**

```python
def nombre_funcion(parámetros):
    # Bloque de código
    return valor
```

📌 **Ejemplo simple:**

```python
def saludar():
    print("Hola, bienvenido a Python")

saludar()
```

---

### 🔁 Funciones con parámetros

```python
def saludar(nombre):
    print(f"Hola, {nombre}!")

saludar("Mario")
```

---

### 🔄 Funciones que retornan valores

```python
def cuadrado(numero):
    return numero ** 2

resultado = cuadrado(5)
print(resultado)  # 25
```

---

### 🔹 Parámetros con valor por defecto

```python
def saludar(nombre="invitado"):
    print(f"Hola, {nombre}")

saludar()            # Hola, invitado
saludar("Lucía")     # Hola, Lucía
```

---

### 🔹 Múltiples parámetros

```python
def sumar(a, b):
    return a + b

print(sumar(3, 4))  # 7
```

---

### 📥 Argumentos por posición y por nombre

```python
def mostrar_datos(nombre, edad):
    print(f"{nombre} tiene {edad} años")

mostrar_datos("Ana", 25)                    # Por posición
mostrar_datos(edad=30, nombre="Lucía")     # Por nombre
```

---

## 🧠 Ámbito de variables (Scope)

* **Local**: dentro de una función.
* **Global**: definida fuera de cualquier función.

```python
x = 10  # global

def funcion():
    x = 5  # local
    print(x)

funcion()     # 5
print(x)      # 10
```

⚠️ Para modificar una variable global dentro de una función, usa `global`, aunque **no se recomienda** salvo que sea estrictamente necesario.

---

## 🧪 Funciones anónimas: `lambda`

Una **función lambda** es una función corta que se define en una sola línea.

```python
cuadrado = lambda x: x ** 2
print(cuadrado(6))  # 36
```

Ideal para funciones simples, como claves de ordenación o filtros:

```python
nombres = ["Ana", "Pedro", "Luis"]
nombres.sort(key=lambda x: len(x))
print(nombres)
```

---

## 📦 Funciones como objetos

Las funciones pueden pasarse como argumentos a otras funciones:

```python
def aplicar(func, valor):
    return func(valor)

def triple(x):
    return x * 3

print(aplicar(triple, 4))  # 12
```

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Conversión de grados

```python
def celsius_a_fahrenheit(c):
    return c * 9/5 + 32

print(celsius_a_fahrenheit(25))  # 77.0
```

---

### Ejemplo 2: Determinar si un número es par

```python
def es_par(n):
    return n % 2 == 0

print(es_par(8))   # True
```

---

### Ejemplo 3: Calcular factorial (versión iterativa)

```python
def factorial(n):
    resultado = 1
    for i in range(1, n+1):
        resultado *= i
    return resultado

print(factorial(5))  # 120
```

---

## ✅ Buenas prácticas

* Da **nombres descriptivos** a tus funciones (`calcular_total`, `es_valido`).
* Evita que una función tenga muchas responsabilidades.
* Si una función es muy larga, considera dividirla.
* Documenta tus funciones con **docstrings**:

```python
def sumar(a, b):
    """Devuelve la suma de dos números."""
    return a + b
```

---

## ⚠️ Errores comunes

| Error                         | Descripción                                 |
| ----------------------------- | ------------------------------------------- |
| Llamar función sin paréntesis | `print` vs `print()`                        |
| `TypeError`                   | Número incorrecto de argumentos             |
| Olvidar `return`              | La función devuelve `None` por defecto      |
| Confundir variables locales   | Modificar una variable sin declararla antes |

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Crea una función que calcule el área de un rectángulo. Usa parámetros por defecto para ancho y alto.

---

### 📝 Ejercicio 2

Escribe una función que reciba un número y devuelva `True` si es primo y `False` si no.

---

### 📝 Ejercicio 3

Crea una función llamada `procesar_lista(lista)` que reciba una lista de números y devuelva otra lista con los números al cuadrado **solo si son pares**.

---

Perfecto. Entramos ahora en un módulo esencial para escribir programas más robustos y profesionales:

---

# 🧠 Módulo 6: Manejo de errores y excepciones en Python

---

## 🎯 Objetivos del módulo

* Comprender qué son las excepciones y cómo se producen.
* Aprender a manejar errores de forma controlada con bloques `try`, `except`, `else` y `finally`.
* Capturar distintos tipos de excepciones específicas.
* Crear y lanzar excepciones personalizadas.
* Aplicar buenas prácticas para depuración y seguridad del código.

---

## ⚠️ ¿Qué es una excepción?

Una **excepción** es un error que ocurre **durante la ejecución** del programa. Si no se maneja, detiene el programa.

📌 Ejemplos comunes:

```python
print(10 / 0)            # ZeroDivisionError
int("texto")             # ValueError
lista = [1, 2, 3]
print(lista[10])         # IndexError
```

---

## 🔧 Manejo de excepciones con `try` y `except`

📘 **Sintaxis básica:**

```python
try:
    # Código que puede lanzar una excepción
except TipoDeError:
    # Código que se ejecuta si ocurre ese error
```

🔍 **Ejemplo:**

```python
try:
    numero = int(input("Introduce un número: "))
    print("El doble es:", numero * 2)
except ValueError:
    print("Debes introducir un número válido.")
```

---

## 🧱 Múltiples except

```python
try:
    x = int(input("Número: "))
    y = 10 / x
except ZeroDivisionError:
    print("No se puede dividir entre cero.")
except ValueError:
    print("Entrada inválida. Introduce un número.")
```

📌 Puedes capturar varios tipos de error en el mismo bloque:

```python
except (ZeroDivisionError, ValueError):
    print("Error en entrada o división.")
```

---

## 🔎 Usar `else` y `finally`

| Bloque    | Función                                                   |
| --------- | --------------------------------------------------------- |
| `else`    | Se ejecuta si **no ocurre ninguna excepción** en el `try` |
| `finally` | Se ejecuta **siempre**, ocurra o no una excepción         |

📘 **Ejemplo:**

```python
try:
    n = int(input("Dame un número positivo: "))
    assert n > 0
except ValueError:
    print("Eso no es un número.")
except AssertionError:
    print("Debe ser un número mayor que cero.")
else:
    print("Todo correcto.")
finally:
    print("Programa finalizado.")
```

---

## 🧨 Lanzar tus propias excepciones con `raise`

Puedes forzar una excepción si se cumple cierta condición:

```python
edad = int(input("Edad: "))
if edad < 0:
    raise ValueError("La edad no puede ser negativa.")
```

---

## 🧪 Crear tus propias excepciones

```python
class EdadInvalida(Exception):
    """Excepción personalizada para edades incorrectas."""
    pass

edad = -5
if edad < 0:
    raise EdadInvalida("Edad no válida: debe ser positiva")
```

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Control de entrada numérica

```python
def pedir_numero():
    try:
        n = int(input("Número entero: "))
        return n
    except ValueError:
        print("No es un número válido.")
        return pedir_numero()
```

---

### Ejemplo 2: Leer archivo con manejo de errores

```python
try:
    with open("datos.txt", "r") as archivo:
        contenido = archivo.read()
        print(contenido)
except FileNotFoundError:
    print("El archivo no existe.")
```

---

### Ejemplo 3: División segura

```python
def dividir(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "Error: división por cero"
```

---

## ✅ Buenas prácticas

* **No atrapes errores genéricos** (evita `except:` sin especificar).
* Usa mensajes claros al usuario o registros (`logging`) para errores.
* No abuses del control de errores para lógicas normales del programa.
* Divide en funciones pequeñas para aislar errores.

---

## ⚠️ Errores comunes

| Error                  | Causa                                    |
| ---------------------- | ---------------------------------------- |
| `except:` sin tipo     | Oculta errores graves o inesperados      |
| No usar `finally`      | Recursos como archivos quedan sin cerrar |
| `raise` mal usado      | Lanza excepción incorrecta o sin mensaje |
| Reintentar sin control | Puede crear bucles infinitos al fallar   |

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Pide un número al usuario. Si no es un entero, vuelve a pedirlo hasta que lo sea. Luego imprime su cuadrado.

---

### 📝 Ejercicio 2

Crea una función `dividir_seguro(a, b)` que devuelva el resultado de dividir `a` entre `b` o `"Error"` si `b == 0`.

---

### 📝 Ejercicio 3

Simula la apertura de un archivo `usuarios.txt`. Si no existe, crea uno vacío.

---

Perfecto, seguimos con un módulo clave para estructurar proyectos Python de forma profesional:

---

# 🧠 Módulo 7: Módulos y Paquetes en Python

---

## 🎯 Objetivos del módulo

* Comprender qué son los **módulos** y cómo ayudan a organizar el código.
* Aprender a importar y reutilizar código desde otros archivos.
* Crear y usar **paquetes** (carpetas de módulos).
* Usar algunos **módulos estándar** comunes de Python.
* Aplicar buenas prácticas para estructurar proyectos modulares.

---

## 📦 ¿Qué es un módulo?

Un **módulo** es simplemente un archivo `.py` que contiene funciones, clases o variables que pueden ser reutilizadas desde otros archivos.

📘 Ejemplo:
Supongamos que tienes un archivo llamado `saludos.py`:

```python
# saludos.py
def saludar(nombre):
    print(f"Hola, {nombre}!")
```

Puedes importarlo desde otro archivo:

```python
import saludos

saludos.saludar("Mario")
```

---

## 🔁 Tipos de importación

| Forma de importar                       | Ejemplo                         |
| --------------------------------------- | ------------------------------- |
| Módulo completo                         | `import math`                   |
| Módulo con alias                        | `import math as m`              |
| Función específica de un módulo         | `from math import sqrt`         |
| Múltiples elementos                     | `from math import sin, cos, pi` |
| Todo el contenido (⚠️ poco recomendado) | `from math import *`            |

---

## 🧩 Crear tus propios módulos

### Estructura de ejemplo:

```
mi_proyecto/
│
├── main.py
└── operaciones.py
```

📘 `operaciones.py`:

```python
def sumar(a, b):
    return a + b
```

📘 `main.py`:

```python
from operaciones import sumar

print(sumar(3, 4))  # 7
```

---

## 📚 Módulos estándar de Python

Python incluye muchos módulos útiles ya incorporados:

| Módulo     | Descripción                          | Ejemplo de uso            |
| ---------- | ------------------------------------ | ------------------------- |
| `math`     | Funciones matemáticas avanzadas      | `math.sqrt(25)`           |
| `random`   | Generación de números aleatorios     | `random.randint(1, 10)`   |
| `datetime` | Fecha y hora                         | `datetime.datetime.now()` |
| `os`       | Interacción con el sistema operativo | `os.getcwd()`             |
| `sys`      | Acceso a parámetros y entorno        | `sys.argv`                |
| `json`     | Lectura y escritura de datos JSON    | `json.dumps(diccionario)` |

---

## 📂 ¿Qué es un paquete?

Un **paquete** es una carpeta que contiene un conjunto de módulos y un archivo especial `__init__.py`.

📘 Estructura:

```
mi_app/
├── __init__.py
├── utilidades.py
└── calculos/
    ├── __init__.py
    └── matematicas.py
```

Puedes importar así:

```python
from calculos.matematicas import sumar
```

---

## 📄 El archivo `__init__.py`

Este archivo puede estar vacío o contener código de inicialización para el paquete.
Es necesario para que Python trate esa carpeta como un paquete (especialmente en versiones antiguas).

---

## 📦 Uso de `__main__` para ejecución directa

Puedes crear módulos que se ejecutan **directamente o importados** sin conflictos:

```python
# archivo.py
def saludar():
    print("¡Hola!")

if __name__ == "__main__":
    saludar()  # Solo se ejecuta si se corre este archivo directamente
```

---

## ✅ Buenas prácticas

* Usa nombres descriptivos para tus módulos (`usuarios.py`, `facturas.py`).
* Agrupa funcionalidades en paquetes lógicos.
* Evita usar `import *` (reduce la claridad).
* Usa alias (`as`) para acortar nombres largos.
* Documenta tus módulos con docstrings.

---

## ⚠️ Errores comunes

| Error                 | Causa                                                   |
| --------------------- | ------------------------------------------------------- |
| `ModuleNotFoundError` | El módulo no existe o no está en el `PYTHONPATH`        |
| Confusión con nombres | Mismo nombre para módulo y variable o función           |
| Importación circular  | Dos módulos se importan mutuamente                      |
| Uso de `import *`     | Oculta el origen de funciones y puede causar conflictos |

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Crear módulo `calculadora.py`

```python
def sumar(a, b):
    return a + b

def restar(a, b):
    return a - b
```

Usarlo desde otro archivo:

```python
from calculadora import sumar

print(sumar(10, 5))  # 15
```

---

### Ejemplo 2: Uso de módulo `math`

```python
import math

print(math.pi)
print(math.sqrt(16))  # 4.0
```

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Crea un archivo llamado `utilidades.py` con una función `es_par(n)` que devuelva `True` si `n` es par.

Desde otro archivo, impórtala y úsala.

---

### 📝 Ejercicio 2

Crea una carpeta llamada `geometria` con dos módulos: `rectangulos.py` y `circulos.py`.

* En `rectangulos.py`, crea una función `area_rectangulo(base, altura)`.
* En `circulos.py`, crea `area_circulo(radio)`.

Crea un archivo principal `main.py` que importe y utilice ambas funciones.

---

Perfecto, pasamos ahora a un tema esencial para trabajar con datos persistentes:

---

# 🧠 Módulo 8: Lectura y escritura de archivos en Python

---

## 🎯 Objetivos del módulo

* Comprender cómo abrir, leer, escribir y cerrar archivos con Python.
* Usar la función `open()` de forma segura con el bloque `with`.
* Manipular archivos de texto (`.txt`) y trabajar con líneas y contenido.
* Aprender a manejar archivos CSV y JSON de forma básica.
* Aplicar buenas prácticas de trabajo con archivos.

---

## 📂 Abrir y cerrar archivos

📘 Función básica:

```python
archivo = open("nombre.txt", "modo")
# trabajar con el archivo...
archivo.close()
```

| Modo  | Significado                |
| ----- | -------------------------- |
| `'r'` | Leer (por defecto)         |
| `'w'` | Escribir (sobrescribe)     |
| `'a'` | Añadir al final            |
| `'x'` | Crear (falla si ya existe) |
| `'b'` | Modo binario (`rb`, `wb`)  |

---

### ✅ Uso recomendado: `with open()`

```python
with open("archivo.txt", "r") as f:
    contenido = f.read()
```

🎯 Ventajas:

* Cierra el archivo automáticamente.
* Más seguro frente a errores.

---

## 📖 Leer archivos de texto

```python
with open("datos.txt", "r") as f:
    texto = f.read()
    print(texto)
```

### Leer línea por línea

```python
with open("datos.txt", "r") as f:
    for linea in f:
        print(linea.strip())  # elimina saltos de línea
```

### `readlines()` — devuelve lista de líneas

```python
with open("datos.txt") as f:
    lineas = f.readlines()
```

---

## 📝 Escribir archivos

```python
with open("nuevo.txt", "w") as f:
    f.write("Hola mundo\n")
    f.write("Segunda línea")
```

📘 El modo `'w'` **sobrescribe** el contenido del archivo.

---

### Añadir contenido sin borrar (`'a'`)

```python
with open("log.txt", "a") as f:
    f.write("Nueva entrada\n")
```

---

## 📊 Leer y escribir archivos CSV

### Leer CSV

```python
import csv

with open("datos.csv", newline='') as archivo:
    lector = csv.reader(archivo)
    for fila in lector:
        print(fila)
```

### Escribir CSV

```python
import csv

with open("datos.csv", "w", newline='') as archivo:
    escritor = csv.writer(archivo)
    escritor.writerow(["Nombre", "Edad"])
    escritor.writerow(["Ana", 30])
```

---

## 🧾 Trabajar con archivos JSON

### Convertir Python → JSON y guardar:

```python
import json

persona = {"nombre": "Luis", "edad": 25}

with open("persona.json", "w") as archivo:
    json.dump(persona, archivo)
```

### Leer JSON → Python:

```python
import json

with open("persona.json") as archivo:
    datos = json.load(archivo)

print(datos["nombre"])
```

---

## ✅ Buenas prácticas

* Usa `with open()` siempre que sea posible.
* Especifica `encoding="utf-8"` si trabajas con acentos o caracteres especiales.
* Controla excepciones al leer/escribir archivos.
* Usa `strip()` para limpiar líneas.

---

## ⚠️ Errores comunes

| Error                   | Causa                                        |
| ----------------------- | -------------------------------------------- |
| `FileNotFoundError`     | El archivo no existe                         |
| `PermissionError`       | No tienes permisos para acceder o escribir   |
| Sobrescribir sin querer | Uso de `'w'` en vez de `'a'`                 |
| No cerrar el archivo    | Si no usas `with open`, puede quedar abierto |

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Guardar resultados

```python
with open("resultado.txt", "w") as f:
    for i in range(1, 6):
        f.write(f"{i} x 2 = {i*2}\n")
```

---

### Ejemplo 2: Leer y contar líneas

```python
with open("notas.txt") as f:
    lineas = f.readlines()
    print("Total líneas:", len(lineas))
```

---

### Ejemplo 3: Cargar lista de tareas desde CSV

```python
import csv

with open("tareas.csv") as archivo:
    lector = csv.reader(archivo)
    for tarea in lector:
        print("Tarea:", tarea[0])
```

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Crea un archivo `log.txt` y escribe la hora actual cada vez que ejecutes el script.

---

### 📝 Ejercicio 2

Pide al usuario su nombre y edad, y guarda la información en un archivo CSV.

---

### 📝 Ejercicio 3

Guarda una lista de diccionarios con información de libros (`título`, `autor`, `año`) en un archivo JSON.

---

Excelente elección. Entramos ahora en uno de los pilares de la programación moderna:

---

# 🧠 Módulo 9: Programación Orientada a Objetos (POO) en Python

---

## 🎯 Objetivos del módulo

* Comprender los conceptos clave de la POO: **clase**, **objeto**, **atributo**, **método**, **herencia**, etc.
* Aprender a crear y usar clases propias.
* Aplicar encapsulamiento, reutilización y extensión de código mediante herencia.
* Entender la diferencia entre métodos de instancia, clase y estáticos.
* Utilizar `__init__`, `self`, `__str__`, y otros métodos especiales.

---

## 🧩 ¿Qué es la Programación Orientada a Objetos?

Es un paradigma basado en **objetos**, los cuales son **instancias de clases** que encapsulan datos (atributos) y comportamientos (métodos).

📘 **Ejemplo real**:
Un coche (objeto) tiene:

* Atributos: color, marca, velocidad.
* Métodos: arrancar(), frenar(), acelerar().

---

## 🏗️ Crear una clase

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad

    def saludar(self):
        print(f"Hola, me llamo {self.nombre} y tengo {self.edad} años.")
```

---

## 🧍 Crear objetos

```python
persona1 = Persona("Lucía", 30)
persona1.saludar()
```

---

## 📚 Componentes de una clase

| Elemento     | Descripción                                       |
| ------------ | ------------------------------------------------- |
| `__init__()` | Método constructor, se ejecuta al crear el objeto |
| `self`       | Referencia al objeto actual                       |
| Atributos    | Datos que describen el estado del objeto          |
| Métodos      | Funciones asociadas al objeto                     |

---

## 🔒 Encapsulamiento

Python no tiene modificadores de acceso estrictos, pero se usan convenciones:

| Tipo              | Ejemplo       | Significado                        |
| ----------------- | ------------- | ---------------------------------- |
| Público           | `self.nombre` | Accesible desde fuera              |
| Protegido (conv.) | `_edad`       | No se debe acceder fuera           |
| Privado (conv.)   | `__dni`       | Interno, no accesible directamente |

---

## 📦 Métodos especiales

| Método     | Función                                |
| ---------- | -------------------------------------- |
| `__init__` | Constructor                            |
| `__str__`  | Representación en forma de texto       |
| `__repr__` | Representación técnica para depuración |
| `__eq__`   | Comparación de igualdad                |

📘 Ejemplo `__str__`:

```python
def __str__(self):
    return f"{self.nombre} ({self.edad} años)"
```

---

## 🧬 Herencia

Una clase puede heredar de otra para **reutilizar código**:

```python
class Empleado(Persona):
    def __init__(self, nombre, edad, salario):
        super().__init__(nombre, edad)
        self.salario = salario

    def saludar(self):
        super().saludar()
        print(f"Soy empleado y gano {self.salario}€.")
```

---

## 🔁 Métodos de clase y estáticos

### `@classmethod`

Accede a la clase en lugar del objeto:

```python
class Persona:
    contador = 0

    def __init__(self, nombre):
        self.nombre = nombre
        Persona.contador += 1

    @classmethod
    def total_personas(cls):
        return cls.contador
```

### `@staticmethod`

No accede ni a la instancia ni a la clase:

```python
class Utilidades:
    @staticmethod
    def es_par(n):
        return n % 2 == 0
```

---

## ✅ Buenas prácticas

* Usa nombres en **CamelCase** para clases (`MiClase`).
* Usa `self` de forma explícita en métodos de instancia.
* Evita el uso excesivo de herencia profunda; prefiere **composición** si es más simple.
* Documenta tus clases y métodos con docstrings.

---

## ⚠️ Errores comunes

| Error                                  | Descripción                               |
| -------------------------------------- | ----------------------------------------- |
| Olvidar `self`                         | En métodos y atributos dentro de la clase |
| No usar `super()`                      | Al heredar y sobrescribir `__init__`      |
| Confundir método de clase con estático | Usar mal los decoradores                  |

---

## 🧠 Ejemplos prácticos

### Ejemplo 1: Clase `Libro`

```python
class Libro:
    def __init__(self, titulo, autor):
        self.titulo = titulo
        self.autor = autor

    def __str__(self):
        return f"{self.titulo} - {self.autor}"
```

---

### Ejemplo 2: Herencia `Vehiculo` → `Coche`

```python
class Vehiculo:
    def __init__(self, marca):
        self.marca = marca

class Coche(Vehiculo):
    def __init__(self, marca, modelo):
        super().__init__(marca)
        self.modelo = modelo
```

---

### Ejemplo 3: Contador de instancias

```python
class Usuario:
    total = 0

    def __init__(self, nombre):
        self.nombre = nombre
        Usuario.total += 1

    @classmethod
    def contar(cls):
        return cls.total
```

---

## 🎯 Ejercicios prácticos

### 📝 Ejercicio 1

Crea una clase `Alumno` con nombre y nota. Añade un método `aprobo()` que devuelva `True` si la nota es ≥ 5.

---

### 📝 Ejercicio 2

Crea una clase `Animal` con un método `hablar()`. Luego crea dos clases `Perro` y `Gato` que hereden de `Animal` y sobrescriban el método.

---

### 📝 Ejercicio 3

Crea una clase `CuentaBancaria` con métodos para depositar, retirar y consultar el saldo.

---

Perfecto, cerraremos el **Nivel Básico** aplicando lo aprendido en un proyecto práctico y realista:

---

# 🧪 Módulo 10: Proyecto Práctico – Gestor de Tareas (To-Do List)

---

## 🎯 Objetivo del proyecto

Crear una aplicación de consola en Python que permita al usuario:

* Añadir tareas con título y estado.
* Mostrar las tareas actuales.
* Marcar tareas como completadas.
* Eliminar tareas.
* Guardar y cargar tareas desde un archivo JSON.

---

## 🧱 Herramientas y conceptos utilizados

* Tipos de datos, condicionales, bucles.
* Funciones propias.
* Manejo de excepciones.
* Archivos (`open()`, `json`).
* Programación orientada a objetos (clases y objetos).
* Módulos y organización del código.

---

## 🧩 Estructura del programa

Tendremos un archivo principal:
📄 `gestor_tareas.py`

Y el programa se organizará de la siguiente manera:

* Una clase `Tarea`
* Una clase `GestorTareas`
* Un menú en consola para interactuar con el usuario
* Lectura/escritura en archivo JSON

---

## 📄 Código completo comentado

```python
import json

class Tarea:
    def __init__(self, titulo, completada=False):
        self.titulo = titulo
        self.completada = completada

    def marcar_completada(self):
        self.completada = True

    def to_dict(self):
        return {"titulo": self.titulo, "completada": self.completada}

    @staticmethod
    def desde_dict(datos):
        return Tarea(datos["titulo"], datos["completada"])


class GestorTareas:
    def __init__(self, archivo="tareas.json"):
        self.archivo = archivo
        self.tareas = []
        self.cargar_tareas()

    def agregar_tarea(self, titulo):
        self.tareas.append(Tarea(titulo))

    def mostrar_tareas(self):
        if not self.tareas:
            print("No hay tareas.")
            return
        for i, tarea in enumerate(self.tareas, 1):
            estado = "✅" if tarea.completada else "❌"
            print(f"{i}. {tarea.titulo} [{estado}]")

    def marcar_completada(self, indice):
        try:
            self.tareas[indice - 1].marcar_completada()
        except IndexError:
            print("Índice no válido.")

    def eliminar_tarea(self, indice):
        try:
            del self.tareas[indice - 1]
        except IndexError:
            print("Índice no válido.")

    def guardar_tareas(self):
        datos = [tarea.to_dict() for tarea in self.tareas]
        with open(self.archivo, "w", encoding="utf-8") as f:
            json.dump(datos, f, indent=4)

    def cargar_tareas(self):
        try:
            with open(self.archivo, "r", encoding="utf-8") as f:
                datos = json.load(f)
                self.tareas = [Tarea.desde_dict(t) for t in datos]
        except FileNotFoundError:
            self.tareas = []


def mostrar_menu():
    print("\n--- GESTOR DE TAREAS ---")
    print("1. Ver tareas")
    print("2. Añadir tarea")
    print("3. Marcar tarea como completada")
    print("4. Eliminar tarea")
    print("5. Salir")


def main():
    gestor = GestorTareas()

    while True:
        mostrar_menu()
        opcion = input("Elige una opción: ")

        if opcion == "1":
            gestor.mostrar_tareas()
        elif opcion == "2":
            titulo = input("Título de la tarea: ")
            gestor.agregar_tarea(titulo)
        elif opcion == "3":
            gestor.mostrar_tareas()
            try:
                i = int(input("Número de tarea a marcar: "))
                gestor.marcar_completada(i)
            except ValueError:
                print("Debes introducir un número válido.")
        elif opcion == "4":
            gestor.mostrar_tareas()
            try:
                i = int(input("Número de tarea a eliminar: "))
                gestor.eliminar_tarea(i)
            except ValueError:
                print("Debes introducir un número válido.")
        elif opcion == "5":
            gestor.guardar_tareas()
            print("Tareas guardadas. ¡Hasta pronto!")
            break
        else:
            print("Opción no válida.")


if __name__ == "__main__":
    main()
```

---

## 🧠 Extensiones sugeridas

* Añadir prioridad a las tareas.
* Permitir editar el título de una tarea.
* Filtrar tareas completadas vs. pendientes.
* Mostrar tareas con colores (usando `colorama` o similares).

---

## ✅ Con este proyecto habrás practicado:

* Entrada y salida por consola.
* Uso de clases, listas, diccionarios.
* Modularización del código.
* Trabajo con ficheros JSON.
* Manejo de errores y validaciones básicas.

---



