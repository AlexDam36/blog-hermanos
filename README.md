# Blog Hermanos

Blog desarrollado en conjunto para practicar desarrollo web, afianzar conocimientos y construir un proyecto de portafolio.

## Tecnologías del proyecto

- **Frontend:** HTML, CSS y JavaScript.
- **Backend:** Python 3.12 y Django 5.2.
- **Base de datos local:** SQLite.
- **Entorno y dependencias:** uv.
- **Revisión y formato de Python:** Ruff.
- **Control de versiones:** Git y GitHub.
- **Editor recomendado:** Visual Studio Code.

---

## 1. Requisitos previos

Antes de empezar, necesitás:

- Git instalado.
- Una cuenta de GitHub.
- Acceso al repositorio como colaborador.
- Visual Studio Code o el editor que prefieras.
- uv instalado.

No hace falta instalar Django por separado ni crear manualmente un entorno virtual: lo prepararemos con uv.

### Instalar uv

Si ya lo tenés instalado, saltá este paso.

Los siguientes comandos descargan y ejecutan el instalador oficial de uv.

**Windows — PowerShell:**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**macOS o Linux:**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Después de instalarlo, cerrá y volvé a abrir la terminal.

### Comprobar las herramientas

```bash
git --version
uv --version
```

Si alguno de los comandos no se reconoce, resolvé su instalación antes de continuar.

---

## 2. Clonar el repositorio

Reemplazá `TU-USUARIO` por el propietario del repositorio:

```bash
git clone https://github.com/alexdam36/blog-hermanos.git
cd blog-hermanos
```

**Todos los comandos siguientes deben ejecutarse dentro de esta carpeta**, donde se encuentra `manage.py`.

Si GitHub indica que no tenés acceso, comprobá que hayas aceptado la invitación como colaborador y estés usando la cuenta correcta.

---

## 3. Instalar Python y las dependencias

```bash
uv python install 3.12
uv sync --locked
```

Esto prepara el entorno virtual local `.venv` e instala las dependencias registradas en `uv.lock`, incluidas las herramientas de desarrollo.

### Archivos importantes

- `pyproject.toml`: declara las dependencias y la configuración de herramientas.
- `uv.lock`: registra las versiones resueltas para compartir el mismo entorno.
- `.venv/`: contiene el entorno virtual de tu computadora y no se sube a Git.

> Para instalar un proyecto existente usamos `uv sync --locked`.
> No vuelvas a ejecutar `uv init`, `startproject` ni `startapp`.

Usaremos `uv run` para ejecutar comandos dentro del entorno del proyecto, sin necesidad de activarlo manualmente.

---

## 4. Configurar las variables de entorno

En la raíz del proyecto:

1. Buscá `.env.example`.
2. Copialo.
3. Renombrá la copia como `.env`.

Podés hacerlo desde el explorador de archivos de VS Code.

**macOS o Linux:**

```bash
cp .env.example .env
```

**Windows — PowerShell:**

```powershell
Copy-Item .env.example .env
```

Hacé esta copia solo durante la primera instalación: si ya tenés `.env`, no lo sobrescribas.

Su contenido para desarrollo local será similar a:

```dotenv
DJANGO_SECRET_KEY=solo-desarrollo-local-no-usar-en-produccion
DJANGO_DEBUG=True
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1
```

### Acuerdos de seguridad

- `.env` es privado y no se sube a Git.
- `.env.example` sí se sube, pero solo con valores de ejemplo.
- No compartir contraseñas, tokens ni claves reales en commits, issues o pull requests.
- Estos valores son para desarrollo local, no para producción.

El proyecto debe tener configurada la carga de `.env` en `config/settings.py`.

---

## 5. Preparar la base de datos local

Aplicá las migraciones existentes:

```bash
uv run python manage.py migrate
```

Creá tu usuario administrador:

```bash
uv run python manage.py createsuperuser
```

Seguí las instrucciones de la terminal.

Cada desarrollador tendrá su propia base de datos local y su propio usuario administrador.

**No compartimos `db.sqlite3` mediante Git.**

> Durante la instalación usamos `migrate`, no `makemigrations`.
> Solo creamos nuevas migraciones cuando modificamos los modelos.

---

## 6. Ejecutar el proyecto

```bash
uv run python manage.py runserver
```

Abrí en el navegador:

```text
http://127.0.0.1:8000/
```

Panel de administración:

```text
http://127.0.0.1:8000/admin/
```

Ingresá al administrador con el usuario que creaste.

Para detener el servidor, presioná `Ctrl + C` en la terminal.

> Si la página principal devuelve 404 pero `/admin/` funciona, revisá si ya implementamos la ruta de inicio. La base inicial puede no tener todavía una página del blog.

---

## 7. Configurar VS Code

Abrí la carpeta raíz del repositorio en VS Code.

### Extensiones recomendadas

Aceptá las recomendaciones del proyecto o instalá:

- Python — Microsoft.
- Pylance — Microsoft.
- Ruff — Astral.

Las recomendaciones están en:

```text
.vscode/extensions.json
```

Los ajustes compartidos están en:

```text
.vscode/settings.json
```

### Seleccionar el intérprete

Si VS Code no detecta el entorno:

1. Abrí la paleta de comandos.
2. Buscá `Python: Select Interpreter`.
3. Seleccioná el intérprete dentro de `.venv`.

Ubicaciones habituales:

**Windows:**

```text
.venv\Scripts\python.exe
```

**macOS o Linux:**

```text
.venv/bin/python
```

---

## 8. Comprobar que la instalación funciona

Ejecutá:

```bash
uv run python manage.py check
uv run python manage.py test
uv run ruff check .
uv run ruff format --check .
```

La configuración inicial está lista cuando:

- Django no informa errores de configuración.
- Las pruebas existentes pasan.
- Ruff no informa problemas.
- Podés abrir el administrador e iniciar sesión.

> Al comienzo puede que todavía no tengamos pruebas propias.
> Que el comando pase no significa que todas las funcionalidades estén probadas.

---

## 9. Estructura del proyecto

La estructura prevista es:

```text
blog-hermanos/
├── .github/
│   ├── pull_request_template.md
│   └── workflows/
│       └── ci.yml
├── .vscode/
│   ├── extensions.json
│   └── settings.json
├── blog/
│   ├── migrations/
│   ├── templates/
│   │   └── blog/
│   ├── static/
│   │   └── blog/
│   │       ├── css/
│   │       └── js/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── config/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── .env.example
├── .gitignore
├── .python-version
├── CONTRIBUTING.md
├── README.md
├── manage.py
├── pyproject.toml
└── uv.lock
```

Las carpetas de plantillas y archivos estáticos se irán completando con las primeras funcionalidades.

### Dónde va cada cosa

| Ubicación | Responsabilidad |
|---|---|
| `blog/models.py` | Datos y relaciones |
| `blog/views.py` | Procesamiento de solicitudes |
| `blog/templates/blog/` | Plantillas HTML |
| `blog/static/blog/css/` | Estilos |
| `blog/static/blog/js/` | Interacciones JavaScript |
| `blog/tests.py` | Pruebas |
| `blog/migrations/` | Cambios en la estructura de datos |
| `config/settings.py` | Configuración del proyecto |
| `config/urls.py` | Rutas principales |

---

## 10. Cómo trabajamos con Git

### Regla principal

**No trabajamos directamente en `main`.**

Creamos una rama por tarea y abrimos una pull request cuando está lista.

### Configurar tu identidad

Si todavía no configuraste Git:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "TU-EMAIL-DE-COMMITS"
```

Usá el correo asociado a tu cuenta de GitHub o el correo privado de commits que GitHub te proporciona.

### Empezar una tarea

Antes de cambiar de rama, comprobá que no tengas cambios sin guardar:

```bash
git status
```

Después:

```bash
git switch main
git pull --ff-only origin main
git switch -c feature/tu-nombre-listado
```

Reemplazá el nombre de la rama por uno relacionado con tu tarea.

Ejemplos:

```text
feature/ana-listado-articulos
feature/luis-detalle-articulo
fix/ana-acceso-borradores
docs/luis-instrucciones-instalacion
```

### Guardar y subir los cambios

Primero, ejecutá las comprobaciones:

```bash
uv run ruff check .
uv run ruff format --check .
uv run python manage.py check
uv run python manage.py test
```

Después, revisá qué vas a subir:

```bash
git status
git diff
git add .
git diff --cached
```

Si todo es correcto:

```bash
git commit -m "feat: agregar listado de artículos"
git push -u origin feature/tu-nombre-listado
```

Usá el nombre real de tu rama en el último comando.

### Abrir una pull request

En GitHub:

1. Abrí una pull request desde tu rama hacia `main`.
2. Describí qué cambia y cómo probarlo.
3. Agregá capturas si cambia la interfaz.
4. Pedile revisión al otro desarrollador.
5. Esperá a que las comprobaciones automáticas pasen.
6. Integrá los cambios después de la aprobación.

### Empezar la siguiente tarea

Una vez integrada la pull request:

```bash
git switch main
git pull --ff-only origin main
uv sync --locked
uv run python manage.py migrate
git switch -c feature/tu-nombre-siguiente-tarea
```

---

## 11. Comandos habituales

### Arrancar el servidor

```bash
uv run python manage.py runserver
```

### Ejecutar pruebas

```bash
uv run python manage.py test
```

### Revisar Python

```bash
uv run ruff check .
```

### Aplicar correcciones automáticas y formato

```bash
uv run ruff check --fix .
uv run ruff format .
```

Revisá los cambios con `git diff` antes de hacer commit.

### Crear migraciones después de modificar modelos

```bash
uv run python manage.py makemigrations
uv run python manage.py migrate
```

Las migraciones nuevas se incluyen en el mismo commit o pull request que modifica los modelos.

### Agregar una dependencia del proyecto

```bash
uv add nombre-del-paquete
```

### Agregar una herramienta de desarrollo

```bash
uv add --dev nombre-del-paquete
```

Incluí los cambios de `pyproject.toml` y `uv.lock` en la pull request.

**No edites `uv.lock` manualmente ni agregues dependencias sin comentar para qué hacen falta.**

---

## 12. Problemas frecuentes

### La terminal no reconoce `uv`

- Cerrá y volvé a abrir la terminal.
- Si ocurre dentro de VS Code, reiniciá VS Code.
- Comprobá que la instalación terminó correctamente.

### `uv sync --locked` informa que el lockfile está desactualizado

Comprobá que descargaste los últimos cambios y que tenés tanto `pyproject.toml` como `uv.lock`.

Si no modificaste dependencias, consultá al otro desarrollador antes de regenerar el archivo.

Si las modificaste intencionalmente, actualizá el lockfile con:

```bash
uv lock
uv sync --locked
```

Revisá e incluí ambos archivos en tu pull request.

### Error relacionado con `DJANGO_SECRET_KEY`

Comprobá que:

- Existe `.env` junto a `manage.py`.
- El archivo no se llama `.env.txt`.
- Contiene `DJANGO_SECRET_KEY`.
- `config/settings.py` carga las variables del archivo.

Reiniciá el servidor después de corregirlo.

### Error `no such table`

Primero ejecutá:

```bash
uv run python manage.py migrate
```

Si continúa, revisá que las migraciones necesarias estén en el repositorio.

No borres la base de datos ni las migraciones como primera solución.

### El puerto 8000 está ocupado

Usá otro puerto:

```bash
uv run python manage.py runserver 8001
```

Abrí:

```text
http://127.0.0.1:8001/
```

### VS Code no encuentra Django

Seleccioná el intérprete de `.venv` y comprobá las dependencias:

```bash
uv sync --locked
```

### Aparece un conflicto de Git

No uses `git push --force` ni descartes archivos para resolverlo a ciegas.

Revisen juntos los archivos en conflicto y acuerden qué cambios conservar.

---

## 13. Acuerdos del equipo

- Mantener `main` funcionando.
- Crear una rama por tarea.
- Preferir pull requests pequeñas.
- Revisar los cambios del otro antes de integrarlos.
- Añadir pruebas para las reglas importantes.
- Avisar antes de modificar modelos o configuración compartida.
- No subir secretos, `.venv`, bases de datos locales ni archivos personales.
- Actualizar este README cuando cambie la instalación.
- Documentar decisiones importantes, aunque los bocetos se hagan en papel.

---

## 14. Checklist de primera instalación

- [ ] Tengo acceso al repositorio.
- [ ] Instalé Git y uv.
- [ ] Cloné el proyecto.
- [ ] Instalé Python 3.12.
- [ ] Ejecuté `uv sync --locked`.
- [ ] Copié `.env.example` a `.env`.
- [ ] Apliqué las migraciones.
- [ ] Creé mi usuario administrador.
- [ ] Configuré el intérprete de VS Code.
- [ ] Pude iniciar el servidor.
- [ ] Pude ingresar a `/admin/`.
- [ ] Ejecuté las comprobaciones locales.
- [ ] Creé una rama para mi primera tarea.

¡Entorno listo para empezar!
