¡Perfecto, Mario! Vamos a ver los **principios SOLID aplicados en Python**, con explicaciones claras y ejemplos prácticos. Estos principios ayudan a escribir código más **mantenible, escalable, testeable y limpio**, y son fundamentales para el diseño de software profesional.

---

# 🧱 Principios SOLID en Python

> SOLID es un acrónimo de 5 principios fundamentales del diseño orientado a objetos:

| Letra | Nombre                          | En español                             |
| ----- | ------------------------------- | -------------------------------------- |
| **S** | Single Responsibility Principle | Principio de Responsabilidad Única     |
| **O** | Open/Closed Principle           | Principio Abierto/Cerrado              |
| **L** | Liskov Substitution Principle   | Principio de Sustitución de Liskov     |
| **I** | Interface Segregation Principle | Principio de Segregación de Interfaces |
| **D** | Dependency Inversion Principle  | Principio de Inversión de Dependencias |

---

## 1. 🧩 S – **Single Responsibility Principle (SRP)**

**Una clase debe tener una sola responsabilidad.**

🔴 *Anti-patrón (violación):*

```python
class Reporte:
    def generar(self):
        return "Reporte generado"

    def guardar_en_archivo(self):
        with open("reporte.txt", "w") as f:
            f.write(self.generar())
```

✅ *Aplicando SRP:*

```python
class Reporte:
    def generar(self):
        return "Reporte generado"

class ReportePersistencia:
    def guardar_en_archivo(self, reporte: Reporte):
        with open("reporte.txt", "w") as f:
            f.write(reporte.generar())
```

---

## 2. 🧰 O – **Open/Closed Principle (OCP)**

**El código debe estar abierto para extensión pero cerrado para modificación.**

🔴 *Mal diseño:*

```python
def calcular_precio(producto, tipo):
    if tipo == "normal":
        return producto.precio
    elif tipo == "vip":
        return producto.precio * 0.8
```

✅ *Aplicando OCP con herencia y polimorfismo:*

```python
class EstrategiaPrecio:
    def calcular(self, producto):
        raise NotImplementedError

class PrecioNormal(EstrategiaPrecio):
    def calcular(self, producto):
        return producto.precio

class PrecioVIP(EstrategiaPrecio):
    def calcular(self, producto):
        return producto.precio * 0.8
```

---

## 3. 🧬 L – **Liskov Substitution Principle (LSP)**

**Una subclase debe poder reemplazar a su clase base sin romper el programa.**

🔴 *Violación:*

```python
class Ave:
    def volar(self):
        print("Estoy volando")

class Pinguino(Ave):
    def volar(self):
        raise Exception("¡No puedo volar!")
```

✅ *Cumpliendo LSP (clases separadas por comportamiento):*

```python
class Ave:
    pass

class AveVoladora(Ave):
    def volar(self):
        print("Estoy volando")

class Pinguino(Ave):
    def nadar(self):
        print("Estoy nadando")
```

---

## 4. 🧩 I – **Interface Segregation Principle (ISP)**

**Los clientes no deben depender de interfaces que no usan.**

🔴 *Violación:*

```python
class Trabajador:
    def trabajar(self): pass
    def comer(self): pass

class Robot(Trabajador):
    def trabajar(self): pass
    def comer(self): raise Exception("¡No como!")
```

✅ *Separando interfaces:*

```python
class Trabajador:
    def trabajar(self): pass

class Comedor:
    def comer(self): pass

class Humano(Trabajador, Comedor):
    def trabajar(self): pass
    def comer(self): pass

class Robot(Trabajador):
    def trabajar(self): pass
```

---

## 5. 🧲 D – **Dependency Inversion Principle (DIP)**

**El código debe depender de abstracciones, no de implementaciones.**

🔴 *Mal diseño:*

```python
class MotorGasolina:
    def arrancar(self):
        print("Arrancando motor gasolina")

class Coche:
    def __init__(self):
        self.motor = MotorGasolina()
```

✅ *Aplicando DIP:*

```python
class Motor:
    def arrancar(self):
        pass

class MotorGasolina(Motor):
    def arrancar(self):
        print("Motor gasolina arrancado")

class MotorElectrico(Motor):
    def arrancar(self):
        print("Motor eléctrico arrancado")

class Coche:
    def __init__(self, motor: Motor):
        self.motor = motor
```

> Ahora puedes hacer: `Coche(MotorElectrico())` o `Coche(MotorGasolina())`

---

## 🧠 Resumen visual

| Principio | En una línea                                               |
| --------- | ---------------------------------------------------------- |
| **S**     | Cada clase tiene un solo motivo para cambiar.              |
| **O**     | Extiende el comportamiento sin modificar código existente. |
| **L**     | Las subclases deben ser sustituibles por sus clases base.  |
| **I**     | Interfaces pequeñas y específicas.                         |
| **D**     | Inyecta abstracciones, no implementaciones concretas.      |

---

¡Perfecto, Mario! Te voy a preparar un **mini proyecto Python limpio y estructurado aplicando los 5 principios SOLID**, usando un ejemplo realista y útil: **un sistema de gestión de facturas con generación, almacenamiento y exportación**.

---

# 📦 Proyecto: `facturador_solid`

> Aplicaremos los principios SOLID a un sistema que:

* Genera facturas de clientes
* Calcula totales con estrategias de descuento
* Permite exportarlas (JSON, TXT, consola…)
* Está desacoplado, es testeable y extensible

---

## 📁 Estructura

```
facturador_solid/
├── main.py
├── modelos/
│   └── factura.py
├── servicios/
│   ├── generador.py
│   ├── descuento.py
│   └── exportador.py
└── interfaces/
    ├── i_descuento.py
    └── i_exportador.py
```

---

## 1️⃣ SRP – **Responsabilidad Única**

✅ Cada clase tiene un solo rol:

* `Factura`: almacena datos
* `GeneradorFactura`: genera la factura
* `ExportadorJSON`: exporta a JSON
* `EstrategiaDescuentoVIP`: calcula descuento

---

## 2️⃣ Código base

### 📄 `modelos/factura.py`

```python
from dataclasses import dataclass
from typing import List

@dataclass
class Item:
    nombre: str
    cantidad: int
    precio_unitario: float

@dataclass
class Factura:
    cliente: str
    items: List[Item]
    total: float
    descuento: float
```

---

### 📄 `interfaces/i_descuento.py`

```python
from abc import ABC, abstractmethod
from modelos.factura import Factura

class IEstrategiaDescuento(ABC):
    @abstractmethod
    def aplicar(self, factura: Factura) -> float:
        pass
```

---

### 📄 `interfaces/i_exportador.py`

```python
from abc import ABC, abstractmethod
from modelos.factura import Factura

class IExportador(ABC):
    @abstractmethod
    def exportar(self, factura: Factura) -> None:
        pass
```

---

### 📄 `servicios/descuento.py` – OCP, LSP

```python
from interfaces.i_descuento import IEstrategiaDescuento

class DescuentoVIP(IEstrategiaDescuento):
    def aplicar(self, factura):
        return factura.total * 0.15

class SinDescuento(IEstrategiaDescuento):
    def aplicar(self, factura):
        return 0.0
```

---

### 📄 `servicios/exportador.py` – ISP, DIP

```python
from interfaces.i_exportador import IExportador
from modelos.factura import Factura
import json

class ExportadorConsola(IExportador):
    def exportar(self, factura: Factura):
        print(f"Factura para {factura.cliente}: Total {factura.total}€")

class ExportadorJSON(IExportador):
    def exportar(self, factura: Factura):
        with open("factura.json", "w") as f:
            json.dump(factura.__dict__, f, indent=2, default=str)
```

---

### 📄 `servicios/generador.py` – DIP, SRP

```python
from modelos.factura import Factura
from interfaces.i_descuento import IEstrategiaDescuento

class GeneradorFactura:
    def __init__(self, estrategia_descuento: IEstrategiaDescuento):
        self.estrategia = estrategia_descuento

    def generar(self, cliente: str, items: list) -> Factura:
        subtotal = sum(i.precio_unitario * i.cantidad for i in items)
        factura = Factura(cliente, items, subtotal, 0.0)
        descuento = self.estrategia.aplicar(factura)
        factura.descuento = descuento
        factura.total = subtotal - descuento
        return factura
```

---

### 📄 `main.py` – Ejecución final

```python
from modelos.factura import Item
from servicios.generador import GeneradorFactura
from servicios.descuento import DescuentoVIP
from servicios.exportador import ExportadorJSON

# Datos simulados
items = [
    Item("Laptop", 1, 1000),
    Item("Ratón", 2, 25)
]

# Generar factura con descuento VIP
generador = GeneradorFactura(DescuentoVIP())
factura = generador.generar("Mario Monzón", items)

# Exportar a JSON
exportador = ExportadorJSON()
exportador.exportar(factura)
```

---

## ✅ Principios aplicados

| Principio | Ejemplo en el proyecto                                                      |
| --------- | --------------------------------------------------------------------------- |
| **S**     | Cada clase tiene una única responsabilidad                                  |
| **O**     | Puedes crear nuevos `EstrategiaDescuento` sin modificar el código existente |
| **L**     | `SinDescuento` y `DescuentoVIP` pueden sustituirse sin romper nada          |
| **I**     | Interfaces separadas para descuentos y exportadores                         |
| **D**     | `GeneradorFactura` y `main.py` dependen de abstracciones (interfaces)       |

---

## 🧪 ¿Quieres probarlo?


