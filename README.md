# Workflows con Diferentes Tipos de Runners en GitHub Actions

Este repositorio demuestra cómo configurar y utilizar diversos tipos de runners (ejecutores) en tus flujos de trabajo (workflows) de GitHub Actions: GitHub-Hosted Runners y Self-Hosted Runners (Autoalojados).

-----

## 🚀 Conceptos Clave

Un "runner" es la máquina virtual o contenedor donde se ejecuta tu código de automatización.

### Tipos de Runners

1.  **GitHub-Hosted Runners:** Máquinas virtuales efímeras proporcionadas y gestionadas por GitHub (ej., Ubuntu, Windows, macOS).
2.  **Self-Hosted Runners (Autoalojados):** Máquinas físicas o virtuales que tú mismo alojas y gestionas, ideales para requisitos específicos de infraestructura.

-----

## 🛠️ Configuración de Workflows

Para especificar el runner de un job, se usa `runs-on:`.

### 1\. Workflows con GitHub-Hosted Runners

```yaml
name: GitHub-Hosted Runners Demo

on:
  push:
    branches:
      - main
  workflow_dispatch

jobs:
  build-ubuntu:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Run on Ubuntu
        run: echo "Running on Ubuntu!"

  test-windows:
    runs-on: windows-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Run on Windows
        run: echo "Running on Windows!"
```

### 2\. Workflows con Self-Hosted Runners (Autoalojados)

```yaml
name: Self-Hosted Runner Demo

on:
  push:
    branches:
      - main
  workflow_dispatch

jobs:
  deploy-to-lan:
    runs-on: gha-lnx # Usa la etiqueta de tu runner autoalojado

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Perform Custom Task
        run: echo "Ejecutando en mi runner autoalojado!"
        shell: bash
```

### 3\. Workflow Híbrido (Combinando tipos de Runners)

```yaml
name: Hybrid Runners Demo

on:
  push:
    branches:
      - main
  workflow_dispatch

jobs:
  build-frontend:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Frontend Code
        uses: actions/checkout@v4
      - name: Build Frontend
        run: echo "Building frontend..."
      - name: Upload Frontend Artifact
        uses: actions/upload-artifact@v4
        with:
          name: frontend-build
          path: build/ # Ruta de ejemplo

  deploy-backend-internal:
    runs-on: gha-lnx
    needs: build-frontend
    steps:
      - name: Checkout Backend Code
        uses: actions/checkout@v4
      - name: Download Frontend Artifact
        uses: actions/download-artifact@v4
        with:
          name: frontend-build
          path: ./frontend-app
      - name: Deploy Backend and Frontend
        run: echo "Desplegando en la infraestructura interna..."
        shell: bash
```

-----

## 🚀 Cómo Empezar

1.  **Clona este repositorio:**
    ```bash
    git clone https://github.com/<tu-usuario>/<nombre-de-este-repo>.git
    cd <nombre-de-este-repo>
    ```
2.  **Configura tu Self-Hosted Runner (si aplica):**
      * Sigue la documentación oficial de GitHub.
      * Asigna una etiqueta (ej., `gha-lnx`).
3.  **Haz cambios y `push`:**
      * Modifica los archivos de workflow en `.github/workflows/`.
      * Haz un `git push` o usa `workflow_dispatch` para ejecutar.

-----
