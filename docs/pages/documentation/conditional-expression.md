# Conditional Expression

Conditional Expression represents an expression that has a conditional operator. 

## Condition

You can use the `Condition` factory method to create a `ConditionalExpression`.

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

## IfThen

The `IfThen` method creates a `ConditionalExpression` that represents a conditional block with an if statement.

```csharp
int num = 60;

Expression ifThenExpr = Expression.IfThen(
    Expression.Constant(num > 10),
    Expression.Call(
            null,
            typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
            Expression.Constant("num is greater than 10")
           )
    );

Expression.Lambda<Action>(ifThenExpr).Compile()();
```

This example shows how to create an expression that represents an if block.

## IfThenElse

The `IfThenElse` method creates a `ConditionalExpression` that represents a conditional block with if and else statements.

```csharp
int num = 60;
Expression ifThenElseExpr = Expression.IfThenElse(
    Expression.Constant(num > 170),
    Expression.Call(
            null,
            typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
            Expression.Constant("num is greater than 10")
        ),
        Expression.Call(
            null,
            typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
            Expression.Constant("num is less than 10")
        )
    );

Expression.Lambda<Action>(ifThenElseExpr).Compile()();
```

This example shows how to create an expression that represents an if-else block.