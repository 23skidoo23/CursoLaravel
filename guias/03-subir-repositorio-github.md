# Guía 03 — Subir un repositorio local a GitHub

> **Cuándo usarla:** ya tienes un proyecto con `git init` y al menos un commit,
> y quieres respaldarlo en GitHub por primera vez.
>
> **Probado con:** Windows 11 · Git 2.50 + Git Credential Manager · GitHub web
> **Fecha:** 2026-11-17

---

## Resumen rápido (copiar y pegar)

```bash
# 0. Crear el repo VACÍO y PRIVADO en https://github.com/new (sin README)

# 1. Conectar y subir
cd /c/xampp/htdocs/<carpeta-del-repo>
git remote add origin https://github.com/<usuario>/<repo>.git
git remote -v
git push -u origin main

# Del día a día en adelante:
git add .
git commit -m "Mensaje claro en español"
git push
```

---

## Conceptos

| Término | Qué es |
|---|---|
| **Repositorio local** | El historial en tu PC (carpeta oculta `.git/`) |
| **Repositorio remoto** | La copia en GitHub (respaldo en la nube) |
| **`origin`** | Apodo de la dirección del remoto |
| **`git push`** | Sube tus **commits** al remoto |
| **`-u`** | Recuerda el destino; después basta con `git push` |

**Flujo completo:**
```
Tu PC ──git add──▶ staging ──git commit──▶ historial local ──git push──▶ GitHub
         ▲
   aquí filtra .gitignore
```
`push` solo sube lo que ya está en commits. Lo que `.gitignore` dejó fuera
(`vendor/`, `.env`) **nunca llega** a GitHub.

---

## Paso 1 — Antes de subir

```bash
git status            # debe decir "working tree clean"
git log --oneline     # ver qué commits se van a subir
```
- Confirma que **no** hay `.env`, contraseñas ni `vendor/` en ningún commit.
- ⚠️ Si un secreto **ya** está en un commit, agregarlo después al `.gitignore`
  **no lo borra** del historial. Pide ayuda antes de hacer push.

---

## Paso 2 — Crear el repositorio en GitHub

1. Ir a **https://github.com/new**.
2. **Repository name:** el nombre del proyecto.
3. **Private** si contiene información de la empresa, notas personales o datos
   internos.
4. **NO** marcar *Add a README*, *.gitignore* ni *license*. Si GitHub crea
   archivos, su historia choca con la tuya.
5. **Create repository** → copiar la URL `https://github.com/<usuario>/<repo>.git`.

---

## Paso 3 — Conectar y subir

```bash
git remote add origin https://github.com/<usuario>/<repo>.git
git remote -v                 # verificar la URL (fetch y push)
git push -u origin main
```

- **La primera vez** se abre el navegador para iniciar sesión (Git Credential
  Manager). Acepta y regresa a la terminal.
- Salida correcta (fragmento):
  ```
  * [new branch]      main -> main
  branch 'main' set up to track 'origin/main'.
  ```

---

## ✅ Checklist final

- [ ] En GitHub aparece el **candado 🔒 Private** junto al nombre
- [ ] El número de commits en GitHub = el de `git log --oneline`
- [ ] **No** aparecen `vendor/` ni `.env` en GitHub
- [ ] `git status` dice `Your branch is up to date with 'origin/main'`

---

## 🐞 Errores comunes

| Mensaje | Causa | Solución |
|---|---|---|
| `remote origin already exists` | Ya conectaste un remoto antes | Ver con `git remote -v`; cambiarlo con `git remote set-url origin <url>` |
| `src refspec main does not match any` | No hay commits o la rama se llama `master` | Hacer un commit primero; ver la rama con `git branch` |
| `rejected ... (fetch first)` | El repo de GitHub no estaba vacío (README creado) | `git pull origin main --allow-unrelated-histories`, resolver y volver a hacer push |
| `Authentication failed` / `403` | Sesión incorrecta o sin permisos | Volver a iniciar sesión; revisar que el repo sea tuyo |
| `Repository not found` | URL mal escrita o repo privado de otra cuenta | Revisar la URL con `git remote -v` |
