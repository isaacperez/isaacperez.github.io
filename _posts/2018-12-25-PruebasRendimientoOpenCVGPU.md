---
layout: post
title:  "Pruebas de rendimiento de OpenCV en GPU"
categories: Anuncios
comments: true
---
A la hora de trabajar con OpenCV siempre he querido tener disponible una GPU para poder realizar los cálculos mucho más rápido. Es por ello que, ahora que dispongo de un PC bastante potente, me he dedicado a hacer una serie de pruebas (no muy rigurosas) que me permitiesen saber de qué mejora estamos hablando.

El código de las pruebas y los datos utilizados se encuentran en el siguiente repositorio: [medida_rendimiento_openCV_GPU](https://github.com/isaacperez/medida_rendimiento_openCV_GPU).

He utilizado bastantes operaciones diferentes para las pruebas y varias ejecuciones para poder promediar los resultados. Algunas de las conclusiones que obtengo de esta prueba son las siguientes:

  - La GPU aporta más contra más grande sea la imagen de entrada. Si tenemos muchas imágenes pequeñas no ganaremos tanto como con una única imágen grande si la operación es global a la imagen.
  - Hay que tener presente el tiempo de transferencia (ida y vuelta) de los datos a la GPU para sumarlo al tiempo de la operación que vayamos a realizar ya que es considerable a partir de ciertos tamaños de imagen.
  - Dependiendo o no de si han sido utilizados antes, algunos algoritmos pueden tomar más tiempo del que en realidad tomarían una vez el programa esté en uso por temas de cache, tanto de la CPU como de la propia GPU (inicialización de los buffers por ejemplo).

Espero que les sirva de ayuda y valoraría muchísimo las aportaciones que puedan hacer acerca de las pruebas y cuáles son sus conclusiones.

Un saludo,
Isaac.
