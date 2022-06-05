---
description: Motivación y escritura del primer shader.
---

# CLASE 28/04

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vRh2vFfAYBz9lQwG5SFxfKJimI2Xh2Bar_P6oRaMafZgalWAVosuBquuz8wxQkgs9kCzYEzfGyyMXAY/pub?delayms=3000&loop=false&start=false" %}

### PARTE PRÁCTICA

Restringimos a las funciones: cos(), step(), smoothstep(), length(), y mix().

En los siguientes links se encuentra el editor con checkpoints:

{% embed url="https://clases-shaders.solsarratea.world/clases/fund0/editor" %}
Sintaxis e introducción a ejes cartesianos
{% endembed %}

{% embed url="https://clases-shaders.solsarratea.world/clases/fund1/editor" %}
Dibujo de figura simple
{% endembed %}

### **NOTAS**

* GPU permite hace muchas operaciones en simultáneo. Esto implica que scribir programas que corran en la GPU permite que tengan mejor performance, y mayor complejidad.

{% embed url="https://www.youtube.com/watch?v=-P28LKWTzrI" %}
GPU vs CPU
{% endembed %}

{% embed url="https://glitch.com/~shader-performance" %}
App para testear el rendimiento de CPU vs GPU en frames
{% endembed %}

*   **FRAGMENT SHADER:** entra posición sale color.

    * Declaración de **variables uniformes:** variables que contienen información "externa" que le programadore le puede pasr. Ejemplo: tiempo.
    * void () **main** {..}: cuerpo principal de ejecución.
    *   **gl\_**{...}: son variables provistas para interacturar (leer y escribir información) en distintos momentos de la pipeline gráfica. Ejemplos:

        **gl\_FragCoord:** permite leer las coordenadas del píxel.

    **gl\_FragColor:** permite escribir el color que se va a renderear.
