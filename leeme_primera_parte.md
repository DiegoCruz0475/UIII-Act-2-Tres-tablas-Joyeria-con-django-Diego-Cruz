🟤 Primera parte
Proyecto: Joyeria
 Lenguaje: Python
 Framework: Django
 Editor: VS Code

1️⃣ Procedimiento para crear carpeta del Proyecto:
 UIII_Joyeria_0475
2️⃣ Procedimiento para abrir VS Code sobre la carpeta
 UIII_Joyeria_0475
3️⃣ Procedimiento para abrir terminal en VS Code
4️⃣ Procedimiento para crear carpeta entorno virtual “.venv” desde terminal de VS Code
python -m venv .venv

5️⃣ Procedimiento para activar el entorno virtual.
En Windows:

 .venv\Scripts\activate


En Mac/Linux:

 source .venv/bin/activate


6️⃣ Procedimiento para activar intérprete de Python.
 Seleccionar el intérprete correspondiente a .venv desde la barra inferior de VS Code.
7️⃣ Procedimiento para instalar Django.
pip install django

8️⃣ Procedimiento para crear proyecto backend_Joyeria sin duplicar carpeta.
django-admin startproject backend_Joyeria .

9️⃣ Procedimiento para ejecutar servidor en el puerto 8475.
python manage.py runserver 8475

🔟 Procedimiento para copiar y pegar el link en el navegador.
http://127.0.0.1:8475/

1️⃣1️⃣ Procedimiento para crear aplicación app_Joyeria.
python manage.py startapp app_Joyeria


1️⃣2️⃣ Aquí el modelo models.py
from django.db import models

# ======================
#   MODELO PROVEEDOR
# ======================
class Proveedor(models.Model):
    id_proveedor = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    direccion = models.CharField(max_length=200)
    telefono = models.CharField(max_length=15)
    correo = models.EmailField()
    tipo_suministro = models.CharField(max_length=100)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ======================
#   MODELO PRODUCTO
# ======================
class Producto(models.Model):
    id_producto = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    material = models.CharField(max_length=100)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    tipo = models.CharField(max_length=100)
    stock = models.IntegerField()
    id_proveedor = models.ForeignKey(Proveedor, on_delete=models.CASCADE, related_name='productos')

    def __str__(self):
        return self.nombre


# ======================
#   MODELO VENTA
# ======================
class Venta(models.Model):
    id_venta = models.AutoField(primary_key=True)
    id_cliente = models.IntegerField()
    id_empleado = models.IntegerField()
    fecha_venta = models.DateField()
    total = models.DecimalField(max_digits=10, decimal_places=2)
    metodo_pago = models.CharField(max_length=50)
    productos = models.ManyToManyField(Producto, related_name='ventas')

    def __str__(self):
        return f"Venta #{self.id_venta} - Total: ${self.total}"


1️⃣2️⃣.5️⃣ Procedimiento para realizar las migraciones (makemigrations y migrate).
python manage.py makemigrations
python manage.py migrate

1️⃣3️⃣ Primero trabajamos con el MODELO: PRODUCTO
1️⃣4️⃣ En views.py de app_Joyeria, crear las funciones con sus códigos correspondientes:
 (inicio_joyeria, agregar_producto, actualizar_producto, realizar_actualizacion_producto, borrar_producto).
1️⃣5️⃣ Crear la carpeta “templates” dentro de “app_Joyeria”.
1️⃣6️⃣ En la carpeta templates, crear los archivos HTML:
 base.html, header.html, navbar.html, footer.html, inicio.html.
1️⃣7️⃣ En el archivo base.html agregar Bootstrap para CSS y JS.
1️⃣8️⃣ En el archivo navbar.html incluir las opciones:
“Sistema de Administración Joyería”


“Inicio”


“Productos”, con submenú:


Agregar Producto


Ver Productos


Actualizar Producto


Borrar Producto


“Proveedores”, con submenú:


Agregar Proveedor


Ver Proveedores


Actualizar Proveedor


Borrar Proveedor


“Ventas”, con submenú:


Agregar Venta


Ver Ventas


Actualizar Venta


Borrar Venta


👉 Incluir iconos solo en las opciones principales, no en los submenús.
1️⃣9️⃣ En el archivo footer.html, incluir:
Derechos de autor


Fecha del sistema


“Creado por Diego Alberto Cruz Acosta, CBTIS 128”
 Mantenerlo fijo al final de la página.


2️⃣0️⃣ En el archivo inicio.html, colocar información sobre el sistema de joyería y una imagen representativa de joyas o accesorios.
2️⃣1️⃣ Crear la subcarpeta producto dentro de
 app_Joyeria/templates.
2️⃣2️⃣ Crear los archivos HTML con su código correspondiente:
 agregar_producto.html,
 ver_productos.html (mostrar tabla con botones ver, editar, borrar),
 actualizar_producto.html,
 borrar_producto.html.
2️⃣3️⃣ No utilizar forms.py.
2️⃣4️⃣ Procedimiento para crear el archivo urls.py en app_Joyeria con el código correspondiente para acceder a las funciones de views.py para operaciones CRUD en productos.
2️⃣5️⃣ Procedimiento para agregar app_Joyeria en settings.py de backend_Joyeria.
2️⃣6️⃣ Realizar las configuraciones correspondientes en urls.py de backend_Joyeria para enlazar con app_Joyeria.
2️⃣7️⃣ Procedimiento para registrar los modelos en admin.py y volver a realizar las migraciones.
python manage.py makemigrations
python manage.py migrate

2️⃣8️⃣ Por lo pronto solo trabajar con “Producto”, dejando pendiente #MODELO: Proveedor y #MODELO: Venta.
2️⃣9️⃣ Utilizar colores suaves, atractivos y modernos en las páginas web.
3️⃣0️⃣ No validar entrada de datos (sin validaciones en formulario).
3️⃣1️⃣ Al inicio crear toda la estructura completa de carpetas y archivos.
3️⃣2️⃣ Proyecto totalmente funcional.
3️⃣3️⃣ Finalmente ejecutar servidor en el puerto 8036.
python manage.py runserver 8036



