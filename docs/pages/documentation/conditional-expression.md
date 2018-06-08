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

## IfThen

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

## IfThenElse

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

## Switch 

The `Expression.Switch` creates a `SwitchExpression` that represents a switch statement that has a default case. 

 - All SwitchCase objects in a SwitchExpression object must have the same type unless the SwitchExpression has the type void.
 - Each SwitchCase object has an implicit break statement, which means that there is no implicit fall through from one case label to another.
 - If switchValue does not match any of the cases, the default case represented by default body is run.

```csharp
ConstantExpression switchValue = Expression.Constant(4);

MethodCallExpression defaultCase = Expression.Call(null, 
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }), 
    Expression.Constant("Default Case"));

MethodCallExpression case1 = Expression.Call(null,
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
    Expression.Constant("First Case"));

MethodCallExpression case2 = Expression.Call(null,
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
    Expression.Constant("Second Case"));

MethodCallExpression case3 = Expression.Call(null,
    typeof(Console).GetMethod("WriteLine", new Type[] { typeof(String) }),
    Expression.Constant("Third Case"));

// This expression represents a switch statement 
// that has a default case.
SwitchExpression switchExpr =
    Expression.Switch(
        switchValue,
        defaultCase,
        new SwitchCase[] 
        {
            Expression.SwitchCase(case1,Expression.Constant(1)),
            Expression.SwitchCase(case2,Expression.Constant(2)),
            Expression.SwitchCase(case3,Expression.Constant(3))
        }
    );

Expression.Lambda<Action>(switchExpr).Compile()();
```