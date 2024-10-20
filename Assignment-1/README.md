# Assignment 1: Space Minesweeper

**Due: Wednesday, September 18, 11:59pm CT**

This assignment involves creating a simple, fun game using 2D graphics. You will learn to:

- Use TypeScript and GopherGfx for some serious programming
- Work with 2D graphics coordinate systems and vectors
- Implement game update loops and event handlers
- Animate computer graphics based on user input

In general, assignments in this class are intended to be implemented in a step-by-step manner.  The rubric below lists all of the features you should implement, starting with the basic functionality and then working up to more advanced features.

You can try a [finished version of the game](https://CS-4388-Fall-2024.github.io/Assignments/Assignment-1/dist) on the course GitHub Pages. This is only a representative example, and you do not need to make your game look or play exactly the same. Your program can have a different look and feel, so long as it satisfies the requirements specified in the rubric.

## Repository Setup

We are using GitHub classroom for submission of programming assignments.  When you accept the first assignment, you will need to select your netid from the class roster. The system will then create a new private repository with template code that is only accessible by you, the instructor, and the TAs. You will need to create a GitHub.com account if you do not already have one.  Note that this is different from the University's Github account.  

**Step 1:** Create your private repository using the following link: https://classroom.github.com/a/N_w2sc8O

**Step 2:** Your repository will be added to the [GitHub course organization](https://github.com/CS-4388-Fall-2024).  You will need to open your repository on GitHub, then go to Settings->Pages and change the build and deployment source to `GitHub Actions`, as shown in the image below.  You **must** complete this step or you will not be able to to deploy the finished assignment.

**Step 3:** Check out the code to your local machine. If you are not familiar with using `git` from the command line, then I recommend using a GUI such as [GitHub Desktop](https://desktop.github.com/) or [GitKraken](https://www.gitkraken.com/). 



![GitHub screenshot](./images/github.jpg)



## Prerequisites

To work with this code, you will first need to install [Node.js 20.11.0 LTS](https://nodejs.org/en/) (or newer) and [Visual Studio Code](https://code.visualstudio.com/). 

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

## Rubric

Graded out of 20 points.

This assignment is divided into seven parts.  The provided starter code implements the complete structure of the game and has extensive comments throughout.  The locations to add your code for each of the requirements listed below is marked with `ADD YOUR CODE HERE`.  Note that you do **not** need to make any changes to the existing code, nor do you need to add any code outside of the areas marked in the comments.  However, you should feel free to change the existing code if you want to do something fun or go beyond the requirements of the assignment.

#### Part 1: Star Movement

- The starter code includes a ship that rotate to point towards the mouse cursor.  In class, we made the ship move towards the position of the mouse.  However, this time we want the ship to always be at the center of the screen.  Therefore, instead of moving the ship, you should move the stars in the opposite direction, thereby creating the *illusion* of ship movement. (2)
- To create a cool effect, we can make the velocity of each star dependent on its size, so that smaller stars appear to move slower than bigger stars.  This creates a depth illusion is known as [parallax](https://en.wikipedia.org/wiki/Parallax).  You can achieve this by considering the size of each star when computing its velocity. (2)

#### Part 2: Mine Movement

- By default, the mines are programmed to slowly move towards the ship.  In order to complete the illusion of the ship flying through through the scene, you will also need to additionally move each mine in the opposite of the ship's direction, similar to the way you moved the stars. (1)
- Next, add some slow rotation to each mine so they appear to spin. (1)

#### Part 3: Laser Spawning

- When the user clicks the mouse, a new instance of the laser object should be added to the scene. (2)

 The comments in the code reference the ShapeInstance class, which was deprecated in GopherGfx. For this part, you can just create a new Mesh2 object for each laser. Alternatively, you could create a single Mesh2 object to use as a template, and then create multiple instances (copies) by calling the createInstance() method.*

- When the laser is created, it should be rotated to point towards the mouse cursor, similar to the way the ship was pointed in that direction. (1)

#### Part 4: Laser Movement

- In the update method, each laser instance should be translated forward so that it appears to be shooting out from the ship. (2)
- When a laser moves outside the boundary of the window, it should be removed from the scene.  We don't want an infinite number of lasers that can slow down our game! (2)

#### Part 5: Mine Collisions

- Complete the code in the `checkForMineCollisions()` method to test for mine-to-mine collisions. (2)  
- When two mines intersect, it should cause them both to explode. (1)

#### Part 6: Laser Collisions

- Complete the code in the `checkForLaserCollisions()` method to test for laser-to-mine collisions (2). 
- When an intersection occurs, the mine should explode and the laser should be removed from the scene. (1)

#### Part 7: Build and Deploy

- To complete the submission, you will need to update your `README.md` file, then build and deploy your project to GitHub Pages as described below. (1)

## Wizard Bonus Challenge

All of the assignments in the course will include great opportunities for students to go beyond the requirements of the assignment and do cool extra work. On each assignment, you can earn **one bonus point** for implementing a meaningful new feature to your program. This should involve some **original new programming**, and should not just be something that can be quickly implemented by copying and slightly modifying existing code.  

For example, in this assignment, you could potentially implement the logic to turn this into a fully functional game.  This could involve making the mines capable of destroying the player's ship or providing an objective to "win" the game.  Alternatively, you could program a new type of enemy or weapon, possibly by importing new assets.  [Kenney](https://www.kenney.nl/assets) is a great source for free assets to use in game development. Or, even better, think of your own cool, creative idea!

A single point may not sound like a lot, but keep in mind that on a 20-point scale, this is equivalent to a 5% bonus! Make sure to document your wizard functionality in the Submission Information portion of this readme file, so that the TAs know what to look for when they grade your program.

The wizard bonus challenge also offers you a chance to show off your skills and creativity!  While grading the assignments the TAs will identify the best four or five examples of people doing cool stuff with computer graphics. We call these students our **wizards**, and after each assignment, the students selected as wizards will have their programs demonstrated to the class.

## Building and Deploying to GitHub Pages

When you have finished the assignment, you should complete the missing information in your `README.md` file. Then, will need to run the following command to generate a build:

```
npm run build
```

This compiles your TypeScript program into a JavaScript bundle that will be placed in the `dist` folder. To complete your submission, you should commit the contents of the `dist` folder and then push to GitHub. This will trigger a server-side workflow that will automatically deploy your build as a website on GitHub pages. Note that you may need to wait a minute or two for the deployment to become active.

Make sure to test everything by pointing your web browser at the GitHub pages URL for your repository:

```
https://CS-4388-Fall-2024.github.io/your-repo-name-here
```

If your program runs correctly, then you are finished! Note that the published JavaScript bundle code generated by the TypeScript compiler has been obfuscated so that it is not human-readable. So, you can feel free to send this link to other students, friends, and family to show off your work!

If you have deployed your build, but your GitHub Pages URL is not working, then you may need to follow one more step to activate it (see screenshots below). First, you will need to go to the Actions tab and click the button to enable workflows. Then, click on "Deploy static content to pages" and then run the workflow manually. After doing this once, any subsequent builds that you push in the future will be deployed automatically.

![GitHub screenshot](./images/workflow1.jpg)

![GitHub screenshot](./images/workflow2.jpg)

## Acknowledgments

The ship graphics were from the Kenney [Space Shooter Redux](https://www.kenney.nl/assets/space-shooter-redux) and [Simple Space](https://www.kenney.nl/assets/simple-space) asset packages. The laser sound effect was obtained from [ZapSplat](https://www.zapsplat.com/).

## License

Material for [CS 4388/CS 5388 Fall 2024](https://github.com/CS-4388-Fall-2024/Syllabus) taught by [Isayas Berhe Adhanom](https://isayasadhanom.me/) is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/). This course material has been adopted from course materials originally developed by [Evan Suma Rosenberg](https://cse.umn.edu/cs/evan-suma-rosenberg).