# Docker container to run Tensorflow and Open CV

the intention is to provide a runtime environment for object detection for a
RaspberryPi.


Approach is to avoid building OpenCV and pulling tensorflow into this container.
_i tried to install, build opencv on raspi now three times and I failed_

I was following [EdjeElectronics' guide for Tensorflow Object Detection on the
Raspi](https://github.com/EdjeElectronics/TensorFlow-Object-Detection-on-the-Raspberry-Pi)

and mixed it with my instructions from [Labinet
Docker](https://github.com/Carstig/labinet_docker).


## Build this container

```
cd rpi-opencv-tensorflow
sudo docker build -t carstig/rpi-opencv-tensforflow .
```


## run this container
I assume you have data or code in `$HOME/object_detection`. If not adapt the
argument to `type=bind` :


```
sudo docker run -it \
   --mount
   type=bind,source=$HOME/object_detection,target=/home/docker/object_detection \
   carstig/rpi-opencv-tensorflow \
   /bin/bash
```



## upload container
```
sudo docker login
sudo docker push carstig/rpi-opencv-tensforflow:latest
```

