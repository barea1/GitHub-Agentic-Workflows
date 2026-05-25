---
description: |
  Este workflow crea informes diarios del estado del repositorio. Recopila la
  actividad reciente (issues, PRs, discusiones, releases, cambios de código)
  y genera issues de GitHub con información de productividad, destacados
  y recomendaciones para el proyecto.

on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

network: defaults

tools:
  github:
    # En repos públicos, `lockdown: false` permite
    # leer issues, pull requests y comentarios de terceros.
    # En repos privados no tiene efecto especial.
    lockdown: false
    min-integrity: none # Este workflow puede examinar y comentar cualquier issue

safe-outputs:
  mentions: false
  allowed-github-references: []
  create-issue:
    title-prefix: "[estado-repo] "
    labels: [report, daily-status]
    close-older-issues: true
source: githubnext/agentics/workflows/repo-status.md@dcdf09723d42ef9b6c75335e4612fd145d4ccdaa
---

# Estado del Repositorio

Crea un informe diario animado del estado del repositorio como un issue de GitHub.

## Qué incluir

- Actividad reciente del repositorio (issues, PRs, discusiones, releases, cambios de código)
- Seguimiento del progreso, recordatorios de objetivos y logros destacados
- Estado del proyecto y recomendaciones
- Próximos pasos concretos para los mantenedores

## Estilo

- Sé positivo, motivador y útil 🌟
- Usa emojis con moderación para amenizar
- Sé conciso — ajusta la longitud según la actividad real
- Escribe todo el informe en español

## Proceso

1. Recopila la actividad reciente del repositorio
2. Estudia el repositorio, sus issues y sus pull requests
3. Crea un nuevo issue de GitHub con tus hallazgos e insights
