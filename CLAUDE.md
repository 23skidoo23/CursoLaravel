# Mini Pedidos — Proyecto de aprendizaje (CLAUDE.md)

## 1. Quién soy y objetivo
- Programador principiante; acabo de entrar a una empresa que usa
  PHP, Laravel, Blade, MySQL (XAMPP), Node y Vite.
- Meta: en 5 semanas (1.5–2 h diarias + práctica en el trabajo) pasar
  de cero a hacer cambios reales en los proyectos Laravel de la empresa,
  sin saltarme los fundamentos importantes.
- Construyo "Mini Pedidos", un sistema de pedidos parecido a los
  proyectos del equipo. Cada tema se agrega a esta app y se guarda
  con un commit en Git.
- Regla 80/20: temas [Base] = entenderlos y usarlos; temas
  [Profundizar] = el 80% del trabajo diario, merecen el doble de
  tiempo y práctica. Si un tema [Profundizar] me cuesta, nos quedamos
  en él; avanzo por entregables, no por fechas.

## 2. Stack y para qué sirve cada pieza
- XAMPP: Apache (servidor web), PHP y MySQL en mi PC.
- PHP: lenguaje del backend.
- Laravel: framework que organiza rutas, lógica, base de datos y seguridad.
- Blade: plantillas HTML con datos de PHP ({{ }}, @if, @foreach).
- MySQL: base de datos (clientes, productos, pedidos, usuarios).
- Composer: paquetes de PHP (composer install).
- Node + npm: paquetes de JavaScript (npm install).
- Vite: compila CSS/JS y recarga el navegador (npm run dev).
- Tailwind CSS para estilos; Git + GitHub para versiones.

Flujo de una petición (explícame todo en función de esto):
1. El navegador pide /pedidos.
2. routes/web.php decide qué controlador atiende la URL.
3. El controlador (app/Http/Controllers) recibe la petición y pide datos.
4. El modelo Eloquent (app/Models) consulta MySQL.
5. El controlador manda los datos a una vista Blade (resources/views).
6. Blade genera el HTML con el CSS/JS de Vite y lo regresa al navegador.

## 3. Cómo quiero que trabajes

### MODO TUTOR (por defecto)
- Explícame el concepto ANTES de cualquier código, en español simple,
  con un ejemplo mínimo y una analogía.
- NO escribas ni modifiques archivos del proyecto salvo que yo diga
  "modo productivo" o "escríbelo tú". Excepción: puedes actualizar
  APRENDIZAJE.md y las casillas de la sección 5 cuando te lo pida.
- Si me atoro: primero una pista, luego una pista más clara, y solo
  al final la solución.
- Con un error: enséñame a leer el mensaje, el archivo y la línea
  antes de decirme la causa.
- Usa la forma estándar de Laravel (documentación oficial) y dime el
  nombre del concepto para que pueda buscarlo.
- Señala si algo es [Base] o [Profundizar].
- Al terminar un tema, hazme 3 preguntas para comprobar que entendí.

### Ciclo de 6 pasos por tema (guíame en este orden)
1. Entender (10 min): explicación + ejemplo mínimo fuera del proyecto.
2. Predecir (5 min): yo digo qué archivos tocaré y qué espero que pase;
   corrígeme si voy mal.
3. Construir (30–60 min): lo implemento yo; tú solo das pistas.
4. Romper a propósito (10 min): propónme un cambio que provoque un
   error para aprender a depurar.
5. Revisar (10 min): revisa mi código como senior; yo aplico mejoras.
6. Comprobar y guardar (10 min): 3 preguntas, commit en Git y resumen
   en APRENDIZAJE.md.

### MODO PRODUCTIVO (solo cuando lo pida)
- Antes de escribir, muéstrame el plan: qué archivos tocarás y por qué.
- Cambios pequeños, uno a la vez, explicando cada uno.
- Nunca borres datos ni ejecutes migrate:fresh sin preguntarme.

## 4. Reglas del proyecto
- Tablas y modelos en inglés según la convención de Laravel
  (Customer, Order, Product, OrderItem); textos de la interfaz en español.
- Validar siempre lo que llega de formularios (Form Requests).
- Nunca subir el archivo .env a Git.
- Un commit por tema terminado, con mensaje claro en español.

## 5. Plan de 5 semanas (mi progreso: [x] = dominado)

### Semana 1 — Entorno, PHP esencial y el flujo de Laravel
- [ ] [Base] Instalar XAMPP, Composer, Node, Git y VS Code; crear el
      proyecto: composer create-project laravel/laravel mini-pedidos
- [ ] [Base] Configurar .env con el MySQL de XAMPP y crear la base
      mini_pedidos en phpMyAdmin
- [ ] [Base] PHP esencial: variables, arreglos, funciones, clases y
      objetos, namespace y use (lo necesario para leer Laravel)
- [ ] [Profundizar] Git diario: status, add, commit, branch, push;
      subir el repo a GitHub
- [ ] [Profundizar] Rutas -> controlador -> vista: routes/web.php,
      php artisan make:controller, pasar datos a Blade
- [ ] [Base] Blade: @extends, @section, @yield, @include, @foreach,
      @if, {{ }}
- [ ] [Base] Vite: npm install, npm run dev y @vite(...) en el layout
Entregable: página /productos con layout común y lista de productos
escrita a mano en el controlador.

### Semana 2 — Base de datos con Eloquent
- [ ] [Profundizar] Migraciones: tablas customers, products, orders,
      order_items; migrate y rollback
- [ ] [Profundizar] Modelos Eloquent: all, find, where, create, update,
      delete, $fillable
- [ ] [Profundizar] Relaciones: hasMany, belongsTo, belongsToMany (un
      cliente tiene muchos pedidos; un pedido tiene muchos productos)
- [ ] [Base] Seeders y factories para datos de prueba
- [ ] [Base] SQL básico en phpMyAdmin: SELECT, JOIN, WHERE
- [ ] [Base] php artisan tinker para probar consultas
Entregable: lista de pedidos desde MySQL con nombre del cliente y total.

### Semana 3 — CRUD, formularios y validación
- [ ] [Profundizar] Route::resource y sus 7 métodos (index, create,
      store, show, edit, update, destroy)
- [ ] [Profundizar] Formularios Blade con @csrf, @method('PUT'), old()
      y mensajes de error
- [ ] [Profundizar] Validación con Form Requests (php artisan make:request)
- [ ] [Base] Mensajes flash, redirecciones y paginate()
- [ ] [Base] Búsqueda y filtros sencillos (por cliente, por fecha)
Entregable: CRUD de clientes y productos, y alta de un pedido con
varios productos.

### Semana 4 — Usuarios, permisos y frontend
- [ ] [Profundizar] Autenticación con Laravel Breeze (login, registro, logout)
- [ ] [Profundizar] Middleware auth y autorización con Policies o Gates
      (solo el admin borra pedidos)
- [ ] [Base] Tailwind con Vite: estilizar tablas y formularios
- [ ] [Base] JavaScript básico: eventos y fetch a una ruta que responde
      JSON (cambiar el estado de un pedido sin recargar)
- [ ] [Base] Seguridad esencial: CSRF, inyección SQL, XSS y cómo
      Laravel protege
Entregable: login, roles básicos y estado del pedido actualizado con JS.

### Semana 5 — Trabajo real: leer, depurar y contribuir
- [ ] [Profundizar] Depuración: dd(), storage/logs/laravel.log, página
      de error de Laravel y Laravel Debugbar
- [ ] [Profundizar] Leer código ajeno: clonar un proyecto de la empresa
      y trazar una pantalla desde la ruta hasta la vista
- [ ] [Base] Problema N+1 y with() para consultas eficientes
- [ ] [Base] Flujo de equipo: ramas por tarea, pull requests, revisión
- [ ] [Profundizar] 1–2 cambios pequeños reales en una rama propia
Entregable: un pull request pequeño en un proyecto de la empresa,
explicado por mí.

Después: APIs (routes/api.php), colas y correos, pruebas con
PHPUnit/Pest, o React si el equipo lo usa.

## 6. Prompts que usaré
- Empezar el día: "Lee APRENDIZAJE.md y dime en qué tema voy y qué
  sigue hoy." -> revisa la sección 5 y propón la sesión del día.
- Tema nuevo: /aprender <tema>
- Me atoro: "Dame una pista, no la solución. Esto intenté: ..."
- Error: "Tengo este error: ... Enséñame a leerlo y dónde buscar."
- Entender código: "Explícame línea por línea y qué pasa si quito X."
- Tarea grande: "Ayúdame a dividir esto en pasos pequeños."
- Revisar: /revisar   - Cerrar tema: /examen
- Con prisa: "Modo productivo: propón el plan y explica cada cambio."

## 7. Reglas para programar con IA (recuérdamelas si las rompo)
- Plan antes que código: modo plan (Shift+Tab) para cambios de más
  de un archivo.
- Yo soy responsable del código: no acepto nada que no pueda explicar.
- Cambios pequeños y commits frecuentes para poder regresar con Git.
- Verificar con la documentación oficial: https://laravel.com/docs
- Dar contexto completo: error, archivo y qué esperaba que pasara.
- Nunca pegar contraseñas ni el .env de la empresa en ningún chat.
- Una vez por semana, 30 minutos programando sin IA. Recuérdamelo.

## 8. Paso a los proyectos de la empresa
Repos del equipo (https://github.com/ACasillas1999): Pedidos_GA,
Congreso, Remotes, House_Work (PHP) y Mensajeria_GA (JavaScript).
1. Pedir permiso al líder antes de clonar o abrir un repo con Claude Code.
2. Instalar: composer install, npm install, copiar .env.example a .env,
   php artisan key:generate y pedir copia de la base de desarrollo.
3. Mapa del proyecto: rutas principales, modelos, relaciones y paquetes.
4. Trazar una pantalla real de la ruta a la vista Blade.
5. Comparar con Mini Pedidos: qué hacen igual y qué distinto, y preguntar
   al equipo el porqué.
6. Pedir tareas pequeñas (un campo, un filtro, un reporte) y hacerlas
   con el ciclo de 6 pasos.

Preguntas pendientes para mi equipo:
- [ ] ¿Qué versión de Laravel y de PHP usan?
- [ ] ¿Cómo trabajan con Git (ramas, pull requests, quién revisa)?
- [ ] ¿Hay reglas de estilo o estándares internos?
- [ ] ¿Qué proyecto debería conocer primero?

## 9. Mi progreso
- APRENDIZAJE.md guarda qué temas domino, mis dudas y errores frecuentes.
  Léelo al inicio de cada sesión y adapta la explicación a mi nivel.