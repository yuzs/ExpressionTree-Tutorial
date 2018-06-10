# IfThenElse

The `Expression.IfThenElse` method creates a `ConditionalExpression` that represents a conditional block with if and else statements.

```csharp
int num = 60;

Expression testCondition = Expression.Constant(num > 10);

MethodCallExpression ifTrueBlock = Expression.Call(null,
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
    Expression.Constant("num is greater than 10"));

MethodCallExpression ifFalseBlock = Expression.Call(null,
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
    Expression.Constant("num is less than 10"));

Expression ifThenElseExpr = Expression.IfThenElse(
    testCondition, 
    ifTrueBlock, 
    ifFalseBlock);

Expression.Lambda<Action>(ifThenElseExpr).Compile()();
```

This example shows how to create an expression that represents an if-else block.

