# Kodelife



{% embed url="https://hexler.net/kodelife" %}

## Helpers



```
uniform sampler2D midi1;
ivec2 midiCoord(int offset)
{
    int x = offset % 32;
    int y = offset / 32;
    return ivec2(x,y);
}

float setMidi(int ccNumber){
  vec4 val = texture(midi1, vec2((1./32.) * midiCoord(3 * 127 + ccNumber)));
  return val.r;
}

#define PI 3.1415926538
float bpm(float bpm, float q, float intensity){
    // Adaptacion de funcion creada por Char Stiles
    // La idea es generar una funcion que sirva como especie de metronomo.
    // Devuelve un valor entre 0 y 1
    float bps = 60./bpm; // beats por segundo
    float bpmVis = tan((time*PI/q)/bps);
     return max(min(abs(bpmVis),1.),0.) * intensity; 
}
float bpmd(float b){
    // Discrete bpm
 return step(0.5,bpm(b,1.,1.)); 
}
float luma(vec3 color) { return dot(color, vec3(0.299, 0.587, 0.114)); }

float rand(vec2 n) {
    return fract(sin(dot(n, vec2(12.9898,12.1414))) * 83758.5453);
}
```
