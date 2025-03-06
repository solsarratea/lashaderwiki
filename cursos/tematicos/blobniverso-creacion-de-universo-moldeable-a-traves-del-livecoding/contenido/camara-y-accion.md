---
description: Recorriendo el espaico 3D
---

# Cámara y Acción

## Script: Transformaciones en el espacio

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
// Traslaciones 
//  p.x += cos(time); //En el eje x
//  p.y += sin(time); //En el eje y
//  p.z += cos(time); //En el eje z

// Rotaiones
//  pR(p.xy,time*0.1); // Rotacion con respecto al eje z
//  pR(p.zy,time*0.2); // Rotacion con respecto al eje x
//  pR(p.xz,time*0.5); // Rotacion con respecto al eje y
    
 //repeat(p,5.);
 //repeatLimit(p,5.,3.);
  return sdfCanvas(p);
}

void main () {
    vec2 pos = uv(); vec3 color;
    //Escalar 
    float fl = mouse.x/resolution.x;
    vec3 rd = normalize(vec3(pos, mix(0.5,2., fl))); 
    vec3 ro = vec3(0.,0.,-3.);
    
    

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
        n += 0.1/(.9+abs(d));
    }
    
 
    color +=n*0.2;
    
    gl_FragColor = vec4(color, 1.0);
}
```

### Notas

* **Zoom in/ Zoom Ou**t: cambiando el F.O.V (valor z de la dirección del rayo)

```glsl
float fl = 1.; // Probar: mouse.x/resolution.x
//Mayor valor, zoom in (los rayos mas paralelos)
//Menor valor. zoom out (los rayos estan mas "abiertos")
vec3 rd = normalize(vec3(pos, mix(0.5,2.,fl)); 
```

* **Rotaciones y traslaciones**: aplicamos operaciones antes de llamar la función que calcula nuestra escena
* Roaciones discretas usando la función **mix()**

```
 float a1,a2; // angulos de rotacion

  a1 = mix(PI, 0.,bpm(10.,1.,1.));
  a1 = mix(-PI,a1, bpm(20.,1.,1.)); 
    
  a2 = mix(PI, 0.,bpm(15.,1.,1.));
  a2 = mix(-PI,a2, bpm(10.,1.,1.)); 
    
  pR(p.xz,a1);
  pR(p.yz,a2); 
```

