# 06 — Módulos de NetExec

## 1. Qué es un módulo

Un módulo amplía el comportamiento base de un protocolo para realizar una tarea concreta.

Modelo mental:

```text
nxc smb TARGET
   |
   +-- funcionalidad base SMB
   |
   +-- módulo
         |
         +-- tarea especializada
```

No memorices módulos sin contexto. Primero entiende el protocolo al que pertenecen y qué información necesitan.

---

## 2. Cómo estudiar módulos

Para cada módulo responde:

1. ¿qué protocolo utiliza?
2. ¿requiere autenticación?
3. ¿requiere privilegios elevados?
4. ¿qué consulta o acción realiza?
5. ¿qué salida genera?
6. ¿qué herramienta manual permitiría comprobar lo mismo?

---

## 3. Consultar módulos disponibles

Las opciones cambian entre versiones. Por eso debes apoyarte en la ayuda de tu instalación:

```bash
nxc --help
nxc smb --help
```

Y en la documentación oficial de NetExec.

Cuando estudies un módulo concreto, anota:

```text
Nombre:
Protocolo:
Objetivo:
Requisitos:
Salida esperada:
Equivalente manual:
```

---

## 4. Analogía

```text
NetExec = navegador
Protocolo = carretera
Módulo = destino concreto
```

El módulo no elimina la necesidad de conocer la carretera. Si no entiendes SMB, un módulo SMB seguirá pareciendo magia.

---

## 5. Clasificación útil

Puedes clasificar módulos por intención:

```text
Enumeración
├── información del sistema
├── usuarios / grupos
├── configuración
└── recursos

Validación
├── credenciales
├── permisos
└── acceso

Post-autenticación de laboratorio
├── consultas administrativas
├── inventario
└── comprobaciones específicas
```

---

## 6. Buen hábito

Antes de ejecutar cualquier módulo en un laboratorio:

```text
¿Qué hace exactamente?
¿Qué permisos necesita?
¿Qué tráfico generará?
¿Modifica algo o solo consulta?
```

Ese hábito es más importante que memorizar 50 nombres de módulos.

---

## 7. Ficha de estudio

Copia esta plantilla para cada módulo que quieras dominar:

```markdown
### Nombre del módulo

- Protocolo:
- Finalidad:
- Requisitos:
- ¿Solo lectura?:
- Comando de ayuda:
- Ejemplo de laboratorio:
- Salida esperada:
- Qué significa la salida:
- Herramienta alternativa:
```

---

## Objetivo del bloque

Debes poder abrir la ayuda de NetExec, localizar módulos relevantes y entenderlos por **función y protocolo**, sin depender de una lista memorizada que puede quedar obsoleta.
