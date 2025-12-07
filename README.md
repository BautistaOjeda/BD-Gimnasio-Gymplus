# Base de Datos Gimnasio – GymPlus (SQLite)

Este repositorio contiene el diseño y las consultas de práctica de una base de datos para el gimnasio ficticio **GymPlus**, que decide dejar de usar planillas y cuadernos para gestionar socios, cuotas y actividades, y pasar a un sistema relacional ordenado y consultable.

---

## 📂 Contenido del repositorio

- `BD_GIMNASIO.db`  
  Base de datos en SQLite lista para usar.

- `Consultas y Subconsultas BD GIMNASIO.txt`  
  Archivo con ejercicios de consulta:  
  - 10 consultas a una sola tabla  
  - 10 consultas con múltiples tablas  
  - 5 subconsultas

- `Problematica GymPlus.docx`  
  Descripción funcional del problema (necesidades del gimnasio).

- `ModeloRelacional_BD.docx`  
  Modelo relacional de tablas, campos y relaciones.

- `Mod-Ent-Rel_BD.png`  
  Diagrama Entidad–Relación.

---

## 🗄️ Modelo de datos

El modelo contiene las siguientes entidades principales:

- **SOCIO**: datos personales de los socios del gimnasio.
- **PLAN**: planes disponibles, cuotas y duración.
- **METODO_PAGO**: formas de pago y recargos.
- **PAGO_CUOTA**: pagos registrados (periodo, monto, estado).
- **ACTIVIDAD**: actividades del gimnasio (cupo y nivel).
- **SOCIO_ACTIVIDAD**: inscripciones a actividades.

Las relaciones permiten saber:

- Qué plan tiene cada socio.
- Cómo pagó y cuánto.
- En qué actividades está inscripto.
- Recaudación por plan, socio, método, etc.

---

## 📌 Temas SQL trabajados

- SELECT / DISTINCT  
- WHERE con filtros, BETWEEN, IN, NOT IN  
- ORDER BY (ASC, DESC)  
- LIMIT  
- JOINs (INNER y LEFT)  
- GROUP BY / HAVING  
- Funciones agregadas: COUNT, SUM, AVG, MAX  
- Subconsultas

---

## 🎯 Objetivo

- Practicar consultas SQL reales en SQLite.
- Reforzar diseño de bases relacionales.
- Tener un proyecto concreto para portfolio.

---

## ▶️ Cómo usar el proyecto

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/BautistaOjeda/BD-Gimnasio-Gymplus.git

2. Abrir la base con SQLite o DB Browser for SQLite:
   ```bash
   sqlite3 BD_GIMNASIO.db

## 🛠 Tecnologías utilizadas

- SQLite
- Modelado entidad–relación (DER)
- Git y GitHub para versionado y publicación
