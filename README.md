# GitHub Agentic Workflows

Repositorio de aprendizaje y laboratorio para **GitHub Agentic Workflows** (`gh-aw`), una herramienta de [GitHub Next](https://githubnext.com/) que permite automatizar repositorios usando agentes de IA (Copilot, Claude, Codex, Gemini) ejecutándose en GitHub Actions.

## Propósito del repositorio

Este repositorio sirve como campo de pruebas para aprender a crear, configurar y ejecutar agentic workflows. Los workflows definen tareas de automatización en lenguaje natural (Markdown) que un agente de IA ejecuta periódicamente o bajo demanda.

## Workflows incluidos

### `daily-repo-status`
Genera un informe diario del estado del repositorio como un Issue de GitHub.

- **Motor IA:** GitHub Copilot (requiere token Enterprise/Business)
- **Frecuencia:** Diaria (schedule) y manual (workflow_dispatch)
- **Output:** Issue etiquetado con `report` y `daily-status`
- **Archivos:**
  - `.github/workflows/daily-repo-status.md` — instrucciones en lenguaje natural (editable)
  - `.github/workflows/daily-repo-status.lock.yml` — workflow compilado (generado automáticamente)

## Cómo funcionan los agentic workflows

```
daily-repo-status.md   →   gh aw compile   →   daily-repo-status.lock.yml
(tú lo escribes)                                (GitHub Actions lo ejecuta)
```

Cada ejecución pasa por 5 jobs en GitHub Actions:

| Job | Función |
|---|---|
| `activation` | Prepara el entorno y renderiza el prompt |
| `agent` | Ejecuta el agente IA en un contenedor sandboxed |
| `detection` | Escanea el output en busca de amenazas |
| `safe_outputs` | Publica en GitHub solo las acciones permitidas |
| `conclusion` | Registra métricas y cierra issues anteriores |

## Prerrequisitos

- [GitHub CLI](https://cli.github.com/) v2.0+
- Extensión `gh aw`: `gh extension install github/gh-aw`
- Cuenta con **GitHub Copilot Enterprise/Business** (o API key de Claude/Gemini/Codex)
- GitHub Actions habilitado en el repositorio

## Configuración inicial

### 1. Instalar la extensión
```bash
gh extension install github/gh-aw
```

### 2. Autenticarse
```bash
gh auth login --scopes repo,workflow
```

### 3. Añadir un workflow
```bash
gh aw add-wizard githubnext/agentics/daily-repo-status
```

### 4. Configurar el secret de Copilot
Crear un PAT en [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new) con permiso **Copilot Requests: Read-only** y añadirlo como secret:
```bash
gh secret set COPILOT_GITHUB_TOKEN --repo <owner>/<repo>
```

### 5. Recompilar tras cambios de frontmatter
```bash
gh aw compile
```

## Comandos útiles

```bash
# Lanzar un workflow manualmente
gh aw run daily-repo-status --repo <owner>/<repo>

# Ver el estado de los workflows
gh aw status

# Auditar un run (diagnóstico)
gh aw audit <run-id>

# Ver los logs
gh aw logs
```

## Notas conocidas

- El `title-prefix` y otros campos del frontmatter requieren recompilar con `gh aw compile` tras cualquier cambio.
- La herramienta está en **desarrollo temprano** — puede cambiar significativamente.
- Bug conocido en `gh-aw-actions v0.74.8`: el archivo `workflow_install_note.md` no se genera en el runner. Workaround aplicado en el `lock.yml`.

## Referencias

- [Documentación oficial](https://github.github.com/gh-aw/)
- [Repositorio de workflows de ejemplo](https://github.com/githubnext/agentics)
- [Feedback de la comunidad](https://github.com/orgs/community/discussions/186451)
