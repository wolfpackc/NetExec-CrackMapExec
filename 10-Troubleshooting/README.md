# 10 — Troubleshooting

## 1. `nxc: command not found`

Comprueba instalación y PATH:

```bash
which nxc
pipx list
```

Si acabas de instalar con pipx:

```bash
pipx ensurepath
```

Abre una nueva shell si fuera necesario.

---

## 2. La sintaxis de Internet no funciona

Primero:

```bash
nxc --version
nxc PROTOCOLO --help
```

Muchos tutoriales antiguos utilizan CrackMapExec (`cme`) o versiones anteriores de NetExec.

Regla:

```text
Ayuda de tu versión > chuleta antigua > comando copiado de un blog
```

---

## 3. `Connection refused`

Normalmente significa que el host responde pero el servicio/puerto no acepta la conexión.

Comprueba con Nmap:

```bash
nmap -p445,5985,389 TARGET
```

Posibles causas:

- servicio detenido;
- puerto cerrado;
- firewall;
- protocolo equivocado;
- IP incorrecta.

---

## 4. Timeout

Puede indicar:

- host apagado;
- filtrado por firewall;
- ruta incorrecta;
- interfaz/VPN equivocada;
- servicio no accesible desde tu segmento.

Comprueba:

```bash
ip addr
ip route
ping TARGET
nmap -Pn -pPUERTO TARGET
```

No asumas que un timeout es un problema de NetExec.

---

## 5. Credenciales aparentemente correctas fallan

Comprueba el contexto:

```text
¿cuenta local?
¿cuenta de dominio?
¿dominio correcto?
¿contraseña interpretada por la shell?
¿usuario habilitado?
¿servicio permite ese método de autenticación?
```

Ejemplo conceptual:

```text
CLIENT01\edu  !=  LAB\edu
```

---

## 6. Caracteres especiales en contraseñas

La shell puede interpretar caracteres especiales.

En un laboratorio, utiliza comillas apropiadas:

```bash
-p 'LabPassword123!'
```

Nunca pegues credenciales reales en documentación pública.

---

## 7. SMB funciona pero WinRM no

Perfectamente posible.

```text
445 abierto  → SMB disponible
5985 cerrado → WinRM no disponible
```

Los servicios son independientes. Una cuenta válida por SMB tampoco garantiza que tenga permiso de inicio remoto por WinRM.

---

## 8. Autenticación correcta pero sin permisos

Distingue:

```text
Credencial válida
      ↓
Usuario autenticado
      ↓
¿qué autorización tiene?
      ↓
usuario normal / operador / administrador / permisos específicos
```

Autenticación y autorización son conceptos distintos.

---

## 9. No aparecen shares esperados

Comprueba:

- qué usuario has utilizado;
- permisos del share;
- permisos NTFS;
- si el recurso existe;
- si estás consultando el host correcto.

Recuerda:

```text
Permiso efectivo ≈ combinación de permisos del recurso + permisos del sistema de archivos
```

---

## 10. El comando funciona con IP pero no con hostname

Investiga resolución de nombres:

```bash
getent hosts HOSTNAME
nslookup HOSTNAME
```

En Active Directory, DNS es fundamental. No soluciones todos los problemas añadiendo entradas manuales sin entender por qué falla la resolución.

---

## 11. Problemas en AD

Comprueba, en este orden:

```text
1. conectividad
2. DNS
3. hora del sistema
4. dominio
5. credenciales
6. protocolo
7. permisos
```

Kerberos es especialmente sensible a DNS, nombres y sincronización temporal.

---

## 12. Método general de diagnóstico

Cuando falle NetExec:

```text
¿Tengo red?
   ↓
¿El host responde?
   ↓
¿El puerto está abierto?
   ↓
¿Es realmente ese servicio?
   ↓
¿NetExec soporta el protocolo?
   ↓
¿Mi sintaxis corresponde a mi versión?
   ↓
¿La identidad es local o dominio?
   ↓
¿Autenticación o autorización es lo que falla?
```

Si sigues ese orden, evitas tratar cada mensaje de error como un problema nuevo.
