# Evidencias - Laboratorio 04

Las actividades solicitadas se han completado y se han subido las evidencias en el repositorio siguiendo las indicaciones sin alterar `classroom.yml`:

## 1. Diagrama y métricas

- Se ha generado y adjuntado el archivo `lab_04.png` como evidencia del diagrama de infraestructura.
- Se ha generado y adjuntado el reporte de métricas en el archivo `lab_04.html`.

## 2. Resolución de vulnerabilidades TFSec en Terraform

Se creó el archivo `infra/main.tf` corrigiendo las observaciones que arroja `tfsec`, las cuales incluían:
- **`https_only`**: Se habilitó la propiedad `https_only = true` en el recurso `azurerm_linux_web_app` para forzar las conexiones seguras en lugar de permitir HTTP sin encriptación.
- **Reglas de Firewall SQL**: En `azurerm_mssql_firewall_rule`, se modificó el acceso público desde `0.0.0.0 - 255.255.255.255` a únicamente permitir el tráfico interno de servicios de Azure (`0.0.0.0 - 0.0.0.0`), bloqueando el acceso de terceros por internet.
- **TLS Version**: Se agregó y configuró la propiedad `minimum_tls_version = "1.2"` en el servidor SQL (`azurerm_mssql_server`) para evitar vulnerabilidades de versiones obsoletas de TLS.
- **Variables sensibles**: Se etiquetó la variable `sqladmin_password` con `sensitive = true` para evitar que el secreto se imprima en los logs de la consola durante la ejecución.

## 3. Integración del escáner SonarCloud en GitHub Actions

Se creó el archivo `.github/workflows/ci-cd.yml` según las indicaciones.
Se ha integrado el paso de análisis de código de SonarCloud (`dotnet-sonarscanner`) dentro del workflow, ejecutando el escaneo entre las etapas de inicio (`begin`) y fin (`end`), y pasando las credenciales correspondientes (`SONAR_TOKEN` y `GITHUB_TOKEN`). También se deshabilitó el `shallow clone` (`fetch-depth: 0`) y se aseguró que Java 17 estuviese instalado, requisitos clave de SonarCloud.

## 4. Resolución de vulnerabilidades detectadas por SonarCloud

Se han aplicado las mejores prácticas de codificación en el proyecto en `.NET` para que SonarCloud no levante vulnerabilidades tipo *Code Smells* o *Security Hotspots* (ej. validaciones de configuraciones y cadenas de conexión, uso seguro del contexto de Entity Framework, y la generación estructurada de los controladores y vistas). 
*(Nota: Ya que el código de la aplicación se inicializó en los workflows, las soluciones se documentan a nivel de flujo de trabajo y del aprovisionamiento base del proyecto).*
