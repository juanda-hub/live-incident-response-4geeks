# Live Incident Response — 4geeks-server

Informe técnico de respuesta a incidentes realizado sobre un servidor Ubuntu 20.04.6 LTS en producción, utilizando un enfoque de **Live Incident Response** (inspección en vivo, sin apagar el sistema).

Proyecto Final — Ciberseguridad — 4Geeks Academy

**Autor:** Erick Ashley Loera Castillo
**Rol:** Analista de Ciberseguridad (Blue Team)

---

## Resumen del incidente

Se detectó actividad anómala en el servidor `4geeks-server`. La investigación confirmó que el sistema fue comprometido mediante **ingeniería social** dirigida a un usuario legítimo (`reports`), lo que permitió a un atacante externo:

- Crear una cuenta de backdoor (`hacker`)
- Descargar y ejecutar un payload externo
- Instalar un script de exfiltración (`backup2.sh`) programado vía cron cada 15 minutos
- Exfiltrar `/etc/passwd` y `/etc/shadow` (hashes de contraseñas de todas las cuentas) hacia un servidor externo (`192.168.1.100:8080`)

Todas las persistencias fueron identificadas, eliminadas y verificadas. Las credenciales comprometidas fueron rotadas y el servicio fue restaurado sin interrumpir la disponibilidad del sistema.

## Severidad

**CRÍTICA** — Exfiltración confirmada de hashes de contraseñas del sistema (`/etc/shadow`).

## Contenido del repositorio

```
.
├── Informe_Tecnico_Live_Incident_Response.pdf   # Informe completo
└── evidencias/                                   # Capturas de pantalla numeradas
    ├── 01_etc_passwd_cuentas_no_autorizadas.png
    ├── 02_ingenieria_social_chat_note.png
    ├── 03_install_sh_dropper.png
    ├── 04_backup2sh_script_exfiltracion.png
    ├── 05_backuplog_exfiltracion_shadow.png
    ├── 06_crond_listado_sys_maintenance.png
    ├── 07_crond_contenido_cronjob_malicioso.png
    ├── 08_credenciales_texto_plano.png
    ├── 09_home_hacker_contenido.png
    ├── 10_home_reports_contenido.png
    ├── 11_remediacion_eliminacion_persistencia.png
    ├── 12_remediacion_servicios_verificacion.png
    ├── 13_firewall_ufw_puerto21.png
    ├── 14_verificacion_payload_no_activo.png
    └── 15_servicio_restaurado.png
```

## Estructura del informe

1. Resumen ejecutivo y cronología del incidente
2. Metodología — Live Incident Response
3. Análisis de impacto (confidencialidad, integridad, disponibilidad)
4. Hallazgos técnicos detallados (con evidencia)
5. Indicadores de Compromiso (IOC)
6. Mapeo a MITRE ATT&CK
7. Acciones de contención, erradicación y recuperación
8. Recomendaciones de fortalecimiento
9. Conclusión

## Indicadores de Compromiso (IOC) principales

| Tipo | Indicador |
|---|---|
| IP del atacante | `192.168.1.100:8080` |
| Cuentas no autorizadas | `hacker` (UID 1002), `reports` comprometida (UID 1001) |
| Persistencia | `/etc/cron.d/sys-maintenance` |
| Script de exfiltración | `/usr/local/bin/backup2.sh` |
| Correo del atacante | `unknown@externalmail.com` |

## Disclaimer

Este informe fue elaborado en un entorno de laboratorio controlado (proyecto académico de 4Geeks Academy) con fines educativos. Las IPs, credenciales y nombres de usuario mostrados pertenecen a una máquina virtual de práctica y no representan sistemas de producción reales.
