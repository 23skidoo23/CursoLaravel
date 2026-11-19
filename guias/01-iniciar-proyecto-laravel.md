# Guía 01 — Iniciar un proyecto Laravel desde cero

> **Cuándo usarla:** crear un proyecto Laravel **nuevo** en Windows con XAMPP.
> Para un proyecto que **ya existe** (por ejemplo, de la empresa), usa la guía
> de clonar un proyecto existente (pendiente).
>
> **Probado con:** Windows 11 · XAMPP (PHP 8.2.12) · Composer 2.11 · Laravel 12.73
> **Fecha:** 2026-11-17

---

## Resumen rápido (copiar y pegar)

```bash
# 1. Verificar herramientas
php --version
composer --version
git --version

# 2. Crear el proyecto (dentro de la carpeta donde lo quieres)
cd /c/xampp/htdocs/<carpeta>
composer create-project laravel/laravel <nombre-proyecto>

# 3. Probarlo (desde aquí, TODO se corre dentro de la carpeta del proyecto)
cd <nombre-proyecto>
php artisan serve          # abrir http://127.0.0.1:8000 · detener con Ctrl+C

# 4. Guardarlo en Git (en otra terminal, también dentro de <nombre-proyecto>)
cd /c/xampp/htdocs/<carpeta>/<nombre-proyecto>
git init
git status                 # revisar que NO aparezcan vendor/ ni .env
git add .
git commit -m "Crear proyecto <nombre> con Laravel <versión>"
```

---

## ⚠️ Antes de empezar: ¿dónde va el repositorio?

| Caso | Raíz del repo (`git init`) | Dónde corro `php artisan` |
|---|---|---|
| **A. Proyecto normal** (el estándar; así son los de la empresa) | **Dentro** de `<nombre-proyecto>/` | En la raíz del repo |
| **B. Repo de capacitación** (mi `CursoLaravel`: bitácora + guías + proyecto) | La carpeta de **afuera** | Siempre después de `cd <nombre-proyecto>` |

- **Usa el caso A** salvo que quieras guardar notas junto al proyecto, como en `CursoLaravel`.
- En los dos casos: `artisan`, `composer.json` y `.env` viven en la carpeta del
  proyecto. **Antes de `php artisan`, mira la ruta del prompt.**
- Los pasos de abajo siguen el **caso A**.

---

## Paso 0 — Requisitos (solo la primera vez en una PC)

| Herramienta | Cómo verificar | Si falla |
|---|---|---|
| PHP | `php --version` | Agregar `C:\xampp\php` al **PATH** (ver abajo) |
| Composer | `composer --version` | Instalar `Composer-Setup.exe` desde getcomposer.org/download |
| Git | `git --version` | Instalar Git for Windows |
| Node / npm | `node --version` | Instalar desde nodejs.org (se usa después con Vite) |

### Agregar PHP al PATH
1. Inicio → buscar **"Variables de entorno"** → *Editar las variables de entorno*.
2. En el cuadro de **arriba** ("Variables de usuario"), seleccionar **Path** → **Editar**.
3. **Nuevo** → escribir `C:\xampp\php` → Aceptar ×3.
4. ⚠️ No tocar `PATHEXT`. No borrar las líneas que ya existen.
5. **Cerrar y reabrir** VS Code y la terminal.

### Instalar Composer
- Descargar **`Composer-Setup.exe`** (sección *Windows Installer*).
- Developer mode: **sin marcar**.
- PHP: `C:\xampp\php\php.exe`.
- Proxy: vacío.

### Configurar Git (solo una vez por PC)
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@correo.com"
```

---

## Paso 1 — Crear el proyecto

```bash
cd /c/xampp/htdocs/<carpeta>
composer create-project laravel/laravel <nombre-proyecto>
```

- **Copia y pega** el comando. Un error de dedo en `laravel/laravel` da
  `Could not find package`.
- Tarda varios minutos. **Espera a que vuelva el prompt `$`** antes de usar
  el proyecto.
- **Versión:** Composer instala la versión más nueva **compatible con tu PHP**.
  Laravel 13 pide PHP 8.3, así que con PHP 8.2 recibirás Laravel 12.
  Para forzar una versión: `composer create-project laravel/laravel <nombre> "12.*"`

### Qué hace solo al terminar (scripts automáticos)
| Script | Resultado |
|---|---|
| `package:discover` | Registra los paquetes instalados |
| `key:generate` | Crea la `APP_KEY` en `.env` (es un secreto) |
| crea `database/database.sqlite` | Base de datos SQLite por defecto |
| `migrate` | Crea las tablas `users`, `cache` y `jobs` en SQLite |

> Para usar **MySQL de XAMPP** hay que cambiar el `.env` (ver la guía de
> configurar MySQL, pendiente).

---

## Paso 2 — Comprobar que funciona

```bash
cd <nombre-proyecto>
php artisan --version      # debe mostrar "Laravel Framework 12.x"
php artisan serve
```

- Abrir **http://127.0.0.1:8000** → debe verse la página de bienvenida.
- `serve` **ocupa la terminal**. Para otros comandos, abre una terminal nueva
  (botón **+** en VS Code). Para detenerlo: **Ctrl+C**.
- Puertos: **8000** = artisan serve · **80** = Apache · **3306** = MySQL.

---

## Paso 3 — Guardar en Git

Dentro de la carpeta del proyecto (caso A):

```bash
cd /c/xampp/htdocs/<carpeta>/<nombre-proyecto>
git init
git status
```

Antes del `add`, **comprobar que Git ignora lo que no debe subir**:
```bash
git check-ignore -v vendor .env
# caso B (repo de afuera): git check-ignore -v <nombre-proyecto>/vendor <nombre-proyecto>/.env
```
Debe responder con la línea del `.gitignore` que los ignora. Laravel ya trae
esas reglas.

```bash
git add .
git status                 # lista en verde: NO debe haber vendor/ ni .env
git commit -m "Crear proyecto <nombre> con Laravel <versión>"
git status                 # debe decir "working tree clean"
```

---

## ✅ Checklist final

- [ ] `php --version` y `composer --version` responden
- [ ] Existe la carpeta del proyecto con `vendor/` y `.env`
- [ ] `php artisan --version` muestra la versión de Laravel
- [ ] La página de bienvenida abre en `http://127.0.0.1:8000`
- [ ] `vendor/` y `.env` están ignorados por Git
- [ ] Primer commit hecho y `working tree clean`

---

## 🐞 Errores comunes

| Mensaje | Causa | Solución |
|---|---|---|
| `php: command not found` | PHP no está en el PATH | Agregar `C:\xampp\php` al Path y reabrir la terminal |
| `Command "create-proyect" is not defined` | Comando mal escrito | Composer sugiere el correcto; responder `y` o corregirlo |
| `Could not find package laraverl/laravel` | Nombre de paquete mal escrito; Composer **no** lo adivina | Escribir exactamente `laravel/laravel` |
| `Could not open input file: artisan` | Estoy en la carpeta equivocada (el prompt no termina en `<nombre-proyecto>`) | `cd <nombre-proyecto>` y repetir |
| `Failed opening required '.../vendor/autoload.php'` | `vendor/` incompleto o no existe | Esperar a que termine Composer, o ejecutar `composer install` |
| `View [x] not found` | La vista no existe en `resources/views/` | Revisar el nombre en `view('...')` sin `.blade.php` |
| `warning: LF will be replaced by CRLF` | Aviso de saltos de línea Linux/Windows | Es solo un aviso; se puede continuar |

---

## 📌 Recordatorios clave

- **`vendor/`** no va a Git porque es **pesado y se regenera** con `composer install`.
- **`.env`** no va a Git porque contiene **secretos** (contraseñas, APP_KEY).
- Un `warning` se lee y se sigue. Un `error` o `fatal` detiene el comando.
- Cuando algo "no se encuentra", el mensaje dice **exactamente qué buscó**.
