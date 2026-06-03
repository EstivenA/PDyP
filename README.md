# 🐷 Granja App

Aplicación web desarrollada con **Spring Boot** para la gestión integral de una granja porcina. Permite administrar clientes, sus cerdos y los planes de alimentación, con dos roles de usuario diferenciados y generación de informes en PDF y XML.

---

## 📋 Descripción

Granja App nació de la necesidad de una organización agrícola de centralizar su información: clientes registrados, inventario de porcinos y sus respectivos regímenes de alimentación. Todo desde una interfaz web sencilla, sin depender de hojas de cálculo ni archivos dispersos.

---

## ✨ Funcionalidades

### 👤 Roles
| Rol | Acceso |
|---|---|
| `ADMIN` | Gestión completa de clientes, porcinos y alimentaciones. Generación de informes PDF. |
| `CLIENTE` | Vista y gestión de sus propios porcinos y alimentaciones. |

### 🛠️ Módulos principales

- **Autenticación manual** — Login y registro con validación de credenciales por base de datos y redirección según rol.
- **Gestión de Clientes** (Admin) — CRUD completo: listar, crear, editar y eliminar clientes.
- **Gestión de Porcinos** — Registro de cerdos con identificación única, raza, edad, peso, dueño y plan de alimentación asignado.
- **Gestión de Alimentaciones** — Planes de alimentación con dosis y descripción, reutilizables entre porcinos.
- **Informe PDF** — El administrador puede descargar un informe completo de todos los clientes con sus porcinos y alimentaciones, generado con **iTextPDF**.
- **Informe XML** — Exportación de datos de clientes, porcinos y alimentaciones en formato XML estructurado.

---

## 🏗️ Arquitectura

```
granja-app/
├── src/main/java/com/granja/granja_app/
│   ├── config/
│   │   └── SecurityConfig.java        # Configuración de Spring Security
│   ├── Controllers/
│   │   ├── AdminHomeC.java            # Rutas /admin/** — gestión completa
│   │   ├── ClienteHomeC.java          # Rutas /cliente/** — vista del cliente
│   │   ├── AuthController.java        # Rutas /auth/login y /auth/register
│   │   ├── AlimentacionController.java
│   │   ├── ClienteController.java
│   │   └── PorcinoController.java
│   ├── model/
│   │   ├── Cliente.java               # Entidad cliente (también actúa como usuario)
│   │   ├── Porcino.java               # Entidad cerdo, ligado a cliente y alimentación
│   │   └── Alimentacion.java          # Entidad plan de alimentación
│   ├── Repository/
│   │   ├── ClienteRepository.java
│   │   ├── PorcinoRepository.java
│   │   └── AlimentacionRepository.java
│   └── GranjaAppApplication.java
├── src/main/resources/
│   ├── templates/                     # Vistas Thymeleaf (.html)
│   ├── static/img/                    # Recursos estáticos
│   └── application.properties
└── pom.xml
```

---

## 🗄️ Modelo de datos

```
Cliente ──< Porcino >── Alimentacion
```

- Un **Cliente** puede tener muchos **Porcinos** (`@OneToMany`).
- Cada **Porcino** pertenece a un **Cliente** y tiene asignada una **Alimentacion** (`@ManyToOne`).
- Una **Alimentacion** puede aplicarse a muchos **Porcinos** (`@OneToMany`).

---

## 🧰 Tecnologías utilizadas

| Capa | Tecnología |
|---|---|
| Backend | Java 17, Spring Boot 3.5.5 |
| Persistencia | Spring Data JPA, Hibernate, MySQL 8 |
| Vistas | Thymeleaf + thymeleaf-extras-springsecurity6 |
| Seguridad | Spring Security (configuración manual) |
| Generación PDF | iTextPDF 5.5.13 |
| Build | Maven (Maven Wrapper incluido) |

---

## ⚙️ Configuración y ejecución

### Prerrequisitos

- Java 17+
- Maven 3.8+ (o usar `./mvnw` incluido)
- MySQL 8 corriendo localmente

### 1. Crear la base de datos

```sql
CREATE DATABASE granja;
```

> La aplicación tiene `spring.jpa.hibernate.ddl-auto=none`, por lo que las tablas deben crearse manualmente o con un script SQL.

### 2. Configurar credenciales

Edita `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/granja?useSSL=false&serverTimezone=UTC
spring.datasource.username=TU_USUARIO
spring.datasource.password=TU_CONTRASEÑA
```

### 3. Ejecutar

```bash
./mvnw spring-boot:run
```

La aplicación queda disponible en `http://localhost:8080`.

---

## 🔐 Rutas principales

| Ruta | Descripción |
|---|---|
| `/auth/login` | Inicio de sesión |
| `/auth/register` | Registro de nuevo cliente |
| `/admin/home` | Panel de administrador |
| `/admin/clientes` | Listado y gestión de clientes |
| `/admin/porcinos` | Listado y gestión de porcinos |
| `/admin/alimentacion` | Listado y gestión de alimentaciones |
| `/admin/clientes/informe` | Descarga informe PDF |
| `/cliente/home/{id}` | Panel del cliente |
| `/cliente/{id}/porcinos` | Porcinos del cliente |
| `/cliente/{id}/alimentaciones` | Alimentaciones del cliente |

---

## 📄 Informes

### PDF
El administrador puede descargar desde `/admin/clientes/informe` un documento PDF con el listado completo de clientes, sus porcinos y la alimentación asignada a cada uno.

### XML
Se puede exportar la información de clientes, porcinos y alimentaciones en un archivo XML estructurado, útil para integraciones externas o respaldos.

---

## 🔒 Notas de seguridad

> Este proyecto fue desarrollado con fines académicos. Para un entorno de producción se recomienda:
> - Activar Spring Security con sesiones o JWT.
> - Encriptar las contraseñas con `BCryptPasswordEncoder`.
> - No deshabilitar CSRF en formularios HTML.
> - Mover credenciales de BD a variables de entorno.

---

## 👨‍💻 Autor

Jefferson Estiven Aristizabal
