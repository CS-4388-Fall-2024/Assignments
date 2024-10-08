# Assignment 3: Earthquake Visualization

**Due: Wednesday, October 16, 11:59pm CDT**

Visualizations incorporating geospatial data are used and analyzed in many different contexts, including navigating a city (as seen in GPS devices), commercial development, and setting governmental policy. This area also receives a significant amount of research attention. For example, the National Renewable Energy Laboratory provides a range of geospatial visualizations in its [Geospatial Data Science Applications and Visualizations](https://www.nrel.gov/gis/applications-visualizations.html), which are used for research, planning, and decision-making.

For this assignment, you will be working with data from NASA and the USGS to visualize on a globe the locations where earthquakes happened between 1905 and 2007.  Your application will be able to morph between two complementary views of the data, a 2D map view and a 3D globe view, as shown below. 

![](./images/sidebyside.png)

 In this assignment, you will learn to:

- Visualize real-world geographical data on a 3D textured globe.
- Apply textures to 3D objects.
- Algorithmically create a deforming 3D mesh and display it using vertex buffers.
- Define normal vectors and texture coordinates for a sphere.
- Convert from spherical coordinates (latitude and longitude) to 3D Cartesian coordinates.

You can try out the [instructor's implementation](https://CS-4388-Fall-2024.github.io/Assignments/Assignment-3/dist) on the course GitHub Pages.  Note that this demo also includes some additional features that are not part of the base assignment requirements, such as the earth rotation, axial tilt, and mouse interactions.

## Repository Setup

We are using GitHub classroom for submission of programming assignments.  When you accept the first assignment, you will need to select your x500 from the class roster. The system will then create a new private repository with template code that is only accessible by you, the instructor, and the TAs. You will need to create a GitHub.com account if you do not already have one. Note that this is different from the University's Github account.

**Step 1:** Create your private repository using the following link: https://classroom.github.com/a/yfOYaKKP

**Step 2:** Your repository will be added to the [GitHub course organization](https://github.com/CS-4388-Fall-2024).  You will need to open your repository on GitHub, then go to Settings->Pages and change the build and deployment source to `GitHub Actions`, as shown in the image below.  You **must** complete this step or you will not be able to to deploy the finished assignment.

**Step 3:** Check out the code to your local machine **using a git client**. If you are not familiar with using `git` from the command line, then I recommend using a GUI-based client such as [GitHub Desktop](https://desktop.github.com/), [GitKraken](https://www.gitkraken.com/), or the [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) extension for VS Code. 

Do **not** simply download the contents of your repository as a ZIP file!  This is not the right way to use version control, and you will be unable to push your local changes back to the GitHub repository. 



![GitHub screenshot](./images/github.jpg)



## Prerequisites

To work with this code, you will first need to install [Node.js 20.17.0 LTS](https://nodejs.org/en/) (or newer) and [Visual Studio Code](https://code.visualstudio.com/). 

I also recommend you install the following useful VS Code extensions:

- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) (makes source control easier)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) (static code analysis tool that can flag errors)
- [JavaScript Debugger](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) (essential for real-time debugging)
- [WebGL GLSL Editor](https://marketplace.visualstudio.com/items?itemName=raczzalan.webgl-glsl-editor) (used for programming shaders later in the course)

## Getting Started

The starter code implements the general structure that we reviewed in lecture.  After cloning your repository, you will need to set up the initial project by pulling the dependencies from the node package manager with:

```
npm install
```

This will create a `node_modules` folder in your directory and download all the dependencies needed to run the project.  Note that this folder is listed in the `.gitignore` file and should not be committed to your repository.  After that, you can compile and run a server with:

```
npm run start
```

Your program should open in a web browser automatically.  If not, you can run it by pointing your browser at `http://localhost:8080`.

## Earth and Earthquake Data

We have included multiple scaled-down versions of this Earth texture with the code distributed on the website, since you need a fairly powerful computer to render even the lowest-quality image from the NASA page. In order of decreasing quality, the following images are provided:

- earth-2k.png: 2048×1024 version of image
- earth-1k.png: 1024×512 version of image
- earth-512.png: 512×256 version of image
- earth-256.png: 256×256 version of image

The instructor's implementation uses the 2k version of the texture. If this runs slowly on your computer, you should feel free to use a lower resolution version of the image. The Earth textures are stored in a [equirectangular projection](https://en.wikipedia.org/wiki/Equirectangular_projection), which simply means that the *x* coordinate corresponds directly to longitude and the *y* coordinate corresponds directly to latitude.

The earthquake dataset contains information about the earthquake's magnitude (a measure of severity) and its longitude and latitude. This data has already been loaded into `EarthquakeRecord` objects by the `EarthquakeDatabase` class.  You will be required to display the earthquakes at the correct locations with animations through time. More information on the earthquakes is available in the data file, and if you are interested in potential wizard ideas, you can try to figure out ways to integrate additional data variables into your visualization.

## Program Requirements

You will be required to write the code that displays the Earth on screen and animates it between flat rectangular map and a three-dimensional globe. You will also need to write the code to display the earthquakes on the Earth. The starter code defines a single directional light source at position (10,10,15).  If your normals are defined correctly, you will see some brighter lighting on the top-right portion of the globe, while the bottom-left portion will appear dark.

#### Part 1: Creating the Flat Map Mesh

Complete the `Earth.createMesh()` method to draw a rectangle subdivided into multiple triangles, representing a flat map of the Earth. The flat map mesh should:

- Lie in the *xy* plane.
- Have *x* values ranging from −*π* to *π*.
- Have *y* values ranging from −*π*/2 to *π*/2.
- Be divided into *n* columns horizontally and *m* rows vertically. Note that this will produce (*n*+1)×(*m*+1) vertices and 2×*n*×*m* triangles.  For simplicity, the starter code sets both of these values using the single constant `meshResolution`.

![](./images/triangles.png)

The best way to go about this is to start with a small example and then build up from there. We recommend:

- Above is an example with 6 columns and 3 rows. Study how vertices are connected into triangles here.
- Work out the indices on paper by hand for a similar small example, say *n* = 4, *m* = 2. Draw a picture, numbering the vertices and develop your own indices array based on your picture.
- When you have the concept down, recreate your picture in code inside the `Earth.createMesh()` method.  As a demonstration, a square with 4 vertices and 2 triangles has been implemented in the starter code, but all of the values are created manually.  You should create the flat map vertices and indices with nested loops, using the `meshResolution` variable to define the number of rows and columns.  
- To check your work, try clicking on the "Debug Mode" checkbox in the app. This draws the edges of each triangle in the mesh so that you can more easily see what is going on, which can be very useful when debugging. 
- In computer graphics, vertex normals need to be defined for lighting calculations. For each vertex in the flat map, the normal should be the vector pointing directly outward towards the camera (0, 0, 1).  An example also provided in the starter code.  Note that the array of normals should be exactly the same length as the vertex array.

#### Part 2: Texturing the Mesh

In the starter code, the Earth texture is loaded and assigned to the example mesh.  However, it won't be displayed until you define the texture coordinates. We will go over this in class, but as a reminder, each vertex requires two coordinates, *u* and *v*, each of which should be a value between 0 and 1.

#### Part 3: Creating the Globe Mesh

Create a second array of vertex positions to draw the Earth as a sphere instead of a rectangle.   You will need to compute a second set of vertices and then assign them to the mesh using the `this.earthMesh.setMorphTargetVertices()` method.

- First, you should complete the `Earth.convertLatLongToSphere() ` method.  If you use this version when creating your mesh, you should end up with sphere geometry instead of a flat map. The mesh connectivity (i.e., the indices that define the triangles) and texture coordinates **do not need to change** when from moving from a plane to a sphere. Only the vertex positions need to change.
- You will need to convert latitude and longitude into the three-dimensional Cartesian coordinates of the corresponding point on the sphere, using the following formulas:
  - **x** = cos(**lat**) × sin(**lon**)
  - **y**= sin(**lat**)
  - **z** = cos(**lat**) × cos(**lon**)


- Be careful to take note that the latitude and longitude in the input file are in degrees, not radians. You can convert them to radians by multiplying by `Math.PI / 180`.

You will also need to compute a second array of normals for the globe mesh.  Because you are now rendering a sphere, you will need to compute new normals to get the lighting on the globe to look good. For each vertex, the normal should be the normalized vector pointing from the center of the sphere (the origin) to the vertex position.  The array of normals should be exactly the same length as the vertex array. You should then store the globe normals using the `this.earthMesh.setMorphTargetNormals()` method.

#### Part 4: Morphing Between the Map and Globe

Smoothly transform the mesh between the flat map and the globe based on user input. The program should start with the Earth displayed as a map. When the user changes the drop down box to "Globe," it should morph into a sphere. If the user changes it back to "Map," it should smoothly morph back to the flat plane.  A GUI event handler is already implemented that sets the value of the `globeMode` boolean variable in the Earth class accordingly. 

- To get started on this, try updating the mesh to instantly change depending on whether the `globeMode` variable is true or false.  You will need to compute a new value for `this.earthMesh.morphAlpha` in in the `Earth.update()` method, where morphAlpha=0 will display the flat map, and morphAlpha=1 will display the globe.  
- Once you have an instant transition working, try creating a smooth morph. Remember that the `update(deltaTime)` function should be used for any sort of physical simulation or animation. Inside that function, you should interpolate between the vertices for the map and for the globe based upon the elapsed time since you began the morph. Check out the `lerp()` functions described in the "Useful Math" section for help with this. 

#### Part 5: Creating the Earthquake Markers

Display on the Earth all the earthquakes that have happened within the past one year of the current visualization time.  The basic structure for this is already implemented; you just need to uncomment a line of code in the `Earth.createEarthquake()` method to start drawing earthquake markers. 

-  By default, the earthquake markers are instances of a sphere mesh.  You are free to leave them as spheres or come up with your own custom geometry.
-  The earthquake positions will initially be computed randomly.  Use the `Earth.convertLatLongToPlane()` method to obtain the earthquake positions such that they match the texture (i.e., an earthquake occurring in California must be displayed in the same location as the California of the Earth texture). One way to tell if your mapping is correct is if you see a lot of earthquakes along the "Ring of Fire" in the Pacific Ocean off the coast of Asia.
-  By default, the earthquake markers are identical.  Modify the code so that their appearance varies based on the earthquake data in some meaningful way. For example, the instructor's implementation used `lerp()` functions to adjust the radius (between some predefined minimum and maximum) and color (between yellow to red) of the marker based on its magnitude. 

#### Part 6: Morphing the Earthquake Positions

Similar to the globe geometry, you will need to compute values for the map and globe positions in the `Earth.createEarthquake()` method.  Because they are in latitude and longitude, you can use the same `Earth.convertLatLongToPlane()` and `Earth.convertLatLongToSphere()` methods as before.  

Both of these positions should be stored as member variables of the earthquake marker as a `Vector3`, so you can add code in the `Earth.animateEarthquakes()` method that updates their positions to remain synchronized with the morphing of the earth mesh. The `lerp()` functions described in the "Useful Math" section will provide a convenient way to implement this feature.

## Useful Math

Here are a few mathematical operations that are very common in graphics and may be useful for this assignment:

**Linear interpolation**: One way to blend smoothly between two values *x* and *y* (which could be reals, or vectors, or matrices, etc.) is to define a function in which the output varies continuously from *x* to *y* as a scalar parameter *alpha* goes from 0 to 1. This function is traditionally abbreviated "lerp."

- lerp(*x*, *y*, *alpha*) = *x* + *alpha* × (*y* − *x*)

Thus, for example:

- lerp(*x*, *y*, 0) = *x*

- lerp(*x*, *y*, 1) = *y*

- lerp(*x*, *y*, 0.5) = (*x* + *y*) / 2

GopherGfx provides `lerp()` functions for several of the built-in types, include scalars, vectors, and colors.  For example:

```typescript
// x, y, and alpha are numbers
result = gfx.MathUtils.lerp(x, y, alpha);

// vectors (this function modifies the calling object)
// x and y are Vector3, alpha is a number
someVector.lerp(x, y, alpha);

// colors (this function modifies the calling object)
// x and y are Color3, alpha is a number
someColor.lerp(x, y, alpha);
```

**Clamping**: A concise way to constrain a value to lie in a specified interval [*a*, *b*] is to define a function clamp(*x*, *a*, *b*) which returns *a* if *x* ≤ *a*, returns *b* if *x* ≥ *b*, and returns *x* otherwise.  GopherGfx conveniently provides a clamp function, which would be called as follows:

```typescript
x = gfx.MathUtils.clamp(x, a, b);
```

**Rescaling**: Suppose you have a value *x* in the range [*xmin*, *xmax*], and you want to find the corresponding value in [*ymin*, *ymax*]. Observe that *x* − *xmin* lies in [0, *xmax* − *xmin*], and (*x* − *xmin*)/(*xmax* − *xmin*) lies in [0, 1], so the desired value is:

- *y* = *ymin* + (*ymax* − *ymin*) × (*x* − *xmin*) / (*xmax* − *xmin*)

GopherGfx conveniently provides a rescale function, which would be called as follows:

```typescript
y = gfx.MathUtils.rescale(x, xmin, xmax, ymin, ymax);
```

## Rubric

Graded out of 20 points. 

**Part 1: Creating the Flat Map Mesh** 

- Flat map vertices computed correctly (2)
- Flat map normals computed correctly (2)

- Mesh triangle indices computed correctly (2)

**Part 2: Texturing the Mesh** 

- Mesh texture coordinates computed correctly (2)

**Part 3: Creating the Globe Mesh**

- Globe vertices computed correctly (2)

- Globe normals computed correctly (2)

**Part 4: Morphing Between the Map and Globe**

- Earth mesh smoothly interpolates between the map and globe (2)

**Part 5: Creating the Earthquake Markers**

- Flat map earthquake marker positions computed correctly (2)
- Adjust appearance of each earthquake marker based on its magnitude or some other meaningful metric (2)

**Part 6: Morphing the Earthquake Positions**

- Earthquake marker positions are smoothly interpolated between the map and globe (2)

**Submission**

- To complete the submission, you will need to update your `README.md` file, build your project, and then commit/push both your code **and** the contents of the `dist` folder to GitHub as described below.  (-1 point deduction for skipping these steps)

Note that the instructor's implementation includes extra functionality that is not part of the base assignment.  This includes animated Earth rotation, axial tilt, and mouse-based interactions. This additional functionality is optional and may be considered for the wizard bonus challenge.

## Wizard Bonus Challenge

All of the assignments in the course will include great opportunities for students to go beyond the requirements of the assignment and do cool extra work. On each assignment, you can earn **one bonus point** for implementing a meaningful new feature to your program. This should involve some original new programming, and should not just be something that can be quickly implemented by copying and slightly modifying existing code. 

There are great opportunities for extra work in this assignment. For example:

- The source website for the Earth texture (see Acknowledgements below) has images for each month of the year. You could animate between the textures based upon the current time of year.
- We have also included a height text file that contains the elevation and bathymetry data for the Earth on an 8-bit scale (0-255, with sea level at 127, and 0 and 255 representing 8 km below and above sea level, respectively. You could use this data file to visualize the shape of the Earth's surface by displacing mesh vertices along their normals. Note that you may need to increase the mesh resolution to make this look good, which may come at a performance cost.
- You could make the earth rotate, implement some mouse-based interaction, or apply an axial tilt (on average, the earth's axial tilt is about 23.4 degrees). 
- Or, come up with a completely novel idea. Creativity is encouraged!

If you do any wizardly work, please describe it in your `README.md` file so we know what to look for!

## Building and Deploying to GitHub Pages

When you have finished the assignment, you should complete the missing information in your `README.md` file. Then, will need to run the following command to generate a build:

```
npm run build
```

This compiles your TypeScript program into a JavaScript bundle that will be placed in the `dist` folder. To complete your submission, you should commit your code changes **and** the contents of the `dist` folder to your repository, and then push to GitHub. This will trigger a server-side workflow that will automatically deploy your build as a website on GitHub pages. Note that you may need to wait a minute or two for the deployment to become active.

Make sure to test everything by pointing your web browser at the GitHub pages URL for your repository:

```
https://CS-4388-Fall-2024.github.io/your-repo-name-here
```

If your program runs correctly, then you are finished! Note that the published JavaScript bundle code generated by the TypeScript compiler has been obfuscated so that it is not human-readable. So, you can feel free to send this link to other students, friends, and family to show off your work!


## Acknowledgments

Earthquake data was obtained from [USGS](http://earthquake.usgs.gov/). This data is in the public domain. Credit: U.S. Geological Survey Department of the Interior/USGS.

The Earth texture was obtained from [NASA Visible Earth](http://visibleearth.nasa.gov/view_cat.php?categoryID=1484). This data is in the public domain. Credit: NASA Earth Observatory.

The star background image was created by Jeremy Perkins on [Unsplash](https://unsplash.com/photos/uhjiu8FjnsQ).

Assignment developed by Daniel Keefe and TAs 2012, 2014.

Ported to MinGfx and additional refinements by Daniel Keefe and TAs 2018-2021.

Ported to GopherGfx and additional refinements by Evan Suma Rosenberg 2022-2024.

## License

Material for [CS 4388/CS 5388 Fall 2024](https://github.com/CS-4388-Fall-2024/Syllabus) taught by [Isayas Berhe Adhanom](https://isayasadhanom.me/) is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/). This course material has been adopted from course materials originally developed by [Evan Suma Rosenberg](https://cse.umn.edu/cs/evan-suma-rosenberg).