Estructura de Carpetas y Archivos Iniciales
Una vez completados los pasos de configuración inicial (1-7), esta será la estructura dentro de UIII_Joyería_8475:

    UIII_Joyería_8475/
    ├── .venv/                         # Entorno virtual
    ├── backend_Joyería/               # Proyecto Django (backend)
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── settings.py                # ✅ Modificar
    │   ├── urls.py                    # ✅ Modificar
    │   └── wsgi.py
    ├── app_Joyería/                   # Aplicación Django
    │   ├── migrations/
    │   ├── __init__.py
    │   ├── admin.py                   # ✅ Modificar
    │   ├── apps.py
    │   ├── models.py                  # ✅ Modificar (Ya tienes el código)
    │   ├── templates/                 # Carpeta de plantillas
    │   │   ├── base.html              # ✅ Crear (Bootstrap)
    │   │   ├── footer.html            # ✅ Crear (Footer fijo)
    │   │   ├── header.html            # ✅ Crear (Puede ser vacío o info)
    │   │   ├── inicio.html            # ✅ Crear (Página de inicio)
    │   │   ├── navbar.html            # ✅ Crear (Barra de navegación)
    │   │   └── proveedor/             # Subcarpeta para CRUD de Proveedor
    │   │       ├── agregar_proveedor.html     # ✅ Crear
    │   │       ├── actualizar_proveedor.html  # ✅ Crear
    │   │       ├── borrar_proveedor.html      # ✅ Crear
    │   │       └── ver_proveedores.html       # ✅ Crear (Tabla)
    │   ├── tests.py
    │   ├── urls.py                    # ✅ Crear
    │   └── views.py                   # ✅ Modificar
    └── manage.py
💻 Código del Proyecto Joyería
A continuación, se presenta el código para los archivos clave.

1. Configuración del Proyecto (backend_Joyería/settings.py)
Debes agregar app_Joyería a INSTALLED_APPS.

Python

# backend_Joyería/settings.py
# ... (otras configuraciones)

    INSTALLED_APPS = [
        # ... (otras apps)
        'app_Joyería',  # ⬅️ ¡Añade esto!
    ]

# ... (otras configuraciones)
2. URLs del Proyecto (backend_Joyería/urls.py)
Se configura la URL raíz para que apunte a las URLs de app_Joyería.

Python

# backend_Joyería/urls.py

    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('app_Joyería.urls')), # ⬅️ Enlaza la app
    ]
3. Modelos (app_Joyería/models.py)
(El código del paso 12 ya está completo, se omite aquí por brevedad, pero debe estar en el archivo.)

4. Registro en Admin (app_Joyería/admin.py)
Registra los modelos para que sean accesibles en el panel de administración.

    Python

# app_Joyería/admin.py

    from django.contrib import admin
    from .models import Proveedor, Producto, Venta

# Registro de los modelos
    admin.site.register(Proveedor)
    admin.site.register(Producto)
    admin.site.register(Venta)
    5. URLs de la Aplicación (app_Joyería/urls.py)
    Crea este archivo para manejar las rutas del CRUD de Proveedor.

Python

    # app_Joyería/urls.py
    
    from django.urls import path
    from . import views

    urlpatterns = [
        # General
        path('', views.inicio_joyeria, name='inicio_joyeria'),

    # CRUD Proveedor
    path('proveedores/agregar/', views.agregar_proveedor, name='agregar_proveedor'),
    path('proveedores/ver/', views.ver_proveedores, name='ver_proveedores'),
    path('proveedores/actualizar/<int:pk>/', views.actualizar_proveedor, name='actualizar_proveedor'),
    path('proveedores/realizar_actualizacion/<int:pk>/', views.realizar_actualizacion_proveedor, name='realizar_actualizacion_proveedor'),
    path('proveedores/borrar/<int:pk>/', views.borrar_proveedor, name='borrar_proveedor'),
]
6. Vistas (app_Joyería/views.py)
Implementación de las funciones para el CRUD de Proveedor.

    Python

# app_Joyería/views.py

    from django.shortcuts import render, redirect, get_object_or_404
    from .models import Proveedor

# Función de Inicio
    def inicio_joyeria(request):
        """Muestra la página de inicio."""
        return render(request, 'inicio.html')

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
# CRUD: PROVEEDOR
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

    def agregar_proveedor(request):
        """Maneja la creación de un nuevo proveedor."""
        if request.method == 'POST':
            # No se realiza validación de datos (Paso 28)
            Proveedor.objects.create(
                nombre=request.POST['nombre'],
                apellido=request.POST['apellido'],
                direccion=request.POST['direccion'],
                telefono=request.POST['telefono'],
                correo=request.POST['correo'],
                tipo_suministro=request.POST['tipo_suministro']
            )
            return redirect('ver_proveedores') # Redirige a la lista después de agregar
        
        return render(request, 'proveedor/agregar_proveedor.html')

    def ver_proveedores(request):
        """Muestra la lista de todos los proveedores."""
        proveedores = Proveedor.objects.all()
        context = {'proveedores': proveedores}
        return render(request, 'proveedor/ver_proveedores.html', context)
    
    def actualizar_proveedor(request, pk):
        """Muestra el formulario para editar un proveedor existente."""
        proveedor = get_object_or_404(Proveedor, pk=pk)
        context = {'proveedor': proveedor}
        return render(request, 'proveedor/actualizar_proveedor.html', context)
    
    def realizar_actualizacion_proveedor(request, pk):
        """Procesa el formulario de actualización y guarda los cambios."""
        if request.method == 'POST':
            proveedor = get_object_or_404(Proveedor, pk=pk)
            
            # Actualiza los campos sin validación
            proveedor.nombre = request.POST['nombre']
            proveedor.apellido = request.POST['apellido']
            proveedor.direccion = request.POST['direccion']
            proveedor.telefono = request.POST['telefono']
            proveedor.correo = request.POST['correo']
            proveedor.tipo_suministro = request.POST['tipo_suministro']
            proveedor.save()
            
            return redirect('ver_proveedores')
        
        # Si no es POST, redirige a la lista o a la página de actualización
        return redirect('ver_proveedores')
    
    
    def borrar_proveedor(request, pk):
        """Elimina un proveedor de la base de datos."""
        proveedor = get_object_or_404(Proveedor, pk=pk)
        
        if request.method == 'POST':
            proveedor.delete()
            return redirect('ver_proveedores')
        
        context = {'proveedor': proveedor}
        return render(request, 'proveedor/borrar_proveedor.html', context)
🎨 Plantillas HTML
Se utilizará Bootstrap para un diseño suave y moderno.

7. Plantilla Base (app_Joyería/templates/base.html)
Incluye Bootstrap CSS y JS.

HTML

    {% load static %}
    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>{% block title %}Joyería{% endblock %}</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
        <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons/font/bootstrap-icons.css" rel="stylesheet">
        <style>
            /* Estilo para mantener el footer fijo y contenido debajo del navbar */
            body {
                display: flex;
                flex-direction: column;
                min-height: 100vh;
                padding-top: 56px; /* Ajuste para el navbar fijo */
                background-color: #f8f9fa; /* Color de fondo suave */
            }
            .content {
                flex-grow: 1;
                padding-bottom: 70px; /* Espacio para el footer fijo */
            }
        </style>
    </head>
    <body>
        {% include 'navbar.html' %}
        
        <div class="content container mt-4">
            {% block content %}
            {% endblock %}
        </div>
    
        {% include 'footer.html' %}
    
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    </body>
    </html>
8. Barra de Navegación (app_Joyería/templates/navbar.html)
Incluye todas las opciones del menú.

HTML

    <nav class="navbar navbar-expand-lg navbar-dark fixed-top" style="background-color: #A0522D;"> <div class="container-fluid">
            <a class="navbar-brand" href="{% url 'inicio_joyeria' %}">
                <i class="bi bi-gem me-2"></i>Sistema de Administración Joyería
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNavDropdown">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'inicio_joyeria' %}"><i class="bi bi-house-door-fill me-1"></i>Inicio</a>
                    </li>
                    
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" id="proveedoresDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                            <i class="bi bi-person-badge-fill me-1"></i>Proveedores
                        </a>
                        <ul class="dropdown-menu" aria-labelledby="proveedoresDropdown">
                            <li><a class="dropdown-item" href="{% url 'agregar_proveedor' %}">Agregar Proveedor</a></li>
                            <li><a class="dropdown-item" href="{% url 'ver_proveedores' %}">Ver Proveedores</a></li>
                            </ul>
                    </li>
                    
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" id="productosDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                            <i class="bi bi-box-seam-fill me-1"></i>Productos
                        </a>
                        <ul class="dropdown-menu" aria-labelledby="productosDropdown">
                            <li><a class="dropdown-item" href="#">Agregar Productos</a></li>
                            <li><a class="dropdown-item" href="#">Ver Productos</a></li>
                            <li><a class="dropdown-item" href="#">Actualizar Productos</a></li>
                            <li><a class="dropdown-item" href="#">Borrar Productos</a></li>
                        </ul>
                    </li>
    
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" id="ventasDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                            <i class="bi bi-cart-fill me-1"></i>Ventas
                        </a>
                        <ul class="dropdown-menu" aria-labelledby="ventasDropdown">
                            <li><a class="dropdown-item" href="#">Agregar Ventas</a></li>
                            <li><a class="dropdown-item" href="#">Ver Ventas</a></li>
                            <li><a class="dropdown-item" href="#">Actualizar Ventas</a></li>
                            <li><a class="dropdown-item" href="#">Borrar Ventas</a></li>
                        </ul>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
9. Footer (app_Joyería/templates/footer.html)
Footer fijo con la información solicitada.

HTML

    <footer class="footer mt-auto py-3 fixed-bottom" style="background-color: #343a40; color: white;">
        <div class="container text-center">
            <span class="text-muted">
                &copy; Derechos de Autor Joyería. | Fecha del Sistema: <script>document.write(new Date().toLocaleDateString());</script> | Creado por **Diego Alberto Cruz Acosta**
            </span>
        </div>
    </footer>
10. Inicio (app_Joyería/templates/inicio.html)
Página de inicio con una imagen.

HTML

    {% extends 'base.html' %}
    {% load static %}
    
    {% block title %}Inicio - Sistema Joyería{% endblock %}
    
    {% block content %}
    <div class="p-5 mb-4 bg-light rounded-3 shadow-sm">
        <div class="container-fluid py-5">
            <h1 class="display-5 fw-bold" style="color: #A0522D;">💎 Bienvenido al Sistema de Administración de Joyería</h1>
            <p class="col-md-8 fs-4">Gestione sus proveedores, productos y ventas de joyería de manera eficiente y moderna.</p>
        </div>
    </div>
    
    <div class="row align-items-center">
        <div class="col-lg-7">
            <p class="lead">
                Este sistema le permite llevar un control detallado de su inventario, la información de sus proveedores y el registro de todas las transacciones de venta. Utilice el menú superior para navegar a las diferentes secciones.
            </p>
            <p class="text-muted">
                Actualmente se encuentra implementada la gestión completa (CRUD) para el módulo de **Proveedores**.
            </p>
        </div>
        <div class="col-lg-5">
            
        </div>
    </div>
    {% endblock %}
    11. Agregar Proveedor (app_Joyería/templates/proveedor/agregar_proveedor.html)
    Formulario de adición de proveedores.
    
    HTML
    
    {% extends 'base.html' %}
    
    {% block title %}Agregar Proveedor{% endblock %}
    
    {% block content %}
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card shadow-sm border-0">
                <div class="card-header" style="background-color: #D2B48C; color: #343a40;">
                    <h3 class="mb-0"><i class="bi bi-person-plus-fill me-2"></i>Agregar Nuevo Proveedor</h3>
                </div>
                <div class="card-body">
                    <form method="POST">
                        {% csrf_token %}
                        <div class="mb-3">
                            <label for="nombre" class="form-label">Nombre</label>
                            <input type="text" class="form-control" id="nombre" name="nombre" required>
                        </div>
                        <div class="mb-3">
                            <label for="apellido" class="form-label">Apellido</label>
                            <input type="text" class="form-control" id="apellido" name="apellido" required>
                        </div>
                        <div class="mb-3">
                            <label for="direccion" class="form-label">Dirección</label>
                            <input type="text" class="form-control" id="direccion" name="direccion" required>
                        </div>
                        <div class="mb-3">
                            <label for="telefono" class="form-label">Teléfono</label>
                            <input type="text" class="form-control" id="telefono" name="telefono" required>
                        </div>
                        <div class="mb-3">
                            <label for="correo" class="form-label">Correo Electrónico</label>
                            <input type="email" class="form-control" id="correo" name="correo" required>
                        </div>
                        <div class="mb-3">
                            <label for="tipo_suministro" class="form-label">Tipo de Suministro</label>
                            <input type="text" class="form-control" id="tipo_suministro" name="tipo_suministro" required>
                        </div>
                        <button type="submit" class="btn" style="background-color: #A0522D; color: white;">Guardar Proveedor</button>
                        <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary">Cancelar</a>
                    </form>
                </div>
            </div>
        </div>
    </div>
    {% endblock %}
12. Ver Proveedores (app_Joyería/templates/proveedor/ver_proveedores.html)
Tabla con botones para Ver, Editar y Borrar.

HTML

    {% extends 'base.html' %}
    
    {% block title %}Lista de Proveedores{% endblock %}
    
    {% block content %}
    <h2 class="mb-4" style="color: #A0522D;"><i class="bi bi-list-columns-reverse me-2"></i>Lista de Proveedores</h2>
    
    <a href="{% url 'agregar_proveedor' %}" class="btn mb-3" style="background-color: #D2B48C; color: #343a40;">
        <i class="bi bi-person-plus-fill me-1"></i> Agregar Nuevo Proveedor
    </a>
    
    {% if proveedores %}
    <div class="table-responsive">
        <table class="table table-hover table-striped shadow-sm">
            <thead class="table-dark" style="background-color: #A0522D; border-color: #A0522D;">
                <tr>
                    <th>ID</th>
                    <th>Nombre Completo</th>
                    <th>Dirección</th>
                    <th>Teléfono</th>
                    <th>Correo</th>
                    <th>Suministro</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                {% for p in proveedores %}
                <tr>
                    <td>{{ p.id_proveedor }}</td>
                    <td>{{ p.nombre }} {{ p.apellido }}</td>
                    <td>{{ p.direccion }}</td>
                    <td>{{ p.telefono }}</td>
                    <td>{{ p.correo }}</td>
                    <td>{{ p.tipo_suministro }}</td>
                    <td>
                        <a href="{% url 'actualizar_proveedor' pk=p.id_proveedor %}" class="btn btn-sm btn-primary me-2" title="Editar">
                            <i class="bi bi-pencil-square"></i> Editar
                        </a>
                        <a href="{% url 'borrar_proveedor' pk=p.id_proveedor %}" class="btn btn-sm btn-danger" title="Borrar">
                            <i class="bi bi-trash"></i> Borrar
                        </a>
                        </td>
                </tr>
                {% endfor %}
            </tbody>
        </table>
    </div>
    {% else %}
        <div class="alert alert-info" role="alert">
            <i class="bi bi-info-circle-fill me-2"></i>No hay proveedores registrados.
        </div>
    {% endif %}
    {% endblock %}
    13. Actualizar Proveedor (app_Joyería/templates/proveedor/actualizar_proveedor.html)
    Formulario pre-cargado para la edición.
    
    HTML
    
    {% extends 'base.html' %}
    
    {% block title %}Actualizar Proveedor{% endblock %}
    
    {% block content %}
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card shadow-sm border-0">
                <div class="card-header" style="background-color: #D2B48C; color: #343a40;">
                    <h3 class="mb-0"><i class="bi bi-arrow-repeat me-2"></i>Actualizar Proveedor: {{ proveedor.nombre }} {{ proveedor.apellido }}</h3>
                </div>
                <div class="card-body">
                    <form method="POST" action="{% url 'realizar_actualizacion_proveedor' pk=proveedor.id_proveedor %}">
                        {% csrf_token %}
                        <div class="mb-3">
                            <label for="nombre" class="form-label">Nombre</label>
                            <input type="text" class="form-control" id="nombre" name="nombre" value="{{ proveedor.nombre }}" required>
                        </div>
                        <div class="mb-3">
                            <label for="apellido" class="form-label">Apellido</label>
                            <input type="text" class="form-control" id="apellido" name="apellido" value="{{ proveedor.apellido }}" required>
                        </div>
                        <div class="mb-3">
                            <label for="direccion" class="form-label">Dirección</label>
                            <input type="text" class="form-control" id="direccion" name="direccion" value="{{ proveedor.direccion }}" required>
                        </div>
                        <div class="mb-3">
                            <label for="telefono" class="form-label">Teléfono</label>
                            <input type="text" class="form-control" id="telefono" name="telefono" value="{{ proveedor.telefono }}" required>
                        </div>
                        <div class="mb-3">
                            <label for="correo" class="form-label">Correo Electrónico</label>
                            <input type="email" class="form-control" id="correo" name="correo" value="{{ proveedor.correo }}" required>
                        </div>
                        <div class="mb-3">
                            <label for="tipo_suministro" class="form-label">Tipo de Suministro</label>
                            <input type="text" class="form-control" id="tipo_suministro" name="tipo_suministro" value="{{ proveedor.tipo_suministro }}" required>
                        </div>
                        <button type="submit" class="btn" style="background-color: #A0522D; color: white;">Guardar Cambios</button>
                        <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary">Cancelar</a>
                    </form>
                </div>
            </div>
        </div>
    </div>
    {% endblock %}
14. Borrar Proveedor (app_Joyería/templates/proveedor/borrar_proveedor.html)
Página de confirmación de eliminación.

HTML

    {% extends 'base.html' %}
    
    {% block title %}Borrar Proveedor{% endblock %}
    
    {% block content %}
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card border-danger shadow-sm">
                <div class="card-header bg-danger text-white">
                    <h3 class="mb-0"><i class="bi bi-exclamation-triangle-fill me-2"></i>Confirmación de Eliminación</h3>
                </div>
                <div class="card-body">
                    <p class="card-text">
                        ¿Estás seguro de que deseas eliminar al proveedor **{{ proveedor.nombre }} {{ proveedor.apellido }}** (ID: **{{ proveedor.id_proveedor }}**)?
                    </p>
                    <p class="text-danger fw-bold">Esta acción no se puede deshacer.</p>
    
                    <form method="POST" action="{% url 'borrar_proveedor' pk=proveedor.id_proveedor %}">
                        {% csrf_token %}
                        <button type="submit" class="btn btn-danger me-2"><i class="bi bi-trash-fill me-1"></i> Sí, Eliminar</button>
                        <a href="{% url 'ver_proveedores' %}" class="btn btn-secondary"><i class="bi bi-x-circle-fill me-1"></i> Cancelar</a>
                    </form>
                </div>
            </div>
        </div>
    </div>
    {% endblock %}
🚀 Pasos Finales de Ejecución
Migraciones (Paso 12.5 y 27):

Bash

    python manage.py makemigrations
    python manage.py migrate
Crear Superusuario (Paso 27): (Opcional, pero útil para verificar el admin.py)

Bash

    python manage.py createsuperuser
Ejecutar Servidor (Paso 31):

Bash

    python manage.py runserver 8475
Acceder (Paso 10): Copia y pega el link http://127.0.0.1:8475/ en tu navegador.

El proyecto está ahora totalmente funcional con el CRUD para el modelo Proveedor.
