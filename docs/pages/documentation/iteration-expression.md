# Iteration Expression 

Iteration Expression or loop expression represents an infinite loop, and it can be exited with **break**.

The following example demonstrates how to create a block expression that contains a LoopExpression object by using the Loop method.

```csharp
ParameterExpression value = Expression.Parameter(typeof(int), "value");

// Creating an expression to hold a local variable. 
ParameterExpression result = Expression.Parameter(typeof(int), "result");

// Creating a label to jump to from a loop.
LabelTarget label = Expression.Label(typeof(int));

ConditionalExpression ifThenElseExpr = Expression.IfThenElse(
    Expression.GreaterThan(value, Expression.Constant(1)),
    Expression.MultiplyAssign(result,
        Expression.PostDecrementAssign(value)),
    Expression.Break(label, result)
);

LoopExpression loop = Expression.Loop(ifThenElseExpr, label);

// Creating a method body.
BlockExpression block = Expression.Block(
    new[] { result },
    Expression.Assign(result, Expression.Constant(1)),
    loop
);

// Compile and run an expression tree.
var factorial = Expression.Lambda<Func<int, int>>(block, value).Compile();

Console.WriteLine(factorial(6));
```

You aren't limited in what you can build using Expression APIs. However, the more complicated expression tree that you want to build, the more difficult the code is to manage and to read.

## While

You can build an expression tree to perform a while loop by using `Expression.Loop` method. Let's say you have the following code.

```csharp
var res = 1;
while (n > 1)
{
    res = res * n;
    n--;
}
return res;
```

Here is the code that is required to build the same functionality. 

 - It is complicated because there isn't any API to build a while loop
 - You need to build a loop that contains a conditional test, and a target label to break out of the loop.

```csharp
var nArgument = Expression.Parameter(typeof(int), "n");
var result = Expression.Variable(typeof(int), "result");

// Creating a label that represents the return value
LabelTarget label = Expression.Label(typeof(int));

var initializeResult = Expression.Assign(result, Expression.Constant(1));

// This is the inner block that performs the multiplication,
// and decrements the value of 'n'
var block = Expression.Block(
    Expression.Assign(result,
        Expression.Multiply(result, nArgument)),
    Expression.PostDecrementAssign(nArgument)
);

// Creating a method body.
BlockExpression body = Expression.Block(
    new[] { result },
    initializeResult,
    Expression.Loop(
        Expression.IfThenElse(
            Expression.GreaterThan(nArgument, Expression.Constant(1)),
            block,
            Expression.Break(label, result)
        ),
        label
    )
);
```

## Foreach

You can build an expression tree to perform a foreach loop by using `Expression.Loop` method.

```csharp
public static Expression ForEach(Expression collection, ParameterExpression loopVar, Expression loopContent)
{
    var elementType = loopVar.Type;
    var enumerableType = typeof(IEnumerable<>).MakeGenericType(elementType);
    var enumeratorType = typeof(IEnumerator<>).MakeGenericType(elementType);

    var enumeratorVar = Expression.Variable(enumeratorType, "enumerator");
    var getEnumeratorCall = Expression.Call(collection, enumerableType.GetMethod("GetEnumerator"));
    var enumeratorAssign = Expression.Assign(enumeratorVar, getEnumeratorCall);

    var moveNextCall = Expression.Call(enumeratorVar, typeof(IEnumerator).GetMethod("MoveNext"));

    var breakLabel = Expression.Label("LoopBreak");

    var ifThenElseExpr = Expression.IfThenElse(
        Expression.Equal(moveNextCall, Expression.Constant(true)),
        Expression.Block(new[] { loopVar },
            Expression.Assign(loopVar, Expression.Property(enumeratorVar, "Current")),
            loopContent
        ),
        Expression.Break(breakLabel)
    );

    var loop = Expression.Loop(ifThenElseExpr, breakLabel);

    var block = Expression.Block(new[] { enumeratorVar },
        enumeratorAssign,
        loop
    );

    return block;
}
```

This function can be easily used as shown below.

```csharp
var list = new List<int>() { 10, 20, 30 };
var collection = Expression.Parameter(typeof(List<int>), "collection");
var loopVar = Expression.Parameter(typeof(int), "loopVar");
var loopBody = Expression.Call(typeof(Console).GetMethod("WriteLine", new[] { typeof(int) }), loopVar);
var loop = ForEach(collection, loopVar, loopBody);
var results = Expression.Lambda<Action<List<int>>>(loop, collection).Compile();
results(list);
```

## For

A similar method for generating 'for' loops.

```csharp
public static Expression For(ParameterExpression loopVar, Expression initValue, Expression condition, Expression increment, Expression loopContent)
{
    var initAssign = Expression.Assign(loopVar, initValue);

    var breakLabel = Expression.Label("LoopBreak");

    var ifThenElseExpr = Expression.IfThenElse(
        condition,
        Expression.Block(
            loopContent,
            increment
        ),
        Expression.Break(breakLabel)
    );

    var loop = Expression.Loop(ifThenElseExpr, breakLabel);

    var block = Expression.Block(new[] { loopVar },
        initAssign,
        loop
    );

    return block;
}
```