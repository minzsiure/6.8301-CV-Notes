# 6.8301 Reading 2: Imaging, Lenses, Cameras as Linear Systems (Ch 5,6,7)

[toc]

# Imaging
**Goal:** 
* describe how light interacts with surfaces and 
* how those interactions are recorded with a camera.

## 1. Light interacting with surfaces
* A light ray is specified by its position, direction, intensity as a function of wavelength, and polarization. 
* Let an incident light ray be of <u>direction **p**</u> and of power, $I_{in}(\lambda)$, as a function of spectral <u>wavelength $\lambda$.</u>
* The power of the outgoing light, reflected in the <u>direction, **q**,</u> is determined by **bidirectional reflection distribution function** (BRDF), $F$, of the surface.
* If the <u>surface normal is $n$</u>, then outgoing power is some function, $F$, of the surface normal $n$, the in- and out-going ray directions $p, q$, the wavelength $\lambda$, and the incoming light power $I_{in}$. $$I_{out} = F(I_{in},n,\lambda,p,q)$$


<div style="text-align:center;">
    <img src="https://i.imgur.com/J79r8Bj.png" width="600">
</div>

* BRDF can be complicated, describing both diffuse and specular components of reflection.
    * **Lambertian surfaces** (completely diffuse reflections) have a simple BRDF, denote $F_L$.
    * $I_{out}$, the outgoing ray intensity, is only a function of the surface orientation relative to the incoming ray direction $p$, wavelength $\lambda$, and incoming light power $I_{in}$. $$I_{out} = F_L(I_{in}(\lambda),n,p)=aI_{in}(\lambda)cos(n \cdot p)$$ 
    * $a$ is the surface reflectance, or albedo. $n$ is the surface normal vector, and $p$ is the direction of the incident light.
    * For a **Lambertian surface**, $I_{out}$ is a function of $p$, not $q$.

* In general, surface reflection behaves ***linearly***: the reflection from the sum of two rays is the sum of the reflections from the two individual rays.
* To associate the reflected light with surfaces, we need to know which light rayys cae from which direction in space - this requires that we form an image (next section).

## 2. Pinhole Camera and Image formation
* The intensity reflecting off a Lambertian surfaces, $I_L$, can be obtained mathematically by integrating the equation for Lamvertian reflections over all possible incoming light directions $p$. So we have: $$I_L = \int_{p} aI(p) \, cos(n \cdot p) \, dp$$


<div style="text-align:center;">
    <img src="https://i.imgur.com/TgFpxud.png" width="600">
</div>

* The intensity $I_L$ reflects off a diffuse wall, thus tells little about the light intensity $I(p)$ from any direction $p$. Thus, we see no pictures in walls.
* We need to **form an image** to learn about $I(p)$ !
    * Forming an image involves identifying which rays came from which directions, so the role of a **camera** is to organize those rays.
    * A **pinhole camera** requires a <u>light-tight enclosure</u>: a small **hole** to let light pass, and a **projection surface** where one senses/views the illumination intensity as a function of position.
* The world is full of **accidental cameras** that create faint images, 
    * ie. most walls appear to have a faint blue tone on the bottom as they reflect the sky. 
    * We can turn the room into a pinhole camera by. closing most windows to help focus the picture that appears in the wall.

<div style="text-align:center;">
    <img src="https://i.imgur.com/xtpvZSO.jpg" width="600">
</div>

### A) Image formation by perspective projection
* A pinhole camera projects 3D coordinates in the world to 2D positions on the projection plane of the camera through the straight line path of each light ray through the pinhole.

<div style="text-align:center;">
    <img src="https://i.imgur.com/4EUMysy.png" width="600">
</div>

* Above is the definition of the **coorindate systems**:
    * The origin is the pinhole.
    * A point $P$ in the world is $P = (X,Y,Z)$, where the Z axis is perpendicular to the camera's sensing plane.
    * The projection plane is behind the pinhole and at a distance $f$ of the origin.
    * Let the coordinates in the projection plane be $p=(x,y)$, parallel to the coordinate axes $X$ and $Y$ but in *opposite* directions, respectively.
* It's useful to create a **virtual camera plane** that is radially symmetrical to the projection plane wrt the origin. 
    * It will create a projected image without the inversion of the image. 
    * The coordinates in the virtual camera plane will have the same sign as the world coordinates.

<div style="text-align:center;">
    <img src="https://i.imgur.com/k73gBdB.png)" width="600">
</div>

* **Perspective projection equations** can be derived geometrically as above. From similar triangles, $x/f = X/Z$ and $y/f=Y/Z$. Thus, $$ x=f\frac{X}{Z}$$ $$y = f \frac{Y}{Z} $$
    * The equation indicates distant objects become smaller through the inverse scaling by $Z$.
    * Perspective projection equations apply to most lens-based cameras and human vision.
* The relationship between camera coordinates $(x,y)$ and image coordinates $(m,n)$ is $$ n = -ax + n_0 $$ $$ m = ay + m_0 $$ where $a$ is a constant, and $(n_0, m_0)$ is the image coordinates of the camera optical axis.
    * Note that $m$ indexes the pixel column and $n$ the pixel row in an image.
    * Note this example is different from the world coordinate we introduced in the previous reading where the origin was not the location of the pinhole camera.


### B) Image formation by orthographic projection
<div style="text-align:center;">
    <img src="https://i.imgur.com/tUI1QZF.png)" width="600">
</div>

* In orthographic projection, as the rays are orthogonal to the projection plane, the **size** of objects is ***independent* of the distance** to the camera.
* The **orthographic projection equations** are generally written as $$x = kX$$ $$ y = kY$$ where the constant scaling factor $k$ accounts for change of *units* and it is jusst a fix global image scaling.
* Orthographic projection is a good model for telephoto lenses, where the size of objects is roughly independent of their distance away.
* We used this projection when building the simple visual system in Reading 1.

### C) How to build an orthographic camera?
* **Requirement:** restrict the light rays so that only perpendicular rays to the plane illuminate the projection plane. 

<div style="text-align:center;">
    <img src="https://i.imgur.com/ybII412.jpg)" width="600">
</div>


* One example is the **soda straw camera**.
    * A set of parallel straws allow parallel light rays to pass from the scene to the projection plane, but extinguish rays passing in all other directions. 
    * The camera does not invert the image and objects do not get smaller as they move away.
* Another example is the Telecentric lens.

# Lenses
<div style="text-align:center;">
    <img src="https://i.imgur.com/tgDmwYE.png)" width="500">
</div>


**Downside of pinhole cameras**: dim images, not much light passes through the small pinhole to the sensing plane of the pinhole camera.

**Solution**: put a lens in the larger aperture (hole) - capture more light while providing a focused image on the sensor plane.

Here we analyze how light passes through lenses under the approximation of **geometric optics**, where we ignore effects (ie. diffraction) due to the wave nature of light.

## 1. Lensmaker's formula
### A) Snell's Law
**Snell's Law** relates the bending angles of refraction, $\theta_1$ and $\theta_2$, to the indices of refraction, $n_1$ and $n_2$, at a material boundary/interface.

* Light changes both its *wavelength* and *speed* as it passes from one material to another.
* The changes at the material interface cause the light to bend, an effect called **refraction**.
* **Snell's Law** describes the amount of light bending depends on 
    1. the change of speed of light within each material ($n$) and 
    2. the orientation of the light ray wrt the interface surface ($\theta$). $$ n_1 sin(\theta_1) = n_2 sin(\theta_2)$$ where $\theta_1$ and $\theta_2$ are the **angles** wrt the surface normal of the incident and outgoing light rays, and $n_1$ and $n_2$ are the **indices of refraction** of the materials in region 1 and region 2.

<div style="text-align:center;">
    <img src="https://i.imgur.com/xVrEax5.png)" width="600">
</div>

* Both the wavelength and the speed of light in a medium are **inversely proportional** to the index of refraction of the medium $n$.
    * $n$ for a vacuum is 1. 

### B) Design a lens surface shape
* A lens, in idealization, focuses light from a surface onto a common posisition at the sensor plane.
* To achieve this, we must find a **surface shape** that allows for this focusing. 
* For small angles $\theta$ in radians, $sin(\theta) \approx \theta$. Assume the index of refraction of air is 1 and the index of refraction of lens glass is $n$, then Snell's Law gives $$\theta_1 = n \theta_2 $$ for small bending angles $\theta_1$ and $\theta_2$.

<div style="text-align:center;">
    <img src="https://i.imgur.com/SSWKomL.png)" width="600">
</div>

We make use of several arrpximations commonly hold for imaging systems:
* ***Paraxial approximation***: the deviations in angle from the optical axes are very small.
* ***Thin lens approximation***: the lens is modeled to have negligible thickness compared with other distances along the optical axis.

Under these approximations, we write simple expressions for the bending angles as a function of $\theta_S$, the lens surface orientation at height $c$.

<div style="text-align:center;">
    <img src="https://i.imgur.com/GkJqCdp.png)" width="600">
</div>

* We can algebraically eliminate the angle $\theta_1$ through $\theta_4$ to find the condition on the lens surface angle, $\theta_S$, as a function of the distance $c$ from the optical axis which allows for the desired focusing to occur: $$ \theta_S = \frac{c}{2(n-1)} (\frac{1}{a} + \frac{1}{b})$$
* Thie relationship creates the effect that every ray emanating at a small angle from point $a$ will be focused to point $b$. 
* The above equation shows that the lens surface angle $\theta_S$ must deviate from flat in linear proportional to the distance $c$ from the center of the lens.
    * Both parabolic and spherical shapes satisfy this constraint ($\theta_S \propto c$) on the lens surface slope.
    * Fora spherical lens surface, curving according to a radius $R$, we have $sin(\theta_S) = c/R$. For small angles $\theta_S$, this reduces to $$\theta_S = \frac{c}{R} $$ 
* Substitute the second equation into the **focusing condition** (first equation) yields the **Lensmaker's Formula**, $$\frac{1}{a} + \frac{1}{b} = \frac{1}{f} $$ where the lens *focal length* $f$ is defined to be $$f = \frac{R}{2(n-1)} $$

![](https://i.imgur.com/eLWs5We.png)
* The lens focuses rays from off-axis points like $P_1$ as well as from the on-axis point $P_0$ by considering off-axis light source to be equivalent to rotating the lens surface by small $\theta_R$.

Lensmaker's Formula generalizes for the case of a lens with different radii of curvature, $R_1$ and $R_2$ on the front and back faces of the lens. Define the left and right surface angles $\theta_{s_1}$ and $\theta_{s_2}$, respectively. We have $$\frac{1}{f} = (n-1)(\frac{1}{R_1} + \frac{1}{R_2}) $$

## 2. Imaging with Lenses [TODO]


<!-- ### A) Depth of Field

### B) Concave Lenses

### C) Lenses in a Telescope -->

## 3. Lens-based lightfield imager [TODO]




# Cameras as Linear systems
## 1. Cameras as linear systems
* Let $x$ represent the light intensities in the world
    * The $j$th component of $x$ gives the intensity of the light at position $j$, heading in the direction of the camera.
* If the camera sensors respond linearly to the light intensity at each sensor, their measurement $y$ will be some linear combination of the light intensities, given by matrix $A$: $$y=Ax $$
    * For conventional lens and pinhole cameras, $y$ are an image of the reflected intensities in the scene $x$, then $A$ is approximately an identity matrix. 
    * Foro more general cameras, we will need to estimate $x$ from $y$.
* With noise, the equation may not be exactly satisfied. $A$ can be non-invertible or poorly conditioned, so we introduce a regularizer. 
    * If the regularization term favors small $x$, the objective term to minimize, E, could be $$ E = |y-Ax|^2 + \lambda |x|^2 $$
    * We solve the closed form by setting its derivative wrt the elements of $x$ to be $0$, thus $$ x = (A^TA+\lambda I)^{-1}A^Ty $$

## 2. More general imagers [TODO]

<!-- ### A) Corner camera -->

