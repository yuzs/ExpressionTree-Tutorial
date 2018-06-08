# Getter/Setter Expression

Properties enable a class to expose a public way of getting and setting values, while hiding implementation or verification code.

 - A get property accessor is used to return the property value.
 - And a set property accessor is used to assign a new value. 

You can also get/set the property using Expression API. Here is a simple `Employee` class which contains two properties.

```csharp
public class Employee
{
    public string Name { get; set; }
    public DateTime BirthDate { get; set; }
}
```

The following class contains implementation for getter/setter using expression APIs

```csharp
public static class ExpressionUtils
{
    public static Action<TEntity, TProperty> CreateSetter<TEntity, TProperty>(Expression<Func<TEntity, TProperty>> property)
    {
        PropertyInfo propertyInfo = ExpressionUtils.GetProperty(property);

        ParameterExpression instance = Expression.Parameter(typeof(TEntity), "instance");
        ParameterExpression parameter = Expression.Parameter(typeof(TProperty), "param");

        var body = Expression.Call(instance, propertyInfo.GetSetMethod(), parameter);
        var parameters = new ParameterExpression[] { instance, parameter };

        return Expression.Lambda<Action<TEntity, TProperty>>(body, parameters).Compile();
    }

    public static Func<TEntity, TProperty> CreateGetter<TEntity, TProperty>(Expression<Func<TEntity, TProperty>> property)
    {
        PropertyInfo propertyInfo = ExpressionUtils.GetProperty(property);

        ParameterExpression instance = Expression.Parameter(typeof(TEntity), "instance");

        var body = Expression.Call(instance, propertyInfo.GetGetMethod());
        var parameters = new ParameterExpression[] { instance };

        return Expression.Lambda<Func<TEntity, TProperty>>(body, parameters).Compile();
    }

    public static PropertyInfo GetProperty<TEntity, TProperty>(Expression<Func<TEntity, TProperty>> expression)
    {
        var member = GetMemberExpression(expression).Member;
        var property = member as PropertyInfo;
        if (property == null)
        {
            throw new InvalidOperationException(string.Format("Member with Name '{0}' is not a property.", member.Name));
        }
        return property;
    }

    private static MemberExpression GetMemberExpression<TEntity, TProperty>(Expression<Func<TEntity, TProperty>> expression)
    {
        MemberExpression memberExpression = null;
        if (expression.Body.NodeType == ExpressionType.Convert)
        {
            var body = (UnaryExpression)expression.Body;
            memberExpression = body.Operand as MemberExpression;
        }
        else if (expression.Body.NodeType == ExpressionType.MemberAccess)
        {
            memberExpression = expression.Body as MemberExpression;
        }

        if (memberExpression == null)
        {
            throw new ArgumentException("Not a member access", "expression");
        }

        return memberExpression;
    }
}
```
## Setter

You can set the property value of a specified object by using the `CreateSetter` method.

```csharp
var setterNameProperty = ExpressionUtils.CreateSetter<Employee, string>(x => x.Name);
var setterBirthDateProperty = ExpressionUtils.CreateSetter<Employee, DateTime>(x => x.BirthDate);

Employee emp = new Employee();

setterNameProperty(emp, "John");
setterBirthDateProperty(emp, new DateTime(1990, 6, 5));

Console.WriteLine("Name: {0}, DOB: {1}", emp.Name, emp.BirthDate);
```

## Getter

You can get the property value of a specified object by using the `CreateGetter` method.

```csharp
var getterNameProperty = ExpressionUtils.CreateGetter<Employee, string>(x => x.Name);
var getterBirthDateProperty = ExpressionUtils.CreateGetter<Employee, DateTime>(x => x.BirthDate);

Employee emp1 = new Employee()
{
    Name = "John",
    BirthDate = new DateTime(1990, 6, 5)
};

var name = getterNameProperty(emp1);
var birthDate = getterBirthDateProperty(emp1);

Console.WriteLine("Name: {0}, DOB: {1}", name, birthDate);
```
