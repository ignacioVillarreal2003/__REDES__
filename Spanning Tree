# Spanning Tree

## Contexto de redundancia

En redes, la redundancia significa tener más de un camino posible entre dos puntos. Eso se hace para que, si un enlace o un switch falla, la red siga funcionando por otro camino.

Eso es muy útil porque aumenta la disponibilidad, pero trae un problema importante: en una red Ethernet, si hay varios caminos activos al mismo tiempo, los switches pueden empezar a reenviar tramas una y otra vez formando bucles. Ahí es donde aparece Spanning Tree.

La idea central es esta: **la redundancia es buena para no quedarnos sin conexión, pero los bucles son malos porque desordenan el tráfico de la red**. Entonces se necesita un mecanismo que permita tener enlaces redundantes, pero dejando algunos caminos bloqueados para evitar ciclos.

## Qué es Spanning Tree

Un **spanning tree** es una topología lógica sin bucles que se construye sobre una red física que sí puede tener redundancia.

Dicho de forma simple: la red puede estar conectada por muchos enlaces, pero Spanning Tree elige una sola estructura de caminos activos para que entre dos puntos haya un recorrido principal sin ciclos.

La palabra “tree” hace referencia a un árbol, porque un árbol no tiene bucles. Y “spanning” significa que cubre toda la red. O sea: es una forma de conectar todos los switches sin que existan caminos circulares.

En una red con Spanning Tree, no necesariamente todos los enlaces quedan apagados. Algunos quedan en estado de reenvío y otros se bloquean, pero siguen ahí como respaldo por si falla un enlace activo.

---

# Problemas

## Inestabilidad de la tabla MAC

La tabla MAC de un switch guarda qué dirección MAC está detrás de qué puerto. Eso permite reenviar tramas de forma eficiente.

El problema aparece cuando hay un bucle. Si una trama puede circular por más de un camino, el switch puede recibir la misma dirección MAC por puertos distintos en momentos diferentes. Entonces actualiza la tabla una y otra vez, asociando esa MAC a puertos distintos.

Eso se llama **MAC flapping** o inestabilidad de la tabla MAC.

### Qué provoca

* El switch “se confunde” sobre dónde está un dispositivo.
* Puede reenviar tráfico por el puerto incorrecto.
* La red se vuelve errática.
* Aumenta mucho el tráfico innecesario.

En una red sin bucles, una MAC corresponde normalmente a un puerto estable. En una red con bucles, esa relación deja de ser confiable.

## Tormentas de broadcast

Una trama broadcast es una trama que se envía a todos los dispositivos de la red, por ejemplo una consulta ARP.

En una red con bucles, una trama broadcast puede multiplicarse porque cada switch la reenvía por todos sus puertos menos por donde la recibió. Si hay varios caminos entre switches, esa misma trama puede volver a entrar por otro lado y seguir circulando.

Eso genera una **tormenta de broadcast**.

### Por qué es grave

* Satura los enlaces.
* Consume capacidad de los switches.
* Hace que la red quede lenta o inutilizable.
* Puede afectar incluso a dispositivos que no tenían nada que ver con el problema inicial.

Como las tramas broadcast no tienen un destino único, si existen bucles pueden propagarse sin control.

## Tramas duplicadas

Cuando una trama viaja por más de un camino en paralelo, puede llegar varias veces al mismo destino.

Eso produce **tramas duplicadas**.

### Consecuencias

* El receptor puede procesar la misma información más de una vez.
* Se desperdicia ancho de banda.
* Pueden aparecer errores en aplicaciones que no esperan duplicados.
* La red pierde eficiencia y previsibilidad.

Este problema no solo afecta al broadcast, también puede pasar con tramas unicast si hay bucles y rutas múltiples activas sin control.

---

# Qué es STP

**STP** significa **Spanning Tree Protocol**.

Es un protocolo de capa 2 que se usa en switches para evitar bucles en redes Ethernet con redundancia.

Su función principal es detectar la topología de la red y decidir qué enlaces deben quedar activos y cuáles deben bloquearse, para que la red funcione como un árbol lógico sin ciclos.

STP no elimina físicamente los enlaces redundantes. Lo que hace es **bloquear lógicamente algunos puertos** para que no formen parte del camino activo normal. Si un enlace principal falla, STP puede recalcular la topología y activar un camino alternativo.

En resumen: **STP permite tener redundancia sin que la red se destruya por bucles**.

---

# Problemas que resuelve

STP existe para resolver los tres problemas principales que aparecen cuando hay lazos en la capa 2:

## 1. Evita tormentas de broadcast

Al bloquear enlaces redundantes, evita que una trama broadcast se replique sin control y circule indefinidamente.

## 2. Evita la inestabilidad de la tabla MAC

Como elimina los bucles, cada dirección MAC tiende a ser aprendida por un puerto estable. Eso evita que el switch cambie constantemente la asociación MAC-puerto.

## 3. Evita tramas duplicadas

Al asegurarse de que haya un único camino lógico entre switches, evita que la misma trama llegue varias veces por rutas distintas.

## Idea práctica de STP

Si tenés tres switches conectados formando un triángulo, físicamente tenés redundancia, pero lógicamente eso sería peligroso porque la trama podría dar vueltas.

STP va a elegir uno de los enlaces para bloquearlo. Así queda una topología sin ciclos.

Si se cae un enlace activo, STP puede desbloquear uno de los enlaces que estaba en espera y mantener conectada la red.

---

# Versiones de STP

Con el tiempo aparecieron distintas versiones de Spanning Tree para mejorar velocidad, eficiencia y compatibilidad.

## STP clásico

Es el estándar original, basado en IEEE 802.1D.

### Características

* Evita bucles en la red.
* Tiene convergencia relativamente lenta.
* Cuando hay un cambio en la topología, tarda en recalcular y estabilizarse.

### En qué casos se usaba

Fue la base histórica de Spanning Tree en redes Ethernet.

## RSTP

Significa **Rapid Spanning Tree Protocol**. Es la evolución más conocida de STP y corresponde a IEEE 802.1w.

### Qué mejora

* Converge mucho más rápido que STP clásico.
* Reacciona mejor ante fallas.
* Reduce el tiempo en que un enlace pasa a estar disponible.

### Idea principal

Hace lo mismo que STP, pero de forma más rápida y eficiente.

## MSTP

Significa **Multiple Spanning Tree Protocol**, estándar IEEE 802.1s.

### Qué aporta

Permite asociar múltiples VLAN a diferentes instancias de spanning tree.

### Para qué sirve

En redes grandes con muchas VLAN, evita tener una sola lógica de bloqueo para todo. Así se puede distribuir mejor el tráfico y aprovechar mejor los enlaces redundantes.

## PVST y variantes

En algunos entornos, especialmente Cisco, aparecieron variantes como **PVST** y **PVST+**.

### Qué hacen

Permiten tener una instancia de Spanning Tree por VLAN.

### Ventaja

Mejor control y mejor uso de caminos en redes con varias VLAN.

### Desventaja

Más consumo de recursos y mayor complejidad.

---

# Cómo entenderlo fácil

Pensalo así:

* La red física puede tener varios caminos.
* Eso es bueno porque da respaldo.
* Pero si dejás todos los caminos activos en capa 2, aparecen bucles.
* Los bucles generan:

  * tormentas de broadcast,
  * duplicación de tramas,
  * inestabilidad en la tabla MAC.
* STP resuelve eso eligiendo un camino lógico principal y bloqueando los demás.
* Si algo falla, vuelve a reorganizar la red.

---

# STP – Conceptos Avanzados

## Root Switch (Root Bridge)

El **root switch** (o root bridge) es el switch principal de toda la red en STP. Es el punto de referencia a partir del cual se calcula toda la topología.

* Solo hay **un root bridge** por red STP.
* Todos los demás switches calculan su mejor camino hacia él.
* Desde el root se “organiza” la red en forma de árbol.

En términos simples: **todo gira alrededor del root switch**.

---

## Root Port

El **root port** es el puerto que tiene cada switch (que no es root) para llegar al root bridge por el camino más eficiente.

Características:

* Cada switch no root tiene **un solo root port**.
* Es el puerto con **menor costo hacia el root**.
* Siempre está en estado de forwarding (activo).

---

## Puertos Designados

Un **puerto designado** es el puerto que se encarga de enviar tráfico hacia un segmento de red específico.

Características:

* Hay **un puerto designado por segmento**.
* Es el puerto que tiene el **mejor camino hacia el root** dentro de ese segmento.
* Está en estado forwarding.

---

## Estados de los puertos

Los puertos en STP no pasan directamente a estar activos, sino que atraviesan distintos estados para evitar problemas.

### Blocking

* El puerto está bloqueado.
* No envía ni recibe tráfico de datos.
* Solo escucha BPDUs.
* Sirve para evitar bucles.

### Listening

* Empieza a participar en STP.
* No reenvía tráfico todavía.
* Evalúa la topología.

### Learning

* Empieza a aprender direcciones MAC.
* Aún no reenvía tráfico.
* Se prepara para activarse.

### Forwarding

* Estado activo.
* Envía y recibe tráfico normalmente.
* Aprende direcciones MAC.

### Disabled

* El puerto está apagado o administrativamente deshabilitado.
* No participa en STP.

---

## Temporizadores

STP usa temporizadores para controlar cómo se propaga la información y cuánto tardan los cambios.

### Hello

* Intervalo entre BPDUs enviados por el root.
* Valor típico: 2 segundos.

### Max Age

* Tiempo que un switch espera antes de considerar inválida la información STP.
* Valor típico: 20 segundos.

### Forward Delay

* Tiempo que un puerto pasa en estados Listening y Learning.
* Valor típico: 15 segundos por estado.

---

## BID (Bridge ID)

El **Bridge ID (BID)** es el identificador único de cada switch en STP.

Se usa para elegir el root bridge.

### Composición

```
BID = Prioridad + System ID (VLAN) + Dirección MAC
```

* **Prioridad**: valor configurable (más bajo = mejor).
* **System ID (VLAN)**: identifica la VLAN.
* **MAC**: dirección única del switch.

El switch con el **BID más bajo** se convierte en root.

---

## BPDU (Bridge Protocol Data Unit)

Las **BPDU** son mensajes que los switches intercambian para ejecutar STP.

Sirven para:

* Elegir el root bridge.
* Compartir información de costos.
* Detectar cambios en la topología.

Sin BPDUs, STP no podría funcionar.

---

# Operación de STP en la Topología

STP sigue una serie de pasos para organizar la red.

## 1. Selección del Switch Root

Todos los switches envían BPDUs anunciando su BID.

* El switch con el **BID más bajo** gana.
* Ese switch se convierte en el root bridge.

---

## 2. Selección de Puertos Raíz en Switches No Root

Cada switch calcula el mejor camino hacia el root.

* Se evalúa el **costo de la ruta**.
* El puerto con menor costo se convierte en **root port**.

---

## 3. Selección de Puertos Designados en Cada Segmento

Para cada segmento de red:

* Se elige el switch con mejor camino al root.
* Su puerto en ese segmento será el **designado**.
* Ese puerto queda en forwarding.

---

## 4. Puertos Redundantes en Blocking

Los puertos que no son:

* root port
* ni designados

quedan en estado **blocking**.

Estos puertos:

* No transmiten tráfico.
* Evitan bucles.
* Quedan como respaldo.

---

# Selección del Root Bridge

## Regla principal

Se elige el switch con el **BID más bajo**.

### Recordatorio

```
BID = Prioridad + MAC (+ VLAN)
```

### Cómo se decide

1. Menor prioridad gana.
2. Si hay empate, gana la MAC más baja.

---

## Costo de la Ruta Raíz (Root Path Cost)

El **root path cost** es el costo total desde un switch hasta el root.

### Cómo se calcula

* Cada enlace tiene un costo (según velocidad).
* Se suman los costos de todos los enlaces hasta el root.

Ejemplo conceptual:

* Enlace rápido → menor costo
* Enlace lento → mayor costo

### Para qué sirve

* Determina el **mejor camino hacia el root**.
* El puerto con menor costo se elige como **root port**.

---

## Idea clave para entender todo junto

STP hace lo siguiente:

1. Elige un switch central (root).
2. Cada switch encuentra su mejor camino hacia él.
3. En cada segmento se elige un puerto que “mande”.
4. Los demás puertos redundantes se bloquean.

Resultado:

* Red sin bucles.
* Red con redundancia (pero controlada).
* Caminos alternativos listos para activarse.

---

# RSTP (Rapid Spanning Tree Protocol)

RSTP es una evolución de STP (estándar IEEE 802.1w) diseñada para **acelerar la convergencia de la red**. Es decir, permite que los cambios en la topología (por ejemplo, caída de un enlace) se resuelvan mucho más rápido que en STP clásico.

La idea principal de RSTP es la misma que STP: evitar bucles en capa 2. Pero cambia **cómo lo hace**, optimizando los tiempos y simplificando procesos.

---

# Proceso de sincronización

RSTP introduce un mecanismo mucho más rápido basado en **propuestas y acuerdos** entre switches.

## Secuencia básica

### 1. Inicialización de puertos

Cuando un puerto se habilita:

* Se convierte en **designated port**.
* Comienza en estado **discarding** (equivalente a blocking, pero más general).

---

### 2. Cada switch se cree root inicialmente

Al arrancar:

* Cada switch se considera a sí mismo como root bridge.
* Empieza a enviar BPDUs anunciándose como root.

Estas BPDUs incluyen un **bit de proposal (propuesta)** activado.

---

### 3. Recepción de una BPDU superior

Cuando un switch recibe una BPDU con mejor información (mejor root):

* Acepta al otro switch como root o como mejor camino al root.
* El puerto por donde recibió esa BPDU se convierte en **root port**.

---

### 4. Respuesta con Agreement

El switch responde con una BPDU con el **bit de agreement (acuerdo)** activado.

Esto significa:

* “Acepto tu propuesta”
* “Sincronizamos la topología”

---

### 5. Transición rápida a forwarding

Una vez que hay acuerdo entre switches:

* Ambos puertos pasan **directamente a forwarding**.
* Se evita el paso lento por listening y learning típico de STP.

---

## Idea clave

RSTP reemplaza la espera basada en temporizadores por un **mecanismo de negociación directa entre switches**.

Eso es lo que lo hace mucho más rápido.

---

# Tipos de enlaces en RSTP

RSTP distingue tipos de enlaces para optimizar su comportamiento.

## Point-to-Point

* Conexión directa entre dos switches.
* Permite el uso del mecanismo rápido de proposal/agreement.
* Es el caso ideal para RSTP.

## Edge

* Puerto conectado a un dispositivo final (PC, servidor, etc.).
* No hay riesgo de bucles.
* Pasa **directamente a forwarding**.

Equivale al concepto de **PortFast** en STP.

## Point-to-Point Edge

Es una combinación conceptual:

* Puerto que es rápido como edge, pero en un entorno con switches.
* Se usa cuando se detecta que el enlace permite transición rápida.

---

# Comparación RSTP vs STP

## Similitudes

RSTP mantiene la lógica base de STP:

* Eligen el **root bridge** con las mismas reglas (BID más bajo).
* Los switches seleccionan su **root port** igual (menor costo).
* Los **puertos designados** se eligen con los mismos criterios.

Es decir: **la lógica de elección es igual**, cambia la velocidad y el mecanismo.

---

## Estados de los puertos

### STP

* Disabled
* Blocking
* Listening
* Learning
* Forwarding

### RSTP

* Discarding
* Learning
* Forwarding

### Diferencia clave

RSTP simplifica estados:

* **Discarding** reemplaza blocking + listening.
* Reduce pasos intermedios.
* Permite transiciones más rápidas.

---

## Roles de los puertos

### STP

* Root port
* Designated port
* Blocked port

### RSTP

* Root port
* Designated port
* Alternate port
* Backup port

### Nuevos roles en RSTP

#### Alternate port

* Camino alternativo hacia el root.
* Está en discarding.
* Puede activarse rápidamente si falla el root port.

#### Backup port

* Redundancia dentro del mismo segmento.
* También en discarding.
* Menos común que alternate.

---

## Diferencia clave de funcionamiento

### STP

* Depende de **temporizadores**.
* Cambios lentos (pueden tardar 30–50 segundos).
* Pasa por varios estados antes de activar un puerto.

### RSTP

* Usa **negociación (proposal/agreement)**.
* Cambios casi inmediatos.
* Menos dependencia de temporizadores.

---

# Idea final para entender RSTP

* STP evita bucles, pero es lento.
* RSTP hace lo mismo, pero **mucho más rápido**.
* La clave está en:

  * negociación directa entre switches,
  * menos estados,
  * nuevos roles de puertos,
  * detección inteligente del tipo de enlace.

---

# Verificación en switches Cisco con STP y RSTP

Para entender bien STP/RSTP en Cisco, conviene imaginar una red simple y ver qué pasa con los comandos reales. La idea es no aprender los comandos de memoria solamente, sino entender **qué te está mostrando cada uno** y **para qué sirve**.

---

## Ejemplo real de topología

Supongamos tres switches Cisco:

* `SW1`
* `SW2`
* `SW3`

Están conectados en triángulo:

* `SW1` conectado con `SW2`
* `SW2` conectado con `SW3`
* `SW3` conectado con `SW1`

Además, todos manejan la VLAN 1, que normalmente existe por defecto.

### Qué problema habría sin STP

Esa forma de triángulo crea redundancia, pero también un bucle.
Entonces STP/RSTP va a hacer esto:

* elegir un switch como root bridge,
* elegir un root port en los otros switches,
* elegir un puerto designado por segmento,
* bloquear el enlace sobrante.

---

# 1. Ver interfaces en estado up

## Comando

```bash
show ip int brief | in up
```

o también:

```bash
show ip interface brief | include up
```

## Qué hace

Muestra un resumen de las interfaces IP y filtra solo las que están activas.

### Qué significa “up”

En Cisco, una interfaz puede tener dos estados importantes:

* **Status**: si la interfaz física está conectada o no.
* **Protocol**: si la capa 2/capa 3 de esa interfaz está funcionando.

Cuando ves `up/up`, significa que la interfaz está operativa.

## Para qué sirve en STP

Antes de estudiar spanning tree, primero conviene verificar que los enlaces físicos realmente estén activos.
Si un puerto no está `up`, STP ni siquiera lo va a considerar como un camino utilizable.

---

# 2. Ver la configuración de spanning tree

## Comando

```bash
show run | in span
```

o más claro:

```bash
show running-config | include spanning-tree
```

## Qué hace

Muestra las líneas de configuración relacionadas con spanning tree.

Podés ver cosas como:

* `spanning-tree mode rapid-pvst`
* `spanning-tree vlan 1 root primary`
* `spanning-tree portfast`

## Para qué sirve

Te ayuda a confirmar si el switch está usando:

* STP clásico,
* RSTP,
* PVST+,
* Rapid-PVST,
* configuraciones de prioridad,
* portfast en puertos de acceso.

---

# 3. Ver VLANs

## Comando

```bash
show vlan brief
```

## Qué hace

Muestra las VLAN configuradas, sus nombres y los puertos asignados.

## Por qué importa

STP trabaja por VLAN en muchos modos de Cisco, especialmente en PVST y Rapid-PVST.
Entonces, antes de mirar spanning tree, conviene saber:

* qué VLAN existe,
* qué puertos están en esa VLAN,
* si el puerto pertenece a una VLAN de usuario o de infraestructura.

---

# 4. Ver el estado general de spanning tree

## Comando

```bash
show spanning-tree
```

## Qué muestra

Este comando es uno de los más importantes. Muestra:

* cuál es el root bridge,
* qué puertos están activos,
* qué roles tienen,
* qué estados tienen,
* qué costo tiene cada camino,
* información por VLAN, si el switch trabaja por VLAN.

## Qué vas a ver normalmente

En una salida típica aparecen campos como:

* `This bridge is the root`
* `Root ID`
* `Bridge ID`
* `Port`
* `Role`
* `State`
* `Cost`

---

# 5. Entender root y bridge con `show spanning-tree`

Supongamos que ejecutás:

```bash
show spanning-tree
```

Y ves algo parecido a esto:

```bash
VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     0011.2233.4455
             Cost        4
             Port        1 (GigabitEthernet0/1)

  Bridge ID  Priority    32769
             Address     00AA.BBCC.DDEE
```

## Cómo interpretarlo

### Root ID

Es la información del switch raíz de esa VLAN.

### Bridge ID

Es la información del switch que estás consultando.

### Priority

Mientras más baja, mejor para ser root.

### Address

Si dos switches tienen la misma prioridad, gana la MAC más baja.

### Cost

Es el costo total del camino hacia el root.

### Port

Indica por cuál puerto llega este switch al root, es decir, su root port.

---

# 6. Ver spanning tree por VLAN

## Comando

```bash
show spanning-tree vlan 1
```

Si la VLAN fuera 10:

```bash
show spanning-tree vlan 10
```

## Qué hace

Muestra toda la información STP de una VLAN específica:

* root bridge de esa VLAN,
* bridge actual,
* roles de puertos,
* estados de puertos,
* costos,
* timers.

## Cuándo usarlo

Cuando querés analizar una VLAN puntual y no toda la tabla general.

---

# 7. Ver resumen de una VLAN

## Comando

```bash
show spanning-tree vlan 1 summary
```

## Qué hace

Da una vista resumida del estado de STP para esa VLAN.

Es útil cuando querés algo más rápido que el detalle completo.

---

# 8. Ver detalle de una VLAN

## Comando

```bash
show spanning-tree vlan 1 detail
```

## Qué hace

Muestra información más profunda sobre la VLAN, por ejemplo:

* cuándo cambió la topología,
* qué puerto generó el cambio,
* detalles de la convergencia,
* eventos recientes de STP.

## Para qué sirve

Es muy útil cuando hubo una falla o una reconvergencia y querés saber qué pasó exactamente.

---

# 9. Ejemplo completo de interpretación

Supongamos que tenés estos tres switches:

* `SW1` con prioridad normal
* `SW2` con prioridad normal
* `SW3` con prioridad normal

Si todos tienen la misma prioridad, el root se define por la **MAC más baja**.

Ahora imaginá que querés forzar que `SW1` sea root.

En `SW1` configurás:

```bash
conf t
spanning-tree vlan 1 root primary
end
```

## Qué hace esto

Cisco ajusta la prioridad automáticamente para que ese switch tenga una prioridad menor que los demás y así gane la elección de root.

No tenés que calcular todo manualmente; Cisco lo hace por vos.

---

# 10. Cambiar prioridad manualmente

## Comando base

```bash
conf t
spanning-tree vlan 1 priority 4096
end
```

## Qué hace

Le asigna una prioridad baja al switch.

## Regla importante

* Cuanto **más baja** la prioridad, **más probabilidad** de ser root bridge.
* La prioridad debe ser múltiplo de 4096 en muchos entornos Cisco.

## Ejemplo práctico

Si querés que `SW1` sea root y `SW2` y `SW3` queden como secundarios, podés hacer:

En `SW1`:

```bash
conf t
spanning-tree vlan 1 priority 4096
end
```

En los otros dos:

```bash
conf t
spanning-tree vlan 1 priority 32768
end
```

Con eso, `SW1` debería ganar.

---

# 11. Ver opciones con `?`

## Comando

```bash
spanning-tree vlan 1 ?
```

## Qué hace

Te muestra las opciones disponibles para esa línea.

Esto es útil para aprender sin memorizar todo.

Por ejemplo, puede mostrar opciones como:

* `root`
* `priority`
* otras variantes según el IOS

---

## Comando

```bash
spanning-tree vlan 1 root ?
```

## Qué hace

Te muestra las opciones para configurar el root de forma automática.

Normalmente aparece algo como:

* `primary`
* `secondary`

### `root primary`

Hace que ese switch se convierta en root principal.

### `root secondary`

Hace que ese switch quede como respaldo, por si el root principal falla.

---

# 12. Ver el modo STP del switch

## Comando

```bash
spanning-tree mode ?
```

## Qué hace

Te muestra los modos disponibles.

Normalmente vas a ver algo como:

* `pvst`
* `rapid-pvst`
* `mst`

## Comando recomendado en muchos laboratorios Cisco

```bash
spanning-tree mode rapid-pvst
```

## Qué hace

Activa Rapid-PVST, que es la versión rápida de STP por VLAN.

---

# 13. PortFast para dispositivos finales

## Comando

```bash
spanning-tree portfast
```

## Qué hace

Hace que un puerto de acceso vaya mucho más rápido a forwarding.

## Cuándo usarlo

En puertos conectados a dispositivos finales como:

* PCs,
* impresoras,
* servidores,
* cámaras,
* teléfonos IP.

## Cuándo no usarlo

No deberías activarlo en enlaces entre switches, porque ahí sí puede haber bucles.

---

# 14. Ejemplo real de laboratorio paso a paso

## Escenario

Tenés tres switches conectados en triángulo:

* `SW1` puerto G0/1 hacia `SW2`
* `SW2` puerto G0/1 hacia `SW3`
* `SW3` puerto G0/1 hacia `SW1`

Y querés que `SW1` sea root para la VLAN 1.

---

## Paso 1: verificar interfaces activas

En cada switch:

```bash
show ip int brief | in up
```

Así confirmás que los enlaces están arriba.

---

## Paso 2: ver qué modo STP está activo

```bash
show spanning-tree
```

Si ves `rstp` o `rapid-pvst`, ya sabés en qué modo está trabajando.

---

## Paso 3: revisar la configuración

```bash
show run | in span
```

Buscás si ya existe algo como:

```bash
spanning-tree mode rapid-pvst
```

o configuraciones de prioridad.

---

## Paso 4: elegir el root

En `SW1`:

```bash
conf t
spanning-tree vlan 1 root primary
end
```

## Qué pasa con eso

`SW1` pasa a tener la mejor prioridad para esa VLAN y se convierte en root bridge.

---

## Paso 5: verificar el resultado

En `SW1`:

```bash
show spanning-tree vlan 1
```

Deberías ver algo como:

```bash
This bridge is the root
```

En `SW2` y `SW3` verás que el `Root ID` corresponde a `SW1`.

---

## Paso 6: ver los roles de puerto

En `SW2` y `SW3`, el comando:

```bash
show spanning-tree vlan 1
```

te va a mostrar algo como:

* un puerto en `Root Port`
* otro en `Designated Port`
* el puerto redundante en `Blocking` o `Discarding`, según sea STP o RSTP

### Qué significa

* **Root Port**: el mejor camino hacia el root bridge.
* **Designated Port**: el puerto que “representa” al segmento.
* **Blocking/Discarding**: el puerto que se deja apagado para evitar el bucle.

---

# 15. Cómo leer una salida típica

Podés ver algo así:

```bash
Interface           Role Sts Cost      Prio.Nbr Type
-------------------  ---- --- --------- -------- -------------
Gi0/1               Root FWD  4         128.1    P2p
Gi0/2               Altn BLK  4         128.2    P2p
```

## Interpretación

* `Gi0/1` es el root port y está en forwarding.
* `Gi0/2` es un puerto alternativo y está bloqueado.
* `P2p` indica que es un enlace point-to-point.

---

# 16. Qué significa point-to-point y edge en la práctica

## Point-to-point

Es un enlace entre dos switches.

* Normalmente participa en STP/RSTP.
* Puede usar el mecanismo rápido de proposal/agreement en RSTP.

## Edge

Es un puerto hacia un dispositivo final.

* No debería formar bucles.
* Puede activarse rápido con portfast.

### Ejemplo

* Puerto hacia una PC: edge
* Puerto hacia otro switch: point-to-point

---

# 17. Cómo conectar todo lo aprendido

Cuando administrás switches Cisco con STP, normalmente seguís esta lógica:

1. Verificás que los enlaces estén arriba.
2. Revisás el modo STP.
3. Identificás la VLAN.
4. Observás quién es el root bridge.
5. Comprobás root ports y designated ports.
6. Confirmás qué puerto quedó bloqueado.
7. Si necesitás, ajustás prioridad para elegir el root correcto.
8. En puertos hacia usuarios, activás portfast.

---
