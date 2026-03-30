# 🛒 SpringMarket — Marketplace de Productos

> **Módulo 6 – Desarrollo de aplicaciones JEE con Spring Framework**  
> Aplicación web estilo Mercado Libre donde usuarios pueden publicar y comprar productos.

---

## 📌 Descripción del Proyecto

**SpringMarket** es un marketplace de productos construido con Spring Boot.  
Los compradores pueden explorar el catálogo y realizar órdenes de compra.  
Los vendedores (ADMIN) pueden publicar, editar y eliminar productos.  
Todo el sistema expone APIs REST para integrarse con clientes externos.

---

## 🛠️ Tecnologías Utilizadas

| Tecnología | Versión | Rol |
|---|---|---|
| Java | 17 | Lenguaje base |
| Spring Boot | 3.2.5 | Framework principal |
| Spring MVC | 6.x | Capa web (patrón MVC) |
| Spring Data JPA | 3.2.x | Acceso a datos |
| Spring Security | 6.x | Autenticación y autorización |
| Thymeleaf | 3.x | Motor de plantillas HTML |
| H2 Database | 2.x | Base de datos embebida |
| Maven | 3.9.x | Gestor de dependencias |
| Lombok | 1.18.x | Reducción de boilerplate |

---

## ⚙️ Configuración en start.spring.io

Ingresá a **https://start.spring.io** y configurá exactamente así:

```
Project:      Maven Project
Language:     Java
Spring Boot:  3.2.5

Project Metadata:
  Group:        com.alkemy
  Artifact:     springmarket
  Name:         SpringMarket
  Description:  Marketplace de productos con Spring Boot
  Package name: com.alkemy.springmarket
  Packaging:    Jar
  Java:         17

Dependencies — agregar estas 8:
  ✅ Spring Web
  ✅ Spring Data JPA
  ✅ Spring Security
  ✅ H2 Database
  ✅ Thymeleaf
  ✅ Spring Boot DevTools
  ✅ Lombok
  ✅ Validation
```

Hacé clic en **GENERATE** → descomprimí el ZIP → abrí en tu IDE.

---

## 📁 Estructura del Proyecto

```
springmarket/
├── src/
│   ├── main/
│   │   ├── java/com/alkemy/springmarket/
│   │   │   ├── SpringMarketApplication.java
│   │   │   ├── config/
│   │   │   │   └── SecurityConfig.java
│   │   │   ├── controller/
│   │   │   │   ├── HomeController.java
│   │   │   │   ├── ProductoController.java
│   │   │   │   ├── OrdenController.java
│   │   │   │   ├── LoginController.java
│   │   │   │   └── rest/
│   │   │   │       ├── ProductoRestController.java
│   │   │   │       └── OrdenRestController.java
│   │   │   ├── model/
│   │   │   │   ├── Producto.java
│   │   │   │   ├── Usuario.java
│   │   │   │   └── Orden.java
│   │   │   ├── repository/
│   │   │   │   ├── ProductoRepository.java
│   │   │   │   ├── UsuarioRepository.java
│   │   │   │   └── OrdenRepository.java
│   │   │   └── service/
│   │   │       ├── ProductoService.java
│   │   │       ├── UsuarioService.java
│   │   │       └── OrdenService.java
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── data.sql
│   │       └── templates/
│   │           ├── index.html
│   │           ├── login.html
│   │           ├── productos/
│   │           │   ├── lista.html
│   │           │   └── formulario.html
│   │           └── ordenes/
│   │               └── mis-ordenes.html
└── pom.xml
```

---

## 📦 LECCIÓN 1 — El Gestor de Proyectos (Maven)

### pom.xml completo

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
        <relativePath/>
    </parent>

    <groupId>com.alkemy</groupId>
    <artifactId>springmarket</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>SpringMarket</name>
    <description>Marketplace de productos con Spring Boot</description>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>

        <!-- Spring MVC (controladores web) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Data JPA (base de datos) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- Spring Security (login, roles) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <!-- Thymeleaf (vistas HTML) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>

        <!-- Thymeleaf + Spring Security (sec:authorize en HTML) -->
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity6</artifactId>
        </dependency>

        <!-- H2: base de datos embebida en memoria -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- Validation (@NotBlank, @Email, etc.) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>

        <!-- DevTools: recarga automática -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- Testing -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

### Comandos Maven desde consola

```bash
# Limpiar archivos compilados anteriores
mvn clean

# Compilar + testear + instalar en repositorio local
mvn install

# Generar el JAR ejecutable en target/
mvn package

# Ejecutar la aplicación directamente
mvn spring-boot:run

# Saltar tests para ir más rápido en desarrollo
mvn package -DskipTests
```

---

## 🏗️ LECCIÓN 2 — Spring MVC (Modelo, Vista, Controlador)

### 📄 Producto.java

```java
package com.alkemy.springmarket.model;

import jakarta.persistence.*;
import jakarta.validation.constraints.*;
import lombok.*;

@Entity
@Table(name = "productos")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Producto {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "El nombre es obligatorio")
    @Size(min = 3, max = 120)
    @Column(nullable = false)
    private String nombre;

    @NotBlank(message = "La descripción es obligatoria")
    @Column(columnDefinition = "TEXT")
    private String descripcion;

    @NotNull(message = "El precio es obligatorio")
    @DecimalMin(value = "0.01", message = "El precio debe ser mayor a 0")
    @Column(nullable = false)
    private Double precio;

    @NotNull(message = "El stock es obligatorio")
    @Min(value = 0, message = "El stock no puede ser negativo")
    @Column(nullable = false)
    private Integer stock;

    @NotBlank(message = "La categoría es obligatoria")
    private String categoria;

    private String imagenUrl;

    @Column(nullable = false)
    private Boolean activo = true;
}
```

### 📄 Usuario.java

```java
package com.alkemy.springmarket.model;

import jakarta.persistence.*;
import jakarta.validation.constraints.*;
import lombok.*;

@Entity
@Table(name = "usuarios")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Usuario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "El nombre es obligatorio")
    @Size(min = 2, max = 60)
    private String nombre;

    @Email(message = "Ingresá un email válido")
    @NotBlank(message = "El email es obligatorio")
    @Column(unique = true, nullable = false)
    private String email;

    @NotBlank(message = "El teléfono es obligatorio")
    private String telefono;

    private String direccion;

    @Column(unique = true)
    private String username;
}
```

### 📄 Orden.java

```java
package com.alkemy.springmarket.model;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "ordenes")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Orden {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "usuario_id", nullable = false)
    private Usuario usuario;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "producto_id", nullable = false)
    private Producto producto;

    @Column(nullable = false)
    private Integer cantidad;

    @Column(nullable = false)
    private Double total;

    @Column(nullable = false)
    private LocalDateTime fecha = LocalDateTime.now();

    // PENDIENTE, PAGADO, ENVIADO, ENTREGADO, CANCELADO
    @Column(nullable = false)
    private String estado = "PENDIENTE";
}
```

### 📄 HomeController.java

```java
package com.alkemy.springmarket.controller;

import com.alkemy.springmarket.service.ProductoService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
@RequiredArgsConstructor
public class HomeController {

    private final ProductoService productoService;

    @GetMapping("/")
    public String inicio(Model model,
                         @RequestParam(required = false) String categoria) {
        if (categoria != null && !categoria.isBlank()) {
            model.addAttribute("productos", productoService.buscarPorCategoria(categoria));
            model.addAttribute("categoriaActiva", categoria);
        } else {
            model.addAttribute("productos", productoService.listarActivos());
        }
        model.addAttribute("categorias", productoService.listarCategorias());
        return "index";
    }
}
```

### 📄 ProductoController.java

```java
package com.alkemy.springmarket.controller;

import com.alkemy.springmarket.model.Producto;
import com.alkemy.springmarket.service.ProductoService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

@Controller
@RequestMapping("/productos")
@RequiredArgsConstructor
public class ProductoController {

    private final ProductoService productoService;

    @GetMapping
    public String listar(Model model) {
        model.addAttribute("productos", productoService.listarTodos());
        return "productos/lista";
    }

    @GetMapping("/nuevo")
    @PreAuthorize("hasRole('ADMIN')")
    public String formularioNuevo(Model model) {
        model.addAttribute("producto", new Producto());
        return "productos/formulario";
    }

    @PostMapping("/guardar")
    @PreAuthorize("hasRole('ADMIN')")
    public String guardar(@Valid @ModelAttribute Producto producto,
                          BindingResult result,
                          RedirectAttributes flash) {
        if (result.hasErrors()) {
            return "productos/formulario";
        }
        productoService.guardar(producto);
        flash.addFlashAttribute("success", "✅ Producto publicado correctamente");
        return "redirect:/";
    }

    @GetMapping("/editar/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public String editar(@PathVariable Long id, Model model) {
        model.addAttribute("producto", productoService.buscarPorId(id));
        return "productos/formulario";
    }

    @GetMapping("/eliminar/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public String eliminar(@PathVariable Long id, RedirectAttributes flash) {
        productoService.eliminar(id);
        flash.addFlashAttribute("success", "🗑️ Producto eliminado");
        return "redirect:/";
    }
}
```

### 📄 LoginController.java

```java
package com.alkemy.springmarket.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class LoginController {
    @GetMapping("/login")
    public String login() {
        return "login";
    }
}
```

### 📄 templates/index.html (Pantalla principal del marketplace)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/extras/spring-security">
<head>
    <title>SpringMarket | Comprar y vender fácil</title>
    <meta charset="UTF-8"/>
    <link rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"/>
    <style>
        body { background-color: #f0f0f0; }
        .navbar { background-color: #ffe600 !important; }
        .navbar-brand { color: #333 !important; font-weight: 800; font-size: 1.5rem; }
        .btn-categoria { background: white; border: 1px solid #ddd; font-size: .85rem; }
        .btn-categoria.activa { background: #333; color: white; border-color: #333; }
        .card-producto { transition: transform .2s; cursor: pointer; }
        .card-producto:hover { transform: translateY(-3px); box-shadow: 0 6px 20px rgba(0,0,0,.15); }
        .precio { color: #00a650; font-size: 1.35rem; font-weight: 700; }
        .envio-gratis { color: #00a650; font-size: .78rem; font-weight: 600; }
    </style>
</head>
<body>

<!-- NAVBAR -->
<nav class="navbar shadow-sm py-2">
    <div class="container d-flex justify-content-between align-items-center">
        <a class="navbar-brand" th:href="@{/}">🛒 SpringMarket</a>
        <div class="d-flex gap-2 align-items-center">
            <a sec:authorize="hasRole('ADMIN')"
               th:href="@{/productos/nuevo}"
               class="btn btn-dark btn-sm fw-semibold">+ Publicar producto</a>

            <span sec:authorize="isAuthenticated()" class="d-flex align-items-center gap-2">
                <small class="text-dark fw-semibold" sec:authentication="name"></small>
                <a th:href="@{/logout}" class="btn btn-outline-dark btn-sm">Salir</a>
            </span>
            <span sec:authorize="!isAuthenticated()">
                <a th:href="@{/login}" class="btn btn-outline-dark btn-sm">Ingresar</a>
            </span>
        </div>
    </div>
</nav>

<!-- FILTRO DE CATEGORÍAS -->
<div class="bg-white border-bottom py-2 shadow-sm">
    <div class="container d-flex flex-wrap gap-2">
        <a th:href="@{/}" class="btn btn-sm btn-categoria"
           th:classappend="${categoriaActiva == null} ? ' activa' : ''">Todos</a>
        <a th:each="cat : ${categorias}"
           th:href="@{/(categoria=${cat})}"
           th:text="${cat}"
           class="btn btn-sm btn-categoria"
           th:classappend="${categoriaActiva == cat} ? ' activa' : ''">
        </a>
    </div>
</div>

<!-- ALERTAS FLASH -->
<div class="container mt-3">
    <div th:if="${success}" class="alert alert-success alert-dismissible fade show">
        <span th:text="${success}"></span>
        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    </div>
</div>

<!-- GRILLA DE PRODUCTOS -->
<div class="container my-4">
    <h6 class="mb-3 text-muted"
        th:text="${categoriaActiva != null ? categoriaActiva : 'Todos los productos'}">
    </h6>
    <div class="row row-cols-2 row-cols-md-4 g-3">
        <div class="col" th:each="p : ${productos}">
            <div class="card h-100 border-0 card-producto shadow-sm">
                <img th:src="${p.imagenUrl != null ? p.imagenUrl :
                              'https://via.placeholder.com/300x200?text=Producto'}"
                     class="card-img-top" alt="imagen"
                     style="height:170px;object-fit:cover"/>
                <div class="card-body pb-1">
                    <p class="small text-truncate mb-1" th:text="${p.nombre}"></p>
                    <div class="precio" th:text="'$' + ${p.precio}"></div>
                    <div class="envio-gratis" th:if="${p.precio >= 5000}">🚚 Envío gratis</div>
                    <div class="mt-1">
                        <span class="badge bg-light text-dark border small"
                              th:text="${p.categoria}"></span>
                        <span class="badge bg-danger ms-1 small"
                              th:if="${p.stock <= 5 && p.stock > 0}"
                              th:text="'¡Solo ' + ${p.stock} + '!'"></span>
                        <span class="badge bg-secondary ms-1 small"
                              th:if="${p.stock == 0}">Sin stock</span>
                    </div>
                </div>
                <div class="card-footer bg-white border-0 pb-3 pt-2">
                    <button class="btn btn-warning w-100 btn-sm fw-semibold"
                            th:disabled="${p.stock == 0}">
                        Comprar
                    </button>
                    <!-- Controles admin -->
                    <div sec:authorize="hasRole('ADMIN')" class="d-flex gap-1 mt-1">
                        <a th:href="@{/productos/editar/{id}(id=${p.id})}"
                           class="btn btn-outline-secondary btn-sm w-50">Editar</a>
                        <a th:href="@{/productos/eliminar/{id}(id=${p.id})}"
                           class="btn btn-outline-danger btn-sm w-50"
                           onclick="return confirm('¿Eliminar?')">Borrar</a>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div th:if="${#lists.isEmpty(productos)}" class="text-center py-5">
        <h5 class="text-muted">No hay productos disponibles.</h5>
        <a sec:authorize="hasRole('ADMIN')" th:href="@{/productos/nuevo}"
           class="btn btn-warning mt-2 fw-semibold">+ Publicar primer producto</a>
    </div>
</div>

<footer class="bg-dark text-white text-center py-3 mt-5">
    <small>© 2025 SpringMarket — Alkemy Módulo 6</small>
</footer>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### 📄 templates/productos/formulario.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Publicar Producto | SpringMarket</title>
    <meta charset="UTF-8"/>
    <link rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"/>
</head>
<body class="bg-light">
<div class="container py-5" style="max-width: 620px">
    <div class="card shadow-sm border-0">
        <div class="card-header bg-warning fw-bold fs-5 py-3">📦 Publicar producto</div>
        <div class="card-body p-4">
            <form th:action="@{/productos/guardar}" th:object="${producto}" method="post">
                <input type="hidden" th:field="*{id}"/>

                <div class="mb-3">
                    <label class="form-label fw-semibold">Nombre del producto</label>
                    <input type="text" th:field="*{nombre}" class="form-control"
                           placeholder="Ej: iPhone 14 128GB Negro"/>
                    <small class="text-danger" th:errors="*{nombre}"></small>
                </div>

                <div class="mb-3">
                    <label class="form-label fw-semibold">Descripción</label>
                    <textarea th:field="*{descripcion}" class="form-control" rows="3"
                              placeholder="Describí el producto..."></textarea>
                    <small class="text-danger" th:errors="*{descripcion}"></small>
                </div>

                <div class="row">
                    <div class="col-md-6 mb-3">
                        <label class="form-label fw-semibold">Precio ($)</label>
                        <input type="number" step="0.01" th:field="*{precio}"
                               class="form-control" placeholder="ej: 150000"/>
                        <small class="text-danger" th:errors="*{precio}"></small>
                    </div>
                    <div class="col-md-6 mb-3">
                        <label class="form-label fw-semibold">Stock disponible</label>
                        <input type="number" th:field="*{stock}"
                               class="form-control" placeholder="ej: 10"/>
                        <small class="text-danger" th:errors="*{stock}"></small>
                    </div>
                </div>

                <div class="mb-3">
                    <label class="form-label fw-semibold">Categoría</label>
                    <select th:field="*{categoria}" class="form-select">
                        <option value="">Seleccionar...</option>
                        <option value="Electrónica">Electrónica</option>
                        <option value="Ropa y Accesorios">Ropa y Accesorios</option>
                        <option value="Hogar y Jardín">Hogar y Jardín</option>
                        <option value="Deportes">Deportes</option>
                        <option value="Juguetes">Juguetes</option>
                        <option value="Libros">Libros</option>
                        <option value="Autos y Motos">Autos y Motos</option>
                        <option value="Alimentos">Alimentos</option>
                    </select>
                    <small class="text-danger" th:errors="*{categoria}"></small>
                </div>

                <div class="mb-4">
                    <label class="form-label fw-semibold">URL de imagen (opcional)</label>
                    <input type="url" th:field="*{imagenUrl}" class="form-control"
                           placeholder="https://..."/>
                </div>

                <div class="d-flex gap-2">
                    <button type="submit" class="btn btn-warning fw-bold px-4">Publicar</button>
                    <a th:href="@{/}" class="btn btn-outline-secondary">Cancelar</a>
                </div>
            </form>
        </div>
    </div>
</div>
</body>
</html>
```

### 📄 templates/login.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Ingresar | SpringMarket</title>
    <meta charset="UTF-8"/>
    <link rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"/>
</head>
<body class="bg-light d-flex align-items-center" style="min-height:100vh">
<div class="container" style="max-width:400px">
    <div class="card shadow border-0">
        <div class="card-header bg-warning text-center fw-bold fs-5 py-3">
            🛒 SpringMarket — Ingresá
        </div>
        <div class="card-body p-4">
            <div th:if="${param.error}" class="alert alert-danger">
                ❌ Usuario o contraseña incorrectos
            </div>
            <div th:if="${param.logout}" class="alert alert-success">
                ✅ Sesión cerrada
            </div>
            <form th:action="@{/login}" method="post">
                <div class="mb-3">
                    <label class="form-label">Usuario</label>
                    <input type="text" name="username" class="form-control"
                           placeholder="vendedor / comprador" required/>
                </div>
                <div class="mb-3">
                    <label class="form-label">Contraseña</label>
                    <input type="password" name="password" class="form-control" required/>
                </div>
                <button type="submit" class="btn btn-warning w-100 fw-bold">Ingresar</button>
            </form>
            <hr/>
            <div class="text-center small text-muted">
                Vendedor (ADMIN): <code>vendedor / vend123</code><br/>
                Comprador: <code>comprador / comp123</code>
            </div>
        </div>
    </div>
    <div class="text-center mt-3">
        <a th:href="@{/}" class="text-muted small">← Volver al marketplace</a>
    </div>
</div>
</body>
</html>
```

---

## 🗄️ LECCIÓN 3 — Acceso a Datos con JPA

### 📄 ProductoRepository.java

```java
package com.alkemy.springmarket.repository;

import com.alkemy.springmarket.model.Producto;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;

public interface ProductoRepository extends JpaRepository<Producto, Long> {

    List<Producto> findByActivoTrue();

    List<Producto> findByCategoriaAndActivoTrue(String categoria);

    List<Producto> findByNombreContainingIgnoreCaseAndActivoTrue(String nombre);

    @Query("SELECT DISTINCT p.categoria FROM Producto p WHERE p.activo = true ORDER BY p.categoria")
    List<String> findCategoriasDistintas();

    @Query("SELECT p FROM Producto p WHERE p.stock <= 5 AND p.activo = true")
    List<Producto> findConStockBajo();
}
```

### 📄 UsuarioRepository.java

```java
package com.alkemy.springmarket.repository;

import com.alkemy.springmarket.model.Usuario;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UsuarioRepository extends JpaRepository<Usuario, Long> {
    Optional<Usuario> findByEmail(String email);
    Optional<Usuario> findByUsername(String username);
    boolean existsByEmail(String email);
}
```

### 📄 OrdenRepository.java

```java
package com.alkemy.springmarket.repository;

import com.alkemy.springmarket.model.Orden;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface OrdenRepository extends JpaRepository<Orden, Long> {
    List<Orden> findByUsuarioIdOrderByFechaDesc(Long usuarioId);
    List<Orden> findByProductoId(Long productoId);
    List<Orden> findByEstado(String estado);
}
```

### 📄 ProductoService.java

```java
package com.alkemy.springmarket.service;

import com.alkemy.springmarket.model.Producto;
import com.alkemy.springmarket.repository.ProductoRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class ProductoService {

    private final ProductoRepository productoRepository;

    public List<Producto> listarTodos() {
        return productoRepository.findAll();
    }

    public List<Producto> listarActivos() {
        return productoRepository.findByActivoTrue();
    }

    public List<Producto> buscarPorCategoria(String categoria) {
        return productoRepository.findByCategoriaAndActivoTrue(categoria);
    }

    public List<String> listarCategorias() {
        return productoRepository.findCategoriasDistintas();
    }

    public Producto buscarPorId(Long id) {
        return productoRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("Producto no encontrado: " + id));
    }

    @Transactional
    public Producto guardar(Producto producto) {
        return productoRepository.save(producto);
    }

    @Transactional
    public void eliminar(Long id) {
        productoRepository.deleteById(id);
    }

    @Transactional
    public void descontarStock(Long productoId, int cantidad) {
        Producto p = buscarPorId(productoId);
        if (p.getStock() < cantidad) {
            throw new RuntimeException("Stock insuficiente");
        }
        p.setStock(p.getStock() - cantidad);
        productoRepository.save(p);
    }
}
```

### 📄 OrdenService.java

```java
package com.alkemy.springmarket.service;

import com.alkemy.springmarket.model.Orden;
import com.alkemy.springmarket.model.Producto;
import com.alkemy.springmarket.model.Usuario;
import com.alkemy.springmarket.repository.OrdenRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.time.LocalDateTime;
import java.util.List;

@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class OrdenService {

    private final OrdenRepository ordenRepository;
    private final ProductoService productoService;

    @Transactional
    public Orden crearOrden(Usuario usuario, Long productoId, int cantidad) {
        Producto producto = productoService.buscarPorId(productoId);
        productoService.descontarStock(productoId, cantidad);

        return ordenRepository.save(Orden.builder()
                .usuario(usuario)
                .producto(producto)
                .cantidad(cantidad)
                .total(producto.getPrecio() * cantidad)
                .fecha(LocalDateTime.now())
                .estado("PENDIENTE")
                .build());
    }

    public List<Orden> listarPorUsuario(Long usuarioId) {
        return ordenRepository.findByUsuarioIdOrderByFechaDesc(usuarioId);
    }

    public List<Orden> listarTodas() {
        return ordenRepository.findAll();
    }

    @Transactional
    public void cambiarEstado(Long ordenId, String nuevoEstado) {
        Orden orden = ordenRepository.findById(ordenId)
                .orElseThrow(() -> new RuntimeException("Orden no encontrada"));
        orden.setEstado(nuevoEstado);
        ordenRepository.save(orden);
    }
}
```

### 📄 application.properties

```properties
# ── Servidor ──────────────────────────────────────────────
server.port=8080
spring.application.name=SpringMarket

# ── Base de Datos H2 ──────────────────────────────────────
spring.datasource.url=jdbc:h2:mem:springmarketdb;DB_CLOSE_DELAY=-1
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# ── JPA / Hibernate ───────────────────────────────────────
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# ── Consola H2 (http://localhost:8080/h2-console) ─────────
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# ── Thymeleaf ─────────────────────────────────────────────
spring.thymeleaf.cache=false
```

### 📄 data.sql (productos de prueba)

```sql
INSERT INTO productos (nombre, descripcion, precio, stock, categoria, imagen_url, activo) VALUES
('iPhone 14 128GB Negro',       'Smartphone Apple, pantalla OLED 6.1", chip A15 Bionic.',           250000.00, 8,  'Electrónica',       'https://via.placeholder.com/300x200?text=iPhone+14',     true),
('Samsung TV 55" 4K UHD',       'Smart TV Samsung Crystal, HDR10+, Tizen OS.',                       180000.00, 3,  'Electrónica',       'https://via.placeholder.com/300x200?text=Samsung+TV',    true),
('Zapatillas Nike Air Max 270',  'Running Nike Air Max, talle 42, negro/blanco.',                      45000.00, 15, 'Ropa y Accesorios', 'https://via.placeholder.com/300x200?text=Nike+Air',      true),
('Silla Gamer RGB Ergonómica',   'Silla con reposapiés, soporte lumbar y reposacabezas.',              95000.00, 6,  'Hogar y Jardín',    'https://via.placeholder.com/300x200?text=Silla+Gamer',   true),
('Bicicleta MTB Rodado 29',      'Mountain bike 29", 21 velocidades, frenos a disco.',                120000.00, 4,  'Deportes',          'https://via.placeholder.com/300x200?text=Bicicleta+MTB', true),
('Auriculares Sony WH-1000XM5',  'Cancelación de ruido activa, 30hs batería, Bluetooth 5.2.',          85000.00, 10, 'Electrónica',       'https://via.placeholder.com/300x200?text=Sony+XM5',      true),
('Camiseta Adidas Originals',    'Remera algodón 100%, talle M, varios colores.',                       8500.00, 20, 'Ropa y Accesorios', 'https://via.placeholder.com/300x200?text=Adidas',        true),
('Set Ollas Antiadherentes x5',  'Aluminio con recubrimiento antiadherente, apto inducción.',           35000.00, 7,  'Hogar y Jardín',    'https://via.placeholder.com/300x200?text=Ollas',         true);

INSERT INTO usuarios (nombre, email, telefono, direccion, username) VALUES
('Admin Vendedor', 'vendedor@market.com', '1122334455', 'Av. Corrientes 1234, CABA', 'vendedor'),
('Juan Comprador', 'juan@email.com',      '1199887766', 'Floresta 567, Buenos Aires', 'comprador');
```

---

## 🔐 LECCIÓN 4 — Spring Security

### 📄 SecurityConfig.java

```java
package com.alkemy.springmarket.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true)
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public UserDetailsService userDetailsService(PasswordEncoder encoder) {
        // Vendedor = ADMIN: puede publicar, editar y eliminar productos
        var vendedor = User.builder()
                .username("vendedor")
                .password(encoder.encode("vend123"))
                .roles("ADMIN", "USER")
                .build();

        // Comprador = USER: puede explorar y comprar
        var comprador = User.builder()
                .username("comprador")
                .password(encoder.encode("comp123"))
                .roles("USER")
                .build();

        return new InMemoryUserDetailsManager(vendedor, comprador);
    }

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                // Explorar el catálogo es público
                .requestMatchers("/", "/productos", "/login",
                                 "/h2-console/**", "/css/**", "/js/**").permitAll()
                // Gestión de productos solo ADMIN
                .requestMatchers("/productos/nuevo", "/productos/guardar",
                                 "/productos/editar/**",
                                 "/productos/eliminar/**").hasRole("ADMIN")
                // Comprar requiere login
                .requestMatchers("/ordenes/**").authenticated()
                // Todo lo demás requiere autenticación
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .loginProcessingUrl("/login")
                .defaultSuccessUrl("/", true)
                .failureUrl("/login?error=true")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutUrl("/logout")
                .logoutSuccessUrl("/?logout=true")
                .invalidateHttpSession(true)
                .permitAll()
            )
            .csrf(csrf -> csrf.ignoringRequestMatchers("/h2-console/**"))
            .headers(h -> h.frameOptions(f -> f.sameOrigin()));

        return http.build();
    }
}
```

---

## 🌐 LECCIÓN 5 — APIs RESTful

### 📄 ProductoRestController.java

```java
package com.alkemy.springmarket.controller.rest;

import com.alkemy.springmarket.model.Producto;
import com.alkemy.springmarket.service.ProductoService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/v1/productos")
@RequiredArgsConstructor
public class ProductoRestController {

    private final ProductoService productoService;

    @GetMapping
    public ResponseEntity<List<Producto>> listar() {
        return ResponseEntity.ok(productoService.listarActivos());
    }

    @GetMapping("/{id}")
    public ResponseEntity<Producto> obtener(@PathVariable Long id) {
        return ResponseEntity.ok(productoService.buscarPorId(id));
    }

    @GetMapping("/categoria/{categoria}")
    public ResponseEntity<List<Producto>> porCategoria(@PathVariable String categoria) {
        return ResponseEntity.ok(productoService.buscarPorCategoria(categoria));
    }

    @PostMapping
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Producto> crear(@Valid @RequestBody Producto producto) {
        return ResponseEntity.status(HttpStatus.CREATED)
                             .body(productoService.guardar(producto));
    }

    @PutMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Producto> actualizar(@PathVariable Long id,
                                               @Valid @RequestBody Producto producto) {
        producto.setId(id);
        return ResponseEntity.ok(productoService.guardar(producto));
    }

    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> eliminar(@PathVariable Long id) {
        productoService.eliminar(id);
        return ResponseEntity.noContent().build();
    }
}
```

### 📄 OrdenRestController.java

```java
package com.alkemy.springmarket.controller.rest;

import com.alkemy.springmarket.model.Orden;
import com.alkemy.springmarket.service.OrdenService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/v1/ordenes")
@RequiredArgsConstructor
public class OrdenRestController {

    private final OrdenService ordenService;

    @GetMapping
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<List<Orden>> listarTodas() {
        return ResponseEntity.ok(ordenService.listarTodas());
    }

    @GetMapping("/usuario/{usuarioId}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<List<Orden>> porUsuario(@PathVariable Long usuarioId) {
        return ResponseEntity.ok(ordenService.listarPorUsuario(usuarioId));
    }

    @PutMapping("/{id}/estado")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> cambiarEstado(@PathVariable Long id,
                                              @RequestParam String estado) {
        ordenService.cambiarEstado(id, estado);
        return ResponseEntity.ok().build();
    }
}
```

### 🔬 Pruebas con Postman

```
# Listar productos (público, sin auth)
GET http://localhost:8080/api/v1/productos

# Filtrar por categoría
GET http://localhost:8080/api/v1/productos/categoria/Electrónica

# Crear producto (Basic Auth: vendedor / vend123)
POST http://localhost:8080/api/v1/productos
Content-Type: application/json

{
  "nombre": "PlayStation 5 Digital",
  "descripcion": "Consola Sony PS5, 825GB SSD, sin lector de discos.",
  "precio": 320000.00,
  "stock": 5,
  "categoria": "Electrónica",
  "activo": true
}

# Actualizar producto
PUT http://localhost:8080/api/v1/productos/1
Content-Type: application/json

{
  "nombre": "iPhone 14 128GB Negro",
  "descripcion": "Actualizado.",
  "precio": 240000.00,
  "stock": 6,
  "categoria": "Electrónica",
  "activo": true
}

# Eliminar producto
DELETE http://localhost:8080/api/v1/productos/1

# Ver todas las órdenes (solo ADMIN)
GET http://localhost:8080/api/v1/ordenes

# Cambiar estado de una orden a ENVIADO
PUT http://localhost:8080/api/v1/ordenes/1/estado?estado=ENVIADO
```

---

## 🚀 Cómo ejecutar

```bash
# 1. Clonar el repositorio
git clone https://github.com/TU_USUARIO/springmarket.git
cd springmarket

# 2. Compilar
mvn clean install

# 3. Ejecutar
mvn spring-boot:run

# 4. Abrir en el navegador
http://localhost:8080             → Marketplace (catálogo)
http://localhost:8080/productos   → Lista completa de productos
http://localhost:8080/login       → Pantalla de login
http://localhost:8080/h2-console  → Consola base de datos H2
```

| Usuario | Contraseña | Rol | Permisos |
|---|---|---|---|
| `vendedor` | `vend123` | ADMIN | Publicar, editar, eliminar productos |
| `comprador` | `comp123` | USER | Explorar y comprar |

---

## ✅ Checklist de entrega

| Lección | Tarea | Estado |
|---|---|---|
| L1 | Proyecto Maven creado desde start.spring.io | ☐ |
| L1 | pom.xml con todas las dependencias | ☐ |
| L1 | Comandos `clean`, `install`, `package` ejecutados | ☐ |
| L2 | Entidades `Producto`, `Usuario`, `Orden` | ☐ |
| L2 | Controladores MVC (`@Controller`) | ☐ |
| L2 | Vistas Thymeleaf con formularios | ☐ |
| L3 | Repositorios JPA con queries personalizadas | ☐ |
| L3 | Servicios con `@Service` | ☐ |
| L3 | H2 configurado + datos de prueba en data.sql | ☐ |
| L4 | Spring Security con roles ADMIN y USER | ☐ |
| L4 | Rutas protegidas correctamente | ☐ |
| L4 | Login y logout funcionales | ☐ |
| L5 | REST controllers con CRUD completo | ☐ |
| L5 | Endpoints probados con Postman | ☐ |

---

*Proyecto desarrollado repaso Java Marzo 2026*
*Desarrollado por Sabina Romero*
