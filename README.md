# Demo Spring Boot + H2 (file) + H2 Console

> **Bilingual README / README bilingüe**  
> English first · Español más abajo

---

<img width="1919" height="40" alt="Captura de pantalla 2025-08-30 072608" src="https://github.com/user-attachments/assets/00cb3b64-b75c-4d15-a030-d15fb5817f0f" />
<img width="1914" height="1000" alt="Captura de pantalla 2025-08-30 072559" src="https://github.com/user-attachments/assets/1fc35c0e-7f3b-4c0f-8305-682c788d08d3" />

## Overview

Minimal project with **Spring Boot 3.5.x**, **JPA/Hibernate**, and **H2** in **file** mode.  
Includes the H2 Web Console and an optional sample REST endpoint to verify data.

Works on Windows/PowerShell, Git Bash, Linux and macOS.

### Tech Stack
- Java 17+  
- Spring Boot 3.5.x  
- Spring Data JPA  
- H2 Database (file mode)  
- Maven

### Project Layout
```
src/
 └─ main/
     ├─ java/com/example/demo/
     │   ├─ DemoApplication.java
     │   ├─ Country.java                 # optional example
     │   ├─ CountryRepository.java       # optional example
     │   └─ CountryController.java       # optional example
     └─ resources/
         ├─ application.properties
         ├─ schema.sql                   # if you don’t use ddl-auto
         └─ data.sql
```

---

## ⚙️ Configuration (English)

`src/main/resources/application.properties`
```properties
spring.application.name=demo

# H2 in file mode (DB files under ./data)
spring.datasource.url=jdbc:h2:file:./data/demo
spring.datasource.username=sa
spring.datasource.password=password

# Dev schema strategy (pick ONE)
spring.jpa.hibernate.ddl-auto=create
spring.jpa.defer-datasource-initialization=true

# H2 Web Console
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# (Optional) app port
# server.port=8081
```

If you prefer managing the schema yourself, remove `ddl-auto` and use:

`schema.sql`
```sql
CREATE TABLE countries (
  id   BIGINT PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);
```

`data.sql`
```sql
INSERT INTO countries (id, name) VALUES (1, 'USA');
INSERT INTO countries (id, name) VALUES (2, 'France');
INSERT INTO countries (id, name) VALUES (3, 'Brazil');
INSERT INTO countries (id, name) VALUES (4, 'Italy');
INSERT INTO countries (id, name) VALUES (5, 'Canada');
```

---

## ▶️ Run (English)

### Maven
```bash
# Default port (8080)
mvn clean spring-boot:run
```

Run on another port:

- **PowerShell (Windows)**
```powershell
mvn '-Dspring-boot.run.arguments=--server.port=8081' spring-boot:run
```

- **Git Bash / Linux / macOS**
```bash
mvn -Dspring-boot.run.arguments="--server.port=8081" spring-boot:run
```

### IntelliJ IDEA
- Open the project → **Run** `DemoApplication`.  
- Port: Run/Debug Configuration → *Program arguments*: `--server.port=8081`
  (or *VM options*: `-Dserver.port=8081`).

---

## 🗃️ H2 Console (English)

- URL: `http://localhost:<port>/h2-console` (e.g., `http://localhost:8081/h2-console`)  
- **JDBC URL**: `jdbc:h2:file:./data/demo`  
- **User**: `sa`  
- **Password**: `password` *(or blank, if that’s how you set it)*

Database files live in `./data/demo.*` inside the project.

---

## 🌐 Sample REST Endpoint (English, optional)

`Country.java`
```java
@Entity @Table(name = "countries")
public class Country {
  @Id private Long id;
  private String name;
  public Country() {}
  public Country(Long id, String name) { this.id = id; this.name = name; }
  // getters & setters
}
```

`CountryRepository.java`
```java
public interface CountryRepository extends JpaRepository<Country, Long> {}
```

`CountryController.java`
```java
@RestController
@RequestMapping("/api/countries")
public class CountryController {
  private final CountryRepository repo;
  public CountryController(CountryRepository repo) { this.repo = repo; }
  @GetMapping public List<Country> all() { return repo.findAll(); }
}
```

Quick test:
```bash
curl http://localhost:8081/api/countries
```

---

## 🛠️ Troubleshooting (English)

**“Web server failed to start. Port 8080 was already in use.”**

Free port 8080 (PowerShell):
```powershell
Get-NetTCPConnection -LocalPort 8080 -State Listen | Select OwningProcess,LocalAddress,LocalPort,State
$pid = (Get-NetTCPConnection -LocalPort 8080 -State Listen).OwningProcess
Get-Process -Id $pid
Stop-Process -Id $pid -Force   # only if it’s safe to kill it
```
Or just run on another port (see “Run” section).

**Java versions mismatch**  
Align the JDK used by Maven and IntelliJ (Settings → Build Tools → Maven → JDK) or set `JAVA_HOME`.

**Security**  
H2 and `ddl-auto=create` are **for development only**. Do not expose `/h2-console` publicly.

---

## ⬆️ Git Quick Start (English)

Push to an existing GitHub repo (replace with your repo URL if needed):
```bash
git remote set-url origin https://github.com/AlejoCNYT/Codelab-Persistencia-Memoria-H2.git
git pull --rebase origin main
git push -u origin main
```

Ignore H2 database files:
```
data/
*.mv.db
*.trace.db
```

---

# 🇪🇸 Descripción

Proyecto mínimo con **Spring Boot 3.5.x**, **JPA/Hibernate** y **H2** en modo **archivo**.  
Incluye la consola web de H2 y un endpoint REST opcional para verificar datos.

Funciona en Windows/PowerShell, Git Bash, Linux y macOS.

### Tecnologías
- Java 17+  
- Spring Boot 3.5.x  
- Spring Data JPA  
- H2 Database (modo file)  
- Maven

### Estructura
```
src/
 └─ main/
     ├─ java/com/example/demo/
     │   ├─ DemoApplication.java
     │   ├─ Country.java                 # ejemplo opcional
     │   ├─ CountryRepository.java       # ejemplo opcional
     │   └─ CountryController.java       # ejemplo opcional
     └─ resources/
         ├─ application.properties
         ├─ schema.sql                   # si no usas ddl-auto
         └─ data.sql
```

---

## ⚙️ Configuración (Español)

`src/main/resources/application.properties`
```properties
spring.application.name=demo

# H2 en modo archivo (BD en ./data dentro del proyecto)
spring.datasource.url=jdbc:h2:file:./data/demo
spring.datasource.username=sa
spring.datasource.password=password

# Estrategia de esquema en desarrollo (elige UNA)
spring.jpa.hibernate.ddl-auto=create
spring.jpa.defer-datasource-initialization=true

# Consola H2
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# (Opcional) puerto de la app
# server.port=8081
```

Si prefieres manejar el esquema a mano, elimina `ddl-auto` y usa:

`schema.sql`
```sql
CREATE TABLE countries (
  id   BIGINT PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);
```

`data.sql`
```sql
INSERT INTO countries (id, name) VALUES (1, 'USA');
INSERT INTO countries (id, name) VALUES (2, 'France');
INSERT INTO countries (id, name) VALUES (3, 'Brazil');
INSERT INTO countries (id, name) VALUES (4, 'Italy');
INSERT INTO countries (id, name) VALUES (5, 'Canada');
```

---

## ▶️ Ejecución (Español)

### Maven
```bash
# Puerto por defecto (8080)
mvn clean spring-boot:run
```

Cambiar puerto:

- **PowerShell (Windows)**
```powershell
mvn '-Dspring-boot.run.arguments=--server.port=8081' spring-boot:run
```

- **Git Bash / Linux / macOS**
```bash
mvn -Dspring-boot.run.arguments="--server.port=8081" spring-boot:run
```

### IntelliJ IDEA
- Abre el proyecto → **Run** `DemoApplication`.  
- Puerto: Run/Debug Configuration → *Program arguments*: `--server.port=8081`
  (o *VM options*: `-Dserver.port=8081`).

---

## 🗃️ H2 Console (Español)

- URL: `http://localhost:<puerto>/h2-console` (p.ej., `http://localhost:8081/h2-console`)  
- **JDBC URL**: `jdbc:h2:file:./data/demo`  
- **User**: `sa`  
- **Password**: `password` *(o en blanco, si así lo definiste)*

La BD física queda en `./data/demo.*` dentro del proyecto.

---

## 🌐 Endpoint de ejemplo (Español, opcional)

`Country.java`
```java
@Entity @Table(name = "countries")
public class Country {
  @Id private Long id;
  private String name;
  public Country() {}
  public Country(Long id, String name) { this.id = id; this.name = name; }
  // getters y setters
}
```

`CountryRepository.java`
```java
public interface CountryRepository extends JpaRepository<Country, Long> {}
```

`CountryController.java`
```java
@RestController
@RequestMapping("/api/countries")
public class CountryController {
  private final CountryRepository repo;
  public CountryController(CountryRepository repo) { this.repo = repo; }
  @GetMapping public List<Country> all() { return repo.findAll(); }
}
```

Prueba rápida:
```bash
curl http://localhost:8081/api/countries
```

---

## 🛠️ Solución de problemas (Español)

**“Web server failed to start. Port 8080 was already in use.”**

Liberar el 8080 (PowerShell):
```powershell
Get-NetTCPConnection -LocalPort 8080 -State Listen | Select OwningProcess,LocalAddress,LocalPort,State
$pid = (Get-NetTCPConnection -LocalPort 8080 -State Listen).OwningProcess
Get-Process -Id $pid
Stop-Process -Id $pid -Force   # solo si es seguro
```
O ejecuta en otro puerto (ver “Ejecución”).

**Versiones de Java mezcladas**  
Alinea el JDK de Maven e IntelliJ (Settings → Build Tools → Maven → JDK) o ajusta `JAVA_HOME`.

**Seguridad**  
H2 y `ddl-auto=create` son para **desarrollo**. No expongas `/h2-console` públicamente.

---

## ⬆️ Guía rápida Git (Español)

Empujar a un repo existente en GitHub:
```bash
git remote set-url origin https://github.com/AlejoCNYT/Codelab-Persistencia-Memoria-H2.git
git pull --rebase origin main
git push -u origin main
```

Ignora archivos de la BD H2:
```
data/
*.mv.db
*.trace.db
```

---

## 📄 License / Licencia
MIT (or your preferred license / o la que prefieras).

