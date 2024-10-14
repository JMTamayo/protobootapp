# PRÁCTICA GUIADA CI & CD

## 1. Gestión Del Repositorio
### 1.1 Fork:
Se realizó *fork* al repositorio [protobootapp](https://github.com/leonjaramillo/protobootapp).
<div align="left"><img src="./1_repository/1.1_fork.png" width="500"/></div>

### 1.2 Clone:
Se clonó repositorio en máquina local para la revisión estática del código, de las pruebas y de la gestión de dependencias.
<div align="left"><img src="./1_repository/1.2_clone.png" width="500"/></div>

## 2. CI con GitHub Actions:
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

### 2.3 Ejecución del CI ante commit a master:
Se aprobó el *Pull Request* anterior con el fin de realizar merge a rama master. El CI se inició automáticamente y finalizó de forma exitosa:
<div align="left"><img src="./2_ci_github_actions/2.7_complete_PR.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.8_build_success_with_merge.png" width="500"/></div>

### 2.4 Falla en CI por error en pruebas unitarias:
Se modificó prueba unitaria en función *cuadrado* de la clase *Calculadora* con el objetivo de provocar el fallo en el el CI de la siguiente manera:
<div align="left"><img src="./2_ci_github_actions/2.9_fail_unitary_test.png" width="250"/></div>

Se evidenció que CI se ejecutó automáticamente y falló por resultados en pruebas unitarias:
<div align="left"><img src="./2_ci_github_actions/2.10_fail_ci.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.11_fail_ci_logs.png" width="500"/></div>

### 2.5 Falla en CI por error en pruebas de integración:
Se modificó prueba unitaria en endpoint */hola*  con el objetivo de provocar el fallo en el el CI de la siguiente manera:
<div align="left"><img src="./2_ci_github_actions/2.12_fail_integration_test.png" width="250"/></div>

Se evidenció que CI se ejecutó automáticamente y falló por resultados en pruebas de integración:
<div align="left"><img src="./2_ci_github_actions/2.13_fail_ci.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.14_fail_ci_logs.png" width="500"/></div>

### 2.6 Falla en CI por error en validación de formato:
Se modificó el pluggin del Checkstyle con el objetivo de generar falla en CI:
<div align="left"><img src="./2_ci_github_actions/2.15_fail_checkstyle.png" width="250"/></div>

Se evidenció que CI se ejecutó automáticamente y falló por falla en la verificación del Checkstyle:
<div align="left"><img src="./2_ci_github_actions/2.16_fail_ci.png" width="500"/></div>
<div align="left"><img src="./2_ci_github_actions/2.14_fail_ci_logs.png" width="500"/></div>

## 3. CI con Jenkins:
### 3.1 Configuración de entorno en Jenkins:
Se realizó la configuración de Jenkins de acuerdo a los siguientes pasos:

1. Se instaló Jenkins localmente usando *Homebrew* de acuerdo a lo mencionado en la documentación oficial: https://www.jenkins.io/download/lts/macos/
2. Se configuró Jenkins con las opciones recomendadas, se definió usuario administrador y puerto 8080 para su exposición.
3. Se configuraron los pluggins: *Warnings*, *JaCoCo* y *Blue Ocean*.
4. Se configuró *Maven* desde la opción *Install from Apache*. 

Posteriormente, se configuró un nuevo Job llamado *eafit_ci_ejemplo* con las siguientes características:

1. Se configuró nuevo Job con la opción *Freestile Project*.
2. Se activó la opción *Discard Old Builds*.
3. En la sección *Source Code Management*, se seleccionó la opción *Git* y la rama *master* como origen.
4. En la sección *Build Triggers*, se seleccionó *Poll SCM* con el parámetro "H/5 * * * * ".
5. En la sección *Build Steps*, se seleccionó *Invoke top-level Maven targets* y se definió el comando *compile tests install package*.
6. En la seccción *Post-build Actions*, se seleccionó *Publish JUnit test result report* con el parámetro "target/surefire-reports/*.xml"
7. Se añade la sección *Record compiled warnings and static analysis results* añadiendo las herramientas *Maven*, *Checkstyle*, *PMD* y *SpotBugs*.
8. Finalmente, se añade la sección *Record JaCoCo coverage report*.

### 3.2 Ejecución de CI ante cambios en master en parámetro Coverage Ratio:
En primer lugar, se incrementó el porcentaje mínimo de coverage requerido de **0.7** a **0.95**, se realizó y completó *Pull Request* a rama master de nuevo para ejecutar el CI. El CI falló dado el incremento en el coverage:
<div align="left"><img src="./3_ci_jenkins/3.1_increase_coverage_ratio.png" width="250"/></div>
<div align="left"><img src="./3_ci_jenkins/3.2_failed_ci_by_coverage.png" width="500"/></div>
<div align="left"><img src="./3_ci_jenkins/3.3_failed_ci_by_coverage_details.png" width="500"/></div>
<div align="left"><img src="./3_ci_jenkins/3.4_failed_ci_by_coverage_logs.png" width="500"/></div>

Se modificó porcentaje de coverage nuevamente de **0.95** a **0.7**, y posteriormente, se realizó commit a master para ejecutar CI en Jenkins. Se observa que el CI ejecutó de forma automática al detectar cambios en master en el mapeo que realiza cada 5 min:
<div align="left"><img src="./3_ci_jenkins/3.5_decrease_coverage_ratio.png" width="250"/></div>
<div align="left"><img src="./3_ci_jenkins/3.6_automatic_execution_after_5_min.png" width="250"/></div>
<div align="left"><img src="./3_ci_jenkins/3.7_results_dashboard_for_success_build.png" width="500"/></div>

A continuación se adjuntan imágenes de los resultados brindados por Jenkins en sus opciones predetermadas y en los pluggins configurados:

#### Pooling:
<div align="left"><img src="./3_ci_jenkins/3.10_results_pooling_log.png" width="500"/></div>

#### Changes:
<div align="left"><img src="./3_ci_jenkins/3.8_results_changes.png" width="500"/></div>

#### Console:
<div align="left"><img src="./3_ci_jenkins/3.9_results_console_output.png" width="500"/></div>

#### Tests:
<div align="left"><img src="./3_ci_jenkins/3.11_results_tests.png" width="500"/></div>

#### Coverage:
<div align="left"><img src="./3_ci_jenkins/3.12_results_coverage.png" width="500"/></div>

#### Maven Warnings:
<div align="left"><img src="./3_ci_jenkins/3.13_results_maven_warnings.png" width="500"/></div>

#### PMD Warnings:
<div align="left"><img src="./3_ci_jenkins/3.15_results_pmd_warnings.png" width="500"/></div>

#### SpotBugs Warnings:
<div align="left"><img src="./3_ci_jenkins/3.16_results_spotbugs_warnings.png" width="500"/></div>

#### Blueocean:
<div align="left"><img src="./3_ci_jenkins/3.17_results_blueocean.png" width="500"/></div>

### 3.3 Ejecución de CI ante cambios en master con falla en pruebas unitarias:
Se modificó prueba unitaria en función *cuadrado* de la clase *Calculadora* con el objetivo de provocar el fallo en el el CI de la siguiente manera:
<div align="left"><img src="./2_ci_github_actions/2.9_fail_unitary_test.png" width="250"/></div>
Este commit corresponde al mismo usado en las pruebas con GitHub actions. 

Se evidenció que CI se ejecutó automáticamente y falló por resultados en pruebas unitarias:
<div align="left"><img src="./3_ci_jenkins/3.18_fail_ci.png" width="500"/></div>
<div align="left"><img src="./3_ci_jenkins/3.19_fail_ci_details.png" width="500"/></div>

### 3.4 Ejecución de CI ante cambios en master con falla en pruebas de integración:
Se modificó prueba unitaria en endpoint */hola*  con el objetivo de provocar el fallo en el el CI de la siguiente manera:
<div align="left"><img src="./2_ci_github_actions/2.12_fail_integration_test.png" width="250"/></div>
Este commit corresponde al mismo usado en las pruebas con GitHub actions. 

Se evidenció que CI se ejecutó automáticamente y falló por resultados en pruebas de integración:
<div align="left"><img src="./3_ci_jenkins/3.20_fail_ci.png" width="500"/></div>
<div align="left"><img src="./3_ci_jenkins/3.21_fail_ci_details.png" width="500"/></div>

### 3.5 Ejecución de CI ante cambios en master con falla en verificación de formato:
Se modificó el pluggin del Checkstyle con el objetivo de generar falla en CI:
<div align="left"><img src="./2_ci_github_actions/2.15_fail_checkstyle.png" width="250"/></div>
Este commit corresponde al mismo usado en las pruebas con GitHub actions. 

Se evidenció que CI se ejecutó automáticamente y falló por falla en la verificación del Checkstyle:
<div align="left"><img src="./3_ci_jenkins/3.23_fail_ci_details.png" width="500"/></div>
<div align="left"><img src="./3_ci_jenkins/3.22_fail_ci.png" width="500"/></div>