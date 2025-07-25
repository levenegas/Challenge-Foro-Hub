# 🚀 FORUMS API

API REST para un sistema de **FOROS** con autenticación **JWT**, roles de usuario y gestión de artículos.

---

## 📝 DESCRIPCIÓN

Proyecto backend en **Java 17** con **Spring Boot 3.5.3** que permite:

- Gestión de usuarios y artículos
- Autenticación y autorización mediante **JWT**
- Roles: `USER` y `ADMIN`
- Seguridad con **Spring Security**
- Persistencia en **PostgreSQL** con **Spring Data JPA**

---

## 📂 ESTRUCTURA DEL PROYECTO
├── config
│   ├── JacksonConfig.java
│   ├── SecurityConfig.java
├── controllers
│   ├── ArticuloController.java
│   ├── AuthController.java
│   ├── UsuarioController.java
├── DTOs
│   ├── ArticuloRequest.java
│   ├── ArticuloUpdateRequest.java
├── exceptions
│   ├── CustomExceptions.java
│   ├── GlobalExceptionHandler.java
├── profiles
│   ├── Articulo.java
│   ├── Rol.java
│   ├── Usuario.java
├── repository
│   ├── ArticuloRepository.java
│   ├── UsuarioRepository.java
├── security
│   ├── CustomUserDetails.java
│   ├── CustomUserDetailsService.java
│   ├── JwtAuthenticationFilter.java
│   ├── JwtUtil.java
├── services
│   ├── ArticuloService.java
│   ├── SecurityConfig.java
│   ├── UsuarioService.java
├── ForumsApplication.java
├── Readme


---

## 🛠 TECNOLOGÍAS USADAS

- **Java 17**
- **Spring Boot 3.5.3**
- **Spring Security** (con BCrypt y JWT)
- **JWT (jjwt)**
- **Spring Data JPA**
- **PostgreSQL**
- **Hibernate Validator**
- **Lombok**
- **Maven**

---

## 🔑 FUNCIONALIDADES PRINCIPALES

- Registro y gestión de usuarios con roles (`USER`, `ADMIN`)
- Autenticación vía JWT con generación y validación de tokens
- Control de acceso basado en roles
- CRUD de artículos con control de propiedad (solo autor puede modificar/eliminar)
- Manejo centralizado de errores con respuestas HTTP adecuadas

---

## 🚦 ENDPOINTS PRINCIPALES

| Método | Ruta                      | Descripción                          | Roles permitidos        |
|--------|---------------------------|--------------------------------------|-------------------------|
| `POST` | `/api/login`              | Login y obtención de token JWT       | Público                 |
| `GET`  | `/api/usuarios`           | Listar todos los usuarios            | ADMIN                   |
| `POST` | `/api/usuarios`           | Crear nuevo usuario                  | Público                 |
| `GET`  | `/api/usuarios/{username}`| Obtener usuario por username         | Público                 |
| `GET`  | `/api/usuarios/me`        | Obtener usuario autenticado          | Usuario autenticado     |
| `GET`  | `/api/articulos`          | Listar artículos (propios o todos)   | Usuario autenticado     |
| `POST` | `/api/articulos`          | Crear artículo                       | Usuario autenticado     |
| `PUT`  | `/api/articulos/{id}`     | Actualizar artículo (solo autor)     | Autor del artículo      |
| `DELETE`| `/api/articulos/{id}`    | Eliminar artículo (solo autor)       | Autor del artículo      |

---

## ⚙️ CONFIGURACIÓN REQUERIDA

1. **Base de datos PostgreSQL**  
   Crear base de datos y usuario con permisos.

2. **Archivo `application.properties` o `application.yml`**  
   Configurar conexión y JWT secret:

# Nombre de la aplicación
spring.application.name=forums

# Configuración de la conexión a PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/forums
spring.datasource.username=postgres
spring.datasource.password=postgres123
spring.datasource.driver-class-name=org.postgresql.Driver

# Configuración JPA / Hibernate
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Flyway migraciones
spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration

# Clave secreta JWT codificada en Base64
jwt.secret=ZXlKaGJHY2lPaUpJVXpJMU5pSXNJblI1Y0NJNklrcFhWQ0o5LmV5SnpkV0lpT2lJeE1qTTBOVFkzT0Rrd0lpd2libUZ0WlNJNklrcHZhRzRnUkc5bElpd2lZV1J0YVc0aU9uUnlkV1VzSW1saGRDSTZNVGMxTXpJeE9EQXpNbjAuX1kxNzlpaFN3ZTdXbGNpdHJmQmxGbC1QZ3NWMi1fYnVXcHR0NnNCMDVkMA==

## 🗄️ MIGRACIONES FLYWAY SQL
V1__init_schema.sql
-- Crear tabla de usuarios
CREATE TABLE usuario (
    id BIGSERIAL PRIMARY KEY,
    username VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);

-- Crear tabla de artículos
CREATE TABLE articulo (
    id BIGSERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL,
    author_id BIGINT,
    CONSTRAINT fk_articulo_usuario FOREIGN KEY (author_id) REFERENCES usuario (id)
);

V2__add_rol.sql
-- Agregar columna 'rol' a tabla usuario con valor por defecto 'USER'
ALTER TABLE usuario
ADD COLUMN rol VARCHAR(10) NOT NULL DEFAULT 'USER';
V3__add_usuario_id_to_articulo.sql

V3__add_usuario_id_to_articulo.sql
(Si en V1 ya está la columna author_id, esta migración puede ser para reforzar la clave foránea o para otras modificaciones, ejemplo:)
-- Asegurar constraint de clave foránea en articulo
ALTER TABLE articulo
ADD CONSTRAINT fk_articulo_usuario FOREIGN KEY (author_id) REFERENCES usuario (id);

