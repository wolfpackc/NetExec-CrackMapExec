# 08 — NetExec frente a otras herramientas

Este bloque responde a una pregunta importante: **¿qué herramientas puedes dejar de usar a diario y cuáles debes seguir sabiendo?**

## Nmap vs NetExec

| Nmap | NetExec |
|---|---|
| Descubre hosts | Trabaja contra servicios concretos |
| Escanea puertos | Habla protocolos específicos |
| Fingerprinting | Enumeración/autenticación |
| NSE | Módulos y acciones por protocolo |
| Base de reconocimiento | Automatización post-descubrimiento |

```text
Nmap → ¿qué existe?
NetExec → ¿qué puedo preguntar a lo que existe?
```

Conclusión: **Nmap no se sustituye**.

---

## smbclient vs NetExec

```text
NetExec
  ├── enumeración rápida
  ├── varios hosts
  └── visión resumida

smbclient
  ├── navegación interactiva
  ├── archivos
  └── trabajo manual sobre un share
```

Aprende ambos, pero NetExec puede convertirse en tu primera herramienta para enumerar SMB.

---

## rpcclient vs NetExec

`rpcclient` permite consultas RPC manuales y específicas.

NetExec automatiza muchas consultas frecuentes relacionadas con hosts Windows y dominio.

```text
NetExec = automatización
rpcclient = profundidad/manualidad RPC
```

No necesitas memorizar cada comando de `rpcclient` desde el principio, pero sí entender para qué existe.

---

## Evil-WinRM vs NetExec

```text
nxc winrm
  ↓
valido acceso / automatizo

Evil-WinRM
  ↓
sesión interactiva
```

No son competidores directos.

---

## ldapsearch vs NetExec

NetExec simplifica enumeraciones frecuentes.

`ldapsearch` permite construir consultas LDAP muy específicas.

```text
NetExec = preguntas comunes rápidas
ldapsearch = consulta LDAP manual y precisa
```

---

## BloodHound vs NetExec

NetExec obtiene datos y realiza consultas concretas.

BloodHound modela relaciones y rutas de Active Directory mediante grafos.

```text
NetExec → datos puntuales
BloodHound → relaciones
```

---

## SSH vs NetExec SSH

Para conectarte normalmente:

```bash
ssh usuario@host
```

Para mantener un flujo automatizado y consistente sobre varios hosts autorizados, el soporte SSH de NetExec puede resultar útil.

---

## Telnet

NetExec no es la herramienta que debes asociar mentalmente a Telnet.

Utiliza herramientas como:

```text
telnet
nc
Nmap/NSE según tarea
```

---

## FTP

Para FTP siguen siendo más naturales clientes y herramientas específicas:

```text
ftp
lftp
Nmap/NSE
```

---

## HTTP / Web

NetExec no sustituye:

- Burp Suite;
- curl;
- ffuf;
- navegadores;
- scanners web específicos.

---

# Qué memorizar realmente

No intentes recordar 300 comandos.

Memoriza esta estructura:

```text
Nmap
  ↓
servicio
  ↓
¿NetExec soporta su protocolo?
  ├── sí → nxc PROTOCOLO TARGET
  │          ↓
  │      enumero / valido
  │          ↓
  │      ¿necesito profundidad?
  │          └── herramienta especializada
  │
  └── no → herramienta específica
```

## Kit mínimo recomendado

```text
Nmap        → reconocimiento
NetExec     → Windows/AD y protocolos compatibles
smbclient   → SMB interactivo
Evil-WinRM  → WinRM interactivo
Burp/curl   → web
ssh         → SSH interactivo
Wireshark   → paquetes
```

Con ese núcleo puedes resolver una enorme cantidad de situaciones de laboratorio sin intentar memorizar una herramienta distinta para cada pequeño paso.
