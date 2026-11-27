# Registro de seguridad · Sistema Alumnos

Estudiante: Kevin Fuenzalida   Repositorio: https://github.com/kevin-fuenzalida/sistema-alumnos-ev3   Fecha: 27-11-2026

Completa este registro a medida que avanzas. Cada fila debe poder seguirse hasta un **build de Jenkins**
y un **commit** (usa el identificador `VULN-xx` en el mensaje del commit).

## 1. Revisiones de seguridad realizadas

| # | Etapa del SDLC | Herramienta / técnica | Build | Resultado (resumen) |
|---|---|---|---|---|
| R1 | Código (SAST) | Bandit en el pipeline | #2 | 2 High / 2 Medium / 1 Low. En #11: 0 / 0 / 0 |
| R2 | Dependencias (SCA) | pip-audit en el pipeline | #2 | 19 vulnerabilidades en 4 paquetes. Pendiente de actualizar |
| R3 | Pruebas dinámicas (DAST) | OWASP ZAP sobre staging | #3 | 3 High / 3 Medium / 1 Low. En #11: 0 / 0 / 0 |
| R4 | Revisión manual | Lectura de `app.py` | — | Errores internos visibles en la búsqueda (VULN-02), clave de administrador débil (VULN-03) e intentos de acceso sin límite (VULN-08) |
| R5 | Producción | Grafana + Prometheus | — | Pendiente |

## 2. Vulnerabilidades identificadas y corregidas

La ubicación corresponde al código inicial (commit `b954202`).

| ID | Vulnerabilidad (CWE) | Detectada por | Ubicación | Riesgo | Corrección aplicada | Commit | Build que lo verifica |
|---|---|---|---|---|---|---|---|
| VULN-01 | XSS en /hello (CWE-79) | ZAP | `app.py:154` | Alto | Escapar el texto recibido | `9689a45` | #4 |
| VULN-02 | Inyección SQL y errores visibles (CWE-89, CWE-209) | ZAP, Bandit y revisión manual | `app.py:205` | Alto | Consulta parametrizada y mensaje de error genérico | `c25102a`, `329df5b` | #6 |
| VULN-03 | Contraseña con MD5 y clave débil (CWE-327) | Bandit y revisión manual | `app.py:221` | Alto | Hash seguro en credencial de Jenkins y clave nueva | `e32ef5c` | #7 |
| VULN-04 | Clave secreta en el código (CWE-798) | Bandit | `app.py:20` | Bajo | Clave desde el entorno o generada al azar | `10cc9b6` | #8 |
| VULN-05 | Sin protección CSRF (CWE-352) | ZAP | `app.py:213` | Medio | Token anti-CSRF ligado a la sesión | `00c980d` | #9 |
| VULN-06 | Faltan cabeceras de seguridad (CWE-693) | ZAP | `app.py:125` | Medio y bajo | Cabeceras CSP, X-Frame-Options y nosniff | `b711e99` | #10 |
| VULN-07 | Depuración activa y escucha en todas las interfaces (CWE-489, CWE-605) | Bandit | `app.py:241` | Alto y medio | Depuración desactivada y escucha local | `0563b1c` | #11 |
| VULN-08 | Intentos de acceso sin límite (CWE-307) | Revisión manual | `app.py:213` | Medio (evaluación manual) | Pendiente: se aplicará tras detectar el ataque en Grafana | Pendiente | Pendiente |

## 3. Resultados de OWASP ZAP

| Alerta de ZAP | Riesgo | URL de ejemplo | VULN relacionada | Cómo se mitiga | ¿Mitigada? Build |
|---|---|---|---|---|---|
| | | | | | |

## 4. Monitoreo e incidentes

| Hora | Panel / métrica de Grafana | Qué se observó | Entorno | Clasificación | Acción de respuesta | Verificación |
|---|---|---|---|---|---|---|
| | | | | | | |

## 5. Gestión de dependencias

| Paquete | Versión inicial | Vulnerabilidades (IDs) | Versión final | Origen del cambio (PR de Dependabot / manual) | ¿Pruebas, pip-audit y ZAP siguen OK? Build |
|---|---|---|---|---|---|
| Flask | 2.2.2 | | | | |
| Werkzeug | 2.2.2 | | | | |
| Jinja2 | 3.1.2 | | | | |
| gunicorn | 21.2.0 | | | | |
| prometheus-client | 0.17.1 | | | | |

## 6. Trazabilidad

Pega aquí (o adjunta) el `TRAZABILIDAD.md` del último build y completa un recorrido por cada actividad del pipeline:

| Actividad | Hallazgo | Build que lo detectó | Commit / PR que lo resolvió | Build que lo verificó | Versión en producción |
|---|---|---|---|---|---|
| Revisión de seguridad (Bandit) | | #__ | | #__ | |
| Prueba automatizada (OWASP ZAP) | | #__ | | #__ | |
| Gestión de dependencias (Dependabot) | | #__ | PR #__ | #__ | |
