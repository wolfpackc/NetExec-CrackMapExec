# 05 — Active Directory con NetExec

## 1. Qué aporta NetExec en AD

NetExec resulta especialmente útil cuando el entorno tiene dominio y quieres automatizar consultas sobre varios equipos o servicios.

Piensa en Active Directory como una base de datos jerárquica de identidades y relaciones:

```text
Dominio
│
├── Usuarios
├── Grupos
├── Equipos
├── OUs
├── Políticas
└── Relaciones y permisos
```

NetExec puede consultar parte de esa información a través de protocolos como SMB y LDAP.

---

## 2. LDAP como puerta al directorio

```text
Cliente
   |
   | consulta LDAP
   v
Controlador de dominio
   |
   +-- objetos
   +-- atributos
   +-- grupos
   +-- equipos
```

Ejemplo de conectividad en laboratorio:

```bash
nxc ldap 192.168.56.10
```

Con credenciales autorizadas:

```bash
nxc ldap 192.168.56.10 -u labuser -p 'PASSWORD' -d LAB.LOCAL
```

Consulta siempre `nxc ldap --help` para ver las capacidades exactas de tu versión.

---

## 3. Qué debes saber enumerar conceptualmente

Antes de aprender flags específicos, debes comprender estas preguntas:

- ¿cuál es el dominio?
- ¿quiénes son los usuarios?
- ¿qué grupos existen?
- ¿qué equipos pertenecen al dominio?
- ¿qué usuarios pertenecen a grupos privilegiados?
- ¿qué controlador de dominio estás consultando?

---

## 4. SMB + LDAP

No los estudies como mundos separados.

```text
SMB
  └── información del host, shares, autenticación Windows

LDAP
  └── información del directorio y objetos del dominio

Juntos
  └── visión mucho más útil del entorno Windows/AD
```

---

## 5. NetExec vs BloodHound

NetExec es excelente para consultar y automatizar información concreta.

BloodHound está orientado a representar **relaciones** y rutas dentro de Active Directory.

Analogía:

```text
NetExec   = hacer preguntas concretas al directorio
BloodHound = dibujar el mapa de relaciones del directorio
```

No son sustitutos completos entre sí.

---

## 6. NetExec vs ldapsearch

`ldapsearch` permite consultas LDAP manuales y muy flexibles.

NetExec empaqueta consultas típicas de pentesting y administración de seguridad para ahorrar sintaxis y tiempo.

Cuando necesites una consulta LDAP muy específica, entender LDAP y filtros sigue siendo necesario.

---

## 7. Autenticación en dominio

Modelo:

```text
LAB.LOCAL
   |
   +-- labuser
          |
          +-- contraseña / mecanismos compatibles
          |
          +-- permisos derivados de grupos y ACLs
```

Una credencial válida no equivale a privilegios administrativos.

---

## 8. Flujo de laboratorio

```text
Nmap
  ↓
Identifico DC / 389 / 445
  ↓
nxc smb DC
  ↓
nxc ldap DC
  ↓
Identifico dominio
  ↓
Autenticación autorizada
  ↓
Consultas de usuarios, grupos y equipos
  ↓
Si necesito relaciones complejas → BloodHound
```

---

## Objetivo del bloque

Debes poder explicar la diferencia entre:

- host Windows;
- controlador de dominio;
- cuenta local;
- cuenta de dominio;
- SMB;
- LDAP;
- grupo;
- privilegio;
- relación de AD.

Si entiendes eso, NetExec deja de parecer una lista de flags y se convierte en una interfaz coherente para consultar el entorno.
