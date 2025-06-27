Comenzamos el **Nivel Avanzado** con uno de los frameworks más modernos, rápidos y eficientes para desarrollo web en Python: **FastAPI**.

---

# 🚀 Módulo 1 – Introducción a FastAPI y desarrollo backend profesional

---

## 🎯 Objetivos del módulo

* Comprender los fundamentos de FastAPI.
* Crear una API RESTful moderna con endpoints funcionales.
* Validar datos con **Pydantic**.
* Conectar la API a una base de datos con **SQLModel** o **SQLAlchemy**.
* Usar **OpenAPI/Swagger** para documentar automáticamente la API.
* Aplicar buenas prácticas para proyectos backend en producción.

---

## 🧰 ¿Qué es FastAPI?

FastAPI es un framework web **asíncrono y moderno** para crear APIs con Python 3.7+ basado en:

* **Pydantic** para validación de datos.
* **Starlette** como motor web.
* Documentación automática con **Swagger UI** y **ReDoc**.

### 🔥 Ventajas:

* Más rápido que Flask.
* Validación automática de datos.
* Tipado fuerte con anotaciones de Python.
* Muy fácil de testear y escalar.

---

## 🛠️ Requisitos previos

Instala las dependencias básicas:

```bash
pip install fastapi uvicorn
```

Para trabajar con bases de datos:

```bash
pip install sqlmodel[all]  # o puedes usar sqlalchemy + pydantic si prefieres
```

---

## 🧪 Proyecto base: API de libros (versión RESTful)

Creamos un proyecto que expone endpoints para:

* Crear libros
* Obtener todos los libros
* Consultar libro por ID
* Eliminar libros
* Actualizar libros

---

## 📁 Estructura del proyecto

```bash
fastapi_libros/
├── main.py
├── models.py
├── database.py
├── crud.py
├── schemas.py
├── test_main.py
└── requirements.txt
```

---

## 📄 `models.py` (modelo + validación con SQLModel)

```python
from sqlmodel import SQLModel, Field
from typing import Optional

class Libro(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    titulo: str
    autor: str
    anio: int
```

---

## 📄 `schemas.py` (DTOs para entrada/salida)

```python
from pydantic import BaseModel

class LibroCreate(BaseModel):
    titulo: str
    autor: str
    anio: int
```

---

## 📄 `database.py` (conexión y creación de tablas)

```python
from sqlmodel import create_engine, SQLModel, Session

DATABASE_URL = "sqlite:///./libros.db"
engine = create_engine(DATABASE_URL, echo=True)

def crear_tablas():
    SQLModel.metadata.create_all(engine)

def get_session():
    with Session(engine) as session:
        yield session
```

---

## 📄 `crud.py` (operaciones de base de datos)

```python
from sqlmodel import Session, select
from models import Libro

def crear_libro(session: Session, libro: Libro):
    session.add(libro)
    session.commit()
    session.refresh(libro)
    return libro

def obtener_libros(session: Session):
    return session.exec(select(Libro)).all()

def obtener_libro(session: Session, libro_id: int):
    return session.get(Libro, libro_id)

def eliminar_libro(session: Session, libro_id: int):
    libro = session.get(Libro, libro_id)
    if libro:
        session.delete(libro)
        session.commit()
        return True
    return False
```

---

## 📄 `main.py` (servidor principal)

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlmodel import Session
from models import Libro
from schemas import LibroCreate
from database import crear_tablas, get_session
import crud

app = FastAPI(title="API de Libros")

@app.on_event("startup")
def on_startup():
    crear_tablas()

@app.post("/libros/", response_model=Libro)
def crear(libro_data: LibroCreate, session: Session = Depends(get_session)):
    libro = Libro(**libro_data.dict())
    return crud.crear_libro(session, libro)

@app.get("/libros/", response_model=list[Libro])
def listar(session: Session = Depends(get_session)):
    return crud.obtener_libros(session)

@app.get("/libros/{libro_id}", response_model=Libro)
def obtener(libro_id: int, session: Session = Depends(get_session)):
    libro = crud.obtener_libro(session, libro_id)
    if not libro:
        raise HTTPException(status_code=404, detail="Libro no encontrado")
    return libro

@app.delete("/libros/{libro_id}")
def borrar(libro_id: int, session: Session = Depends(get_session)):
    if not crud.eliminar_libro(session, libro_id):
        raise HTTPException(status_code=404, detail="Libro no encontrado")
    return {"ok": True}
```

---

## 🚀 Ejecutar el servidor

```bash
uvicorn main:app --reload
```

Accede a la documentación interactiva en:

* 📘 Swagger UI: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
* 📕 ReDoc: [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---

## ✅ Buenas prácticas

* Divide código en modelos, esquemas, base de datos y lógica (CRUD).
* Usa `Depends()` para inyección de dependencias (como la sesión).
* Valida siempre los datos con Pydantic.
* Usa OpenAPI y documentación integrada como estándar de desarrollo.

---

## 📝 Ejercicios sugeridos

1. Añadir endpoint `PUT /libros/{id}` para actualizar un libro.
2. Añadir campo booleano `prestado` al modelo con endpoint para marcarlo como prestado/devuelto.
3. Implementar paginación (`limit`, `offset`) en el `GET /libros/`.
4. Agregar filtros por autor o año.

---

Perfecto. Entramos en una de las partes más importantes para cualquier backend profesional: **Autenticación y autorización**, usando **FastAPI con JWT y OAuth2**.

---

# 🔐 Módulo 2 – Autenticación y autorización en FastAPI (JWT, OAuth2, usuarios y roles)

---

## 🎯 Objetivos del módulo

* Implementar **login seguro** usando tokens JWT.
* Proteger rutas con **OAuth2 Password Flow**.
* Crear y gestionar usuarios y contraseñas en la base de datos.
* Implementar **roles y permisos** básicos.
* Aplicar buenas prácticas de seguridad en APIs web.

---

## 🛠️ Librerías necesarias

```bash
pip install passlib[bcrypt] python-jose[cryptography]
```

---

## 📁 Estructura del proyecto

```bash
fastapi_auth/
├── main.py
├── auth.py         # lógica de autenticación y JWT
├── models.py       # modelos SQLModel
├── schemas.py      # DTOs
├── database.py
├── users.py        # rutas protegidas
└── test_auth.py
```

---

## 🔐 Fundamentos de JWT

Un **JWT (JSON Web Token)** contiene:

* **Header**: algoritmo (ej. HS256)
* **Payload**: datos codificados (ej. user\_id, rol)
* **Signature**: asegura que el contenido no ha sido alterado

---

## 📄 `models.py`

```python
from sqlmodel import SQLModel, Field
from typing import Optional

class Usuario(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    username: str
    email: str
    hashed_password: str
    rol: str = "usuario"
```

---

## 📄 `schemas.py`

```python
from pydantic import BaseModel

class UsuarioCreate(BaseModel):
    username: str
    email: str
    password: str

class UsuarioOut(BaseModel):
    id: int
    username: str
    email: str
    rol: str

class Token(BaseModel):
    access_token: str
    token_type: str
```

---

## 📄 `auth.py`

```python
from datetime import datetime, timedelta
from jose import JWTError, jwt
from passlib.context import CryptContext
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
from sqlmodel import Session, select
from models import Usuario
from database import get_session

# Clave secreta (usa una segura en prod)
SECRET_KEY = "supersecreta"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")

def verificar_password(plain, hashed):
    return pwd_context.verify(plain, hashed)

def hash_password(password):
    return pwd_context.hash(password)

def crear_token(data: dict, expires_delta: timedelta = None):
    to_encode = data.copy()
    expire = datetime.utcnow() + (expires_delta or timedelta(minutes=15))
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

def verificar_token(token: str = Depends(oauth2_scheme), session: Session = Depends(get_session)):
    cred_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="No autorizado",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise cred_exception
    except JWTError:
        raise cred_exception

    user = session.exec(select(Usuario).where(Usuario.username == username)).first()
    if user is None:
        raise cred_exception
    return user
```

---

## 📄 `main.py` (registro y login)

```python
from fastapi import FastAPI, Depends, HTTPException
from database import crear_tablas, get_session
from sqlmodel import Session
from models import Usuario
from schemas import UsuarioCreate, UsuarioOut, Token
from auth import hash_password, verificar_password, crear_token
from fastapi.security import OAuth2PasswordRequestForm

app = FastAPI()

@app.on_event("startup")
def startup():
    crear_tablas()

@app.post("/registro", response_model=UsuarioOut)
def registrar(usuario: UsuarioCreate, session: Session = Depends(get_session)):
    user = Usuario(
        username=usuario.username,
        email=usuario.email,
        hashed_password=hash_password(usuario.password)
    )
    session.add(user)
    session.commit()
    session.refresh(user)
    return user

@app.post("/login", response_model=Token)
def login(form_data: OAuth2PasswordRequestForm = Depends(), session: Session = Depends(get_session)):
    user = session.exec(select(Usuario).where(Usuario.username == form_data.username)).first()
    if not user or not verificar_password(form_data.password, user.hashed_password):
        raise HTTPException(status_code=400, detail="Credenciales incorrectas")
    
    token = crear_token({"sub": user.username})
    return {"access_token": token, "token_type": "bearer"}
```

---

## 📄 `users.py` (rutas protegidas con roles)

```python
from fastapi import APIRouter, Depends, HTTPException
from auth import verificar_token
from models import Usuario

router = APIRouter()

@router.get("/me")
def obtener_usuario_actual(user: Usuario = Depends(verificar_token)):
    return user

@router.get("/admin")
def solo_admin(user: Usuario = Depends(verificar_token)):
    if user.rol != "admin":
        raise HTTPException(status_code=403, detail="Acceso restringido a administradores")
    return {"mensaje": "Bienvenido, administrador"}
```

Y luego en `main.py`:

```python
from users import router as user_router
app.include_router(user_router)
```

---

## 🔐 Probar en Swagger

1. Regístrate con `/registro`.
2. Haz login en `/login` → copia el token.
3. Pulsa "Authorize" en Swagger y pega `Bearer <tu_token>`.
4. Accede a `/me` o `/admin`.

---

## ✅ Buenas prácticas

* Guarda la `SECRET_KEY` y `DATABASE_URL` en variables de entorno.
* Aplica hashing seguro con `bcrypt`.
* Define funciones reutilizables para autenticación y autorización.
* Usa decoradores o dependencias para controlar roles (ej. `admin_required`).
* No expongas contraseñas en las respuestas.

---

## 📝 Ejercicios prácticos

1. Añadir endpoint para cambiar la contraseña del usuario autenticado.
2. Añadir campo `activo: bool` y evitar login si `False`.
3. Implementar función `@admin_required` como decorador o dependencia.
4. Crear endpoint que liste todos los usuarios, solo accesible para admins.

---

Perfecto. Vamos con un módulo muy útil para aplicaciones web completas: formularios, validación avanzada y gestión de archivos, incluyendo **subida de imágenes**.

---

# 🗂️ Módulo 3 – Subida de archivos, formularios y validación avanzada en FastAPI

---

## 🎯 Objetivos del módulo

* Subir y guardar archivos en el servidor (imágenes, PDFs, etc.).
* Procesar formularios HTML con `Form`.
* Validar inputs usando expresiones regulares, longitudes y dependencias personalizadas.
* Controlar tipos MIME y tamaños de archivo.
* Preparar la API para interoperar con frontends reales.

---

## 🛠️ Librerías necesarias

FastAPI ya incluye todo lo necesario para este módulo.
Solo asegúrate de tener `python-multipart`:

```bash
pip install python-multipart
```

Para trabajar con imágenes opcionalmente:

```bash
pip install pillow
```

---

## 📥 Subida de archivos con `UploadFile`

FastAPI maneja archivos grandes de forma eficiente usando `UploadFile` (streaming, sin cargarlo entero en memoria).

### 📄 Endpoint de ejemplo:

```python
from fastapi import FastAPI, File, UploadFile
import shutil
import os

app = FastAPI()

UPLOAD_DIR = "uploads"
os.makedirs(UPLOAD_DIR, exist_ok=True)

@app.post("/subir/")
async def subir_archivo(archivo: UploadFile = File(...)):
    ruta = f"{UPLOAD_DIR}/{archivo.filename}"
    with open(ruta, "wb") as buffer:
        shutil.copyfileobj(archivo.file, buffer)
    return {"nombre_archivo": archivo.filename, "tipo": archivo.content_type}
```

📌 Puedes probarlo desde Swagger UI: `/docs`.

---

## 📑 Enviar múltiples archivos

```python
@app.post("/multi-subida/")
async def subir_varios(archivos: list[UploadFile] = File(...)):
    nombres = []
    for archivo in archivos:
        ruta = f"{UPLOAD_DIR}/{archivo.filename}"
        with open(ruta, "wb") as buffer:
            shutil.copyfileobj(archivo.file, buffer)
        nombres.append(archivo.filename)
    return {"archivos_guardados": nombres}
```

---

## 🧾 Procesar formularios HTML con `Form`

```python
from fastapi import Form

@app.post("/login/")
def login(username: str = Form(...), password: str = Form(...)):
    return {"usuario": username}
```

📌 Ideal para integraciones con frontends o clientes `multipart/form-data`.

---

## 🧰 Validación avanzada con Pydantic

```python
from pydantic import BaseModel, EmailStr, Field

class UsuarioRegistro(BaseModel):
    username: str = Field(min_length=3, max_length=20, regex="^[a-zA-Z0-9_]+$")
    email: EmailStr
    password: str = Field(min_length=8)
```

Esto asegura que:

* El nombre de usuario solo contiene letras, números y guiones bajos.
* El email es válido.
* La contraseña tiene al menos 8 caracteres.

---

## 🧪 Validación condicional (dependencias personalizadas)

```python
from fastapi import Depends, HTTPException

def verificar_token_admin(token: str = Depends(oauth2_scheme)):
    if token != "token-admin-demo":
        raise HTTPException(status_code=403, detail="No autorizado")
```

Y luego:

```python
@app.post("/solo-admin/")
def solo_admin(dep=Depends(verificar_token_admin)):
    return {"mensaje": "Acceso autorizado"}
```

---

## 🖼️ Validar archivos por tipo y tamaño

```python
@app.post("/imagen/")
async def subir_imagen(imagen: UploadFile = File(...)):
    if imagen.content_type not in ["image/png", "image/jpeg"]:
        raise HTTPException(status_code=400, detail="Formato no permitido")
    contenido = await imagen.read()
    if len(contenido) > 2 * 1024 * 1024:
        raise HTTPException(status_code=400, detail="Imagen demasiado grande")
    # Guardar...
```

---

## 🧪 Extra: abrir imagen con Pillow

```python
from PIL import Image

@app.post("/procesar/")
async def procesar_imagen(img: UploadFile = File(...)):
    imagen = Image.open(img.file)
    ancho, alto = imagen.size
    return {"dimensiones": f"{ancho}x{alto}"}
```

---

## ✅ Buenas prácticas

* Usa carpetas separadas por tipo (`uploads/images`, `uploads/docs`, etc.).
* Controla los tamaños máximos de subida.
* No uses el nombre original del archivo sin sanitizar (riesgo de path traversal).
* Genera nombres únicos: `uuid`, `datetime`, etc.

---

## 📝 Ejercicios prácticos

1. Crea un endpoint que permita subir una imagen de perfil y la renombre como `{usuario_id}.jpg`.
2. Crea un endpoint que solo acepte archivos `.pdf` de menos de 5 MB.
3. Diseña un formulario de registro vía `Form`, incluyendo validación de campos (usuario, email, contraseña).
4. Implementa lógica para borrar archivos antiguos del servidor tras cierto tiempo.

---

Excelente decisión. Un backend profesional necesita **tests robustos, automatizados y mantenibles**. Vamos con:

---

# 🧪 Módulo 4 – Testing profesional en FastAPI con Pytest, mocks y cobertura

---

## 🎯 Objetivos del módulo

* Crear tests unitarios y de integración con `pytest`.
* Simular respuestas y componentes externos con `unittest.mock`.
* Medir cobertura de tests con `coverage.py`.
* Usar `TestClient` de FastAPI para probar endpoints.
* Estructurar una carpeta de tests mantenible y clara.

---

## 📦 Librerías necesarias

```bash
pip install pytest httpx coverage pytest-cov
```

> `httpx` es usado internamente por `TestClient` para simular peticiones HTTP.

---

## 📁 Estructura recomendada

```bash
fastapi_proyecto/
├── app/
│   ├── main.py
│   ├── auth.py
│   ├── models.py
│   └── ...
└── tests/
    ├── conftest.py
    ├── test_auth.py
    ├── test_usuarios.py
    └── ...
```

---

## 🧪 `TestClient` para FastAPI

FastAPI ofrece un cliente de pruebas interno:

```python
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_ping():
    response = client.get("/ping")
    assert response.status_code == 200
    assert response.json() == {"mensaje": "pong"}
```

---

## 📄 `tests/conftest.py` (fixtures reutilizables)

```python
import pytest
from fastapi.testclient import TestClient
from app.main import app
from sqlmodel import Session
from app.database import engine, crear_tablas

@pytest.fixture(scope="module")
def client():
    crear_tablas()
    with TestClient(app) as c:
        yield c
```

---

## ✅ Test de endpoints públicos

```python
def test_registro_usuario(client):
    res = client.post("/registro", json={
        "username": "usuario1",
        "email": "usuario1@test.com",
        "password": "secreta123"
    })
    assert res.status_code == 200
    data = res.json()
    assert data["username"] == "usuario1"
    assert "id" in data
```

---

## 🔐 Test de login y rutas protegidas

```python
def test_login_y_token(client):
    res = client.post("/login", data={
        "username": "usuario1",
        "password": "secreta123"
    })
    assert res.status_code == 200
    token = res.json()["access_token"]
    
    # Probar ruta protegida
    headers = {"Authorization": f"Bearer {token}"}
    res2 = client.get("/me", headers=headers)
    assert res2.status_code == 200
    assert res2.json()["username"] == "usuario1"
```

---

## 🧪 Test con mocks (`unittest.mock`)

Simula funciones o dependencias externas (correo, API externa, etc.):

```python
from unittest.mock import patch

@patch("app.auth.crear_token")
def test_login_mock_token(mock_token, client):
    mock_token.return_value = "mocked-token"
    res = client.post("/login", data={"username": "usuario1", "password": "secreta123"})
    assert res.json()["access_token"] == "mocked-token"
```

---

## 📊 Medir cobertura de tests

```bash
coverage run -m pytest
coverage report -m
```

O con `pytest-cov`:

```bash
pytest --cov=app --cov-report=term-missing
```

---

## 🚨 Casos especiales

### Test con errores esperados

```python
def test_login_contrasena_incorrecta(client):
    res = client.post("/login", data={"username": "usuario1", "password": "mal"})
    assert res.status_code == 400
```

### Test con base de datos aislada

Puedes usar una **base SQLite en memoria** o mocks para evitar modificar la real.

---

## ✅ Buenas prácticas

* Nombrar los tests descriptivamente: `test_crear_usuario_valido()`.
* Separar tests por módulos.
* No mezclar test unitarios y de integración en un mismo archivo.
* Usar `pytest.mark.parametrize()` para múltiples entradas.
* Aislar dependencias externas con `mock`.

---

## 📝 Ejercicios propuestos

1. Añadir tests para `/admin`, incluyendo errores de acceso.
2. Testear que no se puede registrar con email duplicado.
3. Mockear una función de envío de correo en el registro.
4. Generar un reporte de cobertura mínimo del 85%.

---

Excelente decisión. Combinaremos **Machine Learning con scikit-learn** y lo integraremos en una API con **FastAPI**, creando una solución lista para producción o integración con frontends.

---

# 🤖 Módulo 5 – Inteligencia Artificial con Python: Scikit-learn + FastAPI

---

## 🎯 Objetivos del módulo

* Entrenar un modelo de ML con **scikit-learn**.
* Guardarlo y reutilizarlo con **joblib**.
* Crear una API en **FastAPI** que reciba datos y devuelva predicciones.
* Validar inputs con **Pydantic**.
* Probar predicciones con `curl` o Swagger UI.

---

## 🧠 Proyecto: Clasificador de flores (Iris dataset)

Entrenaremos un modelo para clasificar flores (setosa, versicolor, virginica) a partir de 4 características.

---

## 🛠️ Instalación de dependencias

```bash
pip install scikit-learn joblib fastapi uvicorn
```

---

## 📁 Estructura del proyecto

```bash
ml_api/
├── model/
│   └── iris_model.joblib
├── train_model.py
├── main.py
└── schemas.py
```

---

## 🧪 Paso 1 – Entrenar y guardar el modelo (`train_model.py`)

```python
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
import joblib

# Cargar datos
iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target, test_size=0.2, random_state=42)

# Entrenar modelo
model = RandomForestClassifier()
model.fit(X_train, y_train)

# Guardar modelo
joblib.dump(model, "model/iris_model.joblib")
print("✅ Modelo guardado.")
```

---

## 📄 `schemas.py` – Validación de entrada

```python
from pydantic import BaseModel, Field

class IrisInput(BaseModel):
    sepal_length: float = Field(..., gt=0)
    sepal_width: float = Field(..., gt=0)
    petal_length: float = Field(..., gt=0)
    petal_width: float = Field(..., gt=0)
```

---

## 📄 `main.py` – Servidor FastAPI con predicción

```python
from fastapi import FastAPI
from schemas import IrisInput
import joblib
import numpy as np

# Cargar modelo
model = joblib.load("model/iris_model.joblib")
app = FastAPI(title="API Clasificación de Flores 🌸")

@app.post("/predecir/")
def predecir(data: IrisInput):
    features = np.array([[data.sepal_length, data.sepal_width, data.petal_length, data.petal_width]])
    pred = model.predict(features)[0]
    clases = ["setosa", "versicolor", "virginica"]
    return {
        "entrada": data.dict(),
        "clase_predicha": clases[pred]
    }
```

---

## 🚀 Ejecutar API

```bash
uvicorn main:app --reload
```

* Abre Swagger UI: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

## 🧪 Ejemplo de entrada JSON

```json
{
  "sepal_length": 5.1,
  "sepal_width": 3.5,
  "petal_length": 1.4,
  "petal_width": 0.2
}
```

Respuesta esperada:

```json
{
  "entrada": {
    "sepal_length": 5.1,
    "sepal_width": 3.5,
    "petal_length": 1.4,
    "petal_width": 0.2
  },
  "clase_predicha": "setosa"
}
```

---

## ✅ Buenas prácticas

* Aislar el modelo en una carpeta y versiónalo (`model/iris_model_v1.joblib`).
* Usar `Field(..., gt=0)` para evitar entradas inválidas.
* Añadir logging para auditoría de predicciones.
* Opcional: guardar historiales de entrada/salida en base de datos para revisión.

---

## 📝 Ejercicios sugeridos

1. Entrenar un modelo diferente (SVM, KNN) y comparar resultados.
2. Añadir probabilidad de predicción (`model.predict_proba()`).
3. Crear un endpoint `/health` que indique si el modelo está cargado correctamente.
4. Implementar autenticación JWT para el endpoint de predicción.

---

Perfecto. Entramos ahora en el mundo del **Big Data con Python**, donde aprenderás a procesar volúmenes masivos de datos con herramientas eficientes como **Pandas, Dask y PySpark**.

---

# 🧠 Módulo 6 – Big Data con Python: Pandas, Dask y PySpark

---

## 🎯 Objetivos del módulo

* Comprender las limitaciones de **Pandas** y cuándo usar alternativas.
* Procesar datasets grandes con **Dask**, sin cargar todo en memoria.
* Trabajar con **PySpark** para procesamiento distribuido.
* Aprender a optimizar operaciones, paralelizar y escalar.
* Ejecutar análisis prácticos en CSVs o datasets masivos.

---

## 📦 Herramientas y librerías

```bash
pip install pandas dask[complete] pyspark
```

---

## 📄 Parte 1 – Pandas (procesamiento en memoria)

### ✅ Ideal para:

* Datos pequeños o medianos (< 2 GB).
* Análisis rápido, limpieza, transformaciones.

```python
import pandas as pd

df = pd.read_csv("ventas.csv")

# Limpiar valores nulos
df = df.dropna()

# Agrupar por país
resumen = df.groupby("pais")["ventas"].sum().sort_values(ascending=False)
print(resumen.head())
```

📉 Limite: se carga todo el dataset en memoria → problemas con grandes volúmenes.

---

## ⚙️ Parte 2 – Dask (paralelismo y eficiencia en un solo equipo)

Dask permite trabajar con **DataFrames tipo Pandas**, pero por bloques y en paralelo.

### 🚀 Ventajas:

* Usa múltiples núcleos automáticamente.
* Maneja datasets más grandes que la RAM.
* API muy parecida a Pandas.

### 🧪 Ejemplo:

```python
import dask.dataframe as dd

df = dd.read_csv("datos_grandes/*.csv")

# Operaciones similares a Pandas
total_por_categoria = df.groupby("categoria")["ventas"].sum().compute()

print(total_por_categoria)
```

### 🧠 Notas:

* `.compute()` es necesario para ejecutar las operaciones.
* Dask ejecuta **perezosamente**, construyendo un grafo de tareas.

---

## 🧩 Parte 3 – PySpark (procesamiento distribuido estilo Hadoop)

### 🔥 Ideal para:

* Procesamiento a escala (decenas o cientos de GB).
* Ambientes con clúster (Spark, Databricks, EMR, etc.).
* Preparación para entornos empresariales.

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Ventas").getOrCreate()

df = spark.read.csv("ventas.csv", header=True, inferSchema=True)

df.groupBy("pais").sum("ventas").orderBy("sum(ventas)", ascending=False).show(5)
```

### 🧠 Características:

* Funciona por defecto en modo local (`local[*]`).
* También trabaja en modo clúster (Spark Standalone, YARN, Kubernetes).
* Usa transformaciones *lazy* y optimización interna.

---

## 📊 Comparación rápida

| Herramienta | Escala | API similar a Pandas  | Paralelismo     | Ideal para          |
| ----------- | ------ | --------------------- | --------------- | ------------------- |
| Pandas      | Baja   | Sí                    | No              | Scripts locales     |
| Dask        | Media  | Sí                    | Sí (multi-core) | CSVs grandes        |
| PySpark     | Alta   | Similar pero distinta | Sí (clúster)    | Empresas, clústeres |

---

## 📝 Ejercicios prácticos

1. Cargar y analizar un dataset de +1 millón de registros con Dask.
2. Comparar el tiempo de ejecución entre Pandas y Dask.
3. Crear una app con PySpark que calcule:

   * Promedio de ventas por región.
   * 10 productos más vendidos.
   * Días con mayores ventas.
4. Exportar los resultados a Parquet desde Dask o PySpark.

---

## ✅ Buenas prácticas

* Para Dask, organiza tus datos por **archivos pequeños**, no un único CSV enorme.
* Usa `Parquet` o `Avro` para mejores lecturas en Big Data.
* Con PySpark, aprovecha `DataFrame API` antes que RDDs.
* Dask se integra bien con FastAPI, Jupyter y Dash.

---

Excelente. Vamos con un módulo clave para entornos profesionales: **despliegue automático y profesional con Docker, Uvicorn, Nginx y GitHub Actions**, cubriendo desde la contenerización hasta la integración continua y la puesta en producción.

---

# 🚀 Módulo 7 – CI/CD y despliegue profesional con Docker, Uvicorn, Nginx y GitHub Actions

---

## 🎯 Objetivos del módulo

* Contenerizar una aplicación FastAPI con **Docker**.
* Servirla eficientemente en producción con **Uvicorn + Gunicorn**.
* Configurar **Nginx** como proxy inverso.
* Automatizar el flujo con **GitHub Actions** para CI/CD.
* Desplegar en un servidor (ej. VPS o contenedor cloud-ready).

---

## 📁 Estructura base del proyecto

```bash
fastapi_proyecto/
├── app/
│   ├── main.py
│   └── ...
├── Dockerfile
├── docker-compose.yml
├── nginx/
│   └── default.conf
├── .github/
│   └── workflows/
│       └── deploy.yml
└── requirements.txt
```

---

## 🐳 Paso 1 – `Dockerfile` para FastAPI + Uvicorn + Gunicorn

```Dockerfile
FROM python:3.11-slim

# Variables de entorno
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Instalar dependencias
WORKDIR /app
COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

# Copiar el proyecto
COPY . .

# Ejecutar con Gunicorn + Uvicorn
CMD ["gunicorn", "-k", "uvicorn.workers.UvicornWorker", "app.main:app", "--bind", "0.0.0.0:8000"]
```

---

## 📦 `requirements.txt` (mínimo)

```
fastapi
uvicorn[standard]
gunicorn
```

---

## ⚙️ Paso 2 – Configurar Nginx como proxy reverso

📄 `nginx/default.conf`:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://web:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 🧩 Paso 3 – `docker-compose.yml` (FastAPI + Nginx)

```yaml
version: '3.8'

services:
  web:
    build: .
    container_name: fastapi_app
    restart: always
    expose:
      - 8000
    volumes:
      - .:/app

  nginx:
    image: nginx:stable
    container_name: fastapi_nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - web
```

---

## 🚀 Paso 4 – Construir y ejecutar

```bash
docker-compose up --build
```

Accede a: [http://localhost](http://localhost)

---

## 🔄 Paso 5 – CI/CD con GitHub Actions

📄 `.github/workflows/deploy.yml`

```yaml
name: CI/CD FastAPI

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repo
      uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: 3.11

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Run tests
      run: |
        pytest

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - name: SSH & Deploy
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.SSH_HOST }}
        username: ${{ secrets.SSH_USER }}
        key: ${{ secrets.SSH_KEY }}
        script: |
          cd /ruta/del/proyecto
          git pull origin main
          docker-compose down
          docker-compose up --build -d
```

> **🔐 Asegúrate de configurar los `secrets` en GitHub:**
> `SSH_HOST`, `SSH_USER`, `SSH_KEY`

---

## ✅ Buenas prácticas

* Usa Gunicorn en producción (mejor gestión de workers).
* No expongas puertos de backend directamente al público.
* Automatiza el testeo en cada `push`.
* Usa certificados HTTPS en Nginx con Let's Encrypt (opcional).
* Mantén tus imágenes livianas (usa Alpine o `--no-cache`).

---

## 📝 Ejercicios propuestos

1. Añadir un `health check` para Nginx y FastAPI.
2. Configurar despliegue automático también en push a `develop`.
3. Versionar imágenes con etiquetas (`image: fastapi_app:v1`).
4. Añadir supervisión con Grafana + Prometheus en otra red Docker.

---

Perfecto, vamos a crear un **Proyecto Final Integrador** que reúna todo lo aprendido en este plan formativo: **Python avanzado, FastAPI, Inteligencia Artificial, Big Data y Despliegue Profesional.**

---

# 🧩 Proyecto Final – Plataforma de predicción de abandono de clientes (Churn Predictor)

---

## 🧠 ¿Qué vamos a construir?

Una **plataforma web** con backend en FastAPI, que:

* Reciba información de clientes.
* Use un modelo de Machine Learning entrenado previamente para predecir si un cliente va a abandonar (churn).
* Guarde los datos y predicciones en un sistema de almacenamiento tipo Big Data (CSV o Parquet).
* Permita consultar estadísticas agregadas.
* Esté desplegada profesionalmente con Docker + Nginx + GitHub Actions.

---

## 🗂️ Estructura del proyecto

```bash
churn_predictor/
├── app/
│   ├── main.py              # FastAPI app
│   ├── model.py             # Carga del modelo
│   ├── schemas.py           # Validación
│   ├── routes.py            # Rutas
│   └── utils.py             # Utilidades
├── ml/
│   └── churn_model.joblib   # Modelo ML entrenado
├── data/
│   └── registros.parquet    # Historico de predicciones
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── train_model.py
├── nginx/
│   └── default.conf
└── .github/
    └── workflows/
        └── deploy.yml
```

---

## 🛠️ 1. Entrenamiento del modelo (`train_model.py`)

```python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
import joblib

df = pd.read_csv("churn_dataset.csv")  # Incluye campos como edad, ingresos, duración, etc.

X = df.drop(columns=["churn"])
y = df["churn"]

model = RandomForestClassifier()
model.fit(X, y)

joblib.dump(model, "ml/churn_model.joblib")
print("✅ Modelo entrenado y guardado.")
```

---

## 🧾 2. `schemas.py` – Entrada de datos del cliente

```python
from pydantic import BaseModel, Field

class ClienteInput(BaseModel):
    edad: int = Field(gt=17, lt=100)
    ingresos: float = Field(gt=0)
    duracion_meses: int = Field(gt=0)
    uso_app: float = Field(gt=0)
    soporte_llamadas: int = Field(ge=0)
```

---

## 🔍 3. `model.py` – Carga del modelo ML

```python
import joblib
import numpy as np

modelo = joblib.load("ml/churn_model.joblib")

def predecir_churn(data: list[float]) -> float:
    return modelo.predict_proba([data])[0][1]
```

---

## 🌐 4. `routes.py` – Rutas API

```python
from fastapi import APIRouter
from schemas import ClienteInput
from model import predecir_churn
import pandas as pd
from pathlib import Path

router = APIRouter()

@router.post("/predecir/")
def predecir(cliente: ClienteInput):
    datos = list(cliente.dict().values())
    prob = predecir_churn(datos)
    
    # Guardar en Parquet
    registro = cliente.dict()
    registro["churn_probabilidad"] = prob
    df = pd.DataFrame([registro])
    path = Path("data/registros.parquet")
    if path.exists():
        df_total = pd.read_parquet(path)
        df_total = pd.concat([df_total, df])
    else:
        df_total = df
    df_total.to_parquet(path, index=False)

    return {"churn_probabilidad": round(prob, 3)}
```

---

## 🖥️ 5. `main.py` – Montar la app

```python
from fastapi import FastAPI
from routes import router

app = FastAPI(title="Predicción de Churn")
app.include_router(router)
```

---

## 🐳 6. `Dockerfile` (igual que antes)

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 🌐 7. `nginx/default.conf` (proxy inverso)

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://web:8000;
    }
}
```

---

## 📦 8. `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: .
    volumes:
      - .:/app
    expose:
      - 8000

  nginx:
    image: nginx:stable
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - web
```

---

## ⚙️ 9. CI/CD con GitHub Actions (`deploy.yml`)

> Igual al módulo anterior, solo cambias ruta del proyecto y servidor destino.

---

## 🧪 10. Ejemplo de entrada JSON para prueba

```json
{
  "edad": 35,
  "ingresos": 40000,
  "duracion_meses": 12,
  "uso_app": 3.4,
  "soporte_llamadas": 5
}
```

---

## ✅ Extras recomendados

* Agregar autenticación con JWT para proteger el endpoint.
* Crear dashboard de administración con Streamlit o Dash.
* Programar backup automático del archivo `registros.parquet`.
* Analizar el archivo con Dask para métricas agregadas.

---

Perfecto, vamos a diseñar un **dashboard de visualización interactivo** para nuestro proyecto de predicción de churn. Utilizaremos **Streamlit**, que permite construir dashboards modernos con Python de forma rápida, simple y personalizable.

---

# 📊 Frontend/Dashboard – Visualización de Predicciones con Streamlit

---

## 🎯 Objetivos del dashboard

* Mostrar estadísticas en tiempo real de predicciones almacenadas.
* Permitir subir nuevos registros CSV o Parquet para analizar.
* Visualizar métricas como:

  * Porcentaje de clientes con alta probabilidad de churn.
  * Histograma de probabilidades.
  * Análisis por edad, ingresos, duración, etc.
* Filtro por rangos y campos específicos.

---

## 🛠️ 1. Instalar dependencias

```bash
pip install streamlit pandas plotly pyarrow
```

---

## 📄 2. Estructura del frontend

```bash
dashboard/
├── app.py
├── assets/
│   └── logo.png (opcional)
└── data/
    └── registros.parquet (acceso al histórico)
```

---

## 🧾 3. `app.py` – Código base del dashboard

```python
import streamlit as st
import pandas as pd
import plotly.express as px
from pathlib import Path

st.set_page_config(page_title="Dashboard Churn", layout="wide")

st.title("📈 Dashboard de Predicción de Abandono (Churn)")
st.markdown("Visualización de predicciones generadas por la IA de FastAPI.")

DATA_PATH = Path("data/registros.parquet")

@st.cache_data
def cargar_datos():
    if DATA_PATH.exists():
        return pd.read_parquet(DATA_PATH)
    return pd.DataFrame()

df = cargar_datos()

if df.empty:
    st.warning("No hay datos de predicciones disponibles aún.")
    st.stop()

# --- Métricas principales
col1, col2, col3 = st.columns(3)
col1.metric("Total registros", len(df))
col2.metric("Prob. Churn promedio", f"{df['churn_probabilidad'].mean():.2f}")
col3.metric("Riesgo Alto (>0.7)", f"{(df['churn_probabilidad'] > 0.7).mean() * 100:.1f}%")

# --- Filtros
st.sidebar.header("🔎 Filtros")
edad_min, edad_max = st.sidebar.slider("Edad", 18, 90, (18, 90))
df_filtrado = df[(df["edad"] >= edad_min) & (df["edad"] <= edad_max)]

# --- Histograma
st.subheader("Distribución de probabilidad de churn")
fig1 = px.histogram(df_filtrado, x="churn_probabilidad", nbins=20, color_discrete_sequence=["indianred"])
st.plotly_chart(fig1, use_container_width=True)

# --- Relación por ingresos
st.subheader("Relación entre ingresos y probabilidad de churn")
fig2 = px.scatter(df_filtrado, x="ingresos", y="churn_probabilidad", color="edad", size="duracion_meses",
                  hover_data=["uso_app", "soporte_llamadas"])
st.plotly_chart(fig2, use_container_width=True)

# --- Tabla de detalle
st.subheader("📋 Tabla de registros")
st.dataframe(df_filtrado.sort_values("churn_probabilidad", ascending=False).reset_index(drop=True))
```

---

## 🖥️ 4. Ejecutar dashboard localmente

```bash
cd dashboard
streamlit run app.py
```

---

## 💡 Funcionalidades opcionales a agregar

* 📤 Subida manual de archivos nuevos `.parquet` o `.csv` para análisis.
* 🔒 Autenticación básica con contraseña o token.
* 📬 Envío de alertas por email si se detectan clientes en riesgo alto.
* 📊 Exportación de resultados como Excel o PDF.

---

## 📦 Despliegue en producción

Puedes desplegar este dashboard como contenedor o servidor independiente:

### 🔹 Opción 1: Docker (Dockerfile)

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY . .
RUN pip install -r requirements.txt

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### 🔹 Opción 2: Desplegar en Streamlit Cloud (gratis)

1. Sube el dashboard a GitHub.
2. Ve a [https://streamlit.io/cloud](https://streamlit.io/cloud) y conéctalo.
3. Añade `data/registros.parquet` o apunta a la API como backend de datos.

---

Perfecto. Separar el **backend (FastAPI)** y el **dashboard (Streamlit)** en dos servicios distintos es una **arquitectura limpia y escalable**, ideal para entornos profesionales. Vamos a organizar los servicios con **Docker Compose**, cada uno en su contenedor, comunicándose mediante red interna.

---

# 🧱 Arquitectura de servicios

```text
┌────────────┐     ┌─────────────┐     ┌─────────────┐
│  NGINX     │────▶│ FastAPI     │     │ Streamlit   │
│ (proxy)    │     └─────────────┘     └─────────────┘
└────────────┘           ▲                  ▲
                         │                  │
                  Predicción de datos   Lectura Parquet
```

---

# 📁 Estructura del proyecto multi-servicio

```bash
churn_project/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── model.py
│   │   ├── routes.py
│   │   └── schemas.py
│   ├── ml/
│   │   └── churn_model.joblib
│   ├── data/
│   │   └── registros.parquet
│   ├── requirements.txt
│   └── Dockerfile
├── dashboard/
│   ├── app.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── data/ (montado desde backend)
├── nginx/
│   └── default.conf
├── docker-compose.yml
└── .github/
    └── workflows/
        └── deploy.yml
```

---

# ⚙️ `docker-compose.yml` completo

```yaml
version: "3.9"

services:
  fastapi:
    build: ./backend
    container_name: churn_backend
    volumes:
      - ./backend/data:/app/data
    expose:
      - 8000
    networks:
      - internal

  streamlit:
    build: ./dashboard
    container_name: churn_dashboard
    volumes:
      - ./backend/data:/app/data
    ports:
      - "8501:8501"
    networks:
      - internal

  nginx:
    image: nginx:stable
    container_name: nginx_gateway
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - fastapi
    networks:
      - internal

networks:
  internal:
    driver: bridge
```

---

# 🧾 NGINX como proxy inverso (opcional)

📄 `nginx/default.conf`:

```nginx
server {
    listen 80;

    location /api/ {
        proxy_pass http://fastapi:8000/;
    }

    location /dashboard/ {
        proxy_pass http://streamlit:8501/;
    }
}
```

Accede luego a:

* API: `http://localhost/api/docs`
* Dashboard: `http://localhost/dashboard/`

---

# 🐳 Dockerfile – Backend (`backend/Dockerfile`)

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

# 🐳 Dockerfile – Dashboard (`dashboard/Dockerfile`)

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

# 🏁 Puesta en marcha

```bash
docker-compose up --build
```

---

# ✅ Ventajas de esta arquitectura

| Aspecto                 | Beneficio                                     |
| ----------------------- | --------------------------------------------- |
| Separación de servicios | Mejor escalabilidad y mantenimiento           |
| Red compartida          | Comunicación segura entre backend y dashboard |
| Volúmenes compartidos   | Dashboard accede a los datos del backend      |
| Preparado para CI/CD    | Cada servicio puede tener su propio flujo     |

---

Perfecto. Vamos a mejorar el **dashboard de visualización de churn** con:

---

# 🔐 Autenticación básica

# 🔔 Alertas de riesgo alto

# 📊 Estadísticas avanzadas

Todo esto manteniendo la simplicidad de Streamlit, pero haciéndolo más útil en producción o entornos internos de análisis.

---

## ✅ 1. AUTENTICACIÓN SIMPLE EN STREAMLIT

No existe login por defecto en Streamlit, pero podemos simular uno con un formulario de inicio de sesión controlado por `st.session_state`.

📄 En `dashboard/app.py` (parte superior):

```python
import streamlit as st

# Configurar sesión
if "autenticado" not in st.session_state:
    st.session_state.autenticado = False

# Formulario de login
if not st.session_state.autenticado:
    st.title("🔒 Acceso restringido")
    usuario = st.text_input("Usuario")
    clave = st.text_input("Contraseña", type="password")
    if st.button("Iniciar sesión"):
        if usuario == "admin" and clave == "1234":
            st.session_state.autenticado = True
        else:
            st.error("Credenciales inválidas")
    st.stop()
```

> 🔐 Puedes mejorar esto usando `.env` o variables de entorno si se requiere más seguridad.

---

## 📈 2. ESTADÍSTICAS AVANZADAS

### 🔹 Porcentaje de riesgo agrupado

```python
riesgo_alto = df[df["churn_probabilidad"] > 0.7]
riesgo_medio = df[(df["churn_probabilidad"] > 0.4) & (df["churn_probabilidad"] <= 0.7)]
riesgo_bajo = df[df["churn_probabilidad"] <= 0.4]

st.subheader("Distribución de riesgo")
col1, col2, col3 = st.columns(3)
col1.metric("🔴 Alto", f"{len(riesgo_alto)} clientes")
col2.metric("🟠 Medio", f"{len(riesgo_medio)} clientes")
col3.metric("🟢 Bajo", f"{len(riesgo_bajo)} clientes")
```

### 🔹 Top 10 clientes con más riesgo

```python
st.subheader("🔝 Clientes con mayor probabilidad de abandono")
top_churn = df.sort_values("churn_probabilidad", ascending=False).head(10)
st.table(top_churn)
```

---

## 📣 3. ALERTAS AUTOMÁTICAS (por app y visuales)

### Visual: Alerta si hay clientes con churn > 90%

```python
if (df["churn_probabilidad"] > 0.9).any():
    st.error("⚠️ Alerta: ¡Clientes con riesgo extremo detectado!")
```

### Opcional: Envío de email con `smtplib` (requiere configuración)

```python
import smtplib
from email.message import EmailMessage

def enviar_alerta():
    msg = EmailMessage()
    msg["Subject"] = "Alerta de Churn"
    msg["From"] = "tudominio@correo.com"
    msg["To"] = "equipo@empresa.com"
    msg.set_content("Se han detectado clientes con churn > 0.9. Revisar el dashboard.")

    with smtplib.SMTP_SSL("smtp.gmail.com", 465) as smtp:
        smtp.login("tudominio@correo.com", "TUPASS")
        smtp.send_message(msg)
```

> ⚠️ Requiere activar acceso a apps menos seguras si usas Gmail. También puedes usar servicios externos como Mailgun o SendGrid.

---

## 📝 Adicional: botón para exportar tabla filtrada

```python
import io

csv = df_filtrado.to_csv(index=False).encode()
st.download_button("⬇️ Descargar datos filtrados", csv, file_name="predicciones.csv", mime="text/csv")
```

---

## ✅ Checklist de mejoras implementadas

| Mejora                           | Estado                    |
| -------------------------------- | ------------------------- |
| 🔐 Login por sesión              | ✅ Implementado            |
| 📣 Alerta por churn > 0.9        | ✅ Visual / Email opcional |
| 📊 Estadísticas por categoría    | ✅ Agregado                |
| 📄 Exportación de tabla filtrada | ✅ Listo                   |

---

Perfecto. Vamos a implementar un **sistema de roles y permisos básicos en el dashboard**, ideal para separar accesos entre usuarios normales (solo visualización) y administradores (que puedan ver alertas, descargar datos, etc.).

---

# 🔐 Sistema de roles y permisos para el dashboard en Streamlit

---

## 🎯 Objetivo

* Incluir autenticación por usuario/contraseña.
* Asociar un rol a cada usuario: `"admin"` o `"viewer"`.
* Condicionar contenido según el rol.

---

## 🗂️ Paso 1: Definir usuarios y roles (puede ser JSON, lista o archivo externo)

📄 En la parte superior de `app.py`:

```python
USUARIOS = {
    "admin": {"password": "1234", "rol": "admin"},
    "juan":  {"password": "5678", "rol": "viewer"}
}
```

> ⚠️ Para producción, se recomienda guardar las contraseñas hasheadas (`bcrypt`, `passlib`) o cargarlas desde un `.env` o archivo externo.

---

## 🔐 Paso 2: Login y gestión de sesión

```python
if "autenticado" not in st.session_state:
    st.session_state.autenticado = False
    st.session_state.rol = None
    st.session_state.usuario = ""

if not st.session_state.autenticado:
    st.title("🔐 Inicio de sesión")
    usuario = st.text_input("Usuario")
    clave = st.text_input("Contraseña", type="password")
    if st.button("Acceder"):
        if usuario in USUARIOS and USUARIOS[usuario]["password"] == clave:
            st.session_state.autenticado = True
            st.session_state.rol = USUARIOS[usuario]["rol"]
            st.session_state.usuario = usuario
            st.success(f"Bienvenido, {usuario} ({st.session_state.rol})")
            st.experimental_rerun()
        else:
            st.error("Credenciales incorrectas")
    st.stop()
```

---

## ⚙️ Paso 3: Control de permisos

Luego del login, ya puedes usar `st.session_state.rol` para mostrar o esconder contenido.

### Ejemplo: alertas solo visibles para `"admin"`

```python
if st.session_state.rol == "admin":
    if (df["churn_probabilidad"] > 0.9).any():
        st.error("⚠️ ¡Clientes con riesgo extremo detectado!")

    st.subheader("🔝 Top 10 clientes más críticos")
    st.table(df.sort_values("churn_probabilidad", ascending=False).head(10))

    # Exportación CSV solo para admins
    csv = df_filtrado.to_csv(index=False).encode()
    st.download_button("⬇️ Exportar datos", csv, file_name="datos_churn.csv", mime="text/csv")
else:
    st.info("Estás en modo lectura. Algunas funciones están limitadas.")
```

---

## 🧼 Paso 4: Agregar botón para cerrar sesión

Al final del archivo:

```python
with st.sidebar:
    if st.button("🔒 Cerrar sesión"):
        st.session_state.autenticado = False
        st.session_state.rol = None
        st.experimental_rerun()
```

---

## 🧪 Roles posibles

| Rol    | Permisos                                                             |
| ------ | -------------------------------------------------------------------- |
| admin  | Ver alertas, estadísticas completas, exportar datos, top 10 clientes |
| viewer | Solo visualización de gráficos, tabla general y filtros              |

Puedes extenderlo con más roles como `analista`, `marketing`, `soporte`, etc., según necesidad.

---

## ✅ Resultado

Sistema completo de autenticación + autorización por rol en Streamlit sin dependencias externas, manteniendo todo encapsulado y listo para producción interna o despliegue privado.

---

Perfecto. Vamos a generar el `Dockerfile` actualizado del dashboard con autenticación, control de roles y un archivo `.env` para ocultar credenciales. También lo integramos en el `docker-compose.yml`.

---

# 🐳 `dashboard/Dockerfile` actualizado

```Dockerfile
FROM python:3.11-slim

# Variables de entorno
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Copiar dependencias e instalar
COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

# Copiar app
COPY . .

# Ejecutar dashboard
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

# 📦 `dashboard/requirements.txt`

Asegúrate de tener:

```txt
streamlit
pandas
plotly
pyarrow
python-dotenv
```

---

# 🔐 `.env` (en el directorio `dashboard/`)

> ⚠️ No lo subas a GitHub (añade `.env` a `.gitignore`)

```env
USER_ADMIN=admin
PASS_ADMIN=1234
USER_VIEWER=juan
PASS_VIEWER=5678
```

---

# 📄 `dashboard/app.py` – Leer variables desde `.env`

### Paso 1: Importar y cargar

```python
import os
from dotenv import load_dotenv

load_dotenv()

USUARIOS = {
    os.getenv("USER_ADMIN"): {"password": os.getenv("PASS_ADMIN"), "rol": "admin"},
    os.getenv("USER_VIEWER"): {"password": os.getenv("PASS_VIEWER"), "rol": "viewer"}
}
```

> Puedes agregar más usuarios si lo deseas (usa otro `.env` para entorno real).

---

# 🔗 `docker-compose.yml` – Integración final

Agrega o edita el bloque del servicio `streamlit`:

```yaml
streamlit:
  build: ./dashboard
  container_name: churn_dashboard
  volumes:
    - ./backend/data:/app/data
    - ./dashboard/.env:/app/.env
  ports:
    - "8501:8501"
  networks:
    - internal
  environment:
    - STREAMLIT_SERVER_HEADLESS=true
```

---

# ✅ Comandos para ejecutar

```bash
docker-compose up --build
```

Accede a:

* Dashboard: `http://localhost:8501`
* Se pedirá usuario y contraseña (ej: `admin / 1234`)

---

# 📌 Mejores prácticas recomendadas

* Cifra las contraseñas con `bcrypt` para producción.
* Usa HTTPS (Let’s Encrypt o proxy inverso con NGINX).
* Administra usuarios desde una base de datos o API backend si el sistema crece.
* Añade logging de accesos o intentos fallidos.

---

Perfecto, Mario. Vamos a reemplazar el acceso del dashboard a archivos Parquet por **peticiones directas al backend FastAPI**, lo que mejora la escalabilidad, seguridad, sincronización y estructura de microservicios reales.

---

# 🔄 Reemplazar Parquet por API REST – Conexión directa FastAPI + Streamlit

---

## 🎯 Objetivo

* El backend FastAPI expone una API REST para:

  * Crear predicciones (ya existente).
  * Consultar predicciones realizadas (nuevo endpoint).
* El dashboard Streamlit consulta directamente estas rutas vía HTTP.
* El almacenamiento (en disco o en DB) queda en manos del backend.

---

## 📦 1. **Modificar el Backend FastAPI** para exponer predicciones

📄 `routes.py` – Añade nuevo endpoint:

```python
from fastapi import APIRouter
from model import predecir_churn
from schemas import ClienteInput
import pandas as pd
from pathlib import Path
from fastapi.responses import JSONResponse

router = APIRouter()
DATA_PATH = Path("data/registros.parquet")

@router.post("/predecir/")
def predecir(cliente: ClienteInput):
    datos = list(cliente.dict().values())
    prob = predecir_churn(datos)

    # Guardar resultado
    registro = cliente.dict()
    registro["churn_probabilidad"] = prob
    df = pd.DataFrame([registro])
    
    if DATA_PATH.exists():
        df_total = pd.read_parquet(DATA_PATH)
        df_total = pd.concat([df_total, df])
    else:
        df_total = df
    df_total.to_parquet(DATA_PATH, index=False)

    return {"churn_probabilidad": round(prob, 3)}

@router.get("/predicciones/")
def obtener_predicciones():
    if DATA_PATH.exists():
        df = pd.read_parquet(DATA_PATH)
        return JSONResponse(content=df.to_dict(orient="records"))
    return JSONResponse(content=[])
```

---

## 🌐 2. **Conectar el Dashboard Streamlit a la API**

📄 `dashboard/app.py` – Reemplaza `cargar_datos()`:

```python
import requests

API_URL = "http://fastapi:8000"  # Cambiar si accedes desde fuera del contenedor

@st.cache_data(ttl=60)
def cargar_datos():
    try:
        r = requests.get(f"{API_URL}/predicciones/")
        if r.status_code == 200:
            return pd.DataFrame(r.json())
    except Exception as e:
        st.error(f"Error al conectarse con el backend: {e}")
    return pd.DataFrame()
```

---

## 🧪 Probar localmente

Accede a `http://localhost:8501`, y el dashboard ya mostrará los datos actualizados en tiempo real directamente desde el backend vía API, sin leer archivos directamente.

---

## 🐳 3. Ajuste en `docker-compose.yml` para acceso entre servicios

Ya que Streamlit y FastAPI comparten red Docker, **usa `fastapi` como hostname** (es el nombre del servicio):

```yaml
services:
  fastapi:
    container_name: fastapi
    ...

  streamlit:
    container_name: dashboard
    environment:
      - API_URL=http://fastapi:8000
    ...
```

Si quieres usar una variable en lugar de hardcodear `http://fastapi:8000`, puedes hacer:

```python
import os
API_URL = os.getenv("API_URL", "http://fastapi:8000")
```

---

## 🛡️ Ventajas clave de usar la API:

| Aspecto           | Archivos locales                | API REST (nuevo enfoque)            |
| ----------------- | ------------------------------- | ----------------------------------- |
| 🔄 Sincronización | Manual o por volumen            | Siempre actualizados                |
| 🔐 Seguridad      | Cualquier app puede leer        | Control de acceso en backend        |
| 📈 Escalabilidad  | Difícil en múltiples instancias | Fácil de extender (BD, cache, etc.) |
| 🧱 Modularidad    | Acoplamiento fuerte             | Microservicios reales               |

---

## 🚀 Próximos pasos opcionales

1. Añadir paginación, filtros y ordenamiento al endpoint `/predicciones/`.
2. Reemplazar Parquet por base de datos (SQLite, PostgreSQL o MongoDB).
3. Incluir un `/predicciones/stats` que devuelva métricas listas para el dashboard.
4. Añadir cache o fondo de sincronización (ej: Redis, Celery).

---

Perfecto, Mario. Vamos a migrar el almacenamiento de predicciones de **archivos Parquet a una base de datos relacional**, específicamente **SQLite** (por simplicidad en local o desarrollo), pero fácilmente escalable a PostgreSQL o MySQL.

---

# 🔁 Migración de almacenamiento de Parquet a Base de Datos (SQLite)

---

## 🔧 Objetivo

* Usar **SQLAlchemy** + **SQLite** para guardar y consultar las predicciones.
* Modificar el backend FastAPI para almacenar y exponer datos desde la DB.
* El dashboard Streamlit seguirá consumiendo los datos desde la API.

---

## ⚙️ Paso 1: Añadir dependencias al backend

📄 `backend/requirements.txt`:

```txt
fastapi
uvicorn
pydantic
pandas
scikit-learn
joblib
sqlalchemy
```

---

## 📦 Paso 2: Configurar la base de datos con SQLAlchemy

📄 `app/db.py`:

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./data/churn.db"

engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

---

📄 `app/models_db.py` – Definir la tabla

```python
from sqlalchemy import Column, Integer, Float
from db import Base

class Prediccion(Base):
    __tablename__ = "predicciones"

    id = Column(Integer, primary_key=True, index=True)
    edad = Column(Integer)
    ingresos = Column(Float)
    duracion_meses = Column(Integer)
    uso_app = Column(Float)
    soporte_llamadas = Column(Integer)
    churn_probabilidad = Column(Float)
```

---

📄 En `main.py` – Crear la tabla si no existe

```python
from db import Base, engine

Base.metadata.create_all(bind=engine)
```

---

## 🚀 Paso 3: Modificar rutas para usar la base de datos

📄 `routes.py`:

```python
from fastapi import APIRouter, Depends
from sqlalchemy.orm import Session
from db import SessionLocal
from models_db import Prediccion
from schemas import ClienteInput
from model import predecir_churn

router = APIRouter()

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@router.post("/predecir/")
def predecir(cliente: ClienteInput, db: Session = Depends(get_db)):
    datos = list(cliente.dict().values())
    prob = predecir_churn(datos)

    pred = Prediccion(**cliente.dict(), churn_probabilidad=prob)
    db.add(pred)
    db.commit()
    db.refresh(pred)

    return {"churn_probabilidad": round(prob, 3)}

@router.get("/predicciones/")
def obtener_predicciones(db: Session = Depends(get_db)):
    registros = db.query(Prediccion).all()
    return [r.__dict__ for r in registros]
```

> 🔐 Puedes limpiar `__dict__` antes de devolverlo si usas modelos con campos privados.

---

## ✅ Resultado: Backend funcionando con base de datos

Ya puedes usar `/predecir/` y `/predicciones/` totalmente sobre SQLite, sin Parquet.

---

## 🧪 Paso 4: El dashboard no necesita cambiar

El dashboard seguirá haciendo:

```python
r = requests.get("http://fastapi:8000/predicciones/")
df = pd.DataFrame(r.json())
```

Solo que ahora el backend devuelve los datos directamente desde la DB.

---

## 📦 Bonus: Persistencia con Docker

En `docker-compose.yml`, agrega volumen al servicio `fastapi`:

```yaml
fastapi:
  ...
  volumes:
    - ./backend/data:/app/data
```

Así se persistirá `churn.db`.

---

## 🔄 Migración completada

| Antes (Parquet)             | Ahora (Base de datos SQLite)      |
| --------------------------- | --------------------------------- |
| Lectura/escritura manual    | ORM con SQLAlchemy                |
| Datos en archivo plano      | Consultas filtradas, persistentes |
| Escalabilidad limitada      | Fácil de migrar a PostgreSQL      |
| No relaciones ni integridad | Total control y normalización     |

---

Perfecto, Mario. Migrar de SQLite a **PostgreSQL** es un paso esencial para llevar tu backend a producción real: más robustez, concurrencia, control de acceso, backups, rendimiento, etc.

Vamos a hacerlo paso a paso.

---

# 🐘 Migración de SQLite a PostgreSQL con SQLAlchemy en FastAPI

---

## 🔧 1. Cambios en dependencias

📄 `backend/requirements.txt`

Asegúrate de incluir:

```txt
psycopg2-binary
sqlalchemy
```

> ⚠️ Usa `psycopg2-binary` solo en desarrollo o contenedores. Para producción, considera `psycopg2`.

---

## 📦 2. Crear contenedor PostgreSQL con Docker Compose

📄 `docker-compose.yml` – Añadir servicio `postgres`

```yaml
services:
  postgres:
    image: postgres:16
    container_name: churn_db
    environment:
      POSTGRES_USER: mario
      POSTGRES_PASSWORD: strongpassword
      POSTGRES_DB: churn_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - internal
    ports:
      - "5432:5432"

  fastapi:
    build: ./backend
    container_name: churn_backend
    depends_on:
      - postgres
    environment:
      - DATABASE_URL=postgresql://mario:strongpassword@postgres:5432/churn_db
    networks:
      - internal
    volumes:
      - ./backend:/app

volumes:
  postgres_data:

networks:
  internal:
    driver: bridge
```

---

## ⚙️ 3. Cambiar la conexión en `db.py`

📄 `backend/app/db.py`:

```python
import os
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = os.getenv("DATABASE_URL", "postgresql://mario:strongpassword@localhost:5432/churn_db")

engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(bind=engine, autocommit=False, autoflush=False)
Base = declarative_base()
```

> ✅ Asegúrate de que `DATABASE_URL` esté configurado como variable de entorno.

---

## 📄 4. Crear la base de datos al iniciar

📄 `main.py`:

```python
from app.db import Base, engine

Base.metadata.create_all(bind=engine)
```

Esto creará la tabla `predicciones` al iniciar el contenedor.

---

## 🧪 5. Probar conexión y migración

Ejecuta:

```bash
docker-compose up --build
```

Y revisa que en los logs de FastAPI veas algo como:

```
INFO:     Uvicorn running on http://0.0.0.0:8000
```

Y si accedes a `http://localhost:8000/predicciones/` devuelve un array vacío `[]`, entonces ¡ya estás usando PostgreSQL!

---

## 🗂️ 6. Opcional: inspeccionar desde tu host

Puedes conectarte a PostgreSQL desde tu máquina con herramientas como:

```bash
psql -h localhost -U mario -d churn_db
```

---

## ✅ Resultado

| Aspecto              | SQLite                 | PostgreSQL                 |
| -------------------- | ---------------------- | -------------------------- |
| Persistencia         | Archivos locales `.db` | Volumen Docker / servidor  |
| Concurrencia         | Limitada               | Alta y estable             |
| Escalabilidad        | Baja                   | Ideal para producción      |
| Compatibilidad cloud | Pobre                  | Total (AWS RDS, Supabase…) |

---

## 🛡️ Seguridad recomendada

* Usa variables de entorno para `DATABASE_URL` (ya está preparado).
* Para producción, activa conexiones seguras (SSL).
* Añade backups automáticos y logs.

---

Perfecto, Mario. Añadir **filtros y paginación al endpoint `/predicciones/`** es un paso esencial para optimizar el rendimiento y la experiencia del dashboard, especialmente cuando la base de datos crezca.

---

# 📥 Mejora del endpoint `/predicciones/` con filtros y paginación

---

## ✅ Objetivo

* Filtrar por:

  * Rango de edad
  * Rango de probabilidad de churn
* Añadir paginación:

  * `limit` (nº de registros por página)
  * `offset` (desde qué índice)
* Opcional: ordenamiento por columnas

---

## 🔧 Paso 1: Actualizar el endpoint en `routes.py`

📄 `backend/app/routes.py`

```python
from fastapi import APIRouter, Depends, Query
from sqlalchemy.orm import Session
from db import SessionLocal
from models_db import Prediccion

router = APIRouter()

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@router.get("/predicciones/")
def obtener_predicciones(
    db: Session = Depends(get_db),
    edad_min: int = Query(0, ge=0),
    edad_max: int = Query(120, le=120),
    churn_min: float = Query(0.0, ge=0.0, le=1.0),
    churn_max: float = Query(1.0, ge=0.0, le=1.0),
    limit: int = Query(100, ge=1, le=1000),
    offset: int = Query(0, ge=0)
):
    query = db.query(Prediccion).filter(
        Prediccion.edad >= edad_min,
        Prediccion.edad <= edad_max,
        Prediccion.churn_probabilidad >= churn_min,
        Prediccion.churn_probabilidad <= churn_max
    )

    total = query.count()
    resultados = query.offset(offset).limit(limit).all()

    return {
        "total": total,
        "limit": limit,
        "offset": offset,
        "resultados": [r.__dict__ for r in resultados]
    }
```

---

## 🔄 Paso 2: Actualizar el dashboard para consumir los filtros

📄 `dashboard/app.py`

```python
# Filtros en la barra lateral
st.sidebar.header("Filtros")
edad_min, edad_max = st.sidebar.slider("Edad", 18, 90, (18, 90))
churn_min, churn_max = st.sidebar.slider("Churn %", 0.0, 1.0, (0.0, 1.0))
limit = st.sidebar.selectbox("Resultados por página", [10, 25, 50, 100])
offset = st.sidebar.number_input("Offset", min_value=0, step=limit)

@st.cache_data(ttl=60)
def cargar_datos():
    params = {
        "edad_min": edad_min,
        "edad_max": edad_max,
        "churn_min": churn_min,
        "churn_max": churn_max,
        "limit": limit,
        "offset": offset
    }
    r = requests.get(f"{API_URL}/predicciones/", params=params)
    data = r.json()
    return pd.DataFrame(data["resultados"]), data["total"]

df, total = cargar_datos()

st.caption(f"Mostrando {len(df)} de {total} registros filtrados")
```

---

## 🧪 Ejemplo de uso en Postman o navegador

```
GET http://localhost:8000/predicciones/?edad_min=30&edad_max=50&churn_min=0.7&limit=25&offset=0
```

---

## 🧱 Resultado final

✔ Backend ahora permite:

* Filtros por rango de edad y churn
* Paginación con `limit` y `offset`
* Total de registros disponibles con los filtros

✔ Dashboard puede:

* Ofrecer navegación por páginas
* Mostrar métricas resumidas
* Mejorar rendimiento (consulta solo lo necesario)

---

## 🧩 Posibles mejoras futuras

* ✅ Ordenar por columnas (`?order_by=churn_probabilidad&desc=true`)
* 🔍 Buscar por cliente (si añades campo de nombre, email, etc.)
* 🚀 Endpoint `/predicciones/stats/` para métricas agregadas

---

Perfecto, Mario. Vamos a proteger tu **API FastAPI** con **OAuth2 y JWT (JSON Web Tokens)**, el estándar más usado en autenticación moderna para APIs.

Esto permite:

* Proteger endpoints sensibles.
* Autenticación sin sesiones.
* Soporte para autorización por roles (más adelante).

---

# 🔐 Seguridad en FastAPI con OAuth2 + JWT

---

## 🧱 Arquitectura general

```
[Cliente Front / Dashboard]
          │
     POST /login   →  recibe token JWT
          │
     GET /predicciones/ con Header:
     Authorization: Bearer <token>
```

---

## ✅ Paso 1: Instalar dependencias

📄 `backend/requirements.txt`

```txt
python-jose[cryptography]
passlib[bcrypt]
```

> `jose`: Para generar/verificar JWT
> `passlib`: Para hashear contraseñas

---

## 🧩 Paso 2: Estructura de seguridad

📄 `security.py`

```python
from datetime import datetime, timedelta
from jose import JWTError, jwt
from passlib.context import CryptContext
from fastapi import HTTPException, status, Depends
from fastapi.security import OAuth2PasswordBearer
from sqlalchemy.orm import Session
from db import SessionLocal

# Clave secreta (usa os.getenv en producción)
SECRET_KEY = "clave-super-secreta"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/login")

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def verificar_password(plain, hashed):
    return pwd_context.verify(plain, hashed)

def hashear_password(password):
    return pwd_context.hash(password)

def crear_token(datos: dict, expires_delta: timedelta = None):
    to_encode = datos.copy()
    expire = datetime.utcnow() + (expires_delta or timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES))
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

def obtener_usuario(db: Session, username: str):
    # Ficticio por ahora. Puedes conectar a tabla de usuarios.
    if username == "admin":
        return {"username": "admin", "hashed_password": hashear_password("1234"), "rol": "admin"}
    return None

def autenticar_usuario(db: Session, username: str, password: str):
    user = obtener_usuario(db, username)
    if not user or not verificar_password(password, user["hashed_password"]):
        return None
    return user

def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        if username is None:
            raise HTTPException(status_code=401)
        return {"username": username}
    except JWTError:
        raise HTTPException(status_code=401)
```

---

## 🔑 Paso 3: Endpoint `/login`

📄 `routes.py` o `auth.py`:

```python
from fastapi import APIRouter, Depends, Form
from fastapi.security import OAuth2PasswordRequestForm
from sqlalchemy.orm import Session
from security import autenticar_usuario, crear_token, get_current_user
from db import SessionLocal

router = APIRouter()

@router.post("/login")
def login(form_data: OAuth2PasswordRequestForm = Depends(), db: Session = Depends(SessionLocal)):
    user = autenticar_usuario(db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(status_code=400, detail="Credenciales inválidas")
    
    token = crear_token({"sub": user["username"]})
    return {"access_token": token, "token_type": "bearer"}
```

---

## 🛡️ Paso 4: Proteger endpoints con `Depends`

```python
@router.get("/predicciones/")
def obtener_predicciones(
    user=Depends(get_current_user),
    db: Session = Depends(get_db),
    ...
):
    # Solo usuarios autenticados acceden
    ...
```

---

## 🧪 Prueba básica con `curl` o Postman

```bash
# Obtener token
curl -X POST http://localhost:8000/login -d "username=admin&password=1234" -H "Content-Type: application/x-www-form-urlencoded"

# Usar token en GET
curl -H "Authorization: Bearer <token>" http://localhost:8000/predicciones/
```

---

## 📘 Documentación automática con `/docs`

FastAPI genera automáticamente:

* Formulario de login en `/docs`
* Uso de token para los endpoints protegidos

---

## 🔄 El dashboard Streamlit también puede usar token

En `dashboard/app.py`, puedes hacer:

```python
TOKEN = "eyJhbGciOi..."  # Obtenido desde login
headers = {"Authorization": f"Bearer {TOKEN}"}
r = requests.get(API_URL + "/predicciones/", headers=headers, params=params)
```

> O automatiza el login con `requests.post('/login', data={...})`

---

## ✅ Ventajas de este sistema

| Característica          | Valor                       |
| ----------------------- | --------------------------- |
| 🔐 Seguridad moderna    | JWT, OAuth2 estándar        |
| 🧩 Escalable            | Puedes añadir roles, scopes |
| 🔄 Integración sencilla | Compatible con frontend     |
| 📋 Documentado          | Swagger `/docs` lo detecta  |

---

¡Perfecto, Mario! Vamos a preparar el **despliegue completo del sistema**: backend FastAPI con PostgreSQL + dashboard Streamlit + autenticación, todo gestionado con Docker y Docker Compose, listo para producción.

---

# 🚀 Despliegue completo en servidor (VPS / Cloud)

---

## 🧱 Arquitectura final

```
          INTERNET
              │
        [ Reverse Proxy ]
              │
 ┌────────────┼────────────┐
 │                            │
 │     FastAPI (API)         │  ← Backend protegido con JWT
 │  + PostgreSQL (DB)        │
 │                            │
 │   Streamlit Dashboard      │  ← Usa la API vía HTTP
 └────────────────────────────┘
```

---

## ✅ Requisitos mínimos

* Un **VPS con Ubuntu (22.04 o superior)** o servicios como DigitalOcean, AWS EC2, Vultr, etc.
* **Docker** y **Docker Compose v2** instalados.
* Un dominio (opcional, recomendado).

---

## 📁 Estructura de proyecto recomendada

```
proyecto/
├── backend/
│   ├── app/
│   ├── requirements.txt
│   └── Dockerfile
├── dashboard/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── nginx/
│   └── default.conf
├── docker-compose.yml
└── .env
```

---

## 🔐 Paso 1: Reverse proxy con Nginx + HTTPS

📄 `nginx/default.conf`:

```nginx
server {
    listen 80;
    server_name tu-dominio.com;

    location /api/ {
        proxy_pass http://fastapi:8000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location / {
        proxy_pass http://streamlit:8501/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 📦 Paso 2: `docker-compose.yml` de producción

```yaml
version: '3.9'

services:
  postgres:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_USER: mario
      POSTGRES_PASSWORD: strongpassword
      POSTGRES_DB: churn_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - internal

  fastapi:
    build: ./backend
    container_name: churn_backend
    environment:
      - DATABASE_URL=postgresql://mario:strongpassword@postgres:5432/churn_db
    depends_on:
      - postgres
    networks:
      - internal

  streamlit:
    build: ./dashboard
    container_name: churn_dashboard
    environment:
      - API_URL=http://fastapi:8000
    depends_on:
      - fastapi
    networks:
      - internal

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - fastapi
      - streamlit
    networks:
      - internal

volumes:
  postgres_data:

networks:
  internal:
    driver: bridge
```

---

## 🌐 Paso 3: (opcional) Añadir HTTPS con Let's Encrypt (recomendado)

Usa [Caddy](https://caddyserver.com) o [nginx-proxy + Let's Encrypt Companion](https://github.com/nginx-proxy/acme-companion) para generación automática de certificados.

O bien instala Certbot manualmente:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d tu-dominio.com
```

---

## 🧪 Paso 4: Desplegar en servidor

1. Copia el proyecto completo al VPS:

```bash
scp -r proyecto/ mario@tu-vps:/home/mario/
```

2. Entra al VPS y ejecuta:

```bash
cd proyecto/
docker-compose up --build -d
```

3. Accede:

* Dashboard: `http://tu-dominio.com/`
* API docs: `http://tu-dominio.com/api/docs`

---

## 🧼 Paso 5: Mantenimiento y seguridad

* ✅ Crea usuarios reales y controla roles.
* 🔐 Cifra tokens y contraseñas (usa `.env` para la SECRET\_KEY).
* 🔄 Añade backup automático de base de datos.
* 📈 Monitoriza con herramientas como UptimeRobot o Grafana.
* 🔄 Usa `watchtower` para actualizaciones automáticas de contenedores si quieres CI/CD simple.

---

## ✅ Resultado: sistema de producción completo

| Componente         | Estado                             |
| ------------------ | ---------------------------------- |
| 🔧 Backend FastAPI | ✅ JWT + PostgreSQL                 |
| 📊 Dashboard UI    | ✅ Protegido por rol + API REST     |
| 🧱 Infraestructura | ✅ Dockerizada, modular y escalable |
| 🌐 Despliegue Web  | ✅ Vía Nginx con opción de HTTPS    |

---

















