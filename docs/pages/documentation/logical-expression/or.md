# Or

You can build an expression tree which contains a0 bitwise **OR** operation using `Expression.Equal` method. For example, you have the following code.

```csharp
bool value = true || false;
Console.WriteLine(value);
```

Here is the code that is required to build the same functionality using expression tree. 

```csharp
Expression greaterThanExpr = Expression.Or(
    Expression.Constant(true),
    Expression.Constant(false)
);

Console.WriteLine(Expression.Lambda<Func<bool>>(greaterThanExpr).Compile()());
```

[Try it online](https://dotnetfiddle.net/k3fxRS)