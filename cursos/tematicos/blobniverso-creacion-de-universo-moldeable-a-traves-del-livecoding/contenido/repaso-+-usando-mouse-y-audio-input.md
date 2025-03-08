# Repaso + usando mouse y audio input

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vRPENsKd9tb8Ds58ZxGAFedWtV7jjvwtKerjUYMLEwUEaQC-NZMyMDAcXwUZQYc20purImbFLzFRn5K/pub?start=false&loop=false&delayms=3000" %}

## Script: Blobs

```glsl
float sdEsfera(vec3 p, vec3 offset, float radio){
   return length(p-offset)-radio;
}

float sdfCanvas(vec3 p){
    float td = length(p-vec3(100.))-0.01;
    
    for(int i=0; i < 10; ++i){
      for(int j=0; j<10; ++j){
          vec4 s = texture2D(channel0, vec2(float(i)/10.,float(j)/10.)); 
          float x = mix(-1.,1.,float(i)/10.);
          float y = mix(-1.,1.,float(j)/10.);
          if (s.r>0.){
              float d = sdEsfera(p,vec3(x,y,0.),0.15);
               td  = smin(d,td,0.2);
          }
         
          
      }
    }
    return td;
}


float map(vec3 p){
    float s1 = sdEsfera(p, vec3(1.,0.,0.),.8);
    //return s1;            // 1. Retornamos 1 esfera
    
    float s2 = sdEsfera(p, vec3(-1.,0.,0.),.8);
    //return smin(s1,s2,1.); // 2. Retornamos 2 esferas "pegadas" 


    return sdfCanvas(p);    // 3. Hacemos el mismo proceso pero 
                            // sobre las cosas dibujadas en el canvas
}  

void main () {
    vec2 pos = uv(); vec3 color;
    vec3 rd = normalize(vec3(pos, mix(0.5,2., 1.))); 
    vec3 ro = vec3(0.,0.,-5.);
    
    

    float marcha; 
    float n;
    
    for (int i=0; i<64; i++)
    {
        vec3 p =ro+marcha*rd;
        float d = map(p); 
        
        if (d < 0.001 || marcha > 50.) {
            break;
        } 
        marcha +=d;
        n += 0.1/(1.4+abs(d));
    }
    
 
    color +=n*0.2;
    
    gl_FragColor = vec4(color, 1.0);
}
```

#### NOTAS

* Las transformaciones _(rotaciones)_ dependen si se aplican en la función **map o** en las funciones **sdEsfera, sdfCanvas.**
*   Vimos ejemplo usando la función **mix()** para controlar las rotaciones

    ```
     float a1,a2; // angulos de rotacion

      a1 = mix(PI, 0.,bpm(10.,1.,1.));
      a1 = mix(-PI,a1, bpm(20.,1.,1.)); 
        
      a2 = mix(PI, 0.,bpm(15.,1.,1.));
      a2 = mix(-PI,a2, bpm(10.,1.,1.)); 
        
      pR(p.xz,a1);
      pR(p.yz,a2); 
    ```

## Inputs de The\_Force:

* **mouse.x/resolution.x** :  es un valor de 0 a 1 que controlamos con la coordenada x del mosue
* **mouse.y/resolution.y :**&#x65;s un valor de 0 a 1 que controlamos con la coordenada y del mosue
* Para la audioreactividad habilitar el micrófono y usar:\
  **bands.x, bands.y, bands.z, bands.w**\
  que respresentan las frequencias  **bajas, medias-bajas, medias-altas y altas**, respectivamente, obtenidas tras aplicar la **Transformada Rápida de Fourier (FFT)** al sonido de entrada.

