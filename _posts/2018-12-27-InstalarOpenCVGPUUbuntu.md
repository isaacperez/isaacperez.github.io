---
layout: post
title:  "Instalación de OpenCV con CUDA en UBUNTU"
categories: Tutoriales
comments: true
---
En este post voy a intentar explicar cómo instalé OpenCV 3.4.4. en Ubuntu 18.04 con CUDA.

Lo primero que necesitaremos es el CUDA Toolkit instalado. Nos puede valer este [post](https://isaacperez.github.io/tutoriales/PrepararPCParaTensorflow/) donde expliqué cómo instalar TensorFlow y CUDA Toolkit.

Una vez tenemos CUDA instalado, podemos pasar a descargarnos el código fuente de OpenCV desde el [repositorio](https://github.com/opencv/opencv) oficial en GitHub.

Para instalar OpenCV necesitaremos instalar algunas librerías extras:

```
sudo apt-get install build-essential cmake git
sudo apt-get install pkg-config unzip ffmpeg qtbase5-dev python-dev python3-dev
sudo apt-get install libgtk-3-dev libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev libjpeg8-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
sudo apt-get install libv4l-dev libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev
sudo apt-get install libvorbis-dev libxvidcore-dev v4l-utils vtk6
sudo apt-get install liblapacke-dev libopenblas-dev libgdal-dev
```

Podemos trabajar desde cualquier carpeta, puesto que OpenCV lo instalaremos en `/usr/local`. Una vez tengamos el código fuente descargado, debemos crear una carpeta (en nuestro caso dentro del directorio del código) para poder compilarlo:

```
mkdir build
cd build
```

Ahora podemos construir el proyecto con cmake para después instalarlo con make:

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D FORCE_VTK=ON -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_CUDA=ON -D WITH_CUBLAS=ON -D CUDA_NVCC_FLAGS="--expt-relaxed-constexpr" -D WITH_GDAL=ON -D WITH_XINE=ON -D BUILD_EXAMPLES=ON ..
```

Instalamos OpenCV (a mi me tardó más de una hora):

```
sudo make install
```

Le indicamos al sistema donde se encuentra instalado OpenCV:

```
sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
sudo ldconfig
```

Ya tenemos OpenCV instalado. Para comprobar que todo ha ido correctamente ejecutaremos un código de prueba. Para ello vamos a necesitar un fichero con el código en C++, `main.cpp`, y un fichero, `makefile`, con las ordenes para make para poder compilar el código.

El fichero `makefile` tiene el siguiente contenido:

```
CC = g++
CFLAGS = -g -Wall
SRCS = main.cpp
PROG = main

OPENCV = `pkg-config opencv --cflags --libs`
LIBS = $(OPENCV)

$(PROG):$(SRCS)
	$(CC) $(CFLAGS) -o $(PROG) $(SRCS) $(LIBS)
```

El fichero `main.cpp` tiene el siguiente contenido:

```c++
#include <opencv2/opencv.hpp>
#include <iostream>

int main()
{
    std::cout << cv::getBuildInformation() << std::endl;

    return 0;

}
```

Para compilar el programa solo es necesario ejecutar la siguiente instrucción en el directorio del proyecto:

```
make
```

Al ejecutar el binario generado (`main`) nos dará como salida información de la versión de OpenCV que tenemos instalada, donde podremos comprobar que todo ha salido correctamente y que tenemos OpenCV disponible para trabajar con la GPU (solo hay que comprobar que existe un apartado llamado `NVIDIA CUDA`).
