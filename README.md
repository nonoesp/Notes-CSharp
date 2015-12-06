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

### List.IndexOf()

```c#
foreach(String s in ListOfStrings)
{
  double i = ListOfStrings.IndexOf(s); // returns the index of the element in the list
}
```

### Print A Formatted String

```c#
string Title = "2001: A Space Odyssey"; // Define a string
int Year = 1968; // Define an integer
double Rating = 7.7; // Define a double

Print("The rating of {0} ({1}) is {2}.", Title, Year, Rating);
// Should print: The rating of 2001: A Space Odyssey (1968) is 7.7.
```

### Remove Items From A List With A Loop

Create a for loop in reverse order.

```c#
for(int i = n.LinkedBeams.Count-1; i >= 0; i--)
{
    this.RemoveBeam(n.LinkedBeams[i]);
}
```

## Visual Studio

### Shortcuts

* Show Inmediate Window `CTRL + ALT + I`

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

### From 3D coordinates (World) to 2D (Screen)

```c#
Vector4d MyPoint3D = new Vector4d(0.0, 0.0, 0.0, 1.0);
Vector4d MyPoint2D = Vector4d.Transform(MyPoint3D, c); // c is the projection matrix applied to your scene
Matrix4d pinv = c.Inverted(); // Obtain inverse of the projection matrix

GL.PushMatrix();
GL.MultMatrix(ref pinv); // Reverse the projection matrix to screen coordinates
GL.PointSize(20.0f);
GL.Begin(PrimitiveType.Points);
{
    GL.Vertex2(MyPoint2D.X, MyPoint2D.Y);
}
GL.End();
GL.PopMatrix();
```

## References

* [Grasshopper Assembly Extension](https://visualstudiogallery.msdn.microsoft.com/9e389515-0719-47b4-a466-04436b491cd6)
* [C\# Coding Conventions](https://msdn.microsoft.com/en-us/library/ff926074.aspx)

## License

Notes-OSX is licensed under the MIT license. (http://opensource.org/licenses/MIT)

## Me

I tweet at [@nonoesp](http://www.twitter.com/nonoesp) and write at [nono.ma/says](http://nono.ma/says). I would love to hear about it if you find these notes are useful. Thanks!
