# 01 — Instalación y sintaxis

## 1. Herramienta a utilizar

En sistemas actuales prioriza **NetExec** y el binario `nxc`.

Comprueba siempre tu versión:

```bash
nxc --version
```

Y la ayuda global:

```bash
nxc --help
```

La ayuda específica de cada protocolo es aún más importante:

```bash
nxc smb --help
nxc winrm --help
nxc ldap --help
nxc ssh --help
nxc mssql --help
```

La documentación y las opciones pueden evolucionar; aprender a leer `--help` forma parte del dominio de la herramienta.

---

## 2. Instalación en Linux

Una instalación habitual recomendada por el proyecto utiliza `pipx`, porque aísla dependencias Python.

```bash
sudo apt install pipx git
pipx ensurepath
pipx install git+https://github.com/Pennyw0rth/NetExec
```

Después:

```bash
nxc --version
```

En Kali puede existir un paquete en repositorios según la versión de la distribución. Antes de mezclar varios métodos de instalación, comprueba qué binario estás ejecutando:

```bash
which nxc
```

---

## 3. Estructura general

```text
nxc <protocol> <target> <options>
```

Ejemplo básico:

```bash
nxc smb 192.168.56.20
```

Descomposición:

```text
nxc         → programa
smb         → protocolo
192.168...  → objetivo
```

Con autenticación en un laboratorio:

```bash
nxc smb 192.168.56.20 -u labuser -p 'LabPassword123!'
```

Piensa así:

```text
PROTOCOLO + TARGET + IDENTIDAD + ACCIÓN
```

---

## 4. Un host, rango o archivo

Un host:

```bash
nxc smb 192.168.56.20
```

Una subred autorizada:

```bash
nxc smb 192.168.56.0/24
```

Lista de objetivos:

```text
192.168.56.20
192.168.56.21
192.168.56.30
```

```bash
nxc smb hosts.txt
```

---

## 5. Usuario, contraseña y dominio

Cuenta local conceptual:

```bash
nxc smb TARGET -u labuser -p 'PASSWORD' --local-auth
```

Cuenta de dominio:

```bash
nxc smb TARGET -u labuser -p 'PASSWORD' -d LAB.LOCAL
```

La diferencia no es cosmética:

```text
TARGET\labuser       → identidad local del equipo
LAB\labuser          → identidad del dominio
```

Si confundes ambas, puedes interpretar incorrectamente un fallo de autenticación.

---

## 6. La salida de NetExec

Una línea de salida suele condensar datos del objetivo y resultado de la acción.

No la leas como texto decorativo. Separa mentalmente:

```text
PROTOCOLO | IP | PUERTO | HOST | DOMINIO | RESULTADO
```

Pregunta siempre:

- ¿contra qué servicio se conectó?
- ¿qué host respondió?
- ¿qué dominio/workgroup reportó?
- ¿la autenticación funcionó?
- ¿se detectó acceso administrativo?

---

## 7. Ayuda de opciones y módulos

Antes de copiar un comando de Internet:

```bash
nxc smb --help
```

Para estudiar módulos disponibles en tu instalación, consulta la ayuda/listado que proporcione tu versión y la ayuda específica del módulo.

Regla:

> La sintaxis que devuelve tu propio `nxc --help` tiene prioridad sobre una chuleta antigua de CME.

---

## 8. Errores típicos de principiante

### Confundir protocolo con puerto

`nxc smb` no significa "escanear el puerto 445". Significa **usar el cliente/protocolo SMB de NetExec contra el objetivo**.

### Usar una contraseña sin comillas

Caracteres como `!`, `$` o espacios pueden ser interpretados por la shell. Usa comillas apropiadas en el laboratorio.

### Olvidar local vs dominio

Un mismo nombre de usuario puede existir localmente y en el dominio.

### Memorizar comandos sin leer la salida

El valor está en interpretar el resultado, no en ejecutar una línea larga.

---

## Checklist del bloque

Debes saber:

- localizar `nxc`;
- comprobar la versión;
- abrir ayuda global y por protocolo;
- identificar protocolo, target, identidad y acción;
- diferenciar autenticación local y de dominio;
- interpretar una línea de salida de forma estructurada.
