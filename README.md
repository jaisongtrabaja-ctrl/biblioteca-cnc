
📘 GUÍA ORGANIZADA – CICLO G71, SOBREMATERIAL Y CÁLCULO DE CONO
(Comparativo Control tipo Fanuc vs HNC)

1️⃣ IMPORTANCIA DEL MANUAL DE PROGRAMACIÓN
Cada torno CNC tiene su propio manual.
No importa si el control es tipo:
Fanuc
HNC
Haas
Mazak
Siempre existen pequeñas variaciones en:
Sintaxis de ciclos
Parámetros obligatorios
Códigos equivalentes
Forma de declarar avances
Conclusión técnica:
Aunque ya sepas programar CNC,
cuando llegues a una máquina nueva:
Pide el manual.
Revisa sintaxis de ciclos.
Verifica diferencias en G71, G70, G76, etc.
La base es la misma.
La sintaxis puede variar.

2️⃣ ELECCIÓN DEL MATERIAL EN BRUTO
Antes de programar, debes saber:
Diámetro final mayor
Longitud final
Material disponible comercialmente
Ejemplo:
La pieza requiere Ø70 mm.
En el comercio los redondos vienen en pulgadas:
2"
2½"
2¾"
3"
etc.
3 pulgadas = 76.2 mm
No comprarás Ø70 exacto.
Trabajarás con Ø76 mm (3”).
Entonces:
Material bruto = Ø76 mm
Ese dato es CRÍTICO para el G71.

3️⃣ ESTRUCTURA BÁSICA DEL PROGRAMA
Encabezado (Fanuc)
%
O0005
T0101
G40 G90 G21 G54
G99 F0.12
M03 S1500

Encabezado (HNC)
%
O0005
T0101
G40 G90 G21
G95 F0.12
M03 S1500

Diferencia clave:
Fanuc usa G99 → avance por revolución
HNC usa G95 → avance por revolución

4️⃣ POSICIÓN INICIAL ANTES DEL CICLO
La herramienta debe ubicarse:
En diámetro del material bruto
2 mm delante de la cara
Ejemplo:
G00 X76 Z2

Esto le dice al ciclo:
“Empieza a desbastar desde Ø76”
Si pones X70 por error,
la máquina intentará quitar 6 mm en una sola pasada.

5️⃣ CICLO G71 – DESBASTE AUTOMÁTICO
Fanuc (2 líneas)
G71 U1.0 R1.0
G71 P10 Q50 U0.5 W0.2 F0.12

Primera línea
U = profundidad radial por pasada
R = retracción
Si U1.0 → baja 1 mm radial
→ baja 2 mm en diámetro

Segunda línea
P = bloque inicio perfil
Q = bloque final perfil
U = sobrematerial en X
W = sobrematerial en Z
Aquí está la parte crítica.

6️⃣ SOBREMATERIAL (LO QUE TE ESTABA COSTANDO)
Hay DOS tipos de U:
🔹 U primera línea
Profundidad por pasada
🔹 U segunda línea
Sobrematerial en X
W = sobrematerial en Z

Regla profesional
El sobrematerial en X debe ser:
≥ radio de la herramienta
Si inserto = R0.4
Sobrematerial recomendado = 0.5 mm
Nunca menos que el radio.
En Z se deja menos:
0.2 mm es típico.
Ejemplo correcto:
G71 U1.0 R1.0
G71 P10 Q50 U0.5 W0.2

Luego:
G70 P10 Q50

El G70 elimina ese sobrematerial.

7️⃣ PERFIL ENTRE P Y Q
Entre P y Q no se programan pasadas.
Se programa la geometría FINAL de la pieza.
Ejemplo:
N10 G01 X54 Z-22
N20 X40 Z-26.04
N30 Z-42
N40 G02 X70 Z-50 R8
N50 Z-73
N60 X76

Eso es la forma final.
El G71 genera automáticamente todas las pasadas.

8️⃣ CÁLCULO GEOMÉTRICO DEL CONO
Problema:
El plano no da la longitud del cono.
Datos:
Diámetro mayor = 54
Diámetro menor = 40
Diferencia = 14 mm
Pero eso es diámetro.
Altura radial = 14 ÷ 2 = 7 mm
Ángulo dado = 60°
Triángulo rectángulo:
Un ángulo = 60°
Otro = 90°
El tercero = 30°
Usamos:
tan(30°) = opuesto / adyacente
tan(30°) = 7 / X
X = 7 / tan(30°)
Resultado:
X ≈ 4.04 mm
Entonces:
Longitud total hasta final del cono:
22 + 4.04 = 26.04 mm
Eso es lo que faltaba en el plano.

9️⃣ DIFERENCIA G71 FANUC vs HNC
Fanuc:
2 líneas
U y W para sobrematerial
HNC:
1 línea
Usa X y Z para sobrematerial
Ejemplo HNC:
G71 U1.0 R1.0 P10 Q50 X0.5 Z0.2

La lógica es la misma.
Solo cambia la sintaxis.

🔟 CAMBIO MENTAL CLAVE
Antes:
Programabas cada pasada.
Ahora:
Defines parámetros.
El control calcula todo.
Eso es programación por ciclo.

🧠 LO QUE DEBES DOMINAR
✔ X trabaja en diámetro
✔ U línea 1 = profundidad radial
✔ U línea 2 = sobrematerial en X
✔ W = sobrematerial en Z
✔ P y Q delimitan geometría
✔ G70 elimina sobrematerial
✔ Conos se calculan con trigonometría radial

11 :

Explicación directa y simple para **torno CNC**:

**Eje Z (longitud de la pieza)**

* `Z0` = la **cara frontal de la pieza**.
* Los valores **negativos en Z** (`Z-10`, `Z-40`, etc.) van **hacia adentro del material**.
* Por eso el desbaste normalmente se hace **desde Z0 hasta el valor negativo que indique el plano**.

Ejemplo:

```
Z0  → inicio en la cara de la pieza
Z-40 → final del desbaste hacia adentro
```

**Eje X (posición de la herramienta respecto al diámetro)**

Según lo que indicas:

* **Número X menor → la herramienta (buril) está más arriba, sin tocar o tocando menos el material.**
* **Número X mayor → la herramienta baja más y entra más en el material, por lo que corta más.**

Ejemplo:

```
X30 → buril más arriba
X40 → baja más
X50 → baja todavía más y corta más material
```

**Regla rápida**

* `Z0` → cara de la pieza.
* `Z negativo` → avance hacia dentro de la pieza.
* `X pequeño` → herramienta más arriba (menos contacto).
* `X grande` → herramienta baja más y corta más material.

**Ejemplo simple**

```
G00 X30 Z2
G01 Z0
G01 X45
G01 Z-40
```

Significado:

1. La herramienta se posiciona arriba de la pieza (`X30`).
2. Va a la cara (`Z0`).
3. Baja (`X45`) hasta tocar y cortar el material.
4. Desbasta hacia adentro hasta `Z-40`.
