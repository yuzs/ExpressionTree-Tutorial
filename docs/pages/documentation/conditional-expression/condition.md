# Conditional Expression

Conditional Expression represents an expression that has a conditional operator. 

## Condition

You can use the `Expression.Condition` factory method to create a `ConditionalExpression`.

```csharp
int num = 60;
 
Expression conditionExpr = Expression.Condition(
    Expression.Constant(num > 10),
    Expression.Constant("num is greater than 10"),
    Expression.Constant("num is smaller than 10")
    );

// The following statement first creates an expression tree,
// then compiles it and then executes it.       
Console.WriteLine(Expression.Lambda<Func<string>>(conditionExpr).Compile()());
```

The Condition method evaluates the first expression which is the test condition and if it is true, the second expression will be executed otherwise the third expression will be executed. 

