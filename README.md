🚀 Guía de Inicio Rápido: Despliegue de Django en Railway
Este proyecto utiliza Django (incluyendo Django REST Framework) y PostgreSQL alojado en Railway.

Sigue estos pasos para clonar, configurar y ejecutar la aplicación en tu entorno local, y para desplegarla en producción.

1. ⚙️ Requisitos Previos
Asegúrate de tener instalado lo siguiente:

Python 3.10+ (Recomendado).

Git.

Una cuenta en Railway para el despliegue y la base de datos.

El paquete psycopg2 requiere herramientas de compilación de C/C++ en algunos sistemas operativos. Si tienes problemas, considera instalar psycopg2-binary.

2. 🌳 Clonar y Configurar el Entorno
Ejecuta los siguientes comandos en tu terminal para obtener el proyecto y configurar un entorno de desarrollo aislado:

A. Clonar el Repositorio
Bash

# Clona el proyecto desde GitHub
git clone https://docs.github.com/es/repositories/creating-and-managing-repositories/quickstart-for-repositories

# Navega al directorio del proyecto
cd django-railway-main
B. Crear y Activar el Entorno Virtual
Bash

# Crea el entorno virtual
python -m venv venv

# Activa el entorno virtual (Windows)
.\venv\Scripts\activate
# O para sistemas basados en Unix/Linux/macOS
# source venv/bin/activate
C. Instalar Dependencias
Asegúrate de tener instaladas las bibliotecas necesarias, incluyendo psycopg2, dj-database-url, python-dotenv, y gunicorn (para producción).

Bash

# Instala todas las dependencias listadas en requirements.txt
pip install -r requirements.txt
3. 💾 Configuración de la Base de Datos Local
Para que la aplicación funcione, debes configurar una base de datos. Usaremos la URL Pública de Railway para conectarnos a la base de datos alojada en producción.

A. Obtener la DATABASE_PUBLIC_URL
Ve al dashboard de tu proyecto en Railway.

Navega al servicio de PostgreSQL y luego a la pestaña Variables.

Copia el valor completo de la variable de entorno DATABASE_PUBLIC_URL.

B. Crear el Archivo .env Local
En la raíz de tu proyecto, crea un archivo llamado .env.

Pega la URL pública que copiaste, asignándola a la variable DATABASE_URL.

Ini, TOML

# .env (Archivo local)
# **Reemplaza [...] con el valor de tu DATABASE_PUBLIC_URL**
DATABASE_URL="postgresql://postgres:[tu_password]@[hostname_publico]:[tu_puerto]/railway"
IMPORTANTE: La conexión debe usar la URL Pública (DATABASE_PUBLIC_URL) para funcionar desde tu máquina local.

4. ▶️ Puesta en Marcha
Una vez configurado el .env, puedes inicializar la base de datos y ejecutar el servidor:

A. Ejecutar Migraciones
Este comando aplica el esquema de la base de datos a tu instancia de PostgreSQL en Railway:

Bash

# Aplica las migraciones a la BD de Railway (usando la URL pública)
python manage.py migrate
B. Crear Superusuario (Opcional)
Si es la primera vez, crea un usuario administrador:

Bash

python manage.py createsuperuser
C. Iniciar el Servidor de Desarrollo
Bash

# Inicia el servidor de desarrollo local
python manage.py runserver
La aplicación estará disponible en http://127.0.0.1:8000/.

5. ☁️ Despliegue a Producción (Railway)
Para que los cambios que hagas localmente se reflejen en tu aplicación desplegada:

A. Preparar y Enviar Cambios
Bash

# Revisa los cambios
git status

# Añade todos los archivos modificados
git add .

# Haz commit con un mensaje descriptivo
git commit -m "Descripción de los cambios"

# Sube los cambios a GitHub (y activa el despliegue automático en Railway)
git push -u origin main
B. Verificar la Conexión de Variables
Si la aplicación en Railway muestra el error ImproperlyConfigured (similar al que tuviste), es porque los servicios no están vinculados:

Ve al dashboard de Railway y navega a tu servicio web.

En la pestaña Variables, asegúrate de que exista una Variable Reference al servicio Postgres. Esto inyectará la URL Interna (DATABASE_URL) y hará que la aplicación se conecte correctamente en el entorno de producción.
