# ExclusiveOr

You can build an expression tree which contains a bitwise **XOR** operation using `Expression.ExclusiveOr` method. For example, you have the following code.

```csharp
var value = 5 ^ 3;
Console.WriteLine(value);
```

Here is the code that is required to build the same functionality using expression tree. 

```csharp
Expression expr = Expression.ExclusiveOr(
    Expression.Constant(5),
	Expression.Constant(3)
);

Console.WriteLine(Expression.Lambda<Func<int>>(expr).Compile()());
```

[Try it online](https://dotnetfiddle.net/GKSkuj)