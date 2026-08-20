# 07 — Laboratorios progresivos

Todos estos ejercicios deben realizarse únicamente en redes propias, CTFs o sistemas expresamente autorizados.

## Laboratorio 1 — Identificar cuándo usar NetExec

Objetivo: traducir puertos en decisiones.

Topología sugerida:

```text
Kali        192.168.56.10
Windows     192.168.56.20
Linux       192.168.56.30
```

Primero:

```bash
nmap -sV 192.168.56.20
```

Crea una tabla:

| Puerto | Servicio | ¿NetExec? | Protocolo/herramienta |
|---|---|---|---|
| 445 | SMB | Sí | `nxc smb` |
| 5985 | WinRM | Sí | `nxc winrm` |
| 80 | HTTP | No como sustituto web | Burp/curl/ffuf |

Objetivo real: aprender a elegir, no ejecutar por ejecutar.

---

## Laboratorio 2 — SMB sin credenciales

```bash
nxc smb 192.168.56.20
```

Anota:

- hostname;
- dominio/workgroup;
- versión aproximada;
- signing/configuración que reporte;
- puerto utilizado.

Después compara con:

```bash
nmap -p445 --script smb-os-discovery 192.168.56.20
```

Pregunta: ¿qué información presenta cada herramienta y de qué forma?

---

## Laboratorio 3 — SMB con cuenta de laboratorio

Crea una cuenta sin privilegios en tu Windows de pruebas.

```bash
nxc smb 192.168.56.20 -u labuser -p 'PASSWORD'
```

Después:

```bash
nxc smb 192.168.56.20 -u labuser -p 'PASSWORD' --shares
```

No busques "comprometer" nada. Analiza la diferencia entre:

```text
conectividad
≠
autenticación
≠
autorización
≠
administrador
```

---

## Laboratorio 4 — NetExec vs smbclient

Primero obtén los shares con NetExec.

Luego entra manualmente en un share de laboratorio:

```bash
smbclient //192.168.56.20/Public -U labuser
```

Conclusión que debes poder explicar:

```text
NetExec enumera rápidamente muchos objetivos.
smbclient permite trabajar manualmente con un recurso SMB concreto.
```

---

## Laboratorio 5 — WinRM

Comprueba puertos:

```bash
nmap -p5985,5986 192.168.56.20
```

Después:

```bash
nxc winrm 192.168.56.20 -u labuser -p 'PASSWORD'
```

Escribe en tus notas:

1. ¿estaba WinRM habilitado?
2. ¿el firewall permitía acceso?
3. ¿la cuenta podía autenticarse?
4. ¿qué aporta NetExec frente a una sesión Evil-WinRM?

---

## Laboratorio 6 — Mini dominio AD

Topología:

```text
Kali              192.168.56.10
DC01 LAB.LOCAL     192.168.56.11
CLIENT01           192.168.56.20
```

Flujo:

```bash
nmap -sV 192.168.56.11
nxc smb 192.168.56.11
nxc ldap 192.168.56.11
```

Con una cuenta de dominio creada por ti:

```bash
nxc smb 192.168.56.11 -u labuser -p 'PASSWORD' -d LAB.LOCAL
nxc ldap 192.168.56.11 -u labuser -p 'PASSWORD' -d LAB.LOCAL
```

Objetivo: distinguir qué aprende SMB del host y qué aprende LDAP del directorio.

---

## Laboratorio 7 — Un mismo usuario, local vs dominio

Crea deliberadamente:

```text
CLIENT01\edu
LAB\edu
```

con contraseñas distintas.

Comprueba ambos contextos en tu laboratorio y documenta por qué `--local-auth` cambia la identidad que estás validando.

---

## Laboratorio 8 — Flujo completo

Partes únicamente de una IP:

```text
192.168.56.20
```

Debes construir esta cadena sin consultar la solución:

```text
1. Descubrimiento
2. Puertos
3. Servicios
4. Selección de protocolo
5. Enumeración NetExec
6. Autenticación autorizada si existe
7. Acción específica
8. Herramienta especializada cuando corresponda
9. Documentación
```

### Plantilla de informe

```markdown
# Host

## Descubrimiento

## Servicios

## NetExec

## Credenciales de laboratorio utilizadas

## Información obtenida

## Herramientas especializadas utilizadas

## Conclusiones

## Qué aprendí del protocolo
```

---

# Criterio de dominio

No des por aprendido un laboratorio hasta poder explicar:

- qué paquete/protocolo lógico estás usando;
- qué servicio remoto responde;
- qué credencial se valida;
- qué permiso permite cada resultado;
- por qué elegirías NetExec o una alternativa.
