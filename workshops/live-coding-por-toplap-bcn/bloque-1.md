# Bloque 1

## Nociones básicas de GLSL

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vRow4ohCvBzvlr1J_qQMQaoEP_R55iZ--e_eY8GKPg0w-FbwRsE4EsDcrWNm5ivKLyKDt-x2fG4HucI/pub?start=false&loop=false&delayms=3000" %}

**NOTAS**

**FRAGMENT SHADER:** entra posición sale color.

* Declaración de **variables uniformes:** variables que contienen información "externa" que le programadore le puede pasr. Ejemplo: tiempo.
* void () **main** {..}: cuerpo principal de ejecución.
*   **gl\_**{...}: son variables provistas para interacturar (leer y escribir información) en distintos momentos de la pipeline gráfica. Ejemplos:

    **gl\_FragCoord:** permite leer las coordenadas del píxel.

**gl\_FragColor:** permite escribir el color que se va a renderear.

## Introducimos ejes de coordenadas y funciones básicas

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vSgf1bWJN63BgLBv_EgWdxfd3nAbnXngsjXG7aw27cvmhXCoeGtggZUic5HU22A1VemQVOotYdYn_CE/pub?start=false&loop=false&delayms=3000" %}

**NOTAS**

* Cada thread “obtiene” información de la posición  mediante  `gl_FragCoord`\
  ¿Cuál es el valor máximo que puede tomar? ¿Y el mínimo?
*   ¿Cómo describimos la posición del píxel? ¿Es única?

    * Sistemas de representación del espacio:\


    <table><thead><tr><th width="112">Nombre</th><th width="92">Output</th><th>Rango</th><th>Origen de coordenas</th></tr></thead><tbody><tr><td>uvN</td><td>(x,y)</td><td>[0,1]x[0,1]</td><td>esquina inferior izquierda</td></tr><tr><td>uv</td><td>(x,y)</td><td>[-1,1]x[-1,1]</td><td>centro de la pantalla</td></tr></tbody></table>
*   Introducimos las funciones:

    &#x20;**cos(), step(), smoothstep(), length(), y mix()**
*   Transformaciones del espacio:

    &#x20;**traslación, rotación, escalar(zoom in-zoom out)**

**HELPERS (CHECKPOINT 1)**

```
vec2 uv(){
    /* Devuelve las posiciones del canvas en rango [-1.,1.]x[-1.,1.] */
    vec2 pos = gl_FragCoord.xy/resolution *2.- 1. ; 
  	pos.x *= resolution.x/resolution.y;
  	return pos;
}

vec2 uvN(){
    /* Devuelve las posiciones del canvas en rango [0.,1.]x[0.,1.] */
    vec2 pos = gl_FragCoord.xy/resolution; 
    return pos;
}

float verEjes(vec2 pos){
    float ejes; 
    ejes += 1.-step(0.009, distance(pos.x,0.));
    ejes += 1.-step(0.009, distance(pos.y,0.));
    return ejes;
}
```

**HELPERS (checkpoint 2)**

```
vec3 tablero(vec2 pos){
  return smoothstep(vec3(0.),vec3(0.001),sin(pos.xxx*20.)*sin(pos.yyy*20.));	
}

vec2 rotacion (vec2 pos, float cantidad){
  return  pos * mat2(cos(cantidad),sin(cantidad),-sin(cantidad),cos(cantidad));
}
```

