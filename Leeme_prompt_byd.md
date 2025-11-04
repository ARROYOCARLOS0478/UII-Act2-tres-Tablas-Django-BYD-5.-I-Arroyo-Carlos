🧩 Primera Parte — Creación y Configuración del Proyecto
1️⃣ Crear la carpeta del proyecto

Abre tu explorador de archivos y crea manualmente una carpeta llamada:

UIII_BYD_0478

2️⃣ Abrir VS Code sobre la carpeta

Abre VS Code

En el menú superior selecciona:
Archivo → Abrir carpeta → UIII_BYD_0478 → Aceptar

3️⃣ Abrir terminal en VS Code

En el menú superior de VS Code:
Ver → Terminal
o presiona:
Ctrl + ñ (Windows) / Ctrl + ` (Mac)

4️⃣ Crear el entorno virtual .venv

En la terminal ejecuta:

python -m venv .venv


Esto creará la carpeta del entorno virtual dentro de tu proyecto.

5️⃣ Activar el entorno virtual

En Windows:

.venv\Scripts\activate


En macOS / Linux:

source .venv/bin/activate


Verás algo así en tu terminal:

(.venv) C:\Users\...\UIII_BYD_0478>

6️⃣ Activar el intérprete de Python en VS Code

Presiona Ctrl + Shift + P

Escribe Python: Select Interpreter

Selecciona el que diga algo como:

.venv\Scripts\python.exe

7️⃣ Instalar Django

En la terminal (con el entorno activado):

pip install django


Verifica la instalación:

django-admin --version

8️⃣ Crear el proyecto sin duplicar carpeta

Asegúrate de estar dentro de la carpeta UIII_BYD_0478 y ejecuta:

django-admin startproject backend_Byd .


El punto (.) evita que se cree una carpeta adicional.

9️⃣ Ejecutar el servidor en el puerto 8047

Ejecuta:

python manage.py runserver 8047

🔟 Copiar y pegar el link en el navegador

Copia el siguiente enlace que aparece en la terminal:

http://127.0.0.1:8047/


y pégalo en tu navegador.
Deberías ver la página de bienvenida de Django.

🧱 Segunda Parte — Crear la Aplicación
1️⃣1️⃣ Crear la aplicación app_Proveedor

Desde la terminal:

python manage.py startapp app_Proveedor

1️⃣2️⃣ Crear el modelo en models.py

Copia el código que tú ya proporcionaste dentro de:

app_Proveedor/models.py

1️⃣2️⃣.5️⃣ Realizar las migraciones

Ejecuta:

python manage.py makemigrations
python manage.py migrate

1️⃣3️⃣ Comenzamos trabajando con el MODELO: PROVEEDOR

Nos enfocamos en este modelo para el CRUD (crear, leer, actualizar, eliminar).

1️⃣4️⃣ Crear las vistas en views.py

En app_Proveedor/views.py agrega:

from django.shortcuts import render, redirect, get_object_or_404
from .models import Proveedor

# Página de inicio
def inicio_proveedor(request):
    return render(request, 'inicio.html')

# Agregar proveedor
def agregar_proveedor(request):
    if request.method == 'POST':
        nombre_empresa = request.POST['nombre_empresa']
        contacto_principal = request.POST['contacto_principal']
        telefono = request.POST['telefono']
        email = request.POST['email']
        direccion = request.POST['direccion']
        Proveedor.objects.create(
            nombre_empresa=nombre_empresa,
            contacto_principal=contacto_principal,
            telefono=telefono,
            email=email,
            direccion=direccion
        )
        return redirect('ver_proveedores')
    return render(request, 'proveedor/agregar_proveedor.html')

# Ver proveedores
def ver_proveedores(request):
    proveedores = Proveedor.objects.all()
    return render(request, 'proveedor/ver_proveedores.html', {'proveedores': proveedores})

# Actualizar proveedor
def actualizar_proveedor(request, id):
    proveedor = get_object_or_404(Proveedor, id=id)
    return render(request, 'proveedor/actualizar_proveedor.html', {'proveedor': proveedor})

# Realizar actualización
def realizar_actualizacion_proveedor(request, id):
    proveedor = get_object_or_404(Proveedor, id=id)
    if request.method == 'POST':
        proveedor.nombre_empresa = request.POST['nombre_empresa']
        proveedor.contacto_principal = request.POST['contacto_principal']
        proveedor.telefono = request.POST['telefono']
        proveedor.email = request.POST['email']
        proveedor.direccion = request.POST['direccion']
        proveedor.save()
        return redirect('ver_proveedores')
    return redirect('ver_proveedores')

# Borrar proveedor
def borrar_proveedor(request, id):
    proveedor = get_object_or_404(Proveedor, id=id)
    if request.method == 'POST':
        proveedor.delete()
        return redirect('ver_proveedores')
    return render(request, 'proveedor/borrar_proveedor.html', {'proveedor': proveedor})

1️⃣5️⃣ Crear la carpeta templates

Dentro de app_Proveedor:

app_Proveedor/
│
├── templates/
│   ├── base.html
│   ├── header.html
│   ├── navbar.html
│   ├── footer.html
│   ├── inicio.html
│   └── proveedor/
│       ├── agregar_proveedor.html
│       ├── ver_proveedores.html
│       ├── actualizar_proveedor.html
│       └── borrar_proveedor.html

1️⃣7️⃣ base.html (con Bootstrap)
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>{% block title %}Sistema BYD{% endblock %}</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">
  {% include 'navbar.html' %}
  <div class="container mt-4">
      {% block content %}{% endblock %}
  </div>
  {% include 'footer.html' %}
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

1️⃣8️⃣ navbar.html (menú con íconos)
<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">
      <i class="bi bi-gear-fill"></i> Sistema de Administración BYD
    </a>
    <ul class="navbar-nav">
      <li class="nav-item"><a class="nav-link" href="/">Inicio</a></li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#">Proveedor</a>
        <ul class="dropdown-menu">
          <li><a class="dropdown-item" href="/agregar_proveedor/">Agregar Proveedor</a></li>
          <li><a class="dropdown-item" href="/ver_proveedores/">Ver Proveedores</a></li>
        </ul>
      </li>
    </ul>
  </div>
</nav>

1️⃣9️⃣ footer.html
<footer class="bg-dark text-white text-center py-3 fixed-bottom">
  <p>© <script>document.write(new Date().getFullYear())</script> - Creado por Alumno Arroyo Carlos, CBTIS 128</p>
</footer>

2️⃣0️⃣ inicio.html
{% extends 'base.html' %}
{% block content %}
<div class="text-center">
  <h2>Bienvenido al Sistema BYD</h2>
  <p>Gestión de Proveedores, Distribuidores y Productos.</p>
  <img src="https://upload.wikimedia.org/wikipedia/commons/7/77/BYD_Logo.svg" width="200">
</div>
{% endblock %}

2️⃣4️⃣ urls.py en app_Proveedor

Crea el archivo:

app_Proveedor/urls.py


y agrega:

from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_proveedor, name='inicio_proveedor'),
    path('agregar_proveedor/', views.agregar_proveedor, name='agregar_proveedor'),
    path('ver_proveedores/', views.ver_proveedores, name='ver_proveedores'),
    path('actualizar_proveedor/<int:id>/', views.actualizar_proveedor, name='actualizar_proveedor'),
    path('realizar_actualizacion_proveedor/<int:id>/', views.realizar_actualizacion_proveedor, name='realizar_actualizacion_proveedor'),
    path('borrar_proveedor/<int:id>/', views.borrar_proveedor, name='borrar_proveedor'),
]

2️⃣5️⃣ Agregar la app en settings.py

Dentro de INSTALLED_APPS agrega:

'app_Proveedor',

2️⃣6️⃣ Configurar urls.py de backend_Byd

En backend_Byd/urls.py:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Proveedor.urls')),
]

2️⃣7️⃣ Registrar modelos en admin.py

En app_Proveedor/admin.py:

from django.contrib import admin
from .models import Proveedor

admin.site.register(Proveedor)


Ejecuta nuevamente migraciones:

python manage.py makemigrations
python manage.py migrate

3️⃣1️⃣ Ejecutar servidor en el puerto 8047
python manage.py runserver 8047


Abre en el navegador:

http://127.0.0.1:8047/


✅ Proyecto totalmente funcional con CRUD básico para Proveedor, estructura completa y diseño Bootstrap moderno.
