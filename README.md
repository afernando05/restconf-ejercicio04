# Ejercicio 04 - DevOps con RESTCONF (Cisco IOS-XE)

Configura Loopback de gestión y SNMP **sin tocar la consola**: todo cambio
pasa por Git -> Pull Request -> pipeline -> RESTCONF.

## Uso local (solo para pruebas)
    pip install -r requirements.txt
    export SANDBOX_USER=...  SANDBOX_PASS=...
    pytest -q
    python src/deploy.py --check   # prueba en seco
    python src/deploy.py           # aplica
    python src/verify.py           # monitoreo

## Mapeo al ciclo DevOps
| Fase | Evidencia |
|---|---|
| Plan | Issue con requisitos |
| Code | config/vars.yml, src/*.py |
| Build | yamllint + flake8 en el pipeline |
| Test | pytest + deploy.py --check |
| Release | PR aprobado, merge a main, tag v1.0 |
| Deploy | job deploy (PUT RESTCONF) |
| Operate | PUT idempotente; re-ejecutar corrige desviaciones |
| Monitor | verify.py + snmpwalk |

## Controles contra cambios manuales
- Rama main protegida (PR + revisión obligatoria)
- Credenciales solo en GitHub Secrets
- Despliegue únicamente desde el pipeline
