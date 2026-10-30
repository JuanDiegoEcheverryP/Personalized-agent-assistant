# Consulta Móvil · MVP local con Codex

MVP web local para consultar `Mobiles Dataset (2025).csv` usando Codex CLI y la sesión de ChatGPT iniciada en esta máquina. No necesita API key, Docker ni paquetes de Python.

## Requisitos e inicio

- Python 3.10 o posterior.
- Codex CLI autenticado (`codex login status`). Si hace falta, ejecuta `codex login`.
- El CSV original en esta carpeta.

Desde PowerShell, inicia:

```powershell
python server.py
```

Abre <http://127.0.0.1:8000>. Mantén la terminal abierta mientras usas el sitio; detén el servidor con `Ctrl+C`.

El servidor escucha solo en `127.0.0.1`. La página y Codex deben ejecutarse en la misma máquina; este MVP no publica el sitio en Internet ni en la red local.

## Páginas

- **Home** (`/`): chat con Codex, dataset verificable, búsqueda, filtro, paginación y descarga del CSV.
- **Sobre el proyecto** (`/proyecto.html`): procedencia, estadísticas del archivo, rol y límites del agente, diccionario tabular de las 15 columnas y ejemplos pulsables.
- **FAQ** (`/faq.html`): respuestas sobre origen, esquema, unidades, precios, vigencia y uso local.

Las páginas comparten `site.css` y navegan entre sí mediante la barra superior. La imagen `UXUI reference/face.png` se usa como avatar del agente; la mascota de cuerpo completo aparece en el hero de Home.

## Fuente y datos

El archivo local procede de la ficha [Mobile Phones Dataset en Kaggle](https://www.kaggle.com/datasets/ranaghulamnabi/mobile-phones-dataset), cuenta `ranaghulamnabi`. La copia local es `Mobiles Dataset (2025).csv`; el asistente trabaja con esa copia y no descarga actualizaciones de Kaggle automáticamente. Consulta la ficha para sus metadatos y condiciones de reutilización.

El archivo tiene 930 registros y 15 columnas. Incluye marca y modelo, especificaciones, cinco columnas de precios de lanzamiento por mercado/moneda y el año. Algunas medidas se guardan como texto con unidades y varios campos pueden contener más de una medida; usa la tabla de tipos y formatos de la página Sobre el proyecto para interpretarlas correctamente.

## Qué ocurre al preguntar

1. `index.html` muestra el catálogo y envía la pregunta junto con el historial reciente a `server.py`.
2. El servidor ejecuta `codex exec` de forma efímera, con acceso de solo lectura al proyecto.
3. Codex consulta `AGENTS.md` y el CSV local y devuelve la respuesta.

La conversación se conserva solo en la página abierta; no se almacena en el servidor. Las solicitudes pasan por la cuenta local de Codex y están sujetas a sus límites y requisitos de conexión.

## Archivos

```text
index.html                     Home: chat y visor del catálogo
proyecto.html                  descripción, procedencia y diccionario de datos
faq.html                       preguntas frecuentes
site.css                       estilos compartidos y adaptación móvil
server.py                      páginas, assets, CSV, descarga y puente a Codex
AGENTS.md                      rol e instrucciones para Codex
Mobiles Dataset (2025).csv     dataset local
relevant info/Datasource.txt    enlace de origen
UXUI reference/                assets y paleta
SESSION_HANDOFF.md             decisiones y progreso de la sesión
```
