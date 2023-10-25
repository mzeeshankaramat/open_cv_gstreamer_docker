#  GPU-accelerated Docker container with OpenCV 4.5, Python 3.8 and GStreamer 

- [opencv](https://github.com/opencv/opencv) + [opencv_contrib](https://github.com/opencv/opencv_contrib)
- Python 3.8.0
- Ubuntu  20.04 LTS
- GStreamer  1.16.3
- FFMPEG
- CUDA  11.0.3
- NVIDIA GPU arch:  30 35 37 50 52 60 61 70 75 
- CUDA_ARCH_PTX = 75
- cuDNN:  8
- OpenCL
- Qt5::OpenGL  5.12.8

Pull the image from here :

- [https://hub.docker.com/r/zeeshankaramat25](https://hub.docker.com/r/zeeshankaramat25)
   ```sh
   $ docker pull zeeshankaramat25/gstreamer-opencv-docker
   ```

## How to run :

- With GPU 
  
    You need to install [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) on your machine. Run the container by this command :

    ```
    $ docker run --gpus all -it --rm  zeeshankaramat25/gstreamer-opencv-docker:latest
    root@vrlap05:/myapp#  
    ```

- With CPU :
    
    If no GPU available on your machine, yet you can use the container with [Docker](https://docs.docker.com/engine/install/)
    ```
    $ docker run -it --rm zeeshankaramat25/gstreamer-opencv-docker:latest
    root@vrlap05:/myapp# 
    ```

## Inspect the GStreamer API out of the OpenCV wrapper 
### Test gst with test src
Type into the container's shell :

```sh
root@vrlap05:/myapp# gst-launch-1.0 videotestsrc ! videoconvert ! autovideosink
```
then a  GStreamer hello world window pops up . See the [documentation](https://gstreamer.freedesktop.org/documentation/tutorials/basic/gstreamer-tools.html?gi-language=python).

 <img src=gstream.png width="200" height="200">

### Test gst with webcam
Type into the container's shell :

```sh
root@vrlap05:/myapp# gst-launch-1.0 v4l2src ! videoconvert ! autovideosink
```
you can switch between multiple cams using v412src device argument: v4l2src device=/dev/video0

 <img src=image.png width="720" height="480">

## OpenCV and  GStreamer debugging

Export OpenCV  [Log Levels ](https://docs.opencv.org/4.5.0/da/db0/namespacecv_1_1utils_1_1logging.html) or GStreamer [Debug Levels](https://gstreamer.freedesktop.org/documentation/tutorials/basic/debugging-tools.html?gi-language=python) into our container's shell . For example:

```sh
root@vrlap05:/myapp# export OPENCV_LOG_LEVEL=INFO 
```
or

```sh
root@vrlap05:/myapp# export GST_DEBUG=2
```

then run one of the examples and browse the output.

## Build your own image 
The [Dockerfile](Dockerfile) culprit for the image may not have a perfect structure . It is just my own assembly.
You may modify, upgrade and build a proper one for your requirements :

```bash
$ docker build -f Dockerfile -t <name>:<tag> .
```
It won't be that straight-forward, you will get some deprecation warnings and compatibility issues. <br />  
