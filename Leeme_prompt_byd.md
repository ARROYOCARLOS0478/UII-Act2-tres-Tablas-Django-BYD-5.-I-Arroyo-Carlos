
¡Excelente\! Este es un plan de proyecto detallado para construir tu aplicación Django. Seguiremos todos los pasos, asegurando que el proyecto **UIII\_BYD\_0478** quede totalmente funcional con el módulo de **`Proveedor`**.

Aquí tienes el procedimiento completo y los códigos necesarios.

-----

## 🚀 Primera Parte: Configuración del Proyecto BYD

### 1\. Procedimiento para crear carpeta del Proyecto: `UIII_BYD_0478`

Abre la terminal de tu sistema operativo (o el Símbolo del Sistema en Windows / Terminal en macOS/Linux) y ejecuta:

```bash
mkdir UIII_BYD_0478
```

### 2\. Procedimiento para abrir VS Code sobre la carpeta

Navega a la carpeta recién creada y abre VS Code:

```bash
cd UIII_BYD_0478
code .
```

### 3\. Procedimiento para abrir terminal en VS Code

Dentro de VS Code, presiona `Ctrl + Ñ` (o \`Ctrl + \`\` en algunos teclados, o ve a **Terminal \> Nueva Terminal**).

### 4\. Procedimiento para crear carpeta entorno virtual `.venv`

Desde la terminal de VS Code (asegúrate de que estás en la raíz del proyecto `UIII_BYD_0478`), ejecuta:

```bash
python -m venv .venv
```

### 5\. Procedimiento para activar el entorno virtual

Ejecuta el comando correspondiente a tu sistema operativo:

  * **Windows (PowerShell):**
    ```powershell
    .venv\Scripts\Activate
    ```
  * **macOS/Linux:**
    ```bash
    source .venv/bin/activate
    ```
    Verás `(.venv)` al inicio de tu prompt, indicando que el entorno está activo.

### 6\. Procedimiento para activar intérprete de Python

Si el entorno virtual está activo (paso 5), VS Code debería detectarlo automáticamente. Si no, haz clic en la versión de Python en la esquina inferior derecha de la ventana de VS Code y selecciona la opción `.venv`.

### 7\. Procedimiento para instalar Django

Con el entorno virtual activado, instala Django:

```bash
pip install django
```

### 8\. Procedimiento para crear proyecto `backend_Byd` sin duplicar carpeta

Crea el proyecto de Django en el directorio actual (la carpeta raíz `UIII_BYD_0478`):

```bash
django-admin startproject backend_Byd .
```

> **Nota:** El punto (`.`) evita la carpeta duplicada.

### 9\. Procedimiento para ejecutar servidor en el puerto 8047

Ejecuta el servidor con el puerto especificado:

```bash
python manage.py runserver 8047
```

### 10\. Procedimiento para copiar y pegar el link en el navegador

Una vez que el servidor esté corriendo, la terminal mostrará un mensaje similar a:

```
Starting development server at http://127.0.0.1:8047/
```

**Copia el enlace:** `http://127.0.0.1:8047/` y pégalo en tu navegador.

### 11\. Procedimiento para crear aplicación `app_Proveedor`

Detén el servidor (`Ctrl+C`) y crea la aplicación:

```bash
python manage.py startapp app_Proveedor
```

-----

## 🛠️ Segunda Parte: Modelos, Migraciones y Vistas

### 12\. Modelos (`models.py`)

Asegúrate de que el archivo `app_Proveedor/models.py` contenga el código que proporcionaste (lo copio aquí por completitud):

```python
# app_Proveedor/models.py

from django.db import models

# --- 1. Modelo Proveedor (7 campos) ---
class Proveedor(models.Model):
    nombre_empresa = models.CharField(max_length=100, unique=True, verbose_name="Nombre de la Empresa") 
    contacto_principal = models.CharField(max_length=100, verbose_name="Contacto Principal") 
    telefono = models.CharField(max_length=15, blank=True, null=True) 
    email = models.EmailField(unique=True) 
    direccion = models.CharField(max_length=200, verbose_name="Dirección") 
    fecha_registro = models.DateTimeField(auto_now_add=True, verbose_name="Fecha de Registro") 
    activo = models.BooleanField(default=True) 

    class Meta:
        verbose_name = "Proveedor"
        verbose_name_plural = "Proveedores"
        ordering = ['nombre_empresa']

    def __str__(self):
        return self.nombre_empresa

# --- 2. Modelo Distribuidor (7 campos) ---
class Distribuidor(models.Model):
    nombre_distribuidor = models.CharField(max_length=100, unique=True, verbose_name="Nombre del Distribuidor") 
    tipo_servicio = models.CharField(max_length=50, choices=[('LOCAL', 'Local'), ('NACIONAL', 'Nacional'), ('INTERNACIONAL', 'Internacional')]) 
    ciudad = models.CharField(max_length=50) 
    pais = models.CharField(max_length=50) 
    tiempo_entrega_dias = models.IntegerField(verbose_name="Tiempo de Entrega (días)") 
    comision = models.DecimalField(max_digits=5, decimal_places=2, help_text="Comisión en porcentaje") 
    ultima_revision = models.DateField(auto_now=True, verbose_name="Última Revisión") 

    class Meta:
        verbose_name = "Distribuidor"
        verbose_name_plural = "Distribuidores"
        ordering = ['nombre_distribuidor']

    def __str__(self):
        return self.nombre_distribuidor

# --- 3. Modelo Producto (7 campos, incluyendo las relaciones) ---
class Producto(models.Model):
    proveedor = models.ForeignKey(
        Proveedor,
        on_delete=models.CASCADE,
        related_name='productos_suministrados',
        verbose_name="ID Proveedor" 
    )

    distribuidores = models.ManyToManyField(
        Distribuidor,
        related_name='productos_distribuidos',
        verbose_name="Distribuidores"
    )

    nombre = models.CharField(max_length=150, verbose_name="Nombre del Producto") 
    sku = models.CharField(max_length=50, unique=True, verbose_name="SKU/Código") 
    precio = models.DecimalField(max_digits=10, decimal_places=2) 
    stock_actual = models.IntegerField(verbose_name="Stock Actual") 
    descripcion = models.TextField(blank=True, verbose_name="Descripción") 

    class Meta:
        verbose_name = "Producto"
        verbose_name_plural = "Productos"
        ordering = ['nombre']

    def __str__(self):
        return self.nombre
```

-----

### 12.5 Procedimiento para realizar las migraciones

Ejecuta los siguientes comandos:

```bash
python manage.py makemigrations app_Proveedor
python manage.py migrate
```

### 14\. Funciones en `app_Proveedor/views.py`

Reemplaza el contenido de `app_Proveedor/views.py` con las funciones CRUD para **`Proveedor`**.

```python
# app_Proveedor/views.py

from django.shortcuts import render, redirect, get_object_or_404
from .models import Proveedor # Solo trabajaremos con Proveedor por ahora
from django.db import IntegrityError # Para manejar errores de unique

# ----------------- Funciones para PROVEEDOR -----------------

# [1] INICIO
def inicio_proveedor(request):
    """Muestra la página de inicio de la app, que es un índice de proveedores."""
    proveedores = Proveedor.objects.all().order_by('nombre_empresa')
    # Usamos ver_proveedores.html para mostrar la tabla
    return render(request, 'proveedor/ver_proveedores.html', {'proveedores': proveedores})

# [2] AGREGAR PROVEEDOR (GET y POST)
def agregar_proveedor(request):
    if request.method == 'POST':
        # Captura de datos del formulario POST (sin forms.py)
        try:
            Proveedor.objects.create(
                nombre_empresa=request.POST.get('nombre_empresa'),
                contacto_principal=request.POST.get('contacto_principal'),
                telefono=request.POST.get('telefono'),
                email=request.POST.get('email'),
                direccion=request.POST.get('direccion'),
                activo=request.POST.get('activo') == 'on' # Checkbox activo
            )
            # Redirigir a la lista de proveedores después de agregar
            return redirect('ver_proveedores')
        except IntegrityError:
            # Manejo básico de datos duplicados (e.g., email o nombre_empresa)
            contexto = {'error': 'Error: El Nombre de la Empresa o Email ya existe.', 'es_post': True}
            return render(request, 'proveedor/agregar_proveedor.html', contexto)
        except Exception as e:
             contexto = {'error': f'Error al guardar: {e}', 'es_post': True}
             return render(request, 'proveedor/agregar_proveedor.html', contexto)
    
    # Si es GET, simplemente renderiza el formulario
    return render(request, 'proveedor/agregar_proveedor.html')


# [3] ACTUALIZAR PROVEEDOR (GET - Muestra el formulario con datos actuales)
def actualizar_proveedor(request, pk):
    proveedor = get_object_or_404(Proveedor, pk=pk)
    return render(request, 'proveedor/actualizar_proveedor.html', {'proveedor': proveedor})


# [4] REALIZAR ACTUALIZACION PROVEEDOR (POST - Guarda los cambios)
def realizar_actualizacion_proveedor(request, pk):
    proveedor = get_object_or_404(Proveedor, pk=pk)
    
    if request.method == 'POST':
        try:
            proveedor.nombre_empresa = request.POST.get('nombre_empresa')
            proveedor.contacto_principal = request.POST.get('contacto_principal')
            proveedor.telefono = request.POST.get('telefono')
            proveedor.email = request.POST.get('email')
            proveedor.direccion = request.POST.get('direccion')
            proveedor.activo = request.POST.get('activo') == 'on' # Checkbox activo
            
            # Guardar en la BD
            proveedor.save()
            return redirect('ver_proveedores')
        except IntegrityError:
            contexto = {'error': 'Error: El Nombre de la Empresa o Email ya existe.', 'proveedor': proveedor}
            return render(request, 'proveedor/actualizar_proveedor.html', contexto)
        except Exception as e:
            contexto = {'error': f'Error al actualizar: {e}', 'proveedor': proveedor}
            return render(request, 'proveedor/actualizar_proveedor.html', contexto)
    
    return redirect('ver_proveedores') # Si no es POST, regresa a la lista


# [5] BORRAR PROVEEDOR (GET - Muestra confirmación/se utiliza como acción directa)
def borrar_proveedor(request, pk):
    proveedor = get_object_or_404(Proveedor, pk=pk)
    
    if request.method == 'POST':
        proveedor.delete()
        return redirect('ver_proveedores')
        
    # Si es GET, muestra la página de confirmación
    return render(request, 'proveedor/borrar_proveedor.html', {'proveedor': proveedor})
```

-----

## 🎨 Tercera Parte: Estructura de Templates y Estética

### 15\. Crear la carpeta `templates` y la subcarpeta `proveedor`

Asegúrate de que la estructura de carpetas sea:

```
UIII_BYD_0478/
└── app_Proveedor/
    ├── templates/
    │   ├── proveedor/  <-- Nueva subcarpeta
    │   │   ├── agregar_proveedor.html
    │   │   ├── ver_proveedores.html
    │   │   ├── actualizar_proveedor.html
    │   │   └── borrar_proveedor.html
    │   ├── base.html
    │   ├── header.html
    │   ├── navbar.html
    │   ├── footer.html
    │   └── inicio.html
    └── ...
```

### 17\. `base.html` (Incluyendo Bootstrap)

Crea el archivo `app_Proveedor/templates/base.html`.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BYD Admin | {% block title %}{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            background-color: #f7f9fc; /* Color muy suave */
        }
        .content-wrap {
            flex: 1;
            padding-top: 20px;
            padding-bottom: 80px; /* Espacio para el footer fijo */
        }
        main {
            padding: 20px;
        }
    </style>
</head>
<body>
    {% include "header.html" %}
    <div class="content-wrap">
        <main class="container">
            {% block content %}
            {% endblock %}
        </main>
    </div>
    {% include "footer.html" %}

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### 18\. `navbar.html`

Crea el archivo `app_Proveedor/templates/navbar.html`.

```html
<nav class="navbar navbar-expand-lg navbar-dark" style="background-color: #007bff;"> 
    <div class="container-fluid">
        <a class="navbar-brand" href="{% url 'inicio' %}">
            <i class="bi bi-gear-fill"></i> Sistema de Administración BYD 
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'inicio' %}">
                        <i class="bi bi-house-door-fill"></i> Inicio
                    </a>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle active" href="#" role="button" data-bs-toggle="dropdown">
                        <i class="bi bi-truck"></i> Proveedor
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_proveedor' %}">Agregar Proveedor</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_proveedores' %}">Ver Proveedores</a></li>
                        <li><a class="dropdown-item disabled" href="#">Actualizar Proveedor</a></li>
                        <li><a class="dropdown-item disabled" href="#">Borrar Proveedor</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle disabled" href="#" role="button" data-bs-toggle="dropdown">
                        <i class="bi bi-box-seam"></i> Distribuidor
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item disabled" href="#">Agregar Distribuidor</a></li>
                        <li><a class="dropdown-item disabled" href="#">Ver Distribuidores</a></li>
                        <li><a class="dropdown-item disabled" href="#">Actualizar Distribuidor</a></li>
                        <li><a class="dropdown-item disabled" href="#">Borrar Distribuidor</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle disabled" href="#" role="button" data-bs-toggle="dropdown">
                        <i class="bi bi-tag"></i> Producto
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item disabled" href="#">Agregar Producto</a></li>
                        <li><a class="dropdown-item disabled" href="#">Ver Productos</a></li>
                        <li><a class="dropdown-item disabled" href="#">Actualizar Producto</a></li>
                        <li><a class="dropdown-item disabled" href="#">Borrar Producto</a></li>
                    </ul>
                </li>

            </ul>
        </div>
    </div>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
</nav>
```

### 16/18. `header.html`

Crea el archivo `app_Proveedor/templates/header.html`. (Usaremos este archivo solo para incluir el navbar y los iconos de Bootstrap).

```html
{% include "navbar.html" %}
```

### 19\. `footer.html`

Crea el archivo `app_Proveedor/templates/footer.html` (fijo al final).

```html
<footer class="footer mt-auto py-3 fixed-bottom text-white" style="background-color: #007bff;">
    <div class="container text-center">
        <span class="text-white-50">
            &copy; Derechos de Autor {{ "now"|date:"Y" }} | 
            Fecha del Sistema: {{ "now"|date:"d/m/Y H:i:s" }} | 
            Creado por Alumno Arroyo Carlos, Cbtis 128
        </span>
    </div>
</footer>
```

### 20\. `inicio.html`

Crea el archivo `app_Proveedor/templates/inicio.html`.

```html
{% extends "base.html" %}

{% block title %}Inicio{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8 text-center">
        <h1 class="display-4" style="color: #007bff;">Sistema de Administración BYD (Build Your Dreams)</h1>
        <p class="lead">Plataforma de gestión de inventarios y logística para proveedores, distribuidores y productos.</p>
        <hr class="my-4">
        
        <div class="card shadow-lg mb-4">
            <div class="card-body">
                <h5 class="card-title" style="color: #28a745;">Módulo de Proveedores Activo</h5>
                <p class="card-text">Utilice el menú 'Proveedor' para administrar, agregar, ver y modificar la información de las empresas proveedoras del proyecto BYD.</p>
            </div>
        </div>

        <img src="https://i.imgur.com/u7q7F7g.jpg" class="img-fluid rounded shadow" alt="Imagen de BYD" style="max-height: 400px; object-fit: cover;">
    </div>
</div>
{% endblock %}
```

### 22\. CRUD de Proveedor (en `app_Proveedor/templates/proveedor/`)

#### 22.1. `agregar_proveedor.html`

```html
{% extends "base.html" %}

{% block title %}Agregar Proveedor{% endblock %}

{% block content %}
<div class="card shadow-lg mx-auto" style="max-width: 600px;">
    <div class="card-header text-white" style="background-color: #28a745;">
        <h4 class="mb-0">➕ Agregar Nuevo Proveedor</h4>
    </div>
    <div class="card-body">
        {% if error %}
        <div class="alert alert-danger" role="alert">
            {{ error }}
        </div>
        {% endif %}
        
        <form method="post" action="{% url 'agregar_proveedor' %}">
            {% csrf_token %}
            <div class="mb-3">
                <label for="nombre_empresa" class="form-label">Nombre de la Empresa</label>
                <input type="text" class="form-control" id="nombre_empresa" name="nombre_empresa" required>
            </div>
            <div class="mb-3">
                <label for="contacto_principal" class="form-label">Contacto Principal</label>
                <input type="text" class="form-control" id="contacto_principal" name="contacto_principal" required>
            </div>
            <div class="mb-3">
                <label for="telefono" class="form-label">Teléfono</label>
                <input type="text" class="form-control" id="telefono" name="telefono">
            </div>
            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" class="form-control" id="email" name="email" required>
            </div>
            <div class="mb-3">
                <label for="direccion" class="form-label">Dirección</label>
                <input type="text" class="form-control" id="direccion" name="direccion" required>
            </div>
            <div class="form-check mb-3">
                <input type="checkbox" class="form-check-input" id="activo" name="activo" checked>
                <label class="form-check-label" for="activo">Activo</label>
            </div>
            <button type="submit" class="btn btn-success w-100">Guardar Proveedor</button>
        </form>
    </div>
</div>
{% endblock %}
```

#### 22.2. `ver_proveedores.html` (Inicio y Ver)

```html
{% extends "base.html" %}

{% block title %}Ver Proveedores{% endblock %}

{% block content %}
<h2 class="mb-4" style="color: #007bff;"><i class="bi bi-list-task"></i> Lista de Proveedores</h2>
<a href="{% url 'agregar_proveedor' %}" class="btn btn-success mb-3"><i class="bi bi-plus-circle-fill"></i> Agregar Nuevo</a>

{% if proveedores %}
<div class="table-responsive">
    <table class="table table-hover table-striped shadow-sm">
        <thead class="text-white" style="background-color: #007bff;">
            <tr>
                <th>ID</th>
                <th>Empresa</th>
                <th>Contacto</th>
                <th>Teléfono</th>
                <th>Email</th>
                <th>Activo</th>
                <th class="text-center">Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for p in proveedores %}
            <tr>
                <td>{{ p.id }}</td>
                <td>{{ p.nombre_empresa }}</td>
                <td>{{ p.contacto_principal }}</td>
                <td>{{ p.telefono|default_if_none:"N/A" }}</td>
                <td>{{ p.email }}</td>
                <td>
                    {% if p.activo %}
                        <span class="badge bg-success">Sí</span>
                    {% else %}
                        <span class="badge bg-danger">No</span>
                    {% endif %}
                </td>
                <td class="text-center">
                    <a href="{% url 'actualizar_proveedor' p.id %}" class="btn btn-sm btn-info text-white me-1" title="Ver/Editar"><i class="bi bi-eye-fill"></i></a>
                    <a href="{% url 'actualizar_proveedor' p.id %}" class="btn btn-sm btn-warning me-1" title="Editar"><i class="bi bi-pencil-fill"></i></a>
                    <a href="{% url 'borrar_proveedor' p.id %}" class="btn btn-sm btn-danger" title="Borrar"><i class="bi bi-trash-fill"></i></a>
                </td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</div>
{% else %}
<div class="alert alert-warning">
    No se encontraron proveedores registrados.
</div>
{% endif %}
{% endblock %}
```

#### 22.3. `actualizar_proveedor.html`

```html
{% extends "base.html" %}

{% block title %}Actualizar Proveedor{% endblock %}

{% block content %}
<div class="card shadow-lg mx-auto" style="max-width: 600px;">
    <div class="card-header text-white" style="background-color: #ffc107;">
        <h4 class="mb-0">✏️ Actualizar Proveedor: {{ proveedor.nombre_empresa }}</h4>
    </div>
    <div class="card-body">
        {% if error %}
        <div class="alert alert-danger" role="alert">
            {{ error }}
        </div>
        {% endif %}

        <form method="post" action="{% url 'realizar_actualizacion_proveedor' proveedor.id %}">
            {% csrf_token %}
            <div class="mb-3">
                <label for="nombre_empresa" class="form-label">Nombre de la Empresa</label>
                <input type="text" class="form-control" id="nombre_empresa" name="nombre_empresa" value="{{ proveedor.nombre_empresa }}" required>
            </div>
            <div class="mb-3">
                <label for="contacto_principal" class="form-label">Contacto Principal</label>
                <input type="text" class="form-control" id="contacto_principal" name="contacto_principal" value="{{ proveedor.contacto_principal }}" required>
            </div>
            <div class="mb-3">
                <label for="telefono" class="form-label">Teléfono</label>
                <input type="text" class="form-control" id="telefono" name="telefono" value="{{ proveedor.telefono|default_if_none:"" }}">
            </div>
            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" class="form-control" id="email" name="email" value="{{ proveedor.email }}" required>
            </div>
            <div class="mb-3">
                <label for="direccion" class="form-label">Dirección</label>
                <input type="text" class="form-control" id="direccion" name="direccion" value="{{ proveedor.direccion }}" required>
            </div>
            <div class="form-check mb-3">
                <input type="checkbox" class="form-check-input" id="activo" name="activo" {% if proveedor.activo %}checked{% endif %}>
                <label class="form-check-label" for="activo">Activo</label>
            </div>
            <button type="submit" class="btn btn-warning w-100">Guardar Cambios</button>
        </form>
        <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary w-100 mt-2">Cancelar</a>
    </div>
</div>
{% endblock %}
```

#### 22.4. `borrar_proveedor.html`

```html
{% extends "base.html" %}

{% block title %}Borrar Proveedor{% endblock %}

{% block content %}
<div class="card shadow-lg mx-auto" style="max-width: 500px;">
    <div class="card-header text-white bg-danger">
        <h4 class="mb-0">❌ Confirmar Eliminación</h4>
    </div>
    <div class="card-body text-center">
        <p class="lead">¿Está seguro que desea eliminar el proveedor?</p>
        <h5 class="text-danger mb-4">"{{ proveedor.nombre_empresa }}" (ID: {{ proveedor.id }})</h5>
        
        <form method="post" action="{% url 'borrar_proveedor' proveedor.id %}">
            {% csrf_token %}
            <button type="submit" class="btn btn-danger btn-lg me-3"><i class="bi bi-trash-fill"></i> Sí, Eliminar</button>
            <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary btn-lg"><i class="bi bi-x-circle-fill"></i> Cancelar</a>
        </form>
    </div>
</div>
{% endblock %}
```

-----

## 🔗 Cuarta Parte: Configuración de URLs

### 24\. Crear `app_Proveedor/urls.py`

Crea este archivo dentro de la carpeta de la aplicación `app_Proveedor`:

```python
# app_Proveedor/urls.py

from django.urls import path
from . import views

urlpatterns = [
    # Inicio del sistema (muestra la página de información)
    path('', views.inicio_proveedor, name='ver_proveedores'), 
    path('inicio/', views.inicio_proveedor, name='inicio'), # Ruta que usa la navbar

    # CRUD Proveedor
    path('proveedor/agregar/', views.agregar_proveedor, name='agregar_proveedor'),
    # 'ver_proveedores' ya está mapeado al path('') y muestra la lista de proveedores
    path('proveedor/actualizar/<int:pk>/', views.actualizar_proveedor, name='actualizar_proveedor'),
    path('proveedor/guardar_actualizacion/<int:pk>/', views.realizar_actualizacion_proveedor, name='realizar_actualizacion_proveedor'),
    path('proveedor/borrar/<int:pk>/', views.borrar_proveedor, name='borrar_proveedor'),
]
```

### 25\. Agregar `app_Proveedor` en `backend_Byd/settings.py`

Abre `backend_Byd/settings.py` y agrega `app_Proveedor` a la lista `INSTALLED_APPS`:

```python
# backend_Byd/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # NUEVA APLICACIÓN
    'app_Proveedor',
]

# ...

# Configuración adicional para Bootstrap Icons (opcional pero recomendado)
STATIC_URL = '/static/'
```

### 26\. Enlazar `app_Proveedor` en `backend_Byd/urls.py`

Abre `backend_Byd/urls.py` y configura el enrutador principal:

```python
# backend_Byd/urls.py

from django.contrib import admin
from django.urls import path, include 

urlpatterns = [
    path('admin/', admin.site.urls),
    # ENLACE CON app_Proveedor
    path('', include('app_Proveedor.urls')),
]
```

-----

## ⚙️ Quinta Parte: Finalización

### 27\. Procedimiento para registrar los modelos en `admin.py` y volver a realizar las migraciones

**27.1. Registrar en `admin.py`**

Abre `app_Proveedor/admin.py` y registra los tres modelos:

```python
# app_Proveedor/admin.py

from django.contrib import admin
from .models import Proveedor, Distribuidor, Producto

# Registro de Proveedor
@admin.register(Proveedor)
class ProveedorAdmin(admin.ModelAdmin):
    list_display = ('nombre_empresa', 'email', 'telefono', 'activo', 'fecha_registro')
    search_fields = ('nombre_empresa', 'email')
    list_filter = ('activo', 'fecha_registro')

# Registro de Distribuidor (Pendiente de uso en vistas, pero registrado)
@admin.register(Distribuidor)
class DistribuidorAdmin(admin.ModelAdmin):
    list_display = ('nombre_distribuidor', 'tipo_servicio', 'ciudad', 'ultima_revision')
    list_filter = ('tipo_servicio', 'pais')

# Registro de Producto (Pendiente de uso en vistas, pero registrado)
@admin.register(Producto)
class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'sku', 'precio', 'stock_actual', 'proveedor')
    list_filter = ('proveedor', 'distribuidores')
    search_fields = ('nombre', 'sku')
    filter_horizontal = ('distribuidores',) # Mejor widget para ManyToMany
```

**27.2. Volver a realizar las migraciones**

Ya hiciste la migración inicial, pero por buena práctica, si hicieras cambios adicionales que afecten a la BD (como las clases `Admin` no lo hacen, es opcional), ejecutarías:

```bash
# Opcional, solo si cambiaste un modelo (no requerido ahora)
# python manage.py makemigrations app_Proveedor
# python manage.py migrate
```

> **Nota:** El código es funcional sin más migraciones en este punto.

**27.3. Crear Superusuario (para acceder al admin)**

```bash
python manage.py createsuperuser
```

Sigue las instrucciones en pantalla para crear un usuario administrador.

### 31\. Finalmente, ejecutar servidor en el puerto 8047

Ejecuta el servidor:

```bash
python manage.py runserver 8047
```

Abre tu navegador en `http://127.0.0.1:8047/` y haz clic en el menú **Proveedor** para empezar a trabajar con el CRUD.
