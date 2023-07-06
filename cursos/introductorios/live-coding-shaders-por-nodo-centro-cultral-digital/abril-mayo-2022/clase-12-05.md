# CLASE 12/05

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vQKzVKgVYEEOUVxbvkXGG6C0P2DPWU43nQvlioY_jl9jjQoCM5B-gYbfWg3gDkggVjpYmrsmaH7vD8v/pub?delayms=3000&loop=false&start=false" %}
Repaso y transformaciones
{% endembed %}

{% embed url="https://docs.google.com/presentation/d/e/2PACX-1vSrajKqeS2KiIR1IO26pXCBtJ3EB4Y-KlqwAGfk670xg-vi9dL9uCLNPpNWgW1u8jbRkqGjPErg1Neb/pub?delayms=3000&loop=false&start=false" %}

**PRÁCTICA**

En el siguiente link se encuentra el editor con checkpoints:

{% embed url="https://clases-shaders.solsarratea.world/clases/trnsf/editor" %}

**NOTAS**

**BONUS - Fin de Curso**

![](<../../../../.gitbook/assets/May-08-2022 00-25-27.gif>)

```
// Author: Sol Sarratea @solquemal
//Title: BONUS - Raymarching
precision mediump float;
uniform float u_time;
uniform vec2 u_resolution;

vec2 uv(){
  /* Devuelve las posiciones del canvas en rango [-1.,1.]x[-1.,1.] */
  vec2 pos = gl_FragCoord.xy/u_resolution; 
  pos.x *= u_resolution.y/u_resolution.x;
  pos = pos *2.-1.;
  return pos;
}

void main() {
    vec3 pos = vec3(uv(),1.);
   
    vec3 cam = vec3(0.,3., -9.);
    cam.z +=u_time;
    vec3 rayo = normalize(pos); 
    
    float marcha = 0.;
    for(int i=0; i < 32; ++i){
        vec3 p = cam + rayo * marcha;
        float d = length(sin(p)+cos(p.yzx))-.5;
        marcha += d;
        if (marcha > 50. || d < 0.01) break; 
     }
    
    vec3 color;
    color += pow(0.908,marcha);
	
    gl_FragColor = vec4(color, 1.);

}


```

