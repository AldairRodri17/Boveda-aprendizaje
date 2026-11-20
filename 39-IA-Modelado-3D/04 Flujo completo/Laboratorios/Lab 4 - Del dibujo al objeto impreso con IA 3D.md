---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: Dibujo propio o Stable Diffusion 1.5 en ComfyUI, TripoSR o Hunyuan3D-2mini, Blender con 3D Print Toolbox, ElegooSlicer, Elegoo Centauri 2 Combo
tags: [ia-modelado-3d, laboratorio]
---
# Lab 4 - Del dibujo al objeto impreso con IA 3D

Parte de: [[39-IA-Modelado-3D/04 Flujo completo/_Índice Flujo completo|Flujo completo]]

**Cubre:** [[Crear la imagen de entrada para IA 3D]] · [[Stable Diffusion en 6 GB de VRAM]] · [[Limpiar un modelo generado por IA en Blender]] · [[De la IA a la impresora (flujo completo)]] · [[TripoSR]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Llevar una idea original (dibujo o imagen generada) a una figura impresa de 70–90 mm, sana para imprimir.

## Pasos

1. Imagen: dibuja tu personaje u objeto original con sombreado simple, en tres cuartos, y fotografíalo/escanéalo; **o** instala ComfyUI (versión portable de Windows) con un checkpoint de SD 1.5 y genera 4 imágenes de un objeto único con fondo blanco.
2. Cierra ComfyUI. Elige la mejor imagen y genera el 3D con TripoSR (GLB) o Hunyuan3D-2mini.
3. Blender: importa, orienta Z arriba y borra piezas flotantes.
4. Escala a la altura objetivo y aplica escala.
5. Aplica Voxel Remesh (prueba 0.3 y 0.5 mm) y elige el que conserve el detalle.
6. Corta una base plana con Bisect o Boolean.
7. Instala [3D Print Toolbox](https://extensions.blender.org/add-ons/print3d-toolbox/) y revisa *Solid*, *Intersections* y *Thickness* (mínimo 1.2 mm). Engrosa o elimina lo que no pase.
8. Si pasa de ~500 mil triángulos, aplica Decimate. Exporta STL.
9. Lamina en ElegooSlicer: orienta para minimizar soportes, usa soportes de árbol si hay voladizos, revisa la vista previa por capas.
10. Imprime (si dudas, primero al 50 %).

## Criterios de terminado

- [ ] La imagen de entrada es original (mi dibujo o generada por mí, sin personajes de marcas).
- [ ] 3D Print Toolbox reporta 0 aristas no manifold y 0 intersecciones.
- [ ] No quedan zonas por debajo de 1.2 mm según *Thickness* (o las corregí).
- [ ] El laminador no pidió reparar la malla y la altura coincide con la objetivo (± 1 mm).
- [ ] La figura impresa salió completa y estable sobre su base.

## Solución

> [!success]- Imágenes negras en SD 1.5
> En ComfyUI no debería pasar porque desactiva FP16 en GTX 16. En AUTOMATIC1111 usa `--upcast-sampling --xformers` y luego `--no-half-vae`; último recurso `--precision full --no-half --medvram` ([guía](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Troubleshooting)).

> [!success]- Si Remesh borra detalles importantes
> Baja el tamaño de vóxel o usa Remesh solo para cerrar y luego esculpe los detalles con pinceles de Blender ([[Pinceles básicos de escultura en Blender]]). Si el original ya era manifold, puedes saltarte Remesh.

> [!success]- Si el laminador muestra huecos en paredes
> Son zonas más delgadas que el ancho de línea: vuelve a Blender y engrósalas, o escala la figura más grande. Ver [[Grosor de pared y relleno según la carga]].

## Notas personales

_Qué me costó, errores que cometí, dudas._
