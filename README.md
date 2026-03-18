
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
----

 **recordatorio claro y directo** para torno CNC:

## 🎯 CASO: tienes longitud (Z) y ángulo → quieres diámetro

## 📐 Fórmula base

\Delta D = 2 \cdot (\Delta Z \cdot \tan(\theta))

---

## ⚠️ REGLA CLAVE (torno)

👉 **θ = la mitad del ángulo del cono**

* Si el plano dice **60°**
* Tú usas: **30°**

---

## 🔧 PASOS (siempre igual)

1. Tomas la **longitud del cono (ΔZ)**
2. Tomas **la mitad del ángulo**
3. Calculas:

   * ΔR = ΔZ × tan(θ)
4. Multiplicas por 2:

   * ΔD = 2 × ΔR

---

## 🧠 FORMATO ULTRA SIMPLE

👉 **Diámetro = 2 × longitud × tan(ángulo/2)**

---

## 📌 EJEMPLO RÁPIDO

* Longitud = 10 mm
* Ángulo = 60°

[
\Delta D = 2 \cdot (10 \cdot \tan(30°)) \approx 11.54
]

---

## ⚠️ ERRORES QUE NO PUEDES COMETER

* ❌ Usar 60° directo
* ❌ Usar diámetro en la tangente
* ❌ Usar una longitud que no sea del cono

---

## 🔥 FRASE PARA MEMORIZAR

👉 **“Multiplico la longitud por la tangente de la mitad del ángulo y lo duplico.”**

---
Aquí tienes la **tabla tipo “chuleta de taller”** para sacar diámetros rápido sin pensar mucho:

---

## 📊 TABLA RÁPIDA – CONOS EN TORNO CNC

| Ángulo del cono | Usas (θ/2) | tan(θ/2) | Fórmula mental    | Resultado rápido |
| --------------- | ---------- | -------- | ----------------- | ---------------- |
| 30°             | 15°        | 0.268    | ΔD ≈ 2 × Z × 0.27 | ΔD ≈ 0.54 × Z    |
| 45°             | 22.5°      | 0.414    | ΔD ≈ 2 × Z × 0.41 | ΔD ≈ 0.82 × Z    |
| 60°             | 30°        | 0.577    | ΔD ≈ 2 × Z × 0.58 | ΔD ≈ 1.15 × Z    |
| 90°             | 45°        | 1.000    | ΔD = 2 × Z × 1    | ΔD = 2 × Z       |

---

## 🧠 Cómo usarla (modo taller)

Ejemplo rápido:

* Longitud del cono = 10 mm
* Ángulo = 60°

👉 Buscas 60°:

* ΔD ≈ **1.15 × 10 = 11.5 mm**

✔ Sin calculadora
✔ Suficientemente preciso para programar

---

## 🔥 Traducción mental rápida

* 30° → diámetro crece lento
* 45° → crecimiento medio
* 60° → crecimiento rápido
* 90° → es un chaflán fuerte (1:1 en radio)

---

## ⚠️ Recuerda siempre

* X trabaja en **diámetro**
* Pero la trigonometría usa **radio**
* Por eso siempre aparece el **×2**

---

Aquí tienes la **tabla inversa (la más usada en taller cuando ya tienes diámetros y quieres la longitud del cono)**:

---

## 📊 TABLA RÁPIDA – SACAR LONGITUD (Z)

| Ángulo del cono | Usas (θ/2) | tan(θ/2) | Fórmula mental | Resultado rápido |
| --------------- | ---------- | -------- | -------------- | ---------------- |
| 30°             | 15°        | 0.268    | Z ≈ ΔD ÷ 0.54  | Z ≈ 1.85 × ΔD    |
| 45°             | 22.5°      | 0.414    | Z ≈ ΔD ÷ 0.82  | Z ≈ 1.21 × ΔD    |
| 60°             | 30°        | 0.577    | Z ≈ ΔD ÷ 1.15  | Z ≈ 0.87 × ΔD    |
| 90°             | 45°        | 1.000    | Z = ΔD ÷ 2     | Z = 0.5 × ΔD     |

---

## 🎯 Fórmula base (por si la olvidas)

\Delta Z = \frac{\Delta D}{2 \cdot \tan(\theta)}}

---

## 🧠 Cómo usarla (ejemplo real tuyo)

* Ø54 → Ø40
  👉 ΔD = 14
* Ángulo = 60°

Buscas en tabla:

👉 Z ≈ **0.87 × 14 ≈ 12.18 mm** ❌

⚠️ Esto NO coincide con tu caso porque:

* Tu plano usa geometría parcial (no todo el cono completo)

👉 Por eso en tu ejercicio real daba **4.04 mm**

---

## 🔥 Regla de oro (muy importante)

👉 Esta tabla funciona cuando:

* El cono es completo entre esos dos diámetros

👉 No funciona cuando:

* Hay radios (R)
* Hay partes interrumpidas
* El cono no es directo

---

## 🧠 Frase para memorizar

👉 **“Para sacar Z: divido el diámetro entre (2 × tangente)”**

---

+---+

# 🔥 CUÁNDO USAR LA TABLA (y cuándo NO)

## ✅ SÍ usas la tabla (caso fácil)

Cuando el cono es **directo**, sin nada raro:

```
Ø grande  ───────────
            \
             \
              \
               Ø pequeño
```

👉 Solo hay:

* 2 diámetros
* 1 ángulo
* 1 longitud

✔ Aquí aplicas la tabla sin pensar
✔ Resultado directo

---

## ❌ NO usas la tabla (tu caso)

Cuando aparece algo como:

* Un **radio (R14)**
* Un arco
* Una transición curva

Ejemplo de tu plano:

```
Ø54 ──\
        \  ← parte recta (cono)
         ) ← R14 (curva)
        /
Ø40 ───
```

👉 Aquí el cono **NO es completo**

---

## ⚠️ Qué pasa si usas la tabla aquí

Te da mal:

* Te salió ~12 mm
* Pero el real era **4.04 mm**

👉 Porque:

* Parte del cambio lo hace el **radio R14**
* No todo es cono

---

## 🧠 ENTONCES ¿QUÉ HACES?

### 🔧 Método correcto (modo taller)

1. Tomas los diámetros:

   * 54 → 40
2. Calculas diferencia:

   * 14 → /2 = 7 (radio)
3. Usas el ángulo (30°)
4. Aplicas tangente SOLO para el tramo recto:

\tan(30^\circ)=\frac{7}{X}

👉 Eso te da solo la parte recta

---

## 🎯 IDEA CLAVE (grábatela)

👉 **Si hay radio (R), la tabla NO sirve directamente**

Porque:

* El radio ya “consume” parte del cambio de diámetro

---

## 🔥 REGLA DE TALLER (nivel pro)

* Cono limpio → usa tabla
* Cono + radio → usa trigonometría directa
* Cono incompleto → analiza la geometría

---

## 🧠 RESUMEN ULTRA SIMPLE

* ✔ Sin R → tabla
* ❌ Con R → cálculo manual
* ✔ Siempre en radio
* ✔ Siempre medio ángulo

---

Bien. Este es el **método rápido de taller (2–3 segundos)** para decidir qué hacer con cualquier plano:

---

# 🧠 MÉTODO VISUAL RÁPIDO

## 1️⃣ Mira la línea

### 🔹 Caso A: línea recta limpia

```
Ø grande
   \
    \
     \
      Ø pequeño
```

👉 ✔ **Es un cono puro**

✅ Usa tabla
✅ O fórmula directa

---

### 🔹 Caso B: aparece una curva (R)

```
Ø grande
   \
    )
   /
Ø pequeño
```

👉 ❌ **Ya NO es cono puro**

🚫 NO uses tabla
✅ Divide el problema

---

## 2️⃣ Regla del “R manda”

👉 Si ves algo como:

* R14
* R8
* R...

💥 **El radio manda la geometría**

---

## 3️⃣ Qué haces entonces

### 🔧 Separas mentalmente:

* Parte recta (cono)
* Parte curva (radio)

👉 Solo calculas el cono **sin meterte con el radio**

---

## 4️⃣ Prueba rápida (para no fallar)

Pregúntate:

👉 “¿Todo el cambio de diámetro es línea recta?”

* ✔ Sí → usa tabla
* ❌ No → hay radio → no uses tabla

---

## 5️⃣ Tu caso aplicado

Tienes:

* Ø54 → Ø40
* Pero hay **R14**

👉 Entonces:

❌ No todo es cono
✔ Parte es curva

👉 Por eso:

* La tabla te daba mal
* La tangente directa sí funcionó

---

## 🔥 FRASE CLAVE (memorízala)

👉 **“Si hay R, la tabla muere.”**

---

## 🧠 BONUS (nivel más alto)

Cuando veas esto en examen o taller:

* Si calculas y no coincide →
  👉 **te están escondiendo un radio o una parte curva**

---




