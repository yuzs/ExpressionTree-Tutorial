# IfThen

The `Expression.IfThen` method creates a `ConditionalExpression` that represents a conditional block with an if statement.

```csharp
int num = 60;

Expression testCondition = Expression.Constant(num > 10);

MethodCallExpression ifTrueBlock = Expression.Call(null,
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
    Expression.Constant("num is greater than 10"));

Expression ifThenExpr = Expression.IfThen(testCondition, ifTrueBlock);

Expression.Lambda<Action>(ifThenExpr).Compile()();
```

This example shows how to create an expression that represents an if block.

