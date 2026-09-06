# Cogoteo RGC — versión con ADMIN real

Esta versión mantiene el ADMIN dentro de la misma página, pero usa Supabase Auth + RLS.

## 1. Crea un proyecto en Supabase

En **SQL Editor**, ejecuta `supabase_schema.sql`.

## 2. Crea tu usuario administrador

Ve a **Authentication > Users** y crea tu usuario con correo y contraseña.

Copia el UUID del usuario y ejecuta en SQL Editor:

```sql
insert into public.profiles (id,is_admin)
values ('TU_USER_UUID', true)
on conflict (id) do update set is_admin=true;
```

## 3. Configura index.html

Busca:

```js
const SUPABASE_URL = 'PEGA_AQUI_TU_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'PEGA_AQUI_TU_SUPABASE_ANON_KEY';
```

Pega los valores de **Project Settings > API**.

La anon key puede estar en el navegador: es pública por diseño.
La seguridad se aplica con Authentication + Row Level Security.

## 4. GitHub / Vercel

Sube:
- `index.html`

Vercel puede desplegar el repositorio directamente.

## Seguridad

- No hay contraseña hardcodeada en el HTML.
- Solo usuarios autenticados con `profiles.is_admin = true` pueden escribir.
- Visitantes anónimos solo pueden leer teams, publicaciones y media.
- Storage solo permite subir/eliminar a administradores.
- Máximo por archivo configurado: 15 MB.
- El frontend también limita videos a 30 segundos.
