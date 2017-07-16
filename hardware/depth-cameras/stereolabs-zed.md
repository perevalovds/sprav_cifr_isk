# Stereolabs ZED

StereoLabs ZED  depth camera  
[https://www.stereolabs.com/zed/specs/](https://www.stereolabs.com/zed/specs/)

![](/assets/zed_product_main.jpg)

This is _passive_ depth camera: it consists of two RGB-cameras spaced from each other \(the distance between the cameras 12 cm\).In contrast, active depth cameras such as Kinect1,2,  Xtion and PrimeSense contain an IR laser, which measures distance.The active camera work well indoors, but do not work in the outdoor scenes containing objects illuminated by sunlight.Therefore, for robotics and interactive public projects there is an urgent need for passive depth cameras, working in the presence of sunlight.

This is relatively _cheap_ depth camera \($ 449\). It’s analogue is a family of cameras Bumblebee2 by PointGrey [https://www.ptgrey.com/bumblebee2-firewire-stereo-vision-camera-systems](https://www.ptgrey.com/bumblebee2-firewire-stereo-vision-camera-systems). These cameras are good, but the cost is a few thousand dollars.

This is a _small_ and _lightweight_ camera. Its size and weight are comparable to Xtion camera.It has mounting hole in its center with a 1/4″ UNC thread for easy fixing onto a standard camera tripod.The camera can beused on the drone [https://developer.nvidia.com/embedded/learn/success-stories/stereolabs](https://developer.nvidia.com/embedded/learn/success-stories/stereolabs).

The camerasupportsOSWindows, Linux, Jetson TK1 and Jetson TX1.

## 1. Installing on Windows

To install ZED camera for Windows you are required USB 3.0 \(it works with USB 2.0 too, but slower\), a modern graphics card NVidia, and installed CUDA 7.5.

1. Camera is shipped with flash drive, containing drivers and user guide.  
   Open the flash drive and run the installer  
   **ZED\_SDK\_WinSetup\_v0.9.4e\_CUDA75\_beta.exe**  
    \(or a newer version\) in the  
   **Windows**  
    folder.  
   \(If the installer does not start, then start  
   **vcredist\_x64.exe**  
    to install the necessary libraries Visual Studio.\)

   Also, you can download these files from  
   [https://www.stereolabs.com/developers/\#download\_anchor](https://www.stereolabs.com/developers/#download_anchor)  
    page

2. If you have not installed CUDA 7.5, the installer will offer to install CUDA.  
   You may agree, but in our case, CUDA installer said that he could not install it on my card.  
   So, we recommend to install the CUDA 7.5 directly from NVidia site   
   [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)  
   .

3. After installation, the computer restarts.
4. Now, connect the camera to a computer.
   \(In our case, Windows shows a message box that the new device drivers are installing, and this message is hung for a long time, with a progress bar at around 20%. But, it turned out that the driver has been successfully installed, so you need to close this message box\)
5. Let’s check that  
   the camera works.  
   To do this, run the  
   **ZED Explorer**  
    program.  
   It will show the picture from both RGB cameras available in ZED:

   ![](https://kuflex.files.wordpress.com/2016/08/zed-1.png?w=960 "ZED-1")

## 2. Calibration

Now it’s a good idea to explore thequality of the depth map computed by ZED camera. But before that, we suggest todo the calibration by running **ZED Calibration** program, and press the _Start_ button. You will a program’s window with a grid and translucent, slightly visible redcircle:

![](https://kuflex.files.wordpress.com/2016/08/zed-2.png?w=960 "ZED-2")

You have to put your camera in front of the screen.Try to position the camera so that it is perpendicular to the axis of the screen plane.In this case, the screen area on this pointing the camera, will appear transparent blue circle or ellipse \(if the camera axis is non-perpendicular to the screen plane\).  
The size of the circle will depend on the distance between the camera and the screen.Your task is to match the blue circle with a red circle.After that, a red circle will change its size and position.You must repeat this procedure several times.  
As a result, the screen displays a message calibraion data is collected, and some computations will started for a while.

## 3. Exploring depth map quality

Now, let’s see how well the camera calculates the depth map. On official demos you can see the camera detects well large objects \(trees, people\) outdoors. Here we examine the work in the room.

Start **ZED Depth Viewer** program.

![](https://kuflex.files.wordpress.com/2016/08/zed-3.png?w=960 "ZED-3")

In the upper left corner of the window it shows left and right cameras as anaglyph \(you can change this appearance style\). At the bottom it shows depth map, at the right – a cloud of 3D-points, which can be rotated with the mouse.

In the depths map bright pixels correspond to the close objects, dark pixels to the distant objects.Black color indicates the areas where the camera refused to calculate the depth.

In order to better explore the quality of the depth map, click the _Settings_ button \(located in the upper right corner of the window\), and decrease decrease display range by adjusting _Depth Clamp_ slider, say, down to 1663 mm \(the default value is 15,000 mm\).

![](https://kuflex.files.wordpress.com/2016/08/zed-4.png?w=904&h=804 "ZED-4")

Then, the depthmap will have more contrast.

![](https://kuflex.files.wordpress.com/2016/08/zed-5.png?w=960 "ZED-5")

We can see that the camera is correctly saw the silhouette of a man and a chair, as well as the surrounding objects. At the same time, the depth map for the human \(from the nose to the hand\) was calculated incorrectly: the part of the wall is erroneously added to the silhouette. This is due to the fact that the walltexture ishomogeneous and passive stereo cameras perform poorly with depthcalculation for areas with homogeneous texture.

### 3.1 Analysis of near objects

The camera does not see \(that is, the depth map is incorrect or black\), or gives the large distortions for the objects located at a distance equal or less than 80 cm to the camera. The screenshotshows ahand, badged close to the camera. It can be seen that the depth map are not computed for the hand.

![](https://kuflex.files.wordpress.com/2016/08/zed-6.png?w=960 "ZED-6")

### 3.2 Analysis of small objects

The camera does not distinguish small objects such as the fingers, as well as the depth map is smeared on visually near objects. The screenshot shows the depth map for the splayed fingers, as well as protrusion of a small area in a depth map from the head to the hand.

![](https://kuflex.files.wordpress.com/2016/08/zed-7.png?w=960 "ZED-7")

### 3.3 Processing the dark areas and the impact of a solar flare

In this experiment we capture a darkened space, opposite the window. As it turned out, **Depth Viewer** automatically adjust the average brightness of the images, so dark images became brighter. And, the depth map is restored correctly:

![](https://kuflex.files.wordpress.com/2016/08/zed-12.png?w=960 "ZED-12")

Note the solar flare at the top of the anaglyph image. It is evident that at this area the depth map was notcalculated \(black spot\).

## 4. Working speed

ZED-camera transmits two video streams \(left and right RGB cameras\) into the computer. The depth map computation is performed using GPU.Therefore, the performance heavily depends on the graphics card.  
We used a laptop with **GeForce GT 650M** video card, and the speed was 11 frames per second at resolution _HD720_.  
At the top of the program you can switch the resolution and frame rate, and for resolution for the _HD720_ where is option _60_ frames per second – that is, for modern graphics cards, you can get a quite good depth calculation speed.  
Also, you can reduce the resolution – there are _VGA_ / _100 FPS_ mode, but currently we can’t turned it on \(the program crashed\).

## 5. Creating a three-dimensional  space models using ZedFu

**ZedFu**programs included in the package allows for the construction of three-dimensional model of the space, by stitching multiple depth maps and images captured from diferent viewpoints.  
This technology is calledData Fusion.There are severalprograms for Data Fusion, working with depth cameras such as the Kinect, and also with conventional \(RGB\) cameras and mobile phones. We had experience with the following programs:

* **Kinect Fusion**  
  [https://msdn.microsoft.com/en-us/library/dn188670.aspx](https://msdn.microsoft.com/en-us/library/dn188670.aspx)  
   – it’s open source program, supporting Kinect1, 2, but works not very good. It’s great as starting point for Data Fusion algorithms study.

* **Scanect**  
  [http://skanect.occipital.com](http://skanect.occipital.com)  
   – it’s paid program, but you can experiment with the trial version, which allows you to publish your scanned models online, see our example  
  [https://sketchfab.com/models/c04c2e056a0341389647f5ff743eff83](https://sketchfab.com/models/c04c2e056a0341389647f5ff743eff83)

* **123D Catch**  
  [http://www.123dapp.com/catch](http://www.123dapp.com/catch)  
   – this free program does work with with a set of photos from an \(RGB\) camera or mobile, and create three-dimensional model of the fine quality. Data p  
  rocessing is performed on a remote server, so the construction of models can take a lot of time.

Let’s make an experiment with **ZedFu.** Run it and click the _Live_ button at the top of the screen to start the capturing video.

\(Sometimes the program hangs after pressing the _Live_ button. To resolve this problem, reconnect the camera and restart the program.\)

You will see the program window with the RGB-image \(top left\), a depth map \(bottom left\) and 3D-model of the current frame at the right:

![](https://kuflex.files.wordpress.com/2016/08/zed-8.png?w=960 "ZED-8")

Now click the _Start_ button at the top of the screen and move the camera slowly.You will how the 3D model at the right will grows.After a while, press the _Stop_ button.The program will start to build a finer 3D model.

Note: you must wait until the program will complete the construction of a model to get the 3D model of the good quality!

The resulted model is saved as the OBJ-file, and in the bottom of the screen you will see the path to the file \(**Documents/ZED/Meshes**\). Also the model will be shown, and you canrotate and move it using the mouse:

![](https://kuflex.files.wordpress.com/2016/08/zed-9.png?w=960 "ZED-9")

Open the folder with the OBJ file. You will find there not only OBJ file itself, but also the files of materials and textures, as well asa filein the PLY format:

![](https://kuflex.files.wordpress.com/2016/08/zed-10.png?w=960 "ZED-10")

OBJ and PLY are standard 3D file formats, and you can use them in most 3D-software. For example, you canopen theOBJ-file in the **Deep Exploration** program \(its paid program, but allows to view files in trial mode\):

![](https://kuflex.files.wordpress.com/2016/08/zed-11.png?w=960 "ZED-11")

Zed camera “sees” objects at a distance of 20 meters, and so allows capturing outdoor scenes:

![](https://kuflex.files.wordpress.com/2016/08/screenshot-112.jpg?w=960 "Screenshot \(112\)")

![](https://kuflex.files.wordpress.com/2016/08/screenshot-111.jpg?w=960 "Screenshot \(111\)")

![](https://kuflex.files.wordpress.com/2016/08/screenshot-110.jpg?w=960 "Screenshot \(110\)")

It can be seen that the quality of the resulting model is not very high, namely, the form of furniture and people is restored inaccurately. At the same time, the overall structure and general location of the objects is restored well.

### Conclusion

The camera is quite suitable for using in mobile robotics and interactive installations in the open air and sunlight. The quality of the depth map is now lower than that of active depth cameras such as Kinect, but it is the specificity of the passive stereovision. There is no doubt that over time the stereocorrespondence algorithms will improve, and the passive cameras will work better.

The camera produces a cloud of points, and therefore potentially it is possible to connect it to the OpenNI / NITE to calculate the coordinates of human body parts for using in interactive installations.

