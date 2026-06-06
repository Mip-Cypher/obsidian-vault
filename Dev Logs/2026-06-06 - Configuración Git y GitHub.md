# Configuración Git + GitHub — Obsidian Vault
**Fecha:** 2026-06-06
**Hora aprox.:** 13:10–13:55 (UTC-5)
**Autor:** Mip-Cypher
**Repositorio:** https://github.com/Mip-Cypher/obsidian-vault

---

## Resumen

Se inicializó el vault de Obsidian como repositorio Git y se subió a GitHub por primera vez. El proceso requirió corregir varios errores: nombre de rama incorrecto, autenticación con token, y conflicto con la rama remota.

---

## Estado ANTES

| Elemento | Estado |
|---|---|
| Repositorio Git | No existía (carpeta sin `.git`) |
| Conexión con GitHub | Ninguna |
| Rama | N/A |
| Autenticación | N/A |

---

## Comandos ejecutados (en orden)

### 1. Inicializar el repositorio
```bash
git init
```
**Resultado:** `Initialized empty Git repository in .git/`
**Por qué:** El vault no tenía control de versiones. Se necesitaba para poder subir a GitHub.

---

### 2. Agregar el remoto
```bash
git remote add origin https://github.com/Mip-Cypher/obsidian-vault.git
```
**Resultado:** Exitoso (sin output = correcto)
**Por qué:** Enlaza el repositorio local con el repositorio creado en GitHub.

---

### 3. Agregar todos los archivos al staging
```bash
git add .
```
**Resultado:** Exitoso. Advertencias de `LF → CRLF` (normales en Windows) en:
- `.obsidian/community-plugins.json`
- `.obsidian/core-plugins.json`
- `.obsidian/graph.json`
- `.obsidian/plugins/obsidian-git/main.js`
- `.obsidian/plugins/obsidian-git/manifest.json`
- `.obsidian/plugins/obsidian-git/obsidian_askpass.sh`
- `.obsidian/plugins/obsidian-git/styles.css`
- `.obsidian/workspace.json`
- `Bienvenido.md`
- `boreka.md`

**Por qué:** Las advertencias LF/CRLF son normales en Windows. Git convierte los saltos de línea automáticamente.

---

### 4. Primer commit
```bash
git commit -m "initial vault commit"
```
**Resultado:** `[master (root-commit) d178d89] initial vault commit`

**Archivos incluidos (15 archivos, 1432 inserciones):**

| Archivo | Líneas |
|---|---|
| `.obsidian/app.json` | 1 |
| `.obsidian/appearance.json` | 1 |
| `.obsidian/community-plugins.json` | 3 |
| `.obsidian/core-plugins.json` | 33 |
| `.obsidian/graph.json` | 22 |
| `.obsidian/plugins/obsidian-git/main.js` | 415 |
| `.obsidian/plugins/obsidian-git/manifest.json` | 10 |
| `.obsidian/plugins/obsidian-git/obsidian_askpass.sh` | 23 |
| `.obsidian/plugins/obsidian-git/styles.css` | 705 |
| `.obsidian/workspace.json` | 209 |
| `Bienvenido.md` | 5 |
| `Pasted image 20260603183406.png` | (binario, 73 KB) |
| `Pasted image 20260603191659.png` | (binario, 58 KB) |
| `Sin título.md` | 1 |
| `boreka.md` | 4 |

---

### 5. Primer intento de push — ERROR (flag incorrecto)
```bash
git push -U origin main   # ❌ ERROR
```
**Error:** `error: unknown switch 'U'`
**Por qué falló:** La flag correcta es `-u` (minúscula), no `-U`.

---

### 6. Segundo intento de push — ERROR (rama incorrecta)
```bash
git push -u origin main   # ❌ ERROR
```
**Error:** `error: src refspec main does not match any`
**Por qué falló:** El commit se creó en la rama `master` (nombre por defecto de Git antiguo), pero se intentaba subir a `main`.

---

### 7. Renombrar la rama de `master` a `main`
```bash
git branch -m master main
```
**Resultado:** Exitoso.
**Por qué:** GitHub usa `main` como rama principal por defecto. Hay que alinear el nombre local.

**Estado antes:** rama `master`
**Estado después:** rama `main`

---

### 8. Tercer intento de push — ERROR (autenticación)
```bash
git push -u origin main   # ❌ ERROR
```
**Error:** `remote: Invalid username or token. Password authentication is not supported.`
**Por qué falló:** GitHub eliminó la autenticación por contraseña en agosto 2021. Se requiere un Personal Access Token (PAT).

---

### 9. Configurar autenticación con PAT
```bash
git remote set-url origin https://TOKEN@github.com/Mip-Cypher/obsidian-vault.git
```
**Resultado:** Exitoso.
**Por qué:** El token se embebe en la URL del remoto para que Git lo use automáticamente en futuros push/pull.

> ⚠️ **SEGURIDAD:** El token fue expuesto en el chat. Se recomienda revocarlo y generar uno nuevo en:
> GitHub → Settings → Developer settings → Personal access tokens

---

### 10. Cuarto intento de push — ERROR (conflicto remoto)
```bash
git push -u origin main   # ❌ ERROR
```
**Error:** `! [rejected] main -> main (fetch first)`
**Por qué falló:** El repositorio de GitHub fue creado con un README automático, lo que generó un commit remoto que no existía localmente.

---

### 11. Force push — EXITOSO ✅
```bash
git push -u origin main --force
```
**Resultado:** `+ 98199a2...d178d89 main -> main (forced update)`
**Por qué:** El contenido local era el correcto y el README de GitHub era descartable. El force push sobreescribió el historial remoto con el local.

---

## Estado DESPUÉS

| Elemento | Estado |
|---|---|
| Repositorio Git | ✅ Inicializado (`d178d89`) |
| Rama local | ✅ `main` |
| Rama remota | ✅ `origin/main` (tracking activo) |
| Conexión con GitHub | ✅ Autenticada con PAT |
| Archivos en GitHub | ✅ 15 archivos subidos |

---

## Consumo de tokens (estimado)

> No hay acceso directo al conteo exacto de tokens por respuesta en esta sesión. Los valores son estimaciones basadas en el tamaño de los mensajes.

| Turno | Tipo | Tokens estimados |
|---|---|---|
| Contexto inicial + comandos del usuario | Input | ~800 |
| Respuestas del asistente (x5) | Output | ~1,200 |
| Herramientas (Bash calls x6) | Tool use | ~400 |
| **Total estimado** | | **~2,400 tokens** |

Para ver el consumo exacto, revisa el panel de uso en: https://console.anthropic.com/usage

---

## Pendientes de seguridad

- [ ] Revocar el token (el que usaste, ya expuesto en el chat) en GitHub
- [ ] Generar un nuevo PAT y actualizar la URL del remoto:
  ```bash
  git remote set-url origin https://NUEVO_TOKEN@github.com/Mip-Cypher/obsidian-vault.git
  ```
- [ ] Considerar agregar `.obsidian/workspace.json` al `.gitignore` (cambia frecuentemente y no es relevante para sincronización entre dispositivos)

---

## Tags
#git #github #setup #devlog #obsidian
