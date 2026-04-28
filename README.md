[README.md](https://github.com/user-attachments/files/27146781/README.md)
# Mi Entrenamiento — Dashboard Strava

Dashboard personal de entrenamientos que se sincroniza automáticamente con Strava cada semana.

## ¿Qué hace?

- Se conecta a tu cuenta de Strava via API
- Cada lunes a las 8am (hora Argentina) actualiza los datos automáticamente via GitHub Actions
- Publica la app en GitHub Pages con una URL pública fija
- Mostrás métricas semanales, gráficos y insights automáticos

---

## Instalación paso a paso

### 1. Crear el repositorio en GitHub

1. Andá a [github.com/new](https://github.com/new)
2. Nombre: `mi-entrenamiento` (o el que prefieras)
3. Visibility: **Public** (necesario para GitHub Pages gratis)
4. Crealo sin README

### 2. Subir los archivos

Copiá la estructura de este proyecto:

```
mi-entrenamiento/
├── .github/
│   └── workflows/
│       └── sync-strava.yml
└── app/
    ├── index.html
    ├── activities.json     ← se genera automáticamente
    ├── athlete.json        ← se genera automáticamente
    └── meta.json           ← se genera automáticamente
```

Subí solo `.github/` y `app/index.html`. Los JSON los genera el workflow.

### 3. Configurar los Secrets en GitHub

Andá a tu repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

Necesitás agregar estos 4 secrets:

| Nombre | Valor |
|--------|-------|
| `STRAVA_CLIENT_ID` | Tu Client ID de Strava (ej: 231596) |
| `STRAVA_CLIENT_SECRET` | Tu Client Secret de Strava |
| `STRAVA_REFRESH_TOKEN` | Tu Refresh Token de Strava |
| `PAT_TOKEN` | Un Personal Access Token de GitHub (ver abajo) |

#### Cómo obtener el PAT_TOKEN de GitHub:
1. GitHub → tu avatar → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. **Generate new token (classic)**
3. Nombre: `strava-sync`
4. Scopes: marcá `repo` y `workflow`
5. Generalo y copialo (solo se muestra una vez)

### 4. Correr el workflow por primera vez

1. Andá a tu repo → **Actions**
2. Buscá "Sync Strava Data"
3. Hacé clic en **Run workflow** → **Run workflow**
4. Esperá ~30 segundos a que termine
5. Verificá que aparecieron `activities.json`, `athlete.json` y `meta.json` en la carpeta `app/`

### 5. Activar GitHub Pages

1. Repo → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, Folder: `/app`
4. Guardá

En 1-2 minutos tu app va a estar en:
```
https://TU_USUARIO.github.io/mi-entrenamiento/
```

---

## Uso semanal con Claude

Una vez configurado, el flujo es:

1. Cada lunes el workflow corre solo y actualiza los datos
2. Cuando querés analizar tus entrenamientos, le decís a Claude:
   > "Analizá mis entrenamientos de esta semana: https://TU_USUARIO.github.io/mi-entrenamiento/activities.json"
3. Claude lee los datos frescos y te da análisis, recomendaciones y un plan

---

## Forzar sincronización manual

Si querés actualizar los datos antes del lunes:
1. Repo → **Actions** → **Sync Strava Data** → **Run workflow**

---

## Personalización

El archivo `app/index.html` contiene todo el código de la app. Podés editar:
- Los colores en la sección `:root` (variables CSS)
- La frase motivacional en el hero
- La cantidad de actividades mostradas en la tabla (buscar `slice(0, 12)`)
- Los umbrales de los insights (buscar `generateInsights`)

---

## Troubleshooting

**El workflow falla con error de token:**
→ Verificá que los 4 secrets estén bien configurados, sin espacios extra.

**La app no carga datos:**
→ Verificá que GitHub Pages esté apuntando a la carpeta `/app` y que el workflow haya corrido al menos una vez.

**El refresh token venció:**
→ Repetí el proceso de autorización OAuth para obtener un nuevo refresh token y actualizalo en los secrets.
