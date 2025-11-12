Title: Raymarching voxels on the CPU with real-time lighting.
Date: 12/11/25

# Rendering Voxels on the CPU!

A creator who goes by the name [Vincent](https://www.youtube.com/@maximilianvincent) on YouTube just released a video showcasing real-time rendering for 3D Voxels entirely on the CPU.

## What was achieved
- Real-Time CPU Rendering: yes, the whole 3D environment is rendered on the CPU, the amount of technical challenges to achieve that is just... holy.
- Advanced Lighting Effects: as if rendering blocks wasn't enough, the demo also features sophisticated real-time lighting, soft shadows, point lights, and even global illumination.
- Dynamic Environment: the scene showcased is not even static, it's a procedural day/night cycle. *the guy is just flexing at this point*
- Runtime Editing: the environment itself can be manipulated in real-time you could remove or add voxels.

## Why this is crazy
I'm gonna put it simply: We don't render on CPUs... **period.**
But this guy just *had* to defy that.
GPUs are optimized to render Graphics, 3D or else, that's their main purpose, that's what they do, and for rendering 3D, you'd normally need a GPU with thousands of cores working in parallel, but on a CPU where you have only 10s of cores at best, that are general-purpose and not even optimized for rendering stuff, that's an insane feat.
And to raymarch (raytracing related technique) Voxels in real-time on a CPU, that makes it an insane feat++, because like raytracing, raymarching emulates how light behaves in the real world on an advanced level.

## Personal take on this
Oh my god.

### [![](https://i.ytimg.com/vi/y0xlGATGlpA/hq720.jpg?sqp=-oaymwEcCNAFEJQDSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&rs=AOn4CLChFo7RZk3QP7CPn1ByKWQzahO4uQ)](https://www.youtube.com/watch?v=y0xlGATGlpA)