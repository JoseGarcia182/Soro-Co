# Soro & Co — RSVP System
## Guía completa de configuración

---

## ARCHIVOS DEL PROYECTO

```
soro-rsvp/
├── index.html        → Página pública de RSVP (para los invitados)
├── admin.html        → Panel administrativo (solo para el cliente)
├── apps-script.gs    → Código para pegar en Google Apps Script
└── README.md         → Esta guía
```

---

## PASO 1 — Crear el Google Sheet

1. Entrá a https://sheets.google.com
2. Creá una hoja nueva y ponele el nombre que quieras (ej: "RSVP Soro & Co")
3. Dejá la hoja abierta, la vas a necesitar en el paso 2

---

## PASO 2 — Configurar el Apps Script

1. En el Google Sheet, hacé clic en el menú **Extensions > Apps Script**
2. Borrá todo el código que aparece por defecto
3. Copiá todo el contenido del archivo `apps-script.gs` y pegalo
4. Hacé clic en 💾 **Save** (el ícono del diskette)
5. Hacé clic en **Deploy > New deployment**
6. En "Select type" elegí **Web app**
7. Completá así:
   - Description: `RSVP Soro Co`
   - Execute as: **Me**
   - Who has access: **Anyone**
8. Hacé clic en **Deploy**
9. Google te va a pedir que autorices → hacé clic en **Authorize access**
   - Elegí tu cuenta de Google
   - Si aparece "Google hasn't verified this app" → clic en **Advanced > Go to RSVP Soro Co (unsafe)**
   - Hacé clic en **Allow**
10. ✅ Copiá la URL que aparece. Tiene esta forma:
    ```
    https://script.google.com/macros/s/XXXXXXXXXXXXXXXXX/exec
    ```
    **Guardá esta URL, la necesitás en el paso 3.**

---

## PASO 3 — Configurar los archivos HTML

### En `index.html`
Buscá esta línea (cerca del final, en el `<script>`):
```js
const APPS_SCRIPT_URL = 'PEGAR_URL_DE_APPS_SCRIPT_AQUI';
```
Reemplazá `PEGAR_URL_DE_APPS_SCRIPT_AQUI` con la URL del paso 2.

### En `admin.html`
Buscá estas dos líneas:
```js
const APPS_SCRIPT_URL = 'PEGAR_URL_DE_APPS_SCRIPT_AQUI';
const ADMIN_PASSWORD  = 'PEGAR_CONTRASENA_AQUI';
```
- Reemplazá la URL con la del paso 2
- Reemplazá `PEGAR_CONTRASENA_AQUI` con la contraseña que quieras
  (ej: `'soroAdmin2024'`)

---

## PASO 4 — Subir a Vercel

1. Creá una carpeta en tu computadora con los 2 archivos:
   - `index.html`
   - `admin.html`

2. Entrá a https://github.com, creá un repositorio nuevo llamado `soro-rsvp`
   y subí ambos archivos (arrastrá y soltá)

3. Entrá a https://vercel.com, conectá con GitHub
   - Seleccioná el repositorio `soro-rsvp`
   - Hacé clic en **Deploy** (sin cambiar nada)

4. ✅ Vercel te da dos URLs:
   - `https://soro-rsvp.vercel.app` → Para los invitados
   - `https://soro-rsvp.vercel.app/admin` → Para el cliente (con contraseña)

---

## RESUMEN RÁPIDO

| Qué                | URL                                      |
|--------------------|------------------------------------------|
| Página de invitados | `tu-proyecto.vercel.app`                |
| Panel admin        | `tu-proyecto.vercel.app/admin`           |
| Google Sheet       | El que creaste en el Paso 1             |

---

## CÓMO FUNCIONA

```
Invitado llena el form → index.html → Apps Script → Google Sheet
                                                         ↑
Cliente abre admin.html → ingresa contraseña → lee Sheet → ve la tabla → descarga Excel
```

---

## PREGUNTAS FRECUENTES

**¿Qué pasa si el invitado manda el form y no aparece en el Sheet?**
→ Verificá que la URL del Apps Script esté bien pegada en `index.html`

**¿Puedo cambiar la contraseña del admin?**
→ Sí, editá la variable `ADMIN_PASSWORD` en `admin.html` y volvé a hacer push a GitHub. Vercel se actualiza solo.

**¿El Google Sheet se actualiza en tiempo real?**
→ Cada vez que alguien confirma, aparece en el Sheet de inmediato. En el panel admin, usá el botón "Actualizar" para recargar los datos.

**¿Puedo agregar más columnas al Sheet?**
→ Para agregar datos (como email), tenés que modificar el formulario en `index.html` y el Apps Script en `apps-script.gs`.
