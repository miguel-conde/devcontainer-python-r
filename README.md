# 🐳 Python + R + Quarto Dev Container

Entorno de desarrollo basado en Docker para proyectos de ciencia de datos, machine learning e IA, con soporte completo para Python, R, Quarto y CmdStanR.

## ✅ Características

- Ubuntu 24.04
- Python 3.12 con `venv`, `pipenv`, `poetry`
- R (última versión) + soporte para `renv`
- Quarto (última versión)
- CmdStanR + Stan
- Docker CLI, Git, herramientas de desarrollo
- Listo para usar con VSCode Remote Containers

## 🚀 Uso

1. Asegúrate de tener instalados:
   - Docker Desktop
   - Visual Studio Code
   - Extensión Dev Containers

2. Abre el proyecto en VSCode.

3. Pulsa `F1` y ejecuta:
   ```
   > Dev Containers: Reopen in Container
   ```

4. El contenedor se construirá y abrirá automáticamente.


## 🔌 Extensiones recomendadas de VSCode

Edita `.devcontainer/devcontainer.json` y añade más extensiones en:

```json
"customizations.vscode.extensions"
```

Ejemplos:

+ `ms-python.python`
+ `ikuyadeu.r`
+ `quarto.quarto`
+ `ms-toolsai.jupyter`
+ `eamodio.gitlens`

## 📁 Estructura de carpetas

```
.mi-proyecto/
├── .devcontainer/
│   ├── Dockerfile
│   └── devcontainer.json
├── notebooks/
├── src/
└── README.md
```

¡Feliz codificación! 😄
