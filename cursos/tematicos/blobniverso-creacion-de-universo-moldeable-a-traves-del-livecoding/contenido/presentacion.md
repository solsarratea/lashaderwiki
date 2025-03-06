# Presentación

Slides\



{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vQssacrlbaMyUMp7vTDZ4IMln7wHRosizPzf__1n5QbCa4vSEzrtkyf06wOPp6OeihCvt-LF8fEQaTT/pub?delayms=3000&loop=false&start=false" %}

### Notas

GPU permite hace muchas operaciones en simultáneo. Esto implica que escribir programas que corran en la GPU permite que tengan mejor performance, y mayor complejidad.

{% embed url="https://shader-performance.glitch.me/" %}
LoadPixels vs shader
{% endembed %}

{% embed url="https://www.youtube.com/watch?v=-P28LKWTzrI" %}
GPU vs CPU
{% endembed %}

##

## Presentación de The\_Force

Es un editor web de GLSL, que compila el código a medida que lo escribamos. A continuación las secciones que empiecen con la palabra **Script:** son para que puedan copiar y pegar el código en el editor online.

Vamos a usar una versión local para ello conectarse a: \
\- red: Myfi\
\- pass: makelovenotwar

<figure><img src="../../../../.gitbook/assets/Screenshot from 2025-02-23 19-09-50 (1).png" alt=""><figcaption><p>Versión Remix de The Force</p></figcaption></figure>

## Script: sintaxis básica de un shader

```glsl
void main() {
    /* Declaracion de variables
       tipo nombre = valor; */

    // Numeros reales
    float numero_0 = 1.0;
    int numero_1 = 1;

    // Vectores 
    // 2 dimensiones [_ ,_ ]
    vec2 v2_0 = vec2(0.,1.);
    vec2 v2_1 = vec2(numero_0,numero_1);

    //3 dimensiones [_ ,_ ,_ ]
    vec3 v3_0 = vec3(0.,0.5,1.);
    vec3 v3_1 = vec3(v2_0,1.);
    vec3 v3_2 = vec3(v2_0,numero_0);

    //4 dimensiones [_ ,_ ,_ ,_ ]
    vec4 v4_0 = vec4(0.,0.25,0.5,1.);
    vec4 v4_1 = vec4(v2_0,v2_1);
    vec4 v4_2 = vec4(v3_0,0.);

    // Acceso a los vectores
    // [r, g, b, a] || [x , y , z , w]

    float numero_2 = v4_0.r; // es equivalente a v4_0.x
    vec2 v2_2 = v4_0.rg;     // es equivalente a v4_0.xy
    vec3 v3_3 = v4_0.gbr;    // es equivalente a v4_0.yzx

    gl_FragColor = vec4(0.5,0.5,1.,1.); // [ (r, g ,b , a) ] con valores entre 0. y 1.

}
```
