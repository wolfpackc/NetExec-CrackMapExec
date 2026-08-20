# 03 — SMB a fondo

SMB es el protocolo que más debes dominar al empezar con NetExec en redes Windows.
<img width="5585" height="1667" alt="mermaid-diagram (2)" src="https://github.com/user-attachments/assets/5cec9471-6d77-4746-acd4-32264641862b" />


---

## 2. Qué aporta NetExec sobre SMB

NetExec puede condensar en una sola salida varias piezas de información del host.

Ejemplo de laboratorio:

```bash
nxc smb 192.168.56.20
```
<img width="1322" height="103" alt="image" src="https://github.com/user-attachments/assets/a47287ef-4e4d-4e66-bda4-acc47834e029" />

---

<img width="1410" height="76" alt="image" src="https://github.com/user-attachments/assets/1dceda9f-ed9d-4bac-8061-1dba58d496cb" />
No memorices la línea. Interpreta:

```text
¿Quién responde?
¿Qué nombre tiene?
¿Pertenece a dominio o workgroup?
¿Qué versión/properties reporta?
¿SMB signing está habilitado/requerido?
```

---

## 3. Enumeración de shares

Con credenciales autorizadas:

```bash
nxc smb 192.168.56.20 -u labuser -p 'PASSWORD' --shares
```

Piensa en un share como una carpeta publicada en red:

```text
Servidor
│
├── C$
├── ADMIN$
├── IPC$
├── Public
└── Backups
```
<img width="1398" height="507" alt="image" src="https://github.com/user-attachments/assets/f8acab1e-cf25-4971-9f99-720b852c6ed6" />



Cada share tiene permisos propios. Poder verlo no implica poder escribir en él.

Preguntas:

- ¿puedo listar?
- ¿puedo leer?
- ¿puedo escribir?
- ¿es administrativo?
- ¿contiene información sensible del laboratorio?

---

## 4. SMB y autenticación

En Windows, SMB está íntimamente relacionado con autenticación NTLM/Kerberos y cuentas locales/de dominio.

```text
Cliente
   |
   | credenciales
   v
Servidor SMB
   |
   +-- valida identidad
   +-- determina permisos
   +-- permite/deniega recursos
```

Una autenticación correcta no implica necesariamente privilegios administrativos.

---

## 5. Cuenta local vs cuenta de dominio

```text
PC01\adminlab      → cuenta local
LAB\eduardo        → cuenta de dominio
```

Ejemplo local:

```bash
nxc smb 192.168.56.20 -u adminlab -p 'PASSWORD' --local-auth
```

Ejemplo dominio:

```bash
nxc smb 192.168.56.20 -u eduardo -p 'PASSWORD' -d LAB.LOCAL
```

---

## 6. NetExec vs smbclient

NetExec es muy cómodo para responder rápidamente:

```text
¿Qué shares existen y qué permisos tengo?
```

`smbclient` es mejor cuando quieres navegar manualmente:

```bash
smbclient //192.168.56.20/Public -U labuser
```

Analogía:

```text
NetExec   = recepcionista que te dice qué habitaciones puedes abrir
smbclient = entras en una habitación y trabajas con los archivos
```

---

## 7. NetExec vs rpcclient

RPC permite consultar diferentes servicios de administración de Windows.

`rpcclient` puede ser útil para realizar consultas específicas y manuales.

NetExec automatiza muchas enumeraciones frecuentes, pero saber que por debajo existen RPC/SAMR/LSA y otros componentes evita tratarlo como magia.

```text
NetExec
   |
   +-- automatiza consultas
          |
          +-- SMB
          +-- RPC y servicios Windows relacionados
```

---

## 8. SMB Signing

SMB signing permite verificar la integridad/autenticidad de mensajes SMB mediante firmas.

No basta con aprender "signing true/false". Debes entender la consecuencia:

```text
Sin protección suficiente
Cliente <--------> Servidor
      riesgo de manipulación/relay según contexto

Con signing requerido
Cliente =====firma=====> Servidor
```
<img width="1086" height="1448" alt="ChatGPT Image 20 ago 2026, 14_21_26" src="https://github.com/user-attachments/assets/cf3f240b-0b7b-4ad7-8bbe-8b6bcfefdf9b" />


NetExec puede ayudarte a identificar esta propiedad del host, pero la interpretación requiere entender SMB y NTLM.

---

## 9. Flujo de estudio SMB

```text
Nmap
  ↓
445 abierto
  ↓
nxc smb TARGET
  ↓
Identifico host / dominio / configuración
  ↓
Tengo credenciales autorizadas
  ↓
Valido autenticación
  ↓
Enumero shares / información permitida
  ↓
Si necesito trabajar con archivos → smbclient
```

---

## 10. Ejercicio de laboratorio

Supón:

```text
Kali:      192.168.56.10
Windows:   192.168.56.20
Usuario:   labuser
Password:  definida por ti
```

Ejecuta en tu red aislada:

```bash
nmap -p445 192.168.56.20
nxc smb 192.168.56.20
nxc smb 192.168.56.20 -u labuser -p 'PASSWORD'
nxc smb 192.168.56.20 -u labuser -p 'PASSWORD' --shares
```

Después responde:

1. ¿Qué datos obtuvo Nmap y cuáles NetExec?
2. ¿Qué cambia al añadir credenciales?
3. ¿Qué shares aparecen?
4. ¿Qué permisos reporta cada share?
5. ¿En qué momento usarías `smbclient`?

---

## Objetivo del bloque

Debes ser capaz de explicar SMB sin mencionar NetExec. Solo entonces NetExec pasa de ser una colección de comandos a ser una herramienta que entiendes.
