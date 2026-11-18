# Pipeline de Datos y Capa de Persistencia con SQLAlchemy ORM

Este proyecto implementa un flujo de trabajo estructurado para la gestión y persistencia de datos relacionales en Python utilizando **SQLAlchemy 2.0**, diseñado específicamente para entornos interactivos como Google Colab.

## 🏗️ Patrón de Datos Utilizado

El proyecto implementa el **Patrón de Mapeo Objeto-Relacional (ORM)** utilizando la arquitectura declarativa moderna de SQLAlchemy 2.0+. 

El modelo de datos relacional se compone de dos entidades principales conectadas mediante integridad referencial:

1. **Modelo `User` (Clientes):** Almacena la información de los usuarios (`id`, `name`, `email`, `average_spend`) y establece una relación de uno a muchos (`relationship`) con las transacciones.
2. **Modelo `Purchase` (Compras):** Almacena las transacciones asociadas (`id`, `amount`, `date`) utilizando una clave foránea (`ForeignKey`) que referencia directamente al cliente correspondiente.

---

## ⚙️ Instrucciones de Configuración del Entorno y Ejecutables

A continuación se detallan los pasos e instrucciones ejecutables para configurar el entorno y desplegar el flujo de datos:

### 1. Instalación de Dependencias
Ejecuta la instrucción para instalar de forma silenciosa las librerías requeridas (SQLAlchemy 2.x y Pydantic):
```python
!pip -q install "SQLAlchemy>=2,<3" pydantic
```

### 2. Importación de Módulos Core y Tipados
Importa los componentes necesarios para tipado, tipos de datos SQL y la gestión de sesiones:
```python
from typing import Optional
from sqlalchemy import String, Float, create_engine, select
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, Session
```

### 3. Creación del Motor de Base de Datos (*Engine*)
Inicializa el punto de contacto central conectando con el archivo local SQLite:
```python
engine = create_engine("sqlite:///clase_orm.db")
print("Engine creado:", engine)
```

### 4. Definición de la Base Declarativa
Establece la clase base central para el registro de metadatos de los modelos ORM:
```python
class Base(DeclarativeBase):
    pass
```

### 5. Consultas, Filtros y Modificaciones
Utiliza las sentencias modernas con `select()` y `Session` para consultar, filtrar por condiciones (`.where()`) y actualizar los objetos de manera transaccional mediante `session.commit()`.
```python
with Session(engine) as session:
    stmt = select(User).where(User.average_spend > 1000)
    users = session.scalars(stmt).all()

for user in users:
    print(user)
```
