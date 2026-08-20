# 09 — Chuleta NetExec

Esta chuleta no sustituye la documentación ni `--help`. Está pensada para recordar el **patrón mental**.

## Sintaxis base

```bash
nxc <protocolo> <target> [opciones]
```

## Ayuda

```bash
nxc --help
nxc smb --help
nxc winrm --help
nxc ldap --help
nxc ssh --help
nxc mssql --help
```

## Reconocimiento básico por protocolo

```bash
nxc smb TARGET
nxc winrm TARGET
nxc ldap TARGET
nxc ssh TARGET
nxc mssql TARGET
```

## Autenticación de laboratorio

```bash
nxc smb TARGET -u labuser -p 'PASSWORD'
nxc winrm TARGET -u labuser -p 'PASSWORD'
nxc ldap TARGET -u labuser -p 'PASSWORD' -d LAB.LOCAL
```

Cuenta local Windows:

```bash
nxc smb TARGET -u labuser -p 'PASSWORD' --local-auth
```

## Shares SMB

```bash
nxc smb TARGET -u labuser -p 'PASSWORD' --shares
```

## Targets

```text
Un host:
192.168.56.20

CIDR:
192.168.56.0/24

Archivo:
hosts.txt
```

## Flujo rápido

```text
Nmap
 ↓
445  → nxc smb
5985 → nxc winrm
389  → nxc ldap
22   → nxc ssh / ssh
1433 → nxc mssql
3389 → nxc rdp / cliente RDP según necesidad
80   → herramientas web
21   → herramientas FTP
23   → herramientas Telnet
```

## Herramientas complementarias

```text
SMB interactivo     → smbclient
RPC manual          → rpcclient
WinRM interactivo   → Evil-WinRM
LDAP específico     → ldapsearch
Relaciones AD       → BloodHound
SSH interactivo     → ssh
Web                 → Burp / curl / ffuf
Paquetes            → Wireshark
Puertos             → Nmap
```

## Preguntas antes de ejecutar

```text
¿qué protocolo?
¿qué target?
¿cuenta local o dominio?
¿qué quiero obtener?
¿es solo lectura?
¿necesito una herramienta más específica?
```

## Frase para recordar

**Nmap descubre → NetExec enumera/valida → herramienta especializada profundiza.**
