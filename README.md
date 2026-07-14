# ⚽ Liga MX Picks

Quiniela privada para amigos. Picks de Local/Empate/Visitante, Rey Pick,
preguntas especiales, tabla general y salón de la fama.

---

## Cómo desplegarla (Render + Supabase, gratis)

### 1. Base de datos en Supabase

1. Entra a [supabase.com](https://supabase.com) y crea un **proyecto nuevo**
   (⚠️ nuevo — no reuses uno que ya tenga tablas de otra app)
2. Guarda la contraseña de la base que te pide al crearlo
3. Ve a **Project Settings → Database → Connection string**
4. Elige la pestaña **Session pooler** (es la compatible con IPv4 — la conexión
   directa no siempre funciona desde Render)
5. Copia la URI. Se ve así:
   ```
   postgresql://postgres.abcdefgh:[YOUR-PASSWORD]@aws-0-us-east-1.pooler.supabase.com:5432/postgres
   ```
6. Reemplaza `[YOUR-PASSWORD]` con la contraseña del paso 2

**No corras ningún SQL.** Las tablas se crean solas al arrancar el servidor.

---

### 2. Código en GitHub

1. Crea un repo nuevo (público o privado, da igual)
2. Sube estos 3 archivos a la raíz:
   - `index.html`
   - `server.js`
   - `package.json`

---

### 3. Servidor en Render

1. Entra a [render.com](https://render.com) → **New → Web Service**
2. Conecta tu repo de GitHub
3. Configura:

   | Campo | Valor |
   |---|---|
   | Language | `Node` |
   | Build Command | `npm install` |
   | Start Command | `npm start` |
   | Instance Type | `Free` |

4. Baja a **Environment Variables** → **Add Environment Variable**:

   | Key | Value |
   |---|---|
   | `DATABASE_URL` | la URI de Supabase del paso 1 |

5. **Create Web Service**

En los logs debes ver:
```
✓ Usuario admin creado (admin / admin123)
✓ Base de datos lista
⚽ Liga MX Picks corriendo en el puerto 10000
```

Tu liga queda en `https://tu-servicio.onrender.com`

---

## Primeros pasos dentro de la app

1. Entra con **admin** / **admin123**
2. **Perfil** → cambia la contraseña (la de fábrica es pública)
3. **Admin** → cambia el código de registro (viene como `ligamx2026`)
   y pásaselo a tus amigos para que se registren
4. **Admin → Equipos** → carga los 18 equipos (con URL de logo, opcional)
5. **Admin → Jornadas** → crea la J1, luego agrega los partidos con fecha y hora
6. **Admin → Preguntas Especiales** → fija la fecha de cierre y agrega las preguntas

---

## Cómo funciona

**Picks.** Cada partido: Local / Empate / Visitante. 1 punto por acierto.
Los picks se cierran solos cuando arranca el primer partido de la jornada.

**Rey Pick ♛.** Una vez por jornada, marcas un partido como Rey:
+1 extra si aciertas, −1 si fallas. Solo puedes usar el Rey **una vez por
equipo en toda la temporada**, y solo sobre el equipo que elegiste ganar
(no aplica en empates).

**Privacidad.** Nadie ve los picks de los demás hasta que arranca el primer
partido de la jornada. Lo bloquea el servidor, no el navegador.

**Preguntas especiales.** Se responden antes de la J1 y valen los puntos que
el admin les asigne. Aparecen en la columna **ESP** de la tabla general.

**Tabla.** Vista *Resumen* (lista con puntos) y vista *General* (la matriz
J1...J17 + ESP + TOTAL).

---

## Notas

- **El servidor se duerme.** En el plan gratis de Render, si nadie entra por
  15 minutos el servicio se apaga. El primero en entrar espera ~40 segundos
  a que despierte. Es normal.
- **Contraseñas olvidadas.** Están encriptadas con bcrypt — nadie puede verlas,
  ni el admin. Solo se pueden resetear.
- **Actualizar la app.** Sube el archivo nuevo a GitHub y Render redespliega
  solo. La base de datos no se toca.
