# SQL — Motor de filtrado de propiedades por requisitos de cliente

Caso práctico de SQL: filtrado multicondicional y análisis sobre una cartera de alojamientos.

## Objetivo

En este caso práctico, una agencia inmobiliaria ficticia recibe una lista de 12 requisitos específicos de un cliente (terraza, aire acondicionado, superficie mínima, distancia a metro y centro, rango de precio, porcentaje de reserva, puntuación mínima de la propiedad y de la agencia) y necesita identificar, mediante SQL, qué propiedades de su cartera cumplen **todas** las condiciones simultáneamente — y justificar cuál es la mejor recomendación final.

## Estructura de datos

4 tablas relacionadas por `CODIGO_ALOJAMIENTO`, cargadas desde ficheros Excel a una base de datos MySQL local (los ficheros no se incluyen en el repositorio):

| Tabla | Contenido |
|---|---|
| `ALOJAMIENTO` | Características físicas: superficie, terraza, aire acondicionado, piscina, salón, nº de baños |
| `UBICACION` | País, distancia a estación de metro, distancia al centro |
| `PRECIO` | Precio de alquiler por noche, porcentaje de reserva |
| `PUNTUACION` | Puntuación de confianza de la propiedad y de la agencia gestora |

## Metodología

**Parte 1 — Filtrado incremental:** una query independiente por cada uno de los 7 bloques de requisitos del cliente, para validar el efecto de cada filtro por separado antes de combinarlos.

**Parte 2 — Query combinada:** un único `JOIN` entre las 4 tablas aplicando los 12 filtros simultáneamente, para obtener la lista final de propiedades candidatas.

**Parte 3 — Preguntas de negocio adicionales:** 7 consultas analíticas independientes (agregaciones, `GROUP BY`, funciones de agregación) para responder preguntas de negocio sobre el conjunto completo de propiedades.

## Resultados clave

**Filtrado del cliente (JOIN de 4 tablas, 12 condiciones):**
- **4 propiedades finalistas** cumplen la totalidad de los requisitos.
- Recomendación final: **ALOJ015 – Dúplex Las Rocas** — mayor superficie de las finalistas (120 m²), puntuación de propiedad de 4,9 y puntuación de agencia de 4,7, dentro del rango de precio solicitado (1.500–2.000 €/noche).

**Preguntas de negocio (sobre el total de la cartera):**
- Superficie total de alojamientos con piscina: **850 m²**
- Alojamiento con mayor superficie: **Chalet Jardín Secreto (180 m²)** · con menor superficie: **Estudio Urbano (35 m²)**
- **16 alojamientos** tienen salón y **14** tienen terraza
- Distribución por país: **España (8)**, Portugal (4), Francia (4), Italia (4)
- Precio medio de alquiler de toda la cartera: **1.568,40 €**
- 4 agencias empatadas en la puntuación más alta (4,9): TerraHouse, UYAlquila, MalagaTurismo, VivaStay

## Stack técnico

SQL (SQLAlchemy sobre motor relacional) · JOIN entre múltiples tablas · Filtrado multicondicional · Funciones de agregación (`SUM`, `AVG`, `MAX`, `MIN`, `COUNT`, `GROUP BY`) · Python (carga de datos desde Excel a SQL)

## Cómo ejecutarlo

1. Crear una base de datos MySQL local llamada `property_matching`.
2. Definir la variable de entorno `MYSQL_PASSWORD` con la contraseña del usuario `root`.
3. Colocar los 4 ficheros `.xlsx` en una carpeta `data/` junto al notebook.
4. Ejecutar el notebook de arriba abajo.

## Autor

Luis Felipe Moran — [LinkedIn](https://www.linkedin.com/in/iamluismoran/)
