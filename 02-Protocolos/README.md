# 02 — Protocolos

## Visión general

NetExec organiza gran parte de su funcionalidad alrededor del **protocolo** que quieres utilizar.

```text
                         NetExec
                           |
      ┌─────────────┬──────┼──────┬─────────────┐
      │             │      │      │             │
     SMB          WinRM   LDAP   SSH          MSSQL
      │             │      │      │             │
 Windows/AD      Remoting   AD   Linux/Unix   SQL Server
```

Según la versión pueden existir protocolos adicionales. Usa siempre:

```bash
nxc --help
```

para comprobar los disponibles en tu instalación.

---

# 1. SMB

Puerto habitual:

```text
445/tcp
```

Es probablemente el protocolo más importante en el aprendizaje inicial de NetExec.

Puede ayudarte a obtener información como:

- hostname;
- dominio/workgroup;
- versión aproximada del sistema;
- configuración/signing según capacidades;
- recursos compartidos;
- usuarios/grupos en determinados contextos;
- sesiones y otra información Windows;
- validación de credenciales autorizadas.

Ejemplo de reconocimiento básico:

```bash
nxc smb 192.168.56.20
```

Modelo mental:

```text
SMB = archivos + autenticación Windows + mucha información del host
```

---

# 2. WinRM

Puertos habituales:

```text
5985/tcp  HTTP
5986/tcp  HTTPS
```

WinRM es la infraestructura de administración remota de Windows sobre WS-Management.

NetExec puede ayudarte a comprobar si unas credenciales autorizadas funcionan contra WinRM.

```bash
nxc winrm 192.168.56.20 -u labuser -p 'PASSWORD'
```

Pero para trabajar interactivamente con una máquina del laboratorio suele resultar más cómoda una herramienta dedicada como Evil-WinRM.

Modelo:

```text
NetExec → ¿puedo autenticarme / qué acceso tengo?
Evil-WinRM → quiero trabajar dentro de una sesión WinRM
```

---

# 3. LDAP

Puertos habituales:

```text
389/tcp   LDAP
636/tcp   LDAPS
```

LDAP es esencial para Active Directory porque permite consultar el directorio.

Conceptualmente contiene objetos como:

```text
Active Directory
│
├── Users
├── Groups
├── Computers
├── OUs
├── Policies / attributes
└── Relationships
```

NetExec puede automatizar numerosas consultas habituales. Para análisis gráfico de relaciones complejas, BloodHound pertenece a otra categoría de herramienta.

---

# 4. SSH

Puerto habitual:

```text
22/tcp
```

Aunque NetExec nació muy ligado a Windows/AD, versiones modernas incorporan soporte para SSH.

Sirve para mantener una sintaxis coherente al validar acceso y realizar determinadas tareas contra sistemas SSH autorizados.

No sustituye al cliente OpenSSH para una sesión interactiva normal:

```bash
ssh usuario@host
```

---

# 5. MSSQL

Puerto habitual:

```text
1433/tcp
```

Microsoft SQL Server aparece con frecuencia en redes empresariales Windows.

Modelo mental:

```text
MSSQL
  |
  +-- autenticación
  +-- bases de datos
  +-- roles/permisos
  +-- consultas
  +-- integración con Windows/AD
```

NetExec facilita comprobaciones y enumeración habituales, mientras que un cliente SQL especializado es mejor para trabajo profundo con consultas y administración.

---

# 6. RDP

Puerto habitual:

```text
3389/tcp
```

RDP es un protocolo de escritorio remoto.

Cuando el soporte de tu versión de NetExec lo permita, puede utilizarse para determinadas comprobaciones relacionadas con RDP.

Para una sesión gráfica real utilizarías un cliente dedicado como FreeRDP o el cliente de Escritorio Remoto de Windows.

---

# 7. WMI

WMI es una infraestructura de administración de Windows y no debe pensarse simplemente como "un puerto" aislado: puede apoyarse en RPC/DCOM y varios componentes del sistema.

Es importante porque históricamente CME/NetExec han utilizado distintos métodos de administración/ejecución remota en Windows.

Primero entiende WMI como tecnología de administración. Después estudia qué capacidades concretas ofrece tu versión de NetExec.

---

# 8. ¿Y Telnet, FTP y HTTP?

Aquí aparece una idea clave:

**NetExec no pretende ser cliente universal para cualquier protocolo existente.**

Para servicios como:

```text
Telnet → telnet / nc / Nmap NSE según tarea
FTP    → ftp / lftp / herramientas específicas
HTTP   → curl / Burp Suite / ffuf / navegadores / scanners web
```

seguirás utilizando herramientas dedicadas.

---

# Mapa rápido

| Servicio | NetExec | Herramienta especializada típica |
|---|---:|---|
| SMB | Muy importante | smbclient, rpcclient |
| WinRM | Sí | Evil-WinRM |
| LDAP / AD | Sí | ldapsearch, BloodHound |
| SSH | Según versión, sí | OpenSSH |
| MSSQL | Sí | sqlcmd / clientes SQL |
| RDP | Según versión | FreeRDP / mstsc |
| WMI | Ecosistema Windows | herramientas WMI/Impacket |
| Telnet | No es foco | telnet / nc |
| FTP | No es foco principal | ftp / lftp |
| HTTP web | No sustituye testing web | Burp / curl / ffuf |

---

## Ejercicio mental

Tienes este resultado de Nmap:

```text
22/tcp   open ssh
80/tcp   open http
445/tcp  open microsoft-ds
5985/tcp open wsman
```

Decisión:

```text
22   → nxc ssh o ssh
80   → herramientas web
445  → nxc smb
5985 → nxc winrm / Evil-WinRM
```

Eso es exactamente la habilidad que debes adquirir: **traducir servicio → protocolo → herramienta adecuada**.
