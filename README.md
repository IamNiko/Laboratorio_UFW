# 🛡️ Laboratorio UFW – Firewall, Escaneo y Análisis de Logs

Este proyecto documenta un laboratorio defensivo realista en ciberseguridad, donde se utilizó una Raspberry Pi como servidor víctima, Parrot OS como equipo atacante y UFW como firewall. El objetivo fue comprender y simular comportamientos reales de red, configuraciones de seguridad, escaneos ofensivos y análisis de logs del sistema.

---

## 📚 Contenido

- `informe_tecnico.md`: Informe profesional tipo SOC con análisis de la infraestructura, reglas de firewall, escaneos, eventos detectados y conclusiones.
- `escaneos/`: Incluye los resultados de escaneos realizados desde Parrot OS:
  - `scan_syn.txt`: Escaneo SYN selectivo a puertos 22 y 23.
  - `full_scan.txt`: Escaneo completo de todos los puertos TCP.
  - `scan_host_10.txt`: Investigación de host desconocido en la red.
  
---

## ⚙️ Tecnologías y herramientas utilizadas

- 🐧 Raspberry Pi OS (servidor víctima)
- 🦜 Parrot OS (máquina atacante)
- 🔥 UFW (Uncomplicated Firewall)
- 🧠 Nmap y Netcat para escaneos ofensivos
- 📑 Journalctl para análisis de logs
- 🖥️ Conexión remota SSH desde Windows 10

---

## 🎯 Objetivos cumplidos

- Configuración avanzada de UFW con reglas por IP y puerto
- Detección y diferenciación de tráfico legítimo, auditado y bloqueado
- Registro de logs con `journalctl` y análisis de eventos del kernel
- Escaneo activo desde red local y evaluación de respuestas
- Documentación detallada del comportamiento del sistema y análisis técnico

---

## 👤 Autor

**NiKO**  
Analista de Seguridad Ofensiva y Defensiva  
github.com/tu_usuario (reemplazar)

---

## ✅ Próximas mejoras

- Automatización de detección de eventos sospechosos
- Integración con fail2ban para protección SSH
- Exportación de logs a Splunk o Grafana + Loki para visualización

