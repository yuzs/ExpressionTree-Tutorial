# Method Access Expression

You can access any method by using `Expression.Call` which creates a MethodCallExpression that represents a call to a method.

In this example, `string.ToUpper()` is called using an expression to convert a string to an upper case.

```csharp
string text = "This is simple text.";
Expression callExpr = Expression.Call(
Expression.Constant(text), typeof(String).GetMethod("ToUpper", new Type[] { }));

// Print out the expression.
Console.WriteLine(callExpr.ToString());

// The following statement first creates an expression tree,
// then compiles it and then executes it.  
Console.WriteLine(Expression.Lambda<Func<String>>(callExpr).Compile()());
```

The output of the above code.

```csharp
"This is simple text.".ToUpper()
THIS IS SIMPLE TEXT.
```

You can also call your custom methods using `Expression.Call`. For example, we have defined the following function which takes two integer arguments and returns the sum of them.

```csharp 
public class MyClass
{
    public static int MyFunc(int a, int b)
    {
        return a + b;
    }
}
```

The direct way to call this function is straightforward.

```csharp 
MyClass.MyFunc(1,2);
```

It will return **3** as an output. Now to access this function using expression API, we can use the Expression.Call`.

```csharp
int param1 = 1;
int param2 = 2;
var c1 = Expression.Constant(param1);
var c2 = Expression.Constant(param2);
var expr = Expression.Call(typeof(Program).GetMethod("MyFunc"), c1, c2);
Func<int> func = Expression.Lambda<Func<int>>(expr).Compile();

Console.WriteLine(func.Invoke());
```