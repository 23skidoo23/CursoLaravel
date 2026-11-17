# APRENDIZAJE.md — Mi bitácora de capacitación (Mini Pedidos)

> Claude lee este archivo al inicio de cada sesión para saber en qué voy.
> Aquí guardo lo que aprendo, mis dudas y mis errores frecuentes.

---

## 📍 Estado actual

- **Semana:** 1 — Entorno, PHP esencial y el flujo de Laravel
- **Tema en curso:** [Base] Instalar herramientas y crear el proyecto
- **Paso del ciclo:** 6 — Comprobar y guardar. `/examen` hecho el 2026-11-17: **6/10** (ver abajo)
- **Adelantado:** parte del tema [Profundizar] Git diario → remote, push (ver guía 03)
- **Siguiente:** repaso de 10 min (los 3 puntos débiles del examen) → tema 2: configurar `.env` con MySQL de XAMPP
  (el tema 2 refuerza justo lo que falló: `.env`, `APP_KEY`, `key:generate`)
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
| `/aprender <tema>` | Al empezar un tema (paso 1) | Analogía, ejemplo mínimo, archivos a tocar, 2 errores típicos |
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
├── .claude/commands/      ← /aprender, /revisar, /examen
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

_(Agregar aquí lo que no me quede claro.)_

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
