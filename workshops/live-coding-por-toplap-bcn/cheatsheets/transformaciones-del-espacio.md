---
description: Helpers para jugar con transformaciones en el espacio
---

# Transformaciones del espacio

```
const float PI = 3.141592;

vec2 tunnel(vec2 p, float size, float time)
{
    float a = atan(p.y, p.x);
    float r = sqrt(dot(p, p));
    return vec2(a / PI, time + (size / r));
}

float distancia_p(vec2 pos, float p){
    float dp= pow(abs(pos.x),p)+pow(abs(pos.y),p);
    return pow(dp,1./p);
}


vec2 tunnelP(vec2 pos, float size, float time,float p)
{
    float a = atan(pos.y, pos.x);
    float r = distancia_p(pos,p); 
    return vec2(a / PI, time + (size / r));
}

vec2 paredes(vec2 pos, float amp, float time){
    pos.y = amp/abs(pos.y);
    pos.x *= pos.y;
    pos = fract(pos+time);	
    return pos; 
}
```

```
float reflejo(inout vec2 pos, float angle){
    vec2 normal = vec2(cos(angle),sin(angle));
    float d = dot(pos, normal);
    pos -= normal*min(0.,d)*2.;
    return smoothstep(0.03,0.,abs(d));
}

vec2 rotacion (vec2 pos, float cantidad){
      return  pos * mat2(cos(cantidad),sin(cantidad),-sin(cantidad),cos(cantidad));
}

```

{% embed url="https://fritzm.github.io/moebius.html" %}
