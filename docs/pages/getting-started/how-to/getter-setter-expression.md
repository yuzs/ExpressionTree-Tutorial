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

## Setter

You can set the property value of a specified object by using the `PropertyInfo.SetValue` method.

```csharp
public static void SetPropertyValue<T, TValue>(this T target, Expression<Func<T, TValue>> memberLamda, TValue value)
{
    var memberSelectorExpression = memberLamda.Body as MemberExpression;
    if (memberSelectorExpression != null)
    {
        var property = memberSelectorExpression.Member as PropertyInfo;
        if (property != null)
        {
            property.SetValue(target, value, null);
        }
    }
}
```
## Getter

You can get the property value of a specified object by using the `PropertyInfo.GetValue` method.

```csharp
public static TValue GetPropertyValue<T, TValue>(this T target, Expression<Func<T, TValue>> memberLamda)
{
    var memberSelectorExpression = memberLamda.Body as MemberExpression;
    if (memberSelectorExpression != null)
    {
        var property = memberSelectorExpression.Member as PropertyInfo;
        if (property != null)
        {
            return (TValue)property.GetValue(target, null);
        }
    }

    return default(TValue);
}
```

You can set/get the property on of a specified object.

```csharp
var customer = new Employee();
customer.SetPropertyValue(c => c.Name, "John");
customer.SetPropertyValue(c => c.BirthDate, new DateTime(1990, 6, 5));

var title = customer.GetPropertyValue(c => c.Name);
var birthDate = customer.GetPropertyValue(c => c.BirthDate);
```