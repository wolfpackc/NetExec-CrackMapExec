# 04 — WinRM y acceso remoto

## 1. Qué es WinRM

WinRM es la implementación de Microsoft de WS-Management para administración remota.

Puertos habituales:

```text
5985/tcp  HTTP
5986/tcp  HTTPS
```

No pienses en WinRM como "una shell". WinRM es la infraestructura de administración remota; una shell es una de las cosas que puede obtenerse a través de ella.

---

## 2. Papel de NetExec

NetExec es útil para comprobar de forma rápida si un objetivo responde por WinRM y si unas credenciales autorizadas funcionan.

```bash
nxc winrm 192.168.56.20 -u labuser -p 'PASSWORD'
```

Modelo mental:

```text
nxc winrm
   |
   +-- ¿está el servicio disponible?
   +-- ¿funciona esta identidad?
   +-- ¿qué nivel de acceso parece tener?
```

---

## 3. NetExec vs Evil-WinRM

```text
NetExec
  └── validación y automatización

Evil-WinRM
  └── sesión interactiva orientada a pentesting/lab
```

Ejemplo conceptual:

```text
nxc confirma acceso
        ↓
quiero trabajar manualmente
        ↓
Evil-WinRM / cliente PowerShell Remoting
```

---

## 4. Qué debes entender antes de usarlo

- diferencia entre servicio y cliente;
- autenticación local vs dominio;
- permisos del usuario;
- WinRM habilitado vs bloqueado por firewall;
- HTTP 5985 no significa necesariamente tráfico de aplicación "web" normal;
- WinRM forma parte de la administración remota de Windows.

---

## 5. Laboratorio básico

```bash
nmap -p5985,5986 192.168.56.20
nxc winrm 192.168.56.20
nxc winrm 192.168.56.20 -u labuser -p 'PASSWORD'
```

Preguntas:

1. ¿está abierto 5985 o 5986?
2. ¿responde WinRM?
3. ¿las credenciales funcionan?
4. ¿necesitas NetExec o una shell interactiva?

---

## Regla

**NetExec te ayuda a responder "¿puedo?"; una herramienta interactiva te ayuda a responder "¿qué hago ahora dentro?".**
