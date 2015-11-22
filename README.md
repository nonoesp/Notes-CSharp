# Notes-CSharp

Notes of C#.

## Table of Contents

* Syntax
* Visual Studio
* OpenGL

## Syntax

### Get The Length of An Array

```c#
int[] numbers = new int[8];
numbers.GetLenth(0); // Returns 8
```

### Print A Formatted String

```c#
string Title = "2001: A Space Odyssey"; // Define a string
int Year = 1968; // Define an integer
double Rating = 7.7; // Define a double

Print("The rating of {0} ({1}) is {2}.", Title, Year, Rating);
// Should print: The rating of 2001: A Space Odyssey (1968) is 7.7.
```

## Visual Studio

### Get The Mouse Position

Depending on the context, we may be willing to get coordinates of the cursor relative to a given window, a control, or relative to the whole screen.

The following method is an event that gets fired whenever you move the mouse on the screen, not necessarily on top of your application's window.

```C#
private void button1_MouseMove(object sender, MouseEventArgs e)
{
    double X = e.Location.X;
    double Y = e.Location.Y;
}
```

Also, the location of the mouse on the screen can be retrieved. There are methods available to both convert screen points to a control and to convert control points to screen coordinates.

```C#
// Methods
Point Control.PointToClient(Point point);
Point Control.PointToScreen(Point point);

// Usage
Point ClientPoint = myControl.PointToClient(ScreenPoint);
Point ScreenPoint = myControl.PointToScreen(ClientPoint);
```

## OpenGL (Visual Studio/C#)

Using the OpenTK library with C# to draw.

### References

* [Picking and Using Selection](https://www.opengl.org/archives/resources/faq/technical/selection.htm)
* [Build A Windows.Forms+GLControl Based App](http://www.opentk.com/doc/chapter/2/glcontrol)
* [OpenTK Examples](https://github.com/andykorth/opentk/tree/master/Source/Examples/OpenGL/1.x)
* [OpenTK Examples: Picking](https://github.com/andykorth/opentk/blob/master/Source/Examples/OpenGL/1.x/Picking.cs)
* [OpenTK Examples: Stencil Buffer](https://github.com/andykorth/opentk/blob/master/Source/Examples/OpenGL/1.x/StencilCSG.cs)

### Change The Scale of The Graphics

```c#
GL.Scale(0.5f, 0.5f, 0.5f); // GL.Scale(double x, double y, double z);
```

### Draw Lines

```C#
GL.Begin(PrimitiveType.Lines);
  GL.Vertex2(200.0, 200.0); // Start Point of first line
  GL.Vertex2(200.0, 250.0); // End Point of first line

  GL.Vertex2(200.0, 250.0); // Start Point of second line
  GL.Vertex2(250.0, 250.0); // End Point of second line
GL.End();
```

### Draw A Polygon

Polygons are closed and filled by default.

```C#
GL.Begin(PrimitiveType.Polygon);
  GL.Vertex2(200.0, 200.0);
  GL.Vertex2(200.0, 250.0);
  GL.Vertex2(250.0, 250.0);
  GL.Vertex2(250.0, 200.0);
GL.End();
```

## OpenGL with Cocoa (Xcode/Objective-C)

Even though these notes are not C#, I will keep them here until this section grows enough to create a single repository of OpenGL notes.

### References

* [Introduction to GLKit](https://developer.apple.com/library/ios/documentation/GLkit/Reference/GLKit_Collection/index.html#//apple_ref/doc/uid/TP40010915)
* [Drawing with OpenGL and GLKit](https://developer.apple.com/library/ios/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/DrawingWithOpenGLES/DrawingWithOpenGLES.html)
* [CocoaGL Tutorial](https://github.com/beelsebob/Cocoa-GL-Tutorial)
* [Build A Windows.Forms+GLControl Based App](http://www.opentk.com/doc/chapter/2/glcontrol)

### [OSX] Subclassing NSOpenGLView

Create a new OS X Cocoa Project with a single View Controller. In the NIB file, drag an NSOpenGLView to your View Controller.

Now, go and add a new Cocoa class to your project, which should be subclassing NSOpenGLView. Call it something like MyNSOpenGLView, CustomNSOpenGLView, or even GraphicsView—you will need to change the class of the NSOpenGLView inside your NIB file to the class name you selected for your NSOpenGLView subclass. We will stick with CustomNSOpenGLView for now.

In the CustomNSOpenGLView.m file, of your class, import the following:

```objc
#import "CustomNSOpenGLView.h"
#import <OpenGL/gl.h> // import OpenGL

@implementation CustomNSOpenGLView
```

Everything should be working properly. Now, we just need to add some setup and drawing code in our class’ `drawRect:` method (which we need to add). *This code is from Apple’s [reference](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Conceptual/OpenGL-MacProgGuide/opengl_drawing/opengl_drawing.html).*

```objc
-(void) drawRect: (NSRect) bounds
{
    glClearColor(0, 0, 0, 0);
    glClear(GL_COLOR_BUFFER_BIT);
    drawAnObject();
    glFlush();
}

// Draw a triangle
static void drawAnObject ()
{
    glColor3f(1.0f, 0.85f, 0.35f);
    glBegin(GL_TRIANGLES);
    {
        glVertex3f(  0.0,  0.6, 0.0);
        glVertex3f( -0.2, -0.3, 0.0);
        glVertex3f(  0.2, -0.3 ,0.0);
    }
    glEnd();
}
```

### Applying Transformation Matrices

There are two different approaches I have managed to apply transformations in order to modify how objects are displayed:

* Multiply a 4x4 matrix (GLKMatrix4) by a column vector (GLKVector3) to obtain the transformed point.
* Multiplying a 4x4 matrix (GLKMatrix4) to the current glMatrixMode(GL_MODELVIEW) after loading the identity matrix.

#### Multiply GLKMatrix4 by a GLKVector3

```objc
// Define a sample vector to draw a point
GLKVector3 point = GLKVector3Make(0.0f, 0.0f, 0.5f);
// Create a transformation matrix (a translation, for instance)
GLKMatrix4 translate = GLKMatrix4MakeTranslate(1.0f, 0.5f, 0.0f);
// Multiply the transformation matrix by the column vector
GLKVector3 point_t = GLKMatrix4MultiplyAndProjectVector3(translate, point);
// Now our point_t has been translated by (1.0f, 0.5f, 0.0f)
// It's coordinates should now be (1.0f, 0.5f, 0.5f);
```

#### Multiply the Model View Matrix by a GLKMatrix4

```objc
// Create a transformation matrix (a translation, for instance)
GLKMatrix4 translate = GLKMatrix4MakeTranslate(1.0f, 0.5f, 0.0f);
// Set the matrix mode to model view
glMatrixMode(GL_MODELVIEW);
// Load the identity matrix
glLoadIdentity();
// Multiply the current matrix by our transformation
glMultMatrixf(translate.m);
```

#### Set An Isometric View

The following code works, but for some reason the Z axis seems to be scaled down. If you discover why, please let me know =)!

```objc
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
glOrtho(0.0, 0.0, self.bounds.size.width, self.bounds.size.height, -100.0, 100.0);

glMatrixMode(GL_MODELVIEW);
glLoadIdentity();
glRotatef(-45.0, 1.0, 0.0, 0.0); // Rotate around Z
glRotatef(-45.0, 0.0, 0.0, 1.0); // Rotate around Y

// Draw here
```

## Math

### References

* [Constructing The LookAt() Matrix Directly](http://www.cs.virginia.edu/~gfx/Courses/1999/intro.fall99.html/lookat.html)
* [Perspective Projection](http://ogldev.org/www/tutorial12/tutorial12.html)

## References

* [Grasshopper Assembly Extension](https://visualstudiogallery.msdn.microsoft.com/9e389515-0719-47b4-a466-04436b491cd6)
* [C\# Coding Conventions](https://msdn.microsoft.com/en-us/library/ff926074.aspx)
