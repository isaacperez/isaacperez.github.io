---
layout: post
title:  "Instalación de Tensorflow"
categories: anuncios
comments: true
---
Hoy en día podemos encontrar multitud de _frameworks_ para desarrollar sistemas basados en _deep learning_. Uno de los más populares es TensorFlow, se trata de un _framework_ de _machine learning_ de código abierto. Fue lanzado por Google en 2015 y ha contando con un gran respaldo de la comunidad desde entonces.

Personalmente, me pasé de Caffe a TensorFlow como una apuesta de futuro. Después de unos meses trabajando únicamente con TensorFlow he de decir que el resultado ha sido muy satisfactorio y es por ello que he querido publicar este post para, los que como yo, quieran ponerse manos a la obra con esta librería.

TensorFlow es compatible con Linux, Windows y Mac OS X. Mi recomendación es trabajar con Linux, concretamente con Ubuntu 16.04 que es el S.O. con el que haremos la instalación.

Disponemos de varias API's para programar en TensorFlow con diferentes lenguajes. Principalmente suelo trabajar en Python por lo que abordaremos la instalación de TensorFlow para Python. Además, es recomendable, si estamos comenzando, realizar la instalación con algún gestor de paquetes como Pip. Para instalar Pip en Ubuntu para Python 2 solo debemos ejecutar estas instrucciones desde la terminal:

```
sudo apt-get update && sudo apt-get -y upgrade
sudo apt-get install python-pip python-dev
```

Para Python 3:

```
sudo apt-get update && sudo apt-get -y upgrade
sudo apt-get install python3-pip python3-dev
```

En cuanto al hardware, normalmente necesitaremos una gran capacidad de cómputo. Una buena CPU puede ser hasta insuficiente si vamos a entrenar redes muy complejas. Lo recomendable sería hacernos con alguna tarjeta gráfica de NVIDIA y, a poder ser, una que cuente con mas de 8 GB de memoria, ya que algunas arquitecturas de redes pueden requerir bastante memoria y nos obligan a trabajar con un _batch size_ muy pequeño (lo digo por experiencia). De hecho, la última versión de TensorFlow solo es compatible con tarjetas gráficas de NVIDIA que tengan una capacidad de cómputo de CUDA (_CUDA Compute Capability_) de 3.5 o mayor (ver [https://developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus)).

Una vez dispongamos de un equipo con una buena tarjeta gráfica y Ubuntu y Pip instalados... necesitaremos un poquitín más de paciencia para instalar todas las librerías y drivers que permitirán a TensorFlow sacar el máximo partido a la tarjeta gráfica. La versión precompilada de TensorFlow que vamos instalar, la 1.5 a fecha de cuando se escribió este post, necesita que se instalen las siguientes librerías: CUDA Toolkit 9.0, el driver asociado a CUDA Toolkit 9.0 y cuDNN v7.0.

Antes de empezar es necesario instalar el compilador gcc y los _headers_ del _kernel_ que se está corriendo:

```
sudo apt-get install build-essential linux-headers-$(uname -r)
```

Para instalar las librerías de CUDA necesarias hay que ejecutar las siguientes instrucciones desde una terminal:

```
wget https://developer.nvidia.com/compute/cuda/9.0/Prod/local_installers/cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb
sudo apt-get update
sudo apt-get install cuda-9.0
```

Será necesario reiniciar el equipo para cargar el driver que hemos instalado. Una vez hayamos reiniciado, accedemos a nuestro `~/.bashrc` y añadimos, al final, las siguientes dos líneas:

```
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

Para comprobar que la instalación de las librerías de CUDA ha finalizado correctamente, podemos ejecutar la siguiente instrucción en la terminal:

```
nvcc --version
```

Deberíamos de ver, entre otra información, la versón de CUDA instalada: `Cuda compilation tools, release 9.0, V9.0.176`.

La versión del driver que se ha instalado se puede consultar con la siguiente instrucción:

```
nvidia-smi
```

En las primeras líneas debe aparecer la versión del driver: `Driver Version: 384.111`.  

Con esto damos por finalizada la instalación de las librerías de CUDA y pasamos ahora a instalar cuDNN 7.0. Para conseguir esta librería será necesario que nos registremos en el siguiente enlace: [cuDNN 7.0](https://developer.nvidia.com/rdp/cudnn-download). Debemos hacer click en la pestaña: `Download cuDNN v7.0.5 (Dec 5, 2017), for CUDA 9.0` y luego en `cuDNN v7.0.5 Library for Linux`. Se descargará un comprimido de nombre `cudnn-9.0-linux-x64-v7.tgz`. Para instalar la librería abrimos una terminal donde se haya descargado el fichero e introducimos las siguientes instrucciones:

```
tar -zxvf cudnn-9.0-linux-x64-v7.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

Para comprobar que cuDNN se ha instalado correctamente hay que ejecutar desde la terminal:

```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

A la salida deberíamos ver:
```
#define CUDNN_MAJOR 7
#define CUDNN_MINOR 0
#define CUDNN_PATCHLEVEL 5
```

Lo que indica que disponemos de cuDNN v7.0.5 en nuestro equipo.

Una vez completados todos los pasos anteriores, podremos, por fin, instalar TensorFlow para Python 2. Para ello, solo necesitamos ejecutar la siguiente instrucción desde la terminal:

```
sudo pip install tensorflow-gpu
```
Para Python 3:

```
sudo pip3 install tensorflow-gpu
```

Si hemos cruzado bien los dedos, debemos tener TensorFlow instalado en nuestro equipo. Para probar que todo ha salido bien solo debemos de ir a una terminal y ejecutar `python` o `python3` para luego introducir el siguiente código de prueba:

```python
import tensorflow as tf
hola = tf.constant('¡Hola, TensorFlow!')
sess = tf.Session()
print(sess.run(hola))
```

Si todo ha ido bien, la salida del sistema debería ser:

```
¡Hola, TensorFlow!
```

¡Esto es todo! ¡A programar!
