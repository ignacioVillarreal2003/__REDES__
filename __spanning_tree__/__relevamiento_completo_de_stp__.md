# Relevamiento completo de STP por VLAN

---

# 1. Objetivo del análisis

Tenés que lograr esto:

1. Saber **cómo está funcionando STP hoy**.
2. Identificar:

   * root bridge por VLAN
   * caminos activos
   * enlaces bloqueados
3. Detectar problemas:

   * roots incorrectos
   * loops potenciales
   * mala distribución de tráfico
4. Dibujar un **mapa lógico de la red (por VLAN)**.
5. Dejar todo listo para:

   * definir nuevos cores como root
   * evitar que al conectarlos se rompa todo

---

# 2. Paso 0 — Preparación (clave)

Antes de empezar:

* Elegí **1 VLAN para empezar** (ej: VLAN 1 o la más usada).
* Después repetís el proceso para todas.

---

# 3. Paso 1 — Identificar el root bridge por VLAN

En varios switches (core, distribución, acceso):

```bash
show spanning-tree vlan 1
```

## Qué tenés que anotar

Para cada switch:

* Root ID (quién es el root)
* Bridge ID (quién soy yo)
* Root Port
* Costo al root

## Objetivo

Confirmar:

* ¿Todos ven el mismo root?
* ¿Ese root es lógico?

### Problema típico

* Un switch de acceso es root → MAL

---

# 4. Paso 2 — Recolectar información de TODOS los switches

Tenés que entrar a cada switch y ejecutar:

```bash
show spanning-tree vlan 1
```

Y guardar:

## Por cada switch

* Nombre del switch
* Bridge ID
* Root ID
* Root port
* Puertos designados
* Puertos bloqueados

---

# 5. Paso 3 — Analizar los puertos

En cada salida mirá esta tabla:

```bash
Interface    Role    State
```

## Interpretación

* Root Port → camino hacia el root
* Designated → puerto activo en el segmento
* Blocking/Discarding → enlace redundante

---

# 6. Paso 4 — Construir el mapa lógico

Ahora viene lo importante.

Con toda la info, armás un mapa así (aunque sea en papel):

## Ejemplo conceptual

```
        SW-CORE
        (ROOT)
        /   \
     SW2     SW3
      |       |
    SW4     SW5
```

Y marcás:

* qué enlace está activo
* cuál está bloqueado

---

## Cómo deducir conexiones

Si en SW2 ves:

```bash
Gi0/1 Root Port
```

→ ese puerto apunta hacia el root.

Si en SW3 ves:

```bash
Gi0/2 Blocking
```

→ ese enlace es redundante.

---

# 7. Paso 5 — Verificar estabilidad

En cada switch:

```bash
show spanning-tree vlan 1 detail
```

## Qué buscar

* topology change count
* last change

## Problema

* Si cambia constantemente → hay loops o inestabilidad

---

# 8. Paso 6 — Detectar problemas reales

Ahora analizás el mapa.

## Problemas típicos

### 1. Root mal ubicado

* Root está en acceso
* Root no está en core

### 2. Caminos ineficientes

* Tráfico pasa por rutas largas
* No usa enlaces principales

### 3. Bloqueos mal distribuidos

* Se bloquea el enlace equivocado
* Se desaprovecha ancho de banda

### 4. Demasiados cambios STP

* Red inestable

---

# 9. Paso 7 — Repetir para TODAS las VLAN

Esto es clave.

Si usás PVST o Rapid-PVST:

* Cada VLAN puede tener **topología distinta**

Entonces repetís:

```bash
show spanning-tree vlan X
```

Para:

* VLAN 1
* VLAN 10
* VLAN 20
* etc.

---

# 10. Paso 8 — Hacer el mapa FINAL

Terminás con algo así:

## VLAN 1

* Root: SW-OLD-CORE1
* Root ports: X
* Bloqueos: Y

## VLAN 10

* Root: SW-OLD-CORE2
* Root ports: X
* Bloqueos: Y

Esto es tu **estado real de la red**.

---

# 11. Paso 9 — Definir el diseño nuevo

Antes de conectar nuevos cores, decidís:

## Nuevo diseño

* Core1 nuevo → root primary
* Core2 nuevo → root secondary

Ejemplo:

| VLAN | Root Primary | Root Secondary |
| ---- | ------------ | -------------- |
| 1    | Core1        | Core2          |
| 10   | Core1        | Core2          |

---

# 12. Paso 10 — Preparar la red actual

ANTES de conectar los nuevos cores:

## En cores actuales

Subís prioridad para que NO sean root:

```bash
conf t
spanning-tree vlan 1 priority 32768
end
```

---

## En nuevos cores (aislados)

```bash
conf t
spanning-tree mode rapid-pvst
spanning-tree vlan 1 root primary
end
```

En el secundario:

```bash
spanning-tree vlan 1 root secondary
```

---

# 13. Paso 11 — Verificación antes de integrar

En nuevos cores:

```bash
show spanning-tree vlan 1
```

Confirmar:

* prioridad baja
* listos para ser root

---

# 14. Paso 12 — Integración controlada

MUY IMPORTANTE:

1. Conectar un solo enlace
2. Verificar STP
3. Esperar estabilidad
4. Conectar siguiente enlace

Comando clave:

```bash
show spanning-tree vlan 1
```

---

# 15. Paso 13 — Validación final

Después de integrar:

## Verificar

* Root correcto
* Puertos correctos
* Sin loops
* Sin cambios constantes

---

# 16. Buenas prácticas clave

## 1. Documentar todo

Tu mapa es clave.

## 2. Nunca conectar sin analizar STP

## 3. Controlar root SIEMPRE

## 4. Usar Rapid-PVST

```bash
spanning-tree mode rapid-pvst
```

## 5. Portfast solo en acceso

---

# 17. Resumen claro

Tenés que:

1. Entrar a cada switch
2. Ejecutar:

   ```bash
   show spanning-tree vlan X
   ```
3. Anotar:

   * root
   * puertos
   * estados
4. Construir mapa lógico
5. Detectar problemas
6. Definir nuevo root (nuevos cores)
7. Preparar prioridades
8. Integrar de forma gradual
