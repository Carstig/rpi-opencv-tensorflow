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
   -v=/opt/vc/bin:/opt/vc/bin \
   --device=/dev/vchiq \
   --device=/dev/vcsm \
   --mount \
   type=bind,source=$HOME/object_detection,target=/home/docker/object_detection \
   carstig/rpi-opencv-tensorflow \
   /bin/bash
```

### testing it
```
raspistill -o cam.jpg
```
it just hangs and does not produce cam.jpg. But on the prompt it works.

1. try from within python
  - python3 ... cv2.VideoCapture(0) -> error
  - python3 - import cv2 - video = cv2.VideoCapture(-1)
  - fail: VIDEOIO ERROR: V4L: can't find camera device
2. basic problem is, that although (even without docker) I can make pics with
raspistill but there is no /dev/video*



### accessing the raspi cam
- ldconfig ist da
? EXPOSE 3000 notwendig im Dockerfile
- /opt/vc/bin/raspistill cmd is found but hangs
? `docker run -v=/opt/vc/bin:/opt/vc/bin --device=/dev/vchiq -p 3000:3000 [my/image:latest]`
- `--privileged` gives access to all
- added --device=/dev/vcsm 




## upload container
```
sudo docker login
sudo docker push carstig/rpi-opencv-tensforflow:latest
```

