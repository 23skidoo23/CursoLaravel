# Guía 02 — Configurar `.env` con el MySQL de XAMPP

> **Cuándo usarla:** un proyecto Laravel que usa SQLite (el default de Laravel 12)
> y quieres pasarlo al **MySQL de XAMPP**. También sirve al clonar un proyecto de la
> empresa: los pasos 1, 2, 4 y 5 son los mismos.
>
> **Probado con:** Windows 11 · XAMPP (PHP 8.2.12, MySQL en puerto 3306) · Laravel 12
> **Fecha:** 2026-11-19

---

## Resumen rápido (copiar y pegar)

```bash
# 1. XAMPP Control Panel → Start en Apache y MySQL (verde, puerto 3306)

# 2. http://localhost/phpmyadmin → Nueva → <nombre_base> · utf8mb4_unicode_ci → Crear

# 3. En .env (sin # y sin espacios alrededor del =):
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=<nombre_base>
DB_USERNAME=root
DB_PASSWORD=

# 4. Dentro de la carpeta del proyecto (donde está artisan):
php artisan migrate
php artisan migrate:status     # todas deben decir "Ran"

# 5. Copiar la misma forma (SIN secretos) a .env.example
```

---

## La idea en una tabla

| | SQLite | MySQL |
|---|---|---|
| Qué es | Un **archivo** (`database/database.sqlite`) | Un **servidor** aparte (XAMPP) |
| Analogía | Un cuaderno en la mochila | Una bodega con guardia |
| Necesita | Nada | Host, puerto, base, usuario y contraseña |

| Qué se crea | Quién lo crea |
|---|---|
| La **base** (la bodega vacía) | **Yo**, en phpMyAdmin, **antes** de `migrate` |
| Las **tablas** (los estantes) | `php artisan migrate` |

---

## Paso 1 — Encender MySQL

1. Abrir **XAMPP Control Panel** (`C:\xampp\xampp-control.exe`).
2. **Start** en **MySQL** y en **Apache** (Apache lo necesita phpMyAdmin).

✔ Las dos filas en **verde**; MySQL muestra el puerto `3306`.

## Paso 2 — Crear la base en phpMyAdmin

1. Abrir `http://localhost/phpmyadmin`.
2. Columna izquierda → **Nueva**.
3. Nombre: `<nombre_base>` (minúsculas, guion bajo). Cotejamiento: `utf8mb4_unicode_ci`.
4. **Crear**.

✔ La base aparece a la izquierda y dice "No se han encontrado tablas".

> `utf8mb4` acepta acentos, ñ y emojis. Es el mismo cotejamiento que usa Laravel por
> defecto (`config/database.php`).

## Paso 3 — Editar `.env`

| Línea | Qué hacer |
|---|---|
| `DB_CONNECTION` | `sqlite` → `mysql` (minúsculas) |
| `DB_HOST`, `DB_PORT`, `DB_USERNAME`, `DB_PASSWORD` | Quitar el `#` (los valores de XAMPP ya vienen bien) |
| `DB_DATABASE` | Quitar el `#` **y cambiar** `laravel` por `<nombre_base>` |

- Atajo VS Code: seleccionar las líneas → **Ctrl+K, Ctrl+U** quita los comentarios.
- **Guardar con Ctrl+S** (si la pestaña tiene ●, no está guardado).

✔ Las 6 líneas empiezan en la columna 1, sin `#` ni espacios, y sin espacios alrededor del `=`.

### ¿Por qué `DB_DATABASE` es una trampa?

En `config/database.php`:
```php
'database' => env('DB_DATABASE', 'laravel'),
//               ① busca en .env   ② respaldo si NO existe
```
Si solo quitas el `#`, el `.env` dice `laravel`, y esa base no existe → error `1049`.

## Paso 4 — Crear las tablas

```bash
cd <ruta-del-proyecto>      # el prompt debe terminar en la carpeta donde está artisan
php artisan migrate
```

✔ Cada migración termina en `DONE`. En phpMyAdmin (F5) aparecen las tablas.

En Laravel 12 nuevo salen **9 tablas**: `users`, `password_reset_tokens`, `sessions`,
`cache`, `cache_locks`, `jobs`, `job_batches`, `failed_jobs` y `migrations`.

> Si `migrate` pregunta si quieres **crear** la base, es porque no existe: revisa el
> paso 2 y el nombre en `.env` antes de contestar.

## Paso 5 — Actualizar `.env.example` y comprobar

1. Copiar la **misma sección `DB_*`** a `.env.example` (sin contraseñas reales).
   Laravel **solo lee `.env`**; `.env.example` es la plantilla que recibe el equipo.
2. Comprobar qué está leyendo Laravel:
   ```bash
   php artisan config:show database.default     # debe decir mysql
   php artisan migrate:status                   # todas en "Ran"
   ```
3. `git status` muestra `.env.example` pero **nunca** `.env` (está en `.gitignore`).

---

## ✅ Checklist final

- [ ] MySQL en verde en XAMPP
- [ ] La base existe en phpMyAdmin con `utf8mb4_unicode_ci`
- [ ] `.env` con las 6 líneas `DB_*` activas y el nombre exacto de la base
- [ ] `php artisan migrate:status` muestra todo en `Ran`
- [ ] `config:show database.default` dice `mysql`
- [ ] `.env.example` actualizado sin secretos
- [ ] `.env` **no** aparece en `git status`

---

## 🐞 Errores comunes

Cómo leerlos: el **código** y el **mensaje** después de `SQLSTATE` dicen la causa; lo
que va entre paréntesis (`Connection`, `Host`, `Database`) son los valores que Laravel
**leyó** de tu `.env`. Lo que dice `vendor\...` no se toca.

| Mensaje | Causa | Solución |
|---|---|---|
| `[2002] ...denegó expresamente dicha conexión` | MySQL apagado | Start en MySQL (XAMPP) |
| `[1049] Unknown database 'x'` | La base no existe o el nombre en `.env` es distinto | Copiar el nombre **exacto** de phpMyAdmin a `DB_DATABASE` |
| `1146 Table 'base.tabla' doesn't exist` | La base existe pero sin tablas | `php artisan migrate` |
| `Could not open input file: artisan` | Terminal en la carpeta equivocada | `cd` a la carpeta del proyecto |
| `Unknown database 'laravel'` | `DB_DATABASE` comentado o sin cambiar | Quitar `#` y poner el nombre real |
| La página de inicio da error de BD sin consultar datos | `SESSION_DRIVER=database`: las sesiones viven en MySQL | Encender MySQL y correr `migrate` |

---

## 📌 Recordatorios clave

- **Base** = la creo yo · **tablas** = las crea `migrate`.
- La tabla `migrations` guarda qué migraciones ya corrieron (nombre + `batch`), por eso
  `migrate` no duplica nada.
- `env('X', 'respaldo')`: usa el valor de `.env`; **solo si falta**, usa el respaldo.
- En los errores, `base.tabla`: el punto separa la base de la tabla.
- `.env` = mis valores (no va a Git) · `config/*.php` = cómo se usan (sí va a Git).
