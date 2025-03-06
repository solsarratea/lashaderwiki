# Intro a GLSL

## Objetivo

* Familiarizarse con la escritura .
* Entender conceptualmente que un **fragment shader** ejecuta las mismas instrucciones en paralelo para **cada pixel** que tienen una **posicion** y renderiza un **color.**
* Entender por qué los  sistemas de referencia son importa

## Slides

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vRow4ohCvBzvlr1J_qQMQaoEP_R55iZ--e_eY8GKPg0w-FbwRsE4EsDcrWNm5ivKLyKDt-x2fG4HucI/pub?start=false&loop=false&delayms=3000" %}

**NOTAS**

**FRAGMENT SHADER:** entra posición sale color.

* Declaración de **variables uniformes:** variables que contienen información "externa" que le programadore le puede pasr. Ejemplo: tiempo.
* void () **main** {..}: cuerpo principal de ejecución.
*   **gl\_**{...}: son variables provistas para interacturar (leer y escribir información) en distintos momentos de la pipeline gráfica. Ejemplos:

    **gl\_FragCoord:** permite leer las coordenadas del píxel.

**gl\_FragColor:** permite escribir el color que se va a renderear.

## Introducimos ejes de coordenadas y funciones básicas

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vQ99JopXV48k_2kdREOpFVWIZHCN-oL808900AbbauCOPPKUPca3B-fYnZiOkcFTdIWxDBep3jDnkCc/pub?start=false&loop=false&delayms=3000" %}

## Script: Sistemas de referencia

```glsl

float verEjes(vec2 pos){
    float ejes; 
    ejes += 1.-step(0.009, distance(pos.x,0.));
    ejes += 1.-step(0.009, distance(pos.y,0.));
    return ejes;
}

void main() {
    vec2 pos; vec3 color;
    pos = uv();
  	color.g = verEjes(pos);
    //Visualizamos los ejes
    color.rb = pos;  //[( r,b )] == [( x,y )]
        
    // Mapeamos valores para entender el rango
    //[( r,b )] == [( 1,0 )] full rojo 
    //[( r,b )] == [( 0,1 )] full azul
    //[( r,b )] == [( 1.,1. )] full rojo y azul
    //[( r,b )] == [( 0.,0. )]  sin rojo, sin azul 
    //Identificamos el rango de la variable x y de la variable y
    
    ////////////////////////////////////////////
    //Cuento la historia de Descartes y **amamos los sistemas de referencia**
    
    //Posicionamos el centro de la pantalla en el medio
    //color.rb = uvN();  //[( r,b )] == [( x,y )]
    
    gl_FragColor = vec4(color,1.); 

}
```

## Script: tiempo + funciones trigonométricas

```glsl
float verEjes(vec2 pos){
    float ejes; 
    ejes += 1.-step(0.009, distance(pos.x,0.));
    ejes += 1.-step(0.009, distance(pos.y,0.));
    return ejes;
}

void main() {
  vec2 pos = uv(); vec3 color;
  
//Parte 1. El tiempo al asignar color
  
// float freq =10.;
//color.r = cos(pos.x*freq+time);
  
// freq =20.;
//color.b = cos(pos.x*freq+time);
//color.g = cos(length(pos)*2.);
  
  
//Parte 2. El tiempo al asignar poscion
//pos.x +=cos(time);
//pos.y += sin(time);

//color += verEjes(pos);
// color.r += 1.-length(pos);
 
 

  gl_FragColor = vec4(color, 1.); // [ (r, g ,b , a) ]
  
    
}
```

## Script: figuras simples

```
void main() {
  vec2 pos = uv(); vec3 color;
  float scale=1.;

  // Introducimos a las SDF : Funciones de distancias de signo
  //--------------------------------
  // Describimos figuras a partir de las posiciones de los pixeles
  float rombo    = smoothstep(0.3,0.31,distance(pos.x,0.)+distance(pos.y*0.5,0.))-scale*0.5;
  float cuadrado = smoothstep(0.3,0.31,max(abs(pos.x*.5),abs(pos.y*.5)))-scale*.5;
  float circulo  = smoothstep(0.3,0.31,length(pos));

  color.r = rombo;
  color.b = cuadrado;
  color.g = circulo;

  //Introducimos a las SDF : Funciones de distancias de signo
  //--------------------------------
  // https://iquilezles.org/www/articles/distfunctions2d/distfunctions2d.htm



  gl_FragColor = vec4(color, 1.); // [ (r, g ,b , a) ]

}
```

**NOTAS**

* Cada thread “obtiene” información de la posición  mediante  `gl_FragCoord`\
  ¿Cuál es el valor máximo que puede tomar? ¿Y el mínimo?
*   ¿Cómo describimos la posición del píxel? ¿Es única?

    * Sistemas de representación del espacio:\


    <table><thead><tr><th width="132">Nombre</th><th width="92">Output</th><th>Rango</th><th>Origen de coordenas</th></tr></thead><tbody><tr><td>uvN</td><td>(x,y)</td><td>[0,1]x[0,1]</td><td>esquina inferior izquierda</td></tr><tr><td>uv</td><td>(x,y)</td><td>[-1,1]x[-1,1]</td><td>centro de la pantalla</td></tr></tbody></table>
