# Reading 1: Introduction, Simple Vision System
[toc]
## 1. Blocks World
* A world composed of a very simple set of objects.
* The simple objects are composed of *flat* surfaces which can be horizontal or vertical, resting on a white horizontal ground plane.

## 2. A Simple Image Formation Model
### A) Parallel/orthographic projection
* Light rays travel parallel to each other and perpendicular to the camera plane.
* Produces images in which objects do not change size wrt to its position to the camera. ie. parallel lines in 3D remain appear as parallel lines in the 2D image.
* Can be generated using camera zoom.

<div style="text-align:center;">
    <img src="https://i.imgur.com/GbSbfQp.png" width="400">
</div>

### B) Perspective projection
* Images are formed by the convergence of the light rays into a single focal point.
* Common in most pictures taken with a camera.


<div style="text-align:center;">
    <img src="https://i.imgur.com/QM21T0p.png" width="400">
</div>



## 3. A Simple Goal
* Map world coordinate $(X,Y,Z)$ to image coordinate $(x,y)$.

$$ x = X$$ $$ y = cos(\theta)Y - sin(\theta) Z$$

<!-- <div style="text-align:center;">
    <img src="https://i.imgur.com/aFN0mWc.png" width="300">
</div> -->

* **Goal:** (reverse direction) Recover world coordinates of all the pixels seen by the camera.
* Other things to consider: 
    * Recover actual color of the surface seen by each pixel $(x,y)$ by discounting illumination effects; because the color of the pixel is a combination of the surface albedo and illumination (color of the light sources + inter-reflections).




## 4. From images to edges and useful features
### A) Edges
* **Definition**: Image regions where there are sharp changes/discontinuities of image intensities.
* Sources of edges
    * Object boundaries
    * Changes in surface orientation
    * Occlusion boundaries (N/A in block world)
    * Contact edges
    * Shadow edges

<div style="text-align:center;">
    <img src="https://i.imgur.com/p5oitBK.png" width="600">
</div>


### B) Extract edges from images
* **Idea**: direction of the gradient indicates a larger variation of intensities, ie.direction is perpendicular to the edge when we are on top of an edge.
* We measure degree of variation of image $I(x,y)$, aka $$\nabla I = (\frac{\partial I}{\partial x}, \frac{\partial I}{\partial y})$$
* Since pixels are discrete, we make approximation $$\frac{\partial I}{\partial x} \backsimeq I(x,y)-I(x-1,y)$$ $$\frac{\partial I}{\partial y} \backsimeq I(x,y)-I(x,y-1)$$


### C) Other quantities to extract from image gradients
* Edge strength $$E(x,y) = |\nabla I(x,y)|$$
* Edge orientation $$\theta(x,y) = \angle \nabla I = arctan \frac{ \partial I/\partial y}{\partial I/\partial x}$$
* Unit norm vector perpendicular to an edge $$n=\frac{\nabla I}{|\nabla I|}$$


### D) Procedures
* Decide which pixels correspond to edges by thresholding the edge strength $E(x,y)$.



## 5. Edges to Surfaces
* $X(x,y) = x$, more complicated for $Y(x,y)$ and $Z(x,y)$.
* $Y(x,y)=0$ for all pixels that belong to the ground

### A) Invariant properties
* **Collinearity**: a <u>straight</u> 3D line will project into a straight line in the image.
* **Cotermination**: if two or more 3D lines <u>terminate</u> at the same point, the corresponding projections terminate at a common point
* **Intersection**: if two 3D lines <u>intersect</u> at a point, the projection will result in two intersecting lines.
* **Smoothness**: a <u>smooth</u> 3D curve will project into a smooth 2D curve.

### B) Non-accidental properties
* **Definition**: properties that will only be observed in the image if they also exist in the world or by accidental alignments between observer and scene structures.
* Under a *generic* view, nonaccidental properties will be shared by the image and the 3D world.

<div style="text-align:center;">
    <img src="https://i.imgur.com/4nr7vz9.png" width="800">
</div>

### C) Occlusion edges
* **Definition**: an occlusion boundary separates two different surfaces at different distances from the observer.
    * ie. in block world, they are boundaries between the objects and the ground.
* Object in front → own the boundary → cues about the 3D geometry to the surface that owns the boundary.
* **Note**: In block world, objects do not occlude each other.


### D) Contact edges
* **Definition**: contact edges are boundaries between two distinct objects but with no depth discontinuity but there is an occlusion (one surface is hidden behind another), and the edge is owned by one of the two surfaces.
* In block world, we assume all objects rest on the ground plane, then $Y(x,y)=0$ on the contact edges.
* In block world, only <u>horizontal</u> edges can be contact edges.



### E) Horizontal/Vertical edges
* **Idea**: formulate linear constraints in terms of $Y(x,y)$, once recovered use eqn 2.2 to recover $Z(x,y)$.
* Anything that deviates from 2D verticality by more than 15 degrees is labeled as 3D horizontal.
* **3D vertical edge** $$\frac{\partial Y}{\partial y} = 1/cos(\theta)$$
* **3D horizontal edge**
    * No change in coordinate Y thus $\partial Y/\partial t = 0$, where the vector $t$ denotes direction tangent to the edge, $t=(-n_y,n_x)$. 
    * We can write this derivative as a function of derivatives along the $x$ and $y$ image coordinates: $$\frac{\partial Y}{\partial t} = \nabla Y \cdot t = -n_y\frac{\partial Y}{\partial x} + n_x \frac{\partial Y}{\partial y}$$



### F) Constraint propagation
* Criteria to propagate information from the boundaries.
* Assume object faces are planar, then flat image regions impose these constraints on the local 3D structure:
    * $$\frac{\partial^2Y}{\partial x^2} = 0, \frac{\partial^2Y}{\partial y^2} = 0, \frac{\partial^2Y}{\partial y \partial x} = 0$$
    * $$\frac{\partial^2Y}{\partial x^2} \backsimeq 2Y(x,y) - Y(x+1,y) - Y(x-1,y)$$
    * Similarly for $\frac{\partial^2Y}{\partial y^2}$.



### G) A simple inference scheme
* Combining all the different constraints described above, we get an overdetermined system of linear equations with the form $a_iY=b_i$.
* We can solve the system of equations by minimizing cost function $J = \sum_i (a_iY-b_i)^2.$
* If some constraints are more important, we can add a weight $w_i$, $$J = \sum_i w_i(a_iY-b_i)^2.$$
* Can be written in matrix from $AY = b$.



## 6. Surfaces to Objects
* **Issue**: the system is unaware of the scene is composed of a set of distinct objects, so ie. we cannot see the occluded part, and many tasks are impossible such as counting the number of cubes.
* **Model-based scene interpretation (one solution)**
    * Have a set of predefined models of the objects, and the system tries to dicide if they are in the image or not, then recover their parameters (pose, color, etc.).
*  Recognition allows indexing properties that are not directly available in the image, ie. we can infer the shape of the invisible surfaces. 
*  Recognizing each geometric figure also implies extracting the parameters of each figure (pose, size, …).