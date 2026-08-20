# NetExec / CrackMapExec — Guía de estudio

Guía técnica y progresiva para aprender **NetExec (`nxc`)** y comprender su relación histórica con **CrackMapExec (`cme`)**.

> En 2026 la referencia principal debe ser **NetExec**. CrackMapExec quedó archivado en 2023 y NetExec continúa activamente la filosofía y gran parte del flujo de trabajo original.

Todo el contenido de este repositorio está planteado para **laboratorios propios, CTFs y sistemas expresamente autorizados**.

---

## Objetivo

No se trata de memorizar comandos. La meta es entender:

1. qué protocolo estás usando;
2. qué servicio remoto estás consultando;
3. qué información puede darte ese protocolo;
4. cuándo NetExec simplifica varias herramientas especializadas;
5. cuándo debes abandonar NetExec y utilizar una herramienta específica;
6. qué significa realmente la salida de `nxc`.

---

## Idea central

Piensa en NetExec como una **consola unificada para hablar con muchos servicios habituales de entornos Windows y Active Directory**.

```text
                       ┌─────────────────────┐
                       │       NetExec       │
                       │        nxc          │
                       └──────────┬──────────┘
                                  │
          ┌───────────────┬───────┼─────────┬───────────────┐
          │               │       │         │               │
         SMB            WinRM    LDAP      SSH            MSSQL
          │               │       │         │               │
   Shares / Hosts     Remoting    AD     Linux/Unix       SQL Server
```

NetExec **no sustituye a Nmap**. Nmap sigue siendo la herramienta base para descubrimiento, puertos y fingerprinting. NetExec entra normalmente después, cuando ya sabes qué servicios existen.

```text
Nmap
  ↓
Descubro servicios
  ↓
NetExec
  ↓
Enumero / valido acceso / automatizo tareas
  ↓
Herramienta especializada si necesito profundidad
```

---

## Estructura del curso

| Orden | Bloque | Contenido |
|---|---|---|
| 00 | [Fundamentos](00-Fundamentos/README.md) | Qué es CME/NXC, arquitectura mental y terminología |
| 01 | [Instalación y sintaxis](01-Instalacion-y-Sintaxis/README.md) | Instalación, ayuda, targets, credenciales y opciones |
| 02 | [Protocolos](02-Protocolos/README.md) | SMB, WinRM, LDAP, SSH, MSSQL, RDP y otros |
| 03 | [SMB a fondo](03-SMB/README.md) | Enumeración Windows, shares, sesiones y permisos |
| 04 | [WinRM y acceso remoto](04-WinRM/README.md) | Qué es WinRM, cuándo usar NXC y cuándo Evil-WinRM |
| 05 | [Active Directory](05-Active-Directory/README.md) | LDAP, dominio, usuarios, grupos y relaciones |
| 06 | [Módulos](06-Modulos/README.md) | Cómo pensar los módulos y cómo consultar su ayuda |
| 07 | [Laboratorios](07-Laboratorios/README.md) | Ejercicios progresivos en red aislada |
| 08 | [Comparativa de herramientas](08-Comparativas/README.md) | NXC vs smbclient, rpcclient, Evil-WinRM, Nmap, etc. |
| 09 | [Chuleta](09-Chuleta/README.md) | Sintaxis y flujo mental resumido |
| 10 | [Troubleshooting](10-Troubleshooting/README.md) | Errores frecuentes, autenticación y conectividad |

---

## Ruta recomendada

```text
Fundamentos
    ↓
Sintaxis global
    ↓
SMB
    ↓
WinRM
    ↓
LDAP / Active Directory
    ↓
MSSQL / SSH / RDP
    ↓
Módulos
    ↓
Laboratorios
    ↓
Chuleta
```

Si puedes explicar cada paso sin mirar comandos, estás aprendiendo la herramienta correctamente.

---

## CME vs NetExec

CrackMapExec fue creado en 2015. Tras cambios de mantenimiento, parte de la comunidad que contribuía al proyecto continuó el desarrollo bajo el nombre **NetExec**. Actualmente el repositorio de NetExec se presenta como la versión actual y mantenida del proyecto.

Por eso en esta guía:

```text
CME / crackmapexec  → contexto histórico y comandos antiguos
NXC / netexec       → herramienta principal que debes aprender
```

Muchos conceptos y comandos son muy parecidos, por lo que saber interpretar documentación antigua de CME sigue siendo útil.

---

## Regla de oro

**Nmap descubre. NetExec conversa con el servicio. La herramienta especializada profundiza.**

Ese modelo mental evita intentar usar NetExec para absolutamente todo.
