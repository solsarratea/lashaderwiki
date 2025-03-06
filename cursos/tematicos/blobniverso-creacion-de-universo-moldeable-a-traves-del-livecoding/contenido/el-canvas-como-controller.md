---
description: >-
  Si dibujamos con colores tenemos más información que nos permite ser más
  orgánicos en el Blobuniberse
---

# El Canvas como controller

<figure><img src="../../../../.gitbook/assets/Screenshot from 2025-03-06 19-22-01.png" alt=""><figcaption></figcaption></figure>

## Script: Demo de esferas con radios variables (color rojo), y ubicación en el eje z (color azul)

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
          if (s.r>0. || s.g >0.){
              float z = mix(.5,-.5,s.b);
              float d = sdEsfera(p,vec3(x,y,z),0.15*s.r);
               td  = smin(d,td,0.4);
          }
         
          
      }
    }
    return td;
}


float map(vec3 p){

  return sdfCanvas(p);
}


vec3 normal(vec3 p) {
  const float e = 0.001;
  return normalize(vec3(
    map(p + vec3(e,   0.0, 0.0))- map(p + vec3(-e,  0.0, 0.0)),
    map(p + vec3(0.0,   e, 0.0)) - map(p + vec3(0.0,  -e, 0.0)),
    map(p + vec3(0.0, 0.0,   e)) - map(p + vec3(0.0, 0.0,  -e))
  ));
}


vec3 luz(vec3 posLuz,  vec3 posActual, vec3 n, vec3 base, vec3 camDir){
    vec3 l = normalize(posLuz - posActual);
    float dif = clamp(dot(l, n),0.,1.);
    vec3 colorDif =base*dif*dif;
      
    vec3 lReflejada =normalize(reflect(-l,n));
 
    float specular = pow(clamp(dot(lReflejada, camDir),0.,1.),500.2);
    specular = min(specular,dif);
    vec3 colorSpec = teal*noise(posActual*3.)*specular;
    
    
    vec3 colorAmb = pink * 0.0001; // Si desean agregar un color ambiente

    vec3 color = colorDif + colorAmb + colorSpec;
    return color;
}

vec3 iluminacion(vec3 posActual, vec3 camaraPos){
    vec3 fuenteDeLuz = vec3(2.,.7,-3.6); //Posicion de luz 1
    vec3 color = vec3(0.8,0.7,0.); //Color de luz 1
    vec3 n = normal(posActual);
    vec3 camDir = normalize(camaraPos - posActual);
       
    vec3 colorFinal = luz(fuenteDeLuz, posActual, n, color, camDir);
    
    
    vec3 fuenteDeLuz2 = vec3(-2.,2.7,-3.6);// Posicion de luz 2
    vec3 color2 = vec3(0.8,0.,0.);   // Color de luz 2
    colorFinal += luz(fuenteDeLuz2, posActual, n, color2, camDir);;
    return colorFinal;
}



void main () {
    vec2 pos = uv(); vec3 color;
    vec3 rd = normalize(vec3(pos, 1.6)); 
    vec3 ro = vec3(0.,0.,-3.);
    
    

    float marcha; 
    float n;
    vec3 p;
    float hit = 0.;
    for (int i=0; i<64; i++)
    {
        p =ro+marcha*rd;
        float d = map(p); 
        
        if (d < 0.001){ 
            hit= 1.;
            break;
        } 
        if(marcha>50.){
            break;
        }
        marcha +=d;
        n += 0.1/(.9+abs(d));
    }
    
    color +=iluminacion(p,ro)*hit;

    
    gl_FragColor = vec4(color, 1.0);
}
```

### Bonus: usar el color verde para algun atributo que les gustaria que cambie!



