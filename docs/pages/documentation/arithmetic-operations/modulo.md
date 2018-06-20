# Modulo

You can build an expression tree which contains an arithmetic remainder operation using `Expression.Modulo` method. For example, you have the following code.

```csharp
int sum = 6 % 5;
Console.WriteLine(sum);
```

Here is the code that is required to build the same functionality using expression tree. 

```csharp
Expression sumExpr = Expression.Modulo (
    Expression.Constant(6),
    Expression.Constant(5)
);

Console.WriteLine(Expression.Lambda<Func<int>>(sumExpr).Compile()());
```

[Try it online](https://dotnetfiddle.net/C9LKEQ)