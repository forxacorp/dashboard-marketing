# Forxa Dashboard — Dashboard Ejecutivo de Marketing

Sitio web pequeño y 100% editable para el dashboard ejecutivo de marketing de
**Forxa Inmobiliaria**. Tiene dos páginas:

- **`index.html`** — el dashboard público (lo que ve dirección).
- **`admin.html`** — el Panel de Control, protegido con contraseña, donde se
  edita **cada dato, texto y color** del dashboard sin tocar código.

Todo el contenido vive en una base de datos gratuita de **Supabase**. El sitio
en sí es HTML/CSS/JS puro (sin frameworks, sin build), así que se despliega
en segundos en **Netlify**.

No necesitas saber programar para usarlo día a día: una vez configurado,
edita todo desde `/admin.html`.

---

## Resumen del proceso (una sola vez)

1. Crear un proyecto en Supabase y correr `supabase/schema.sql`.
2. Crear el usuario del Panel de Control (una contraseña).
3. Pegar 2 datos de configuración en `js/config.js`.
4. Subir el código a GitHub.
5. Conectar/subir el proyecto a Netlify.

Después de esto, el día a día es: entrar a `tusitio.netlify.app/admin.html`,
escribir la contraseña, editar, y hacer clic en **"Guardar cambios"**.

---

## Paso 1 — Crear el proyecto de Supabase y las tablas

1. Ve a **https://supabase.com** → crea una cuenta gratuita (o inicia sesión)
   → **"New project"**.
   - Nombre: `forxa-dashboard` (o el que prefieras).
   - Elige una contraseña de base de datos (guárdala, no es la del panel).
   - Región: la más cercana a Ecuador (por ejemplo, alguna de EE.UU.).
2. Cuando el proyecto esté listo, ve al menú lateral **"SQL Editor"** →
   **"New query"**.
3. Abre el archivo **`supabase/schema.sql`** de esta carpeta, copia **todo**
   su contenido, pégalo en el editor SQL de Supabase y presiona **"Run"**.
   - Esto crea la tabla `dashboard_content`, activa la seguridad (RLS) y
     carga los datos de ejemplo (los de Agosto 2026) para que el sitio
     funcione desde el primer momento.

---

## Paso 2 — Crear el usuario del Panel de Control (la "contraseña simple")

El Panel de Control pide **solo una contraseña** para entrar — por dentro,
usa un usuario real de Supabase Auth con un correo fijo, así que la edición
queda protegida de verdad (no cualquiera con el link puede editar).

1. En Supabase, ve a **Authentication → Users → "Add user"** (o "Invite").
2. Elige **"Create new user"**.
3. Correo: `panel@forxainmobiliaria.com` (debe ser **exactamente** este,
   porque es el que trae configurado `js/config.js` — si prefieres otro
   correo, ver la nota abajo).
4. Contraseña: la que quieras usar para entrar al Panel de Control. Marca
   **"Auto Confirm User"** si aparece la opción (así no pide verificar el
   correo).
5. Guarda. Esa contraseña es la que se escribe en `/admin.html`.

> **¿Quieres usar otro correo o dar acceso a varias personas con su propio
> usuario?** Puedes crear más usuarios en el mismo lugar (cada uno con su
> correo y contraseña) — todos podrán entrar al panel mientras `ADMIN_EMAIL`
> en `js/config.js` sea el correo que usará el botón de login. Si manejas
> varias personas y quieres que cada una tenga su propio correo (no solo
> contraseña), dímelo y te preparo la variante con formulario de correo +
> contraseña; el archivo `js/admin.js` está comentado para facilitar ese
> cambio (busca `signInWithPassword`).

---

## Paso 3 — Configurar `js/config.js`

1. En Supabase, ve a **Project Settings → API**.
2. Copia:
   - **Project URL**
   - **anon public** key (la clave pública, NO la `service_role`)
3. Abre `js/config.js` en cualquier editor de texto y reemplaza:

```js
window.FORXA_CONFIG = {
  SUPABASE_URL: "TU_SUPABASE_URL",        // pega aquí el Project URL
  SUPABASE_ANON_KEY: "TU_SUPABASE_ANON_KEY", // pega aquí la anon public key
  ADMIN_EMAIL: "panel@forxainmobiliaria.com"  // el correo del Paso 2
};
```

Guarda el archivo. La clave `anon public` es segura de exponer en el
navegador — es la misma clave que usan todos los sitios hechos con
Supabase; el acceso de escritura está protegido por las políticas de
seguridad (RLS) que ya creaste en el Paso 1.

---

## Paso 4 — Subir el proyecto a GitHub

**Opción A — desde la web de GitHub (sin usar la terminal):**

1. Descomprime el archivo `.zip` que te entregué.
2. Ve a **https://github.com/new** → crea un repositorio nuevo (por ejemplo
   `forxa-dashboard`), puede ser privado.
3. En la página del repo recién creado, haz clic en **"uploading an existing
   file"**, arrastra **todos** los archivos y carpetas descomprimidos, y
   confirma el commit.

**Opción B — desde la terminal (si tienes `git` instalado):**

```bash
cd forxa-dashboard
git init
git add .
git commit -m "Dashboard ejecutivo de marketing — Forxa Inmobiliaria"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/forxa-dashboard.git
git push -u origin main
```

---

## Paso 5 — Publicar en Netlify

**Opción A — arrastrar y soltar (la más rápida):**

1. Ve a **https://app.netlify.com/drop**
2. Arrastra la carpeta completa del proyecto (con `index.html` en la raíz).
3. Netlify te da un link tipo `https://algo-al-azar.netlify.app` en segundos.
   Puedes cambiar el subdominio en **Site settings → Change site name**.

**Opción B — conectado a GitHub (recomendado, para que cada cambio de código
se publique solo):**

1. En Netlify: **"Add new site" → "Import an existing project" → GitHub**.
2. Elige el repositorio que subiste en el Paso 4.
3. Build command: (déjalo vacío)
   Publish directory: `.`
4. Deploy site.

No hace falta ningún paso de "build" — es HTML/CSS/JS puro.

---

## Cómo editar el contenido día a día

1. Entra a `https://tu-sitio.netlify.app/admin.html`
2. Escribe la contraseña del Paso 2.
3. Edita cualquier sección desde el menú de la izquierda: encabezado,
   insights, KPIs, ventas mensuales, cumplimiento de metas, leads y embudo,
   ROI, fuente de ventas, asesores, social media, pie de página y
   **colores de marca** (cambian el sitio completo en vivo).
4. Para textos en negrita dentro de una nota o insight, escribe
   `**así**` — se convierte automáticamente en **negrita** en el dashboard.
5. Haz clic en **"Guardar cambios"** (arriba a la derecha). El dashboard
   público se actualiza al instante para cualquiera que lo abra.

### La pestaña "JSON avanzado"

Todos los campos del panel cubren el 100% del contenido, pero si alguna vez
quieres ajustar algo muy puntual directamente, la última sección del panel
("JSON avanzado") muestra **todo** el contenido como un solo documento de
texto editable — puedes cambiar literalmente cualquier carácter (de ahí que
"hasta el último punto y coma" sea posible) y aplicarlo con un clic. Es la
misma información que ya editas por formulario, solo que en crudo.

### Botones útiles de la barra superior

- **Descargar JSON** — guarda una copia de respaldo de todo el contenido
  actual en tu computadora.
- **Restaurar ejemplo** — vuelve a los datos de ejemplo de Agosto 2026
  (útil solo para pruebas; pide confirmación antes de aplicar).
- **Ver dashboard ↗** — abre `index.html` en una pestaña nueva.

---

## Estructura del proyecto

```
forxa-dashboard/
├── index.html              Dashboard público
├── admin.html               Panel de Control (protegido con contraseña)
├── css/
│   ├── styles.css           Estilos de marca compartidos
│   └── admin.css            Estilos exclusivos del panel
├── js/
│   ├── config.js            ⚠ Completar en el Paso 3
│   ├── supabase-client.js   Conexión a Supabase
│   ├── data.js               Datos de ejemplo / respaldo (Agosto 2026)
│   ├── dashboard.js          Motor de renderizado del dashboard público
│   └── admin.js              Motor del Panel de Control
├── assets/                  Logo de Forxa (blanco, para fondos oscuros) y favicons
├── supabase/
│   ├── schema.sql            ⚠ Correr en el Paso 1
│   └── seed_data.json        Los mismos datos de ejemplo, en JSON plano
├── netlify.toml
└── README.md                 Este archivo
```

## Notas de seguridad

- La clave `anon public` de Supabase es pública por diseño — protege la
  escritura mediante las políticas RLS del `schema.sql` (solo usuarios
  autenticados pueden guardar cambios), no ocultando la clave.
- **Nunca** copies la clave `service_role` de Supabase en este proyecto — esa
  clave sí es secreta y no debe usarse en el navegador.
- `admin.html` no aparece indexado en buscadores (`noindex`), pero no es
  invisible — la contraseña es lo que realmente protege la edición.
- Si necesitas revocar el acceso de alguien, cambia la contraseña del
  usuario en **Supabase → Authentication → Users**.

## Solución de problemas

- **"Mostrando datos de ejemplo (Supabase no configurado)"** en la esquina
  superior del dashboard → falta completar `js/config.js` (Paso 3) o correr
  `schema.sql` (Paso 1).
- **El login del panel dice "contraseña incorrecta"** → revisa que el
  usuario exista en Supabase con el correo exacto de `ADMIN_EMAIL`
  (Paso 2), y que "Auto Confirm User" haya quedado marcado.
- **Guardo cambios y no se reflejan en el dashboard público** → verifica que
  ambas páginas usen el mismo proyecto de Supabase (mismo `SUPABASE_URL` en
  `js/config.js`) y refresca `index.html`.

---

Hecho para **Forxa Inmobiliaria** · Cuenca, Ecuador.
