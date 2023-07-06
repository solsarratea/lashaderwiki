# Transformaciones del espacio

```
float reflejo(inout vec2 pos, float angle){
    vec2 normal = vec2(cos(angle),sin(angle));
    float d = dot(pos, normal);
    pos -= normal*min(0.,d)*2.;
    return smoothstep(0.03,0.,abs(d));
}
```
