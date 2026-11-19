# APRENDIZAJE.md — Mi bitácora de capacitación (Mini Pedidos)

> Claude lee este archivo al inicio de cada sesión para saber en qué voy.
> Aquí guardo lo que aprendo, mis dudas y mis errores frecuentes.

---

## 📍 Estado actual

- **Semana:** 1 — Entorno, PHP esencial y el flujo de Laravel
- **Tema en curso:** [Base] Tema 2 — Configurar `.env` con el MySQL de XAMPP y crear la base `mini_pedidos`
- **Paso del ciclo:** 6 — Comprobar y guardar. `/examen` hecho el 2026-11-19: **7/10** (ver Sesión 2). Falta: guía 02 + commit
- **Repaso del examen 2:** ✅ hecho el 2026-11-19 (`env()` con respaldo ya claro; `base.tabla`; sesiones en MySQL)
- **Tema 1:** examen 6/10 + ✅ repaso hecho el 2026-11-18 (3/3; reforzar "punto = carpeta" en `view()`)
- **Adelantado:** parte del tema [Profundizar] Git diario → remote, push (ver guía 03)
- **Casillas:** temas 1 y 2 **sin `[x]` a propósito** → al retomarlos, re-explicar más despacio y fácil (ver "Dudas abiertas")
- **Siguiente:** ✅ guía 02 creada → **commit** (bitácora + guías + `.env.example`) → tema 3 [Base]: PHP esencial
  (el tema 3 refuerza funciones con valor por defecto, como `env('X', 'respaldo')`)
- **Guías:** ver carpeta `guias/` (procesos para repetir en otros proyectos)

---

## ✅ Temas dominados

_(Todavía ninguno. Se agregan aquí al aprobar el /examen del tema.)_

---

## 📒 Notas por sesión

### Sesión 1 — 2026-11-14 / 2026-11-17 · Preparar el entorno [Base]

#### 1. Cómo trabajo con Claude Code

- **Modo tutor (por defecto):** Claude explica primero, yo escribo el código,
  él solo da pistas. Para que él escriba debo decir **"modo productivo"** o
  **"escríbelo tú"**.
- **Ciclo de 6 pasos por tema:**
  Entender → Predecir → Construir → Romper a propósito → Revisar → Comprobar y guardar.
- **Comandos personalizados** (viven en `.claude/commands/*.md`, se usan con `/`):

| Comando | Cuándo | Qué hace |
|---|---|---|
| `/progreso` | Al abrir sesión o tras `/clear` | Dice semana, tema, paso del ciclo y qué comando sigue. No toca archivos |
| `/siguiente` | Para avanzar (opcional: `/siguiente <tema>`) | Continúa el paso pendiente o empieza el siguiente tema del plan |
| `/revisar` | Antes del commit (paso 5) | Revisa mi `git diff` como senior. **Necesita Git.** |
| `/examen` | Al cerrar el tema (paso 6) | 3 preguntas, calificación y resumen aquí |

- **`CLAUDE.md`** = las reglas del proyecto; Claude lo lee solo al iniciar.

#### 2. Estado de mi PC

| Herramienta | Versión | Nota |
|---|---|---|
| XAMPP (Apache + MySQL) | — | Instalado en `C:\xampp` |
| PHP | 8.2.12 | Viene con XAMPP en `C:\xampp\php` |
| Composer | 2.11.2 | Instalado con `Composer-Setup.exe` |
| Node / npm | 26.13 / 11.20 | Ya estaba |
| Git | 2.50.1 | Ya estaba |

#### 3. Concepto: el PATH de Windows [Base]

- **Qué es:** la lista de carpetas donde Windows busca los programas que
  escribo en la terminal.
- **Analogía:** es la **agenda de contactos** de la terminal. Si
  `C:\xampp\php` no está en la agenda, al escribir `php` responde
  "no encontrado", aunque PHP sí esté instalado.
- **Cómo lo arreglé:** Variables de entorno → **Path de usuario** (cuadro de
  arriba) → Editar → Nuevo → `C:\xampp\php`.
- **Usuario vs Sistema:** el de usuario solo me afecta a mí y no pide permisos
  de administrador. El de sistema afecta a todos los usuarios de la PC.
- ⚠️ **No confundir con `PATHEXT`** (son las extensiones ejecutables, no se toca).
- ⚠️ Después de cambiar el PATH hay que **cerrar y reabrir** VS Code/terminal.
- **Comprobar:** `php --version`

#### 4. Concepto: Composer [Base]

- **Qué es:** el gestor de paquetes de PHP.
- **Analogía:** un **supermercado con lista de compras**.
  - `composer.json` = la lista de compras (qué paquetes necesita el proyecto).
  - `vendor/` = la despensa donde Composer guarda lo que "compró".
- **Comandos clave:**
  - `composer create-project laravel/laravel <nombre>` → descarga la plantilla
    completa de Laravel **y** instala sus paquetes.
  - `composer install` → lee `composer.json` y vuelve a descargar `vendor/`
    (lo usaré al clonar proyectos de la empresa).
- **Instalación en Windows:** `Composer-Setup.exe`, apuntando a
  `C:\xampp\php\php.exe`.

#### 5. Versiones de Laravel y PHP

- **Laravel 13** (el más nuevo) pide **PHP 8.3** o mayor.
- Mi XAMPP trae **PHP 8.2**, así que Composer elegirá solo **Laravel 12**.
- **Composer siempre elige la versión más nueva que sea compatible con mi PHP.**
- Para aprender da igual: rutas, controladores, Blade y Eloquent funcionan igual.
- ❓ Preguntar al equipo qué versión usan ellos.

#### 6. Estructura inicial de un proyecto Laravel (esperada)

> Hecha a partir de mi predicción. **Verificar** cuando corra `create-project`.

```
CursoLaravel/
├── CLAUDE.md              ← reglas para Claude
├── APRENDIZAJE.md         ← esta bitácora
├── .claude/commands/      ← /progreso, /siguiente, /revisar, /examen
└── mini-pedidos/          ← lo crea Composer
    ├── app/               ← Controladores y Modelos (pasos 3 y 4 del flujo)
    ├── routes/web.php     ← las rutas (paso 2 del flujo)
    ├── resources/views/   ← las vistas Blade (paso 5)
    ├── database/          ← migraciones y seeders (Semana 2)
    ├── public/            ← lo único que ve el navegador
    ├── vendor/            ← paquetes descargados (NO va a Git)
    ├── .env               ← configuración secreta de MI PC (NO va a Git)
    ├── composer.json      ← la "lista de compras"
    └── artisan            ← herramienta de comandos de Laravel
```

**Por ahora solo importan:** `app/`, `routes/` y `resources/views/`.

#### 7. Concepto clave: `vendor/` vs `.env` (¡pregunta de examen!)

**Ninguno se sube a Git, pero por razones DISTINTAS:**

| | `vendor/` | `.env` |
|---|---|---|
| **Contiene** | Código de **otros** (Laravel y paquetes) | **Mis secretos** (contraseña de MySQL, APP_KEY) |
| **No se sube porque…** | Es **pesado y se regenera** con `composer install` | Es **peligroso**: expone contraseñas |
| **Analogía** | Los ingredientes: se manda la **receta** (`composer.json`) | Las **llaves de mi casa**: nunca se comparten |
| **En otro proyecto…** | `composer install` | Copiar `.env.example` a `.env` |

#### 8. El flujo de una petición (la idea central de todo el curso)

```
Navegador pide /pedidos
   → routes/web.php            (¿quién atiende esta URL?)
   → Controlador               (app/Http/Controllers)
   → Modelo Eloquent           (app/Models) → consulta MySQL
   → Controlador manda datos a la vista
   → Vista Blade               (resources/views) → genera HTML
   → Navegador recibe la página
```

#### 9. Git: primeros pasos [Profundizar]

- **Analogía:** Git = **puntos de guardado** de un videojuego; GitHub = las
  partidas **en la nube**.
- `git add` → mete cambios a la **staging area** (la caja). Permite elegir
  **qué** guardar.
- `git commit -m "..."` → cierra la caja con etiqueta y la guarda en el historial.
- `git status` resume carpetas nuevas: ver `mini-pedidos/` **no** significa
  que `vendor/` vaya incluido. Comprobar con `git check-ignore -v <ruta>`.
- `.gitignore` es un **archivo**, y filtra en el `add`, no en el `push`.
- ⚠️ Si un archivo ya entró en un commit, agregarlo después al `.gitignore`
  **no** lo borra del historial.
- `git push -u origin main` → sube los commits; `origin` = apodo del remoto.
- `warning: LF will be replaced by CRLF` → saltos de línea Linux/Windows;
  solo es un aviso.
- **Página de error de Laravel:** aparece porque `.env` tiene `APP_DEBUG=true`.
- **Puertos:** 8000 = `artisan serve` · 80 = Apache · 3306 = MySQL.

#### 10. 📝 Examen del tema 1 — 2026-11-17 · Calificación: **6/10**

| # | Tipo | Pregunta | Nota | Qué faltó |
|---|---|---|---|---|
| 1 | Concepto | Compañero clona el repo sin `vendor/` ni `.env` | 6/10 | Razones bien. Faltaron los pasos: `composer install`, copiar `.env.example` → `.env`, `php artisan key:generate` |
| 2 | Leer código | `.gitignore` y `routes/web.php` | 6/10 | `/public/build` = CSS/JS compilado por Vite. `'/'` = la **URL** raíz. `view('welcome')` → `resources/views/welcome.blade.php` |
| 3 | Error | `MissingAppKeyException` | 6/10 | Comando bien. Pero el archivo a revisar es **`.env`, línea `APP_KEY=`** (vacía), no la clase del error |

**Lo que aprendí del examen:**
- **Regla general de `.gitignore`:** lo que se puede **regenerar con un comando**
  (`vendor/`, `node_modules/`, `public/build/`) o que es **secreto** (`.env`) no se sube.
- **Receta → despensa:** `composer.json` → `vendor/` (`composer install`) ·
  `package.json` → `node_modules/` (`npm install`).
- **Leer un error de Laravel:** el nombre de la clase (`Illuminate\...\MissingAppKeyException`)
  dice **qué tipo** de error es. Ese archivo vive en `vendor/` y **nunca se edita**.
  La causa casi siempre está en **mi** código o en **mi** `.env`.
- `php artisan key:generate` **escribe** una clave aleatoria en `APP_KEY=` dentro de `.env`.

**Repasar antes del tema 2:**
1. Los 3 pasos después de clonar un proyecto Laravel (decirlos de memoria).
2. Qué es `'/'` en `Route::get('/', ...)` y dónde busca Laravel una vista.
3. Abrir `mini-pedidos/.env` y ubicar `APP_KEY`, `APP_DEBUG` y `DB_*`.

> ✅ Repaso hecho el 2026-11-18 (3/3). Lo que más costó: **en `view()`, punto = `/`**
> → `view('productos.lista')` = `resources/views/productos/lista.blade.php`.

### Sesión 2 — 2026-11-18 / 2026-11-19 · Configurar `.env` con MySQL de XAMPP [Base]

#### 1. SQLite vs MySQL

- `DB_CONNECTION` = el **motor** (tipo de base de datos). Se escribe en minúsculas: `mysql`.
- **SQLite** = un **cuaderno**: un archivo (`database/database.sqlite`). Es el default de Laravel 12.
- **MySQL** = una **bodega con guardia**: servidor aparte (XAMPP, puerto 3306) que pide
  dirección (`DB_HOST`), puerta (`DB_PORT`), almacén (`DB_DATABASE`) y gafete (`DB_USERNAME` / `DB_PASSWORD`).
- En `.env`, `#` = **comentario**: Laravel ignora la línea.

#### 2. Quién crea qué

| Qué | Quién | Cuándo |
|---|---|---|
| La **base** `mini_pedidos` (la bodega) | **Yo**, en phpMyAdmin (`utf8mb4_unicode_ci`) | Antes de `migrate` |
| Las **tablas** (los estantes) | `php artisan migrate` | Después |

- Las 3 migraciones de fábrica crean **9 tablas**: `users`, `password_reset_tokens`, `sessions`,
  `cache`, `cache_locks`, `jobs`, `job_batches`, `failed_jobs` + `migrations`.
- La tabla **`migrations`** guarda el **nombre de cada migración ya corrida** y su **`batch`** (lote).
  Por eso correr `migrate` otra vez dice *"Nothing to migrate"*. `migrate:rollback` deshace el último lote.
- `php artisan migrate:status` → lista de migraciones con `Ran` (solo lee, sirve para probar la conexión).

#### 3. `env()` y `config/` [¡pregunta de examen!]

```php
'database' => env('DB_DATABASE', 'laravel'),
//               ① busca en .env   ② valor de respaldo SI NO EXISTE
```

- `config/database.php` = **cómo** conectarse (código, va a Git, igual para todos).
- `.env` = **con qué valores** (secretos, no va a Git, distinto en cada PC).
- `php artisan config:show app.name` → ver qué valor está usando Laravel **de verdad**.

#### 4. Leer un error de base de datos

```
Illuminate\Database\QueryException        ← tipo (ignorar)
SQLSTATE[HY000] [2002] ...                ← ⭐ código + mensaje = la causa
(Connection: mysql, Host:..., Database:...) ← ⭐ valores que Laravel LEYÓ de mi .env
at vendor\...\Connection.php:838          ← vendor = no se toca
```

| Código | Significa | Reviso |
|---|---|---|
| `2002` | No hay servidor: MySQL apagado | Panel de XAMPP |
| `1049` | Unknown database: la base no existe o el nombre está mal | `DB_DATABASE` vs la lista de phpMyAdmin (la fuente de verdad) |
| `1146` | Table doesn't exist: la base existe pero sin tablas | Correr `php artisan migrate` |

- En un mensaje, `mini_pedidos.sessions` = **base.tabla** (el punto separa).

#### 5. Detalles de `.env`

- `APP_NAME="Mini Pedidos"` → con espacio va **entre comillas**.
- `APP_URL=http://localhost:8000` → igual que `artisan serve`.
- `SESSION_DRIVER=database` (y cache, queue) → sesiones en MySQL: con MySQL apagado o sin
  `migrate`, **ni la página de inicio abre**.
- **Laravel solo lee `.env`.** `.env.example` es la plantilla para el equipo: si cambio la
  *forma* de la configuración, la actualizo también, **sin secretos**.
- `.gitignore` tiene `.env` (línea 3), que **no** atrapa a `.env.example` (nombre distinto).
- Estructura: `CursoLaravel` es repo de capacitación (caso B). En proyectos normales el repo
  **es** la carpeta del proyecto (caso A). Antes de `php artisan`, **mirar la ruta del prompt**.

#### 6. 📝 Examen del tema 2 — 2026-11-19 · Calificación: **7/10**

| # | Tipo | Pregunta | Nota | Qué faltó |
|---|---|---|---|---|
| 1 | Concepto | ¿`migrate` crea la base? ¿Duplica tablas? | 8/10 | Bien. Faltó decir que `migrations` guarda **nombre + batch** de lo ya corrido |
| 2 | Leer código | `env('DB_DATABASE', 'laravel')` | 5/10 | Lo entendí al revés: `'laravel'` es el **respaldo** si falta la variable, no lo que siempre usa |
| 3 | Error | `1146 Table 'mini_pedidos.sessions' doesn't exist` | 7/10 | Causa y comando bien. `mini_pedidos.sessions` = base.tabla. Busca `sessions` por `SESSION_DRIVER=database`, no por `config:show` |

**Repasar antes del tema 3:**
1. Explicar en voz alta `env('X', 'respaldo')` con un ejemplo propio.
2. Leer `base.tabla` en un mensaje de error.
3. Abrir `config/session.php` y encontrar la línea con `env('SESSION_DRIVER', ...)`.

---

## 🧠 Preguntas de repaso (para practicar antes del /examen)

1. Instalé PHP, pero `php --version` dice "no encontrado". ¿Qué pasa y cómo lo arreglo?
2. ¿Qué diferencia hay entre `composer.json` y la carpeta `vendor/`?
3. ¿Por qué `vendor/` y `.env` no se suben a Git? Da **una razón distinta** para cada uno.
4. Clono un proyecto de la empresa y no tiene `vendor/` ni `.env`. ¿Qué dos cosas hago?
5. ¿Por qué Composer me instalará Laravel 12 y no el 13?
6. Una petición llega a `/productos`: ¿en qué orden pasa por rutas, controlador, modelo y vista?

---

## ❓ Dudas abiertas

> **Nota para Claude (2026-11-19):** las casillas de los temas 1 y 2 quedan **sin `[x]`**
> a propósito. Cuando se retome cualquiera de estos puntos, **volver a explicarlo más
> despacio, con más detalle y más fácil de digerir** (paso a paso, ejemplos concretos),
> porque la primera vez no me quedó claro. Marcar `[x]` solo cuando yo lo pida.

| Tema | Lo que no me quedó claro |
|---|---|
| 1 | Los 3 pasos después de clonar (olvidé copiar `.env.example` → `.env`) |
| 1 | `view('carpeta.archivo')`: el punto = carpeta; la ruta empieza en `resources/views/` |
| 2 | `env('X', 'respaldo')`: X es una **variable** del `.env`, el respaldo solo si falta |
| 2 | Leer `base.tabla` en mensajes de error |
| 2 | Por qué `SESSION_DRIVER=database` hace que cualquier página use MySQL |
| 2 | `.env` vs `.env.example`: cuál lee Laravel y cuál se comparte |

---

## 🐞 Errores frecuentes (míos)

| Error | Causa | Solución |
|---|---|---|
| `php: command not found` | `C:\xampp\php` no estaba en el PATH | Agregarlo al Path de usuario y reabrir la terminal |
| `Could not find package laraverl/laravel` | Error de dedo; Composer no adivina paquetes | Copiar y pegar comandos; comparar con el nombre entre corchetes/del mensaje |
| `Failed opening required vendor/autoload.php` | Usé artisan antes de que Composer terminara | Esperar el prompt `$`; en proyectos clonados: `composer install` |
| Junté las razones de `vendor/` y `.env` | Confusión de concepto | `vendor` = pesado y regenerable · `.env` = secretos |
| `No application encryption key has been specified` | `APP_KEY=` vacío en `.env` (proyecto clonado) | `php artisan key:generate` |
| Buscar la causa en la clase del error (`Illuminate\...`) | Ese código es de Laravel, vive en `vendor/` | Leer el **mensaje** y revisar **mi** `.env` o **mi** código |
| `Could not open input file: artisan` | Terminal en `CursoLaravel`, no en `mini-pedidos` | `cd mini-pedidos`; mirar la ruta del prompt |
| `SQLSTATE[HY000] [2002] ...denegó... la conexión` | MySQL apagado en XAMPP | Start en MySQL |
| `SQLSTATE[HY000] [1049] Unknown database` | Nombre en `DB_DATABASE` distinto al de phpMyAdmin | Copiar el nombre exacto de phpMyAdmin |
| `1146 Table '...sessions' doesn't exist` | Base creada pero sin `migrate` | `php artisan migrate` |
| Cambié `APP_NAME` solo en `.env.example` | Creí que Laravel lo leía | Laravel **solo lee `.env`**; cambiar los dos |
| Errores de dedo (`pedidios`, `producto`) | Escribir de memoria | Copiar nombres exactos; comprobar con `config:show` |
