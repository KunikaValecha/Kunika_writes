---
date: 2017-04-09T10:58:08-04:00
description: "The end to end process of Image formation from capturing of image to rendering it in digital format"
mathjax: true
featured_image: "/images/invent.jpeg"
tags: ["Image Formation", "Image Projection", "Pinhole Camera"]
title: "The process of 2D Image Formation"
---

This blog has two parts, 

**Part 1 :** Image Projection
**Part 2 :** Image Rendering

In the first section we will see that how is an image captured. In second part we will dive into digitalization of an image signal.

# Part 1 : IMAGE PROJECTION
 
## Concept of Pinhole Camera
 
In order to form an image on 2D grid we need the light rays from illumination source to hit the image sensor. For the purpose we use pinhole camera. A pinhole camera is a simple camera without a lens but with a tiny aperture, pinhole. It comes as a light-proof box with a small hole in one side. Light from a scene passes through the aperture and projects an inverted image on the opposite side of the box, which is known as the camera obscura effect. Most of objects are not luminious in nature. Therefore, object is exposed to a luminious source and the rays from luminious source hits the objects and reflects to the image plane.

![pinhole](/images/pinhole.png)




### Image Formation

If light reflected from a scene is reflected directely on image plane then the silhoutte we get is blurry and indistinct. Here comes the need of focus. Rather than passing all the points hitting on a surface to image plane, we try to be selective and let a few of rays travel. This helps us to get a sharp distinct image of objects in scene. In order to focus and drop out unrequired rays we place an obstacle between the object and the image plane.

| Image Formation      | Output Image       |
| ------------------------------------------- | ------------------------------------ |
| ![Without focus](/images/with_focus.png)    | ![Blurred Image](/images/car_blur.jpg)    |
| ![With focus](/images/without_focus.png ) | ![Blurred Image](/images/car_front.jpeg) |

#### Now that we know, reducing the size of aperture gives us finer image the why don't we reduce it to least possible?

Because, that may cause diffraction and ultimately worsens the image quality.

**Diffraction** refers to various phenomena that occur when a wave encounters an obstacle or opening. It is defined as the bending of waves around the corners of an obstacle or through an aperture into the region of geometrical shadow of the obstacle/aperture. The diffracting object or aperture effectively becomes a secondary source of the propagating wave. 

![Diffraction](/images/diffraction.png)
*Image showing the resulatant images with reducing the aperture size*

The two most importatnt parameters of a camera are, *focal length and Camera Center*

### Focal Length

The distance between the pinhole (aperture) and image plane is known as focal length. The focal length defines the size of object projected onto image and plays important role in camera focus when using lenses to improve camera performance. 

### Camera Center

The coordinates of the pinhole (aperture)

## Image Projection 

Light traels from point \\(O_world\\) through the camera apeture to the sensor surface. The projection onto the sensor's surface throughthe aperture results in flipped images of the object in the world . To avoid this confusion, we usually define a virtual image in front of camera sensor.
![real image](/images/real_image.png)

## The Coordinate systems

While taking a picture there exist a lot of exchange between the coordinate systems. The object in world first has it's location with respect to inertial frame which is supposed to be **East North Up** coordinate system (to be discussed later in the story), then light travels from world coordinate system to image coordinate system and in order to get the position of an object in any coordinate system we need to have the location wrt to another coordinate system and then do conversions.

### Coordinate systems involved

There are three coordinate systems which exchange the location information of any object in scene during the process of image formation. The systems are below,

**World Coordinate System** First we select world frame in which to define coordinates of all objects and camera.

**Camera Coordinate System** The coordinate frame attached to the center of lens of camera is known as camera coordinate system.

**Image Plane Coordinate System** This coordinate system is attached to the virtual image plane. The image system coordinate frame is attached to top left corner of virtual image plane.

![Coordinate Systems](/images/coordinate_sys.png)

In the image above, \\(X_c\\) , \\(Y_c\\) , \\(Z_c\\) represents the camera coordinate system. And, \\(X_s\\) , \\(Y_s\\), \\(Z_s\\) represents the screen coordinate system. The screen is image screen on which irtual image is projected.

To locate an object present in world coordinate system to camera coordinate system we define transl;ation and rotation matrices. These matrices model any transformation between world coordinate system and another.

Then we have camera parameters. There are two types of camera parameters, *Intrinsic and Extrinsic*. Instrinsic parameters are in buit properties of camera, *focal length and camera center*. Extrinsic parameters define the pose of camera wrt world they essentially constitutes of *translational and rotattional matrices*. Intrinsic and Extrinsic parameters together forms the projection matrix of image. 

## Projection from World Coordinates to Image Coordinates

This a two step process:
1. Project from world coordinates -> Camera coordinates
2. Project from camera coordinates -> Image coordinates

![world to image transform](/images/world2img.png)

We then transform image coordinates to pixel coordinates through scaling and offset.

## Mathematics behind

### World -> Camera 

\\(O_{cam}\\) = [R|t] \\(O_{world}\\)

The above transform is performed using rigid body transformation matrix T which comprises of R and t. R denotes the rotation of camera wrt world and t denotes the translation.

### Camera -> Image

 
$$\mathbf{O_{img} = }\\begin{bmatrix} f \\ \\ 0 \\ \\ u_0 \\\ 0 \\ \\ f \\ \\ v_0 \\\ 0 \\ \\ 0 \\ \\ 1 \\end{bmatrix} * O_{cam} = K * O_{cam}$$ 

The K matrix in equation aboe depends on camera intrinsics, such as camera geometry and camera lens characteristics.

Since both transformations are just matrix multiplications, we can define a matrix P as K times R and t,

$$\mathbf{ P = K [R | t]}$$

Thus defining projection from world coordinates -> Image Coordinates into a single step,

$$\mathbf{O_{img} = PO_{world} = K [R | t] O_{world}}$$

The above equation is very important and useful from transformation perspective. You will see more about projection matrix in my upcoming blogs.

# Part 2 : IMAGE RENDERING

## What is an Image?

So far we discussed about the image projection. Now lets get our hands into image rendering. Image is a function, f, from \\(R^2\\) to R:

* f(x, y) gives intensity at position (x, y)
* Images can be defined over a rectangular plane as,
			f:[a, b][c, d] -> [0,1]
* Colored images can be defines as,
$$\mathbf{f(x,y)->}\\begin{bmatrix} r(x,y) \\\ g(x,y) \\\ z(x,y) \\end{bmatrix}$$

![Basic Illumination](/Pinhole_cam/basic.jpg)

An image can be characterised by two components, 

* **Illumination** - i(x,y) is amount of source illumination incident on scene.
* **Reflectance** - r(x,y) is amount of illumination reflected by object in scene.

$$f(x,y) = i(x,y) r(x,y)$$
where,
\\[0<i(x,y)<{\infty} \\ \\  \\ and \\  \\  \\ 0<r(x,y)<1\\]

r(x,y) depends on objects properties. r= 0 implies total absorption whereas r=1 implies complete reflectance.

Typ[ically, 

#### Values of r(x,y):
* Black velvet = 0.01
* Stainless Steel = 0.65
* Snow = 0.93

#### Values of i(x,y):
* Sun on clear bright day = 90,000 Im/\\(m^2\\)
* Cloudy day = 10,000 Im/\\(m^2\\)
* Inside an office = 1000 Im/\\(m^2\\)

An image is essentially a continuous signal which is further sampled on 2D space and each sample is quantized to produce a digital inteface. Each sample produced is a pixel whose value ranges from 0 to 255.

#### Process of Sampling and Quantization :
* **Sampling** is a process used in statistical analysis in which a predetermined number of observations are taken from a larger population. For functions that vary with time, let s(t) be a continuous function (or "signal") to be sampled, and let sampling be performed by measuring the value of the continuous function every T seconds, which is called the sampling interval or the sampling period. Then the sampled function is given by the sequence:
$$s(nT)$$  for integer values of n.
The sampling frequency or sampling rate, \\(f_s\\), is the average number of samples obtained in one second (samples per second), thus \\(f_s\\) = 1/T. 
* **Quantization**  is the process of mapping continuous infinite values to a smaller set of discrete finite values. In the context of simulation and embedded computing, it is about approximating real-world values with a digital representation that introduces limits on the precision and range of a value.
For example, rounding a real number x to the nearest integer value forms a very basic type of quantizer – a uniform one. A typical (mid-tread) uniform quantizer with a quantization step size equal to some value \\(\Delta \\)  can be expressed as
$$\displaystyle Q(x)=\Delta \cdot \left\lfloor {\frac {x}{\Delta }}+{\frac {1}{2}}\right\rfloor$$

where the notation \\(\lfloor \ \rfloor \\) denotes the floor function.
![Sampling and Quantization](/images/sampling.png)
*Figure showing process of sampling followed by quantization*

#### How a sampled and quantised image looks like ?

![Sampled and Quantized image](/images/sampled_img.png)
*Figure showing an example of sampled and quantized output*

As you may observe that the image produced may look pixelated when we increase the height and width of image. To deal with this problem, interpolation comes into the role. 

**Interpolation** is a process in which the data obtained by sampling or experimentation, which represent the values of a function for a limited number of values of the independent variable is densified by some estimated values which fille the intermidiate gaps between the existing data points. 

![Interpolated](/images/Interpolation.png)
*Figure showing Nearest Neighbour Interpolation*

So, in this blog got the surface level knowledge of end to end Image rendering process. We will dive deeper into the concepts mentioned in this blog. Stay tuned!




