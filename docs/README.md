# PRÁCTICA GUIADA CI & CD

## 1. Gestión Del Repositorio
### 1.1 Fork:
Se realizó *fork* al repositorio [protobootapp](https://github.com/leonjaramillo/protobootapp).
<div align="left"><img src="./1_repository/1.1_fork.png" width="500"/></div>

### 1.2 Clone:
Se clonó repositorio en máquina local para la revisión estática del código, de las pruebas y de la gestión de dependencias.
<div align="left"><img src="./1_repository/1.2_clone.png" width="500"/></div>

## 2. Configuración de CI con GitHub Actions:
### 2.1 Workflow:
Se configuró una acción en GitHub a partir de lo especificado en el archivo de configuración **.github/workflows/maven.yml**
<div align="left"><img src="./2_ci_github_actions/2.1_github_actions_config.png" width="200"/></div>

Este CI se ejecutará ante un *Pull Request* a la rama master o ante un nuevo commit en rama master.

### 2.2 Ejecución del CI ante PR:
Se realizó *Pull Request* a *master* con el objetivo de ejecutar el CI:
<div align="left"><img src="./2_ci_github_actions/2.2_execute_ci_with_PR.png" width="500"/></div>

El CI se ejecutó ante la creación del *Pull Request* pero falló por novedades en la configuración de la acción:
<div align="left"><img src="./2_ci_github_actions/2.3_ci_error_due_to_configuration_failures.png" width="500"/></div>

Para corregir esta falla, se activó la opción *Dependency Graph*  en las opciones de seguridad del repositorio:
<div align="left"><img src="./2_ci_github_actions/2.4_enable_dependency_graph.png" width="500"/></div>

Se ejecutó nuevamente el job *Build* faillido:
<div align="left"><img src="./2_ci_github_actions/2.5_rerun_failed_job.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.6_build_success_with_PR.png" width="500"/></div>

Posteriormente, se incrementó el porcentaje mínimo de cóverage requerido de **0.7** a **0.95** y se realizó *Pull Request* a rama master de nuevo para ejecutar el CI:
<div align="left"><img src="./2_ci_github_actions/2.7_increase_coverage_ratio.png" width="250"/></div>

El CI comienza su ejecución de forma automática con el *Pull Request* y se ejecuta de forma exitosa:
<div align="left"><img src="./2_ci_github_actions/2.8_execute_ci_with_PR.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.9_build_success_with_PR.png" width="500"/></div>

### 2.3 Ejecución del CI ante commit a master:
Se aprobó el *Pull Request* anterior con el fin de realizar merge a rama master. El CI se inició automáticamente y finalizó de forma exitosa:
<div align="left"><img src="./2_ci_github_actions/2.10_complete_PR.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.11_execute_ci_with_merge.png" width="500"/></div>