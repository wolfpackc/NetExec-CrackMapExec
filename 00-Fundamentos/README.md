# 00 — Fundamentos

## 1. Qué es NetExec

NetExec es una herramienta de automatización para evaluación de seguridad en redes, especialmente útil en entornos Windows y Active Directory.

No es un escáner de puertos generalista como Nmap. Su fuerte es hablar directamente con protocolos concretos y automatizar tareas repetitivas sobre muchos hosts.

Una forma simple de visualizarlo:

```text
Herramientas especializadas

smbclient   rpcclient   ldapsearch   evil-winrm   mssql tools
     \          |           |            |             /
      \         |           |            |            /
                 NetExec
                   |
           interfaz unificada
```

Esto no significa que NetExec implemente absolutamente todo lo que hacen esas herramientas. Significa que cubre muchas tareas frecuentes con una sintaxis coherente.

---

## 2. Analogía: la multiherramienta

Imagina una caja de herramientas:

- Nmap es el detector que te dice qué puertas existen.
- `smbclient` es una herramienta especializada para trabajar con SMB.
- `rpcclient` permite profundizar en determinadas llamadas RPC.
- Evil-WinRM ofrece una shell interactiva WinRM.
- NetExec es una multiherramienta que incorpora varias funciones habituales para no cambiar constantemente de programa.

La multiherramienta es cómoda, pero si necesitas hacer un trabajo muy específico, vuelves a la herramienta especializada.

---

## 3. Flujo mental

Antes de escribir un comando de NetExec responde:

```text
1. ¿Qué host tengo?
2. ¿Qué puerto/servicio tiene abierto?
3. ¿Qué protocolo corresponde a ese servicio?
4. ¿Tengo credenciales o estoy enumerando sin ellas?
5. ¿Qué dato concreto quiero obtener?
6. ¿Necesito NetExec o una herramienta más especializada?
```

Ejemplo conceptual:

```text
192.168.56.20
    |
    +-- 445/tcp abierto
            |
            +-- SMB
                  |
                  +-- nxc smb 192.168.56.20 ...
```

---

## 4. Arquitectura básica de un comando

La mayoría de órdenes siguen este patrón mental:

```text
nxc <protocolo> <objetivo> [autenticación] [acción]
```

Ejemplos inocuos de laboratorio:

```bash
nxc smb 192.168.56.20
nxc winrm 192.168.56.20
nxc ssh 192.168.56.30
```

Primero eliges **cómo hablar**, después **con quién hablar**, y finalmente **qué quieres preguntar o hacer**.

---

## 5. Targets

NetExec puede trabajar conceptualmente con:

```text
Un host
192.168.56.20

Una subred autorizada
192.168.56.0/24

Una lista de hosts
hosts.txt
```

El valor de NetExec aumenta cuando quieres realizar la misma comprobación sobre varios equipos de un laboratorio.

---

## 6. Autenticación

En redes Windows vas a encontrarte principalmente con:

- usuario y contraseña;
- dominio + usuario + contraseña;
- credenciales locales;
- hashes NTLM en determinados flujos;
- Kerberos en entornos de dominio.

Modelo mental:

```text
Identidad
   |
   +-- cuenta local
   |
   +-- cuenta de dominio
           |
           +-- NTLM
           +-- Kerberos
```

No memorices todavía flags. Primero entiende qué identidad estás intentando usar y contra qué servicio.

---

## 7. Qué significa "Pwn3d!"

En determinadas salidas históricas de CME/NetExec puede aparecer una indicación equivalente a que la cuenta posee privilegios administrativos suficientes sobre el objetivo para determinadas acciones.

No significa "he comprometido mágicamente el ordenador". Debes interpretarlo como una señal de **nivel de acceso** dentro del protocolo que estás utilizando.

---

## 8. Qué NO es NetExec

NetExec no sustituye completamente a:

- Nmap para descubrimiento y puertos;
- Wireshark para análisis detallado de paquetes;
- Burp Suite para testing web profundo;
- Evil-WinRM para una sesión WinRM interactiva cómoda;
- smbclient para determinadas operaciones manuales con archivos;
- ldapsearch/BloodHound para ciertos análisis LDAP y relaciones complejas;
- clientes SQL completos para trabajo profundo con bases de datos.

---

## 9. Qué debes dominar de este bloque

Al terminar deberías poder responder sin mirar notas:

1. ¿Qué diferencia hay entre Nmap y NetExec?
2. ¿Por qué se llama a CME/NXC "navaja suiza"?
3. ¿Qué significa elegir un protocolo?
4. ¿Por qué NetExec no elimina la necesidad de herramientas especializadas?
5. ¿Cuál es el patrón mental de un comando `nxc`?
