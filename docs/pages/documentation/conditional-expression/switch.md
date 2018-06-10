# Switch 

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