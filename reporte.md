## 🛡️ Informe Técnico – Laboratorio de Firewall, Escaneo y Análisis de Logs

### 🎯 Objetivo del laboratorio

Simular un entorno real de ciberseguridad defensiva utilizando una Raspberry Pi como servidor víctima y una máquina con Parrot OS como atacante, para:

* Configurar reglas de firewall (UFW)
* Ejecutar escaneos con Nmap
* Analizar la generación y contenido de logs con `journalctl`
* Detectar eventos reales de conexión y bloquear tráfico
* Documentar el comportamiento del sistema ante tráfico autorizado, bloqueado y auditado

---

## 🧩 Infraestructura

* **Dispositivo víctima**: Raspberry Pi OS 6.3 (Lory)

  * IP: `192.168.1.78`
  * UFW activo
  * SSH habilitado

* **Dispositivo atacante**: Parrot OS (máquina local)

  * IP: `192.168.1.24`

* **Dispositivo desconocido en la red**: `192.168.1.10`

* **Dispositivo de control remoto**: Windows 10

  * Utilizado para conexión SSH segura al servidor víctima

---

## 🔐 Configuración de Firewall (UFW)

```bash
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 192.168.1.24 to any port 22 proto tcp
sudo ufw deny 23
sudo ufw logging full
```

**Estado final:**

```text
Status: active
Default: deny (incoming), allow (outgoing)
Rules:
  22/tcp ALLOW from 192.168.1.24
  23     DENY from anywhere
```

---

## 🔎 Escaneos realizados desde Parrot OS

### 🔸 Escaneo SYN TCP

```bash
sudo nmap -sS -p 22,23 192.168.1.78 -oN scan_syn.txt
```

**Resultado esperado:**

* Puerto 22: abierto (por estar permitido para IP atacante)
* Puerto 23: `filtered` (no respuesta)

### 🔸 Escaneo completo de puertos

```bash
sudo nmap -sS -p- 192.168.1.78 -oN full_scan.txt
```

**Resultado:** mayoría de puertos cerrados o filtrados

### 🔸 Netcat – Simulación de conexión real

```bash
nc -vz 192.168.1.78 23
```

**Resultado:** conexión rechazada (bloqueo UFW)

---

## 📄 Análisis de logs

### 📌 SSH – conexión legítima desde Parrot

```text
May 24 01:33:46 raspberrypi kernel: [UFW AUDIT] IN=wlan0 OUT= ... SRC=192.168.1.24 DST=192.168.1.78 PROTO=TCP SPT=33750 DPT=22 ...
```

✅ Evento auditado, no bloqueado. Conexión permitida desde IP autorizada.

---

### 📌 Bloqueo de puerto denegado (23/tcp)

```text
May 24 XX:XX:XX raspberrypi kernel: [UFW BLOCK] IN=wlan0 SRC=192.168.1.24 DST=192.168.1.78 PROTO=TCP SPT=PORT DPT=23 ...
```

✅ UFW bloqueó intento de conexión al puerto telnet (23) desde IP atacante. Resultado correctamente registrado.

---

## 🕵️ Detección de IP desconocida en la red

### 🔎 Log detectado

```text
May 24 XX:XX:XX raspberrypi kernel: [UFW BLOCK] IN=wlan0 SRC=192.168.1.10 DST=192.168.1.78 ...
```

### 🧪 Escaneo a 192.168.1.10 desde Parrot

```bash
sudo nmap -A 192.168.1.10 -oN scan_host_10.txt
```

**Resultado:**

* Host activo
* Todos los puertos cerrados (respuesta RST)
* Sistema operativo no identificado (demasiadas huellas)
* MAC Address: `26:6F:EE:71:DE:73` (fabricante desconocido)

### 🧠 Interpretación profesional

Dispositivo en la red con comportamiento silencioso, sin puertos abiertos, pero generando conexiones entrantes previamente detectadas. Posibles hipótesis:

* Dispositivo embebido (IoT, TV, router)
* Máquina virtual
* Activo no autorizado

Requiere investigación física o escaneo adicional para su identificación.

---

## 📌 Conclusiones

* La Raspberry Pi respondió correctamente a la política de firewall definida: permitió, auditó y bloqueó el tráfico según la IP origen y puerto destino.
* Los logs de `journalctl` fueron clave para observar intentos de conexión, distinguir entre `AUDIT` y `BLOCK`, y documentar comportamiento del sistema.
* Se identificó un host adicional en la red que debe ser revisado como parte de una política de control de activos.

---

## ✅ Próximos pasos sugeridos

* Implementar fail2ban para proteger SSH con detección de fuerza bruta.
* Activar alertas en tiempo real vía syslog o script personalizado.
* Crear dashboard con Grafana + Loki o enviar logs a Splunk.
* Realizar análisis de puertos UDP y DNS para detección lateral.

---

*Informe elaborado por: NiKO, analista de seguridad ofensiva y defensiva en entorno de laboratorio real.*
