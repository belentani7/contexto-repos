# contexto-repos

Panel de **contexto** (no auditoría) del ecosistema `belentani7`: estado real de todos
los repos git locales, con barra 0-100 % progresiva **rojo → amarillo → verde**, qué
función cumple cada repo, qué le falta y el tiempo estimado de arreglo.

## Contenido

| Archivo | Para qué |
|---|---|
| `contexto_panel.py` | Escanea todos los repos y genera `contexto_panel.json` + `.html`. |
| `contexto_panel.html` | Panel visual (se auto-actualiza cada 30 s si se sirve por HTTP). |
| `contexto_panel.json` | Datos del panel (repos, estado %, falta, tiempo). |
| `espejo_repos.py` | Crea el espejo de carpetas vacías `repos/git/<repo>` y verifica antes de borrar. |
| `espejo_verificacion.md/.json` | Informe: qué es seguro borrar y por qué no el resto. |

## Uso

```powershell
python contexto_panel.py            # escanea y regenera JSON + HTML
python contexto_panel.py --serve 8770   # además sirve y abre el panel

python espejo_repos.py              # dry-run: crea espejo + informe, no borra
python espejo_repos.py --apply      # borra SOLO lo verificado (push OK + GitHub + clone OK)
```

## Regla de borrado

Se borra una carpeta local **solo** si pasa las tres:
1. repo limpio (sin cambios sin commitear) y sin commits sin subir;
2. visible en GitHub (`git ls-remote` responde);
3. clone de prueba OK (`git clone --depth 1`).

Nunca se borra una ruta protegida (home, Desktop, Documents, repos, USO…) ni un repo
que contenga dentro otro repo **no** verificado.

Generado desde la sesión del 2026-09-25.
