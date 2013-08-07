MaximaSharp
===========
MaximaSharp is a simple library that uses Maxima to perform symbolic algebra, plot functions, and do basic operations with C#'s lambda functions, expressions, and strings. Both examples and Maxima are included.

What is Maxima?
---------------
> [Maxima](http://maxima.sourceforge.net/ "Maxima") is a system for the manipulation of symbolic and numerical expressions, including differentiation, integration, Taylor series, Laplace transforms, ordinary differential equations, systems of linear equations, polynomials, and sets, lists, vectors, matrices, and tensors. Maxima yields high precision numeric results by using exact fractions, arbitrary precision integers, and variable precision floating point numbers. Maxima can plot functions and data in two and three dimensions.

Using MaximaSharp
-----------------
Given the following lambda expressions declared in C#:
```csharp
Expression<Func<double, double>> f = x => 3 * Math.Pow(x, 2) + 2 * x 
		+ Math.Pow(Math.Cos(x), 2) + Math.Pow(Math.Sin(x), 2);
Expression<Func<double, double>> g = x => 2 * x + 5 * 2;
Expression<Func<double, double, double>> h = (y, z) => 3 * y + z;
```

### Simplifying ###
Simplifying functions is easy:
```csharp
Console.WriteLine(f.Simplify());
Console.WriteLine(g.Simplify());
Console.WriteLine(h.Simplify());
// Output:
// x => (((3 * (x ^ 2)) + (2 * x)) + 1)
// x => (2 * (x + 5))
// (y, z) => (z + (3 * y))
```

### Differentiating ###
It's also possible to take the derivative of functions:
```csharp
Console.WriteLine(f.Differentiate());
Console.WriteLine(g.Differentiate());
Console.WriteLine(h.Differentiate("y"));
// Output:
// x => ((6 * x) + 2)
// x => 2
// (y, z) => 3
```

### Integrating ###
Definite and indefinite integrals can also be found:
```csharp
Console.WriteLine(f.Integrate().Simplify());
Console.WriteLine(f.Integrate(0, 2));
Console.WriteLine(g.Integrate());
Console.WriteLine(h.Integrate("y"));
// Output:
// x => (x * (((x ^ 2) + x) + 1))
// x => 14
// x => ((x ^ 2) + (10 * x))
// (y, z) => ((y * z) + ((3 * (y ^ 2)) / 2))
```

### Plotting ###
Plot functions easily with gnuplot:
```csharp
Maxima.GnuPlot(@"plot x+5*cos(x)");
Maxima.GnuPlot(@"
	set parametric 
	set pm3d depthorder hidden3d
	set isosamples 30, 20
	splot [-pi:pi][-pi:pi] cos(u)*(cos(v)+3), sin(u)*(cos(v)+3), sin(v) w pm
");
Console.ReadLine();
```
Produces the following graphs:

![Plot of x + 5 * cos(x)](http://farm3.staticflickr.com/2859/9458377507_b8deeb31a1_o.png)
![Plot of cos(u)*(cos(v)+3), sin(u)*(cos(v)+3), sin(v)](http://farm6.staticflickr.com/5321/9461158962_42356e823a_o.png)

### More stuff ###
Evaluate functions:
```csharp
Console.WriteLine(f.At(5));
Console.WriteLine(g.At(10));
// Output:
// 86
// 30
```

Perform basic operations on functions:
```csharp
Console.WriteLine(g.Plus(h));
Console.WriteLine(g.Minus(h));
Console.WriteLine(f.Times(g));
Console.WriteLine(f.Over(g));
// Output:
// (x, y, z) => (((2 * x) + 10) + ((3 * y) + z))
// (x, y, z) => (((2 * x) + 10) - ((3 * y) + z))
// x => (((((3 * Pow(x, 2)) + (2 * x)) + Pow(Cos(x), 2)) + Pow(Sin(x), 2)) * ((2 * x) + 10))
// x => (((((3 * Pow(x, 2)) + (2 * x)) + Pow(Cos(x), 2)) + Pow(Sin(x), 2)) / ((2 * x) + 10))
```

Evaluate strings with Maxima:
```csharp
Console.WriteLine(Maxima.Eval("x + 2 + 2 * x + 3 * 5"));
// Output:
// 3*x+17
```

Convert strings back into expressions:
```csharp
var expr = Maxima.ToExpression("double, double", "x", "10 * x + 5 * cos(x)") 
		as Expression<Func<double, double>>;
Console.WriteLine(expr);
Console.WriteLine(expr.Differentiate().At(0));
// Output:
// x => ((10 * x) + (5 * Cos(x)))
// 10
```
