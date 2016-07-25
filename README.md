# Notes-CSharp

Notes of C#.

## Table of Contents

* Syntax
* Visual Studio
* OpenGL

## Syntax

### Random Number

```c#
Random r = new Random();
int myInteger = r.Next();
double myDouble = r.NextDouble(); // number from 0.00 to 1.00
```

### Even or Odd

```c#
if(num % 2 == 0) { /*Is Even*/  }
```

### Get The Length of An Array

```c#
int[] numbers = new int[8];
numbers.GetLenth(0); // Returns 8
```

### A List of Lists

```c#
List<List<Plane>> ListOfPlanes = new List<List<Plane>>();
```

### List.IndexOf()

```c#
foreach(String s in ListOfStrings)
{
  double i = ListOfStrings.IndexOf(s); // returns the index of the element in the list
}
```

### List.Sort()

Sort the contents of a List using a given parameter of objects contained in the list.

This example assumes there is a list named `PeopleList` which contains objects with an `age` property.

```c#
// Sort ASC by object age
PeopleList.Sort((s1, s2) => s1.age.CompareTo(s2.age));
// Sort DESC by object age
PeopleList.Sort((s1, s2) => s2.age.CompareTo(s1.age));
```

### Set a variable to the My Documents path.

```c#
string mydocpath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
```

### Create a string array with the lines of text

```c#
string[] lines = { "First line", "Second line", "Third line" };
```

### Print A Formatted String

```c#
string Title = "2001: A Space Odyssey"; // Define a string
int Year = 1968; // Define an integer
double Rating = 7.7; // Define a double

Print("The rating of {0} ({1}) is {2}.", Title, Year, Rating);
// Should print: The rating of 2001: A Space Odyssey (1968) is 7.7.
```

### Round Double to Certain Decimals

```c#
double Speed = 0.033314667676557;
Math.Round(Speed, 4); // returns 0.0333
```

### Using StringBuilder to Concat Numerous Strings

> For routines that perform extensive string manipulation (such as apps that modify a string numerous times in a loop), modifying a string repeatedly can exact a significant performance penalty. The alternative is to use StringBuilder, which is a mutable string class. Mutability means that once an instance of the class has been created, it can be modified by appending, removing, replacing, or inserting characters. A StringBuilder object maintains a buffer to accommodate expansions to the string. New data is appended to the buffer if room is available; otherwise, a new, larger buffer is allocated, data from the original buffer is copied to the new buffer, and the new data is then appended to the new buffer. (https://msdn.microsoft.com/en-us/library/system.text.stringbuilder(v=vs.110).aspx)

```csharp
System.Text.StringBuilder sb = new System.Text.StringBuilder();
// No need to add System.Text if you add using System.Text; on top of your file
sb.Append("This is a sentence.");
sb.Append("\nThis is another sentence.");
Console.Write(sb.ToString());
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
