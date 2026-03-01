# biblioteca-cnc
Biblioteca de anotaciones CNC Fanuc, desbaste, rampas, radios y trigonometría aplicada.

📘 BIBLIOTECA CNC – TORNO FANUC
Guía práctica de desbaste, rampas, radios y trigonometría aplicada

PARTE 1 — FUNDAMENTOS DEL TORNO CNC
1. Cómo piensa un torno CNC
Sistema de ejes
En torno:
X = Diámetro (NO radio)
Z = Longitud
Lógica correcta del eje Z
Z0 → Cara frontal de la pieza
Z negativo (-) → Hacia las mordazas (dentro del material)
Z positivo (+) → Hacia el operador (fuera del material)
Visualización:
Z+   (afuera)
Z0   ← Cara frontal
Z-1
Z-2
Z-3
Z-4
Z-5
Z-6  ← Más cerca de las mordazas

Corrección clave
❌ El desbaste no es “de atrás hacia adelante”.
✔ El desbaste longitudinal va de Z0 hacia valores negativos.

2. Movimiento en X vs profundidad real
Como X trabaja en diámetro:
1 mm en X = 0.5 mm reales
2 mm en X = 1 mm real
4 mm en X = 2 mm reales
Ejemplo
De Ø40 a Ø28:
40 − 28 = 12 mm en diámetro
12 ÷ 2 = 6 mm de profundidad real

PARTE 2 — GEOMETRÍA DE MOVIMIENTO
3. Rampas (líneas inclinadas)
Ejemplo:
Inicio: X28 Z-5
Fin: X12 Z-2
Diferencias:
16 mm en X (diámetro)
3 mm en Z
Eso es una línea recta inclinada.
Se programa así:
G01 X12 Z-2

El control interpola automáticamente.

4. Desbaste inclinado manual
Si no usas G71:
Divides X y Z proporcionalmente
Avanzas parcialmente en ambos ejes
⚠ No es un radio.
Es aproximación controlada.
Sirve para:
Reducir carga
Evitar vibración
Preparar acabado

5. Desbaste escalonado
Ejemplo Ø40 a Ø28:
X38
X36
X34
...
X28

O combinado con Z:
G01 X38
G01 Z-12
G01 X36
G01 Z-11

Eso genera escalones.
⚠ Escalones ≠ radio real.

PARTE 3 — RADIOS Y ARCOS
6. Radios reales
Un radio:
No se divide
No se hace por escalones
Es una trayectoria circular exacta
Ejemplo R5:
G18
G01 X40 Z-13
G03 X30 Z-8 R5

Reglas obligatorias
Plano correcto → G18
Punto inicial exacto
Punto final exacto
Sentido correcto → G02 o G03
Si el punto inicial está mal → el radio sale mal.
Si el sentido está mal → invade material.

7. G02 vs G03 en torno
En torno influyen:
Plano activo
Sentido de avance
X en diámetro
Si la geometría es correcta pero el arco no coincide, prueba el sentido contrario.
Eso es diagnóstico técnico.

8. Radio del plano vs radio de herramienta
Radio del plano (R5, R6)
≠
Radio de punta de plaquita
Plaquitas típicas:
0.8 → desbaste
0.4 → acabado
0.2 → fino
Se configura en compensación de herramienta.

PARTE 4 — TRIGONOMETRÍA APLICADA A CONOS
9. Cálculo con ángulos
Caso:
Cateto vertical = 7
Ángulo superior = 30°
Buscar base (Z)
Fórmula correcta:
tan(30°) = base / altura
Entonces:
base = altura × tan(30°)
tan(30°) = 0.577
Z = 7 × 0.577
Z ≈ 4.04 mm

Regla mental útil
Si el ángulo está arriba → multiplicas.
Si el ángulo está abajo junto a la base → divides.

10. Orden correcto para cálculos de cono
Restar diámetros
Dividir entre 2
Dividir ángulo total entre 2
Calcular tangente
Multiplicar

PARTE 5 — CICLOS AUTOMÁTICOS
11. G71 (Desbaste)
Primera línea U:
→ Controla cuánto baja por pasada
→ Define número de pasadas
Segunda línea U:
→ No baja
→ Deja material para acabado
Ejemplo:
Objetivo Ø40
Segunda línea U0.5
El G71 deja Ø41
Luego G70 baja de 41 a 40.

RESUMEN TÉCNICO FINAL
X es diámetro
2 mm en X = 1 mm real
Z0 está en la cara frontal
Z negativo va hacia mordazas
Desbaste longitudinal va hacia negativo
G01 X Z = rampa
Escalones ≠ radio
Radios solo con G02/G03
Punto inicial correcto es obligatorio
G18 es plano del torno
Sentido del arco determina el resultado
G71 primera U = cuánto quito
G71 segunda U = cuánto dejo

Si tienes una cota final en Z-50 y programas:
G71 P.. Q.. U0.5 W1
Entonces:
El desbaste no llegará a Z-50


Se quedará en Z-49 (porque W1 deja 1 mm antes en Z)


Luego, cuando ejecutas:
G70 P.. Q..
El ciclo de acabado sí sigue exactamente el perfil original (el que está entre P y Q) y llega a Z-50 real.

Punto importante
W siempre actúa en dirección Z


No importa si es Z negativo o positivo


Simplemente deja material antes del perfil



Ejemplo claro
Perfil final:
Z-50
Con:
W0.2
Desbaste termina en:
Z-49.8
Acabado (G70) termina en:
Z-50.0 exacto
Así se evita que el desbaste deje marcas profundas en la medida final.
—---
En G71, el perfil no es Z2.
El perfil es únicamente lo que está entre los bloques P y Q.
En tu programa:
G71 P40 Q85 ...
El perfil empieza en N40 y termina en N85.

Entonces ¿qué es Z2?
G00 X76 Z2
Eso es solo posición de seguridad antes de empezar el ciclo.
 No hace parte del perfil.

¿Dónde inicia realmente el perfil?
Aquí:
N40 G00 X0
N45 G01 Z0
El perfil comienza en Z0, que es la cara de la pieza.
 No en Z2.

Regla clara
Z2 → posición de aproximación


Z0 → inicio real de la pieza


Perfil → bloques entre P y Q
