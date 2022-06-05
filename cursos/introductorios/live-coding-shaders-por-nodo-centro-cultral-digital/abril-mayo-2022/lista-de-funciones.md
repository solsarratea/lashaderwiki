# Lista de funciones

Lista completa de funciones:

* [https://www.shaderific.com/glsl-functions](https://www.shaderific.com/glsl-functions)
* [https://thebookofshaders.com/glossary/](https://thebookofshaders.com/glossary/)

\
FUNCIONES\
Trigonometría

* cos(x)
* \-sin(x)
* tan(x)

\
Potencia

* pow(x, 0.5) == sqrt(x)
* pow(x, 1.)
* pow(x, 2.)
* pow(x, n) para cualquier n:=número
* sqrt(x): raiz cuadrada
* inversesqrt(x) 1./sqrt(x)\
  \\

Exponenciales

* exp(x) : base 10
* exp2(x): base 2

\
Logaritmos

* log(x) : base 10
* log2(x) : base 2

\
Geometría

* length(x): longitud de un vector en la norma euclideana.
* distance(x,y): distancia entre x e y
* dot(x,y): producto interno entre x e y (producto escalar)\\

Otras

* abs(x) : valor absoluto de x. Ej. abs(1)=1, abs(-1)=1
* sign(x) : signo de x. Ej sign(1)=1 sign(-1)=0
* floor(x) : el numero entero más cercano que es menor a x. Ejempl: floor(3.14) = 3
* ceiling(x):el numero entero más cercano que es mayor a x. Ejemplo: ceiling(3.14) = 4
* fract(x): la parte fraccional de x. Ejmplo: fract(3.14)= .14;
* min(a,b): minimo valor entre a y b
* max(a,b): minimo valor entre a y b
* clamp(x, a, b): si \`x\` es menor a \`a\` devuelve a, si \`x\` es mayor a \`b\` devuelve \`b\`, sino devuelve \`x\`.
* mod(x, y): devuelve el resto de la division de "x dividido y"
* mix(x, y, a): x\*(1.-a)+y\*a
* step(x, a): devuelve 0 si x <= a, y devuelve 1 si x > a.
* smoothstep(x,a,b): devuelve 0 si x\<a, interpola linalmente entre 0 y 1 si a\<x<=b, devuelve 1 si x > b\\

Ejemplo para usar en explorador de funciones:

`float y = cos(pow(cos(x*x+u_time*.1),1.5)*10.-u_time);`\
\
\
\
\
\
\
\
\
\
\\
