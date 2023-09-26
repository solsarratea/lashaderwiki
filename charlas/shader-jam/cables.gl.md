# Cables.gl

## Demo de Síntesis Granular

### Lectura de spritesheet con shader

{% embed url="https://cables.gl/p/04PUcx" %}

### Script de supercollider:

```supercollider
s.boot;

(
s.meter;
s.plotTree;
s.scope;
)

b = Buffer.readChannel(s, $PATH_TO_AIFF_FILE, channels: [0]);

b.plot

b.play:

FreqScope.new;

({
	var sig;

	sig = GrainBuf.ar(
		2,
		Dust.ar(100),
		0.2,
		b,
		MouseX.kr (0.5,3.1),
		(
			Phasor.ar(0, MouseY.kr (0.1,10,-1) * BufRateScale.ir (b), 0, BufSamples.ir (b)-1)
			+ LFNoise1.ar(10).bipolar (0.0* SampleRate.ir) ) / BufSamples.ir(b),
		2,
		0,
		-1,
		512
	);

	sig = sig* 0.1;

}.play)

{LFSaw.ar (10,1)}.plot(0.2)

```

## Demo de Pixel Sorting

{% embed url="https://cables.gl/p/WT9xnz" %}

## Demo de Steganography

{% embed url="https://cables.gl/p/awgtdz" %}
