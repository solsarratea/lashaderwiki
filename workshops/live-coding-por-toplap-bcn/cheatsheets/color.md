# Color

```
float hue2rgb(float f1, float f2, float hue) {
  // http://www.chilliant.com/rgb2hsv.html

  if (hue < 0.0)
    hue += 1.0;
  else if (hue > 1.0)
    hue -= 1.0;
  float res;
  if ((6.0 * hue) < 1.0)
    res = f1 + (f2 - f1) * 6.0 * hue;
  else if ((2.0 * hue) < 1.0)
    res = f2;
  else if ((3.0 * hue) < 2.0)
    res = f1 + (f2 - f1) * ((2.0 / 3.0) - hue) * 6.0;
  else
    res = f1;
  return res;
}

// Color Space Conversion
vec3 hsl2rgb(vec3 hsl) {
  vec3 rgb;

  if (hsl.y == 0.0) {
    rgb = vec3(hsl.z); // Luminance
  } else {
    float f2;

    if (hsl.z < 0.5)
      f2 = hsl.z * (1.0 + hsl.y);
    else
      f2 = hsl.z + hsl.y - hsl.y * hsl.z;

    float f1 = 2.0 * hsl.z - f2;

    rgb.r = hue2rgb(f1, f2, hsl.x + (1.0 / 3.0));
    rgb.g = hue2rgb(f1, f2, hsl.x);
    rgb.b = hue2rgb(f1, f2, hsl.x - (1.0 / 3.0));
  }
  return rgb;
}

vec3 hsl2rgb(float h, float s, float l) { return hsl2rgb(vec3(h, s, l)); }

// INTERNAL
vec3 RGBtoHCV(vec3 rgb) {
  // Based on work by Sam Hocevar and Emil Persson
  vec4 p = (rgb.g < rgb.b) ? vec4(rgb.bg, -1.0, 2.0 / 3.0)
                           : vec4(rgb.gb, 0.0, -1.0 / 3.0);
  vec4 q = (rgb.r < p.x) ? vec4(p.xyw, rgb.r) : vec4(rgb.r, p.yzx);
  float c = q.x - min(q.w, q.y);
  float h = abs((q.w - q.y) / (6.0 * c + 1e-10) + q.z);
  return vec3(h, c, q.x);
}

vec3 rgb2hsl(vec3 rgb) {
  vec3 hcv = RGBtoHCV(rgb);
  float l = hcv.z - hcv.y * 0.5;
  float s = hcv.y / (1.0 - abs(l * 2.0 - 1.0) + 1e-10);
  return vec3(hcv.x, s, l);
}

vec3 rgb2hsl(float r, float g, float b) { return rgb2hsl(vec3(r, g, b)); }

float luma(vec3 color) { return dot(color, vec3(0.299, 0.587, 0.114)); }

vec3 hueShift( vec3 color, float hueAdjust ){
    const vec3  kRGBToYPrime = vec3 (0.299, 0.587, 0.114);
    const vec3  kRGBToI      = vec3 (0.596, -0.275, -0.321);
    const vec3  kRGBToQ      = vec3 (0.212, -0.523, 0.311);
    const vec3  kYIQToR     = vec3 (1.0, 0.956, 0.621);
    const vec3  kYIQToG     = vec3 (1.0, -0.272, -0.647);
    const vec3  kYIQToB     = vec3 (1.0, -1.107, 1.704);
    float   YPrime  = dot (color, kRGBToYPrime);
    float   I       = dot (color, kRGBToI);
    float   Q       = dot (color, kRGBToQ);
    float   hue     = atan (Q, I);
    float   chroma  = sqrt (I * I + Q * Q);
    hue += hueAdjust;
    Q = chroma * sin (hue);
    I = chroma * cos (hue);
    vec3    yIQ   = vec3 (YPrime, I, Q);
    return vec3( dot (yIQ, kYIQToR), dot (yIQ, kYIQToG), dot (yIQ, kYIQToB) );
}

vec3 saturate(vec3 cin, float amount ){
    const vec3 W = vec3(0.2125, 0.7154, 0.0721);
    vec3 intensity = vec3(dot(cin.rgb, W));
    return mix(intensity, cin.rgb, amount);
}
```
