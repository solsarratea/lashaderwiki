# Demo Doodle grupal



```
// Author: Sol Sarratea @solquemal
// Title: Shader jam

precision mediump float;

uniform float time;
uniform vec2 resolution;
uniform sampler2D doodle;
uniform sampler2D backbuffer;

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

void main() {
    vec2 pos; vec4 color;
    pos = uv();
 		
  	color = texture2D(doodle,uvN());
  
  	color= mix(vec4(0.,0.,0.,1.), color, step(0.9,color.a));
  
  	vec4 backbuffer = texture2D(backbuffer, uvN()*vec2(0.99+sin(pos.y*20.+time)*0.1,1.1+cos(pos.x*10.+time)*0.01));
    
        gl_FragColor = max(backbuffer*0.99,color); 

}
```
