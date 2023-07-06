# Figuras

Extraidas de:

{% embed url="https://iquilezles.org/articles/distfunctions2d" %}

* Círculo:

```
float sdCircle( vec2 p, float r ) { 
    /* Devuelve la distancia al circulo de radio r */
    return length(p) - r; 
}
```

* Cuadrado:

```
float sdBox( in vec2 p, in vec2 b )
{  /* Devuelve la distancia al cuadrado de lados b */

    vec2 d = abs(p)-b;
    return length(max(d,0.0)) + min(max(d.x,d.y),0.0);
}
```

Ejemplo de uso:\
\
`color += smoothstep(0.01,0.0,sdBox(pos, vec2(.3)));`

* Triángulo equilátero:

```
(https://www.shadertoy.com/view/Xl2yDW)
float sdEquilateralTriangle( in vec2 p )
{
    const float k = sqrt(3.0);
    p.x = abs(p.x) - 1.0;
    p.y = p.y + 1.0/k;
    if( p.x+k*p.y>0.0 ) p = vec2(p.x-k*p.y,-k*p.x-p.y)/2.0;
    p.x -= clamp( p.x, -2.0, 0.0 );
    return -length(p)*sign(p.y);
}
```
