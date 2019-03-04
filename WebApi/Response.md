# Response

WebApi 项目应当使用统一的数据结构进行响应。
以便客户端能够解析响应的内容，正确性等。

在 EAM 系统中，所有的 **Action** 都使用 **Infrastructure.Response** 作为响应类型，
该类型主要能够描述请求是否被正确的执行。

它还有一个子类是 **Infrastructure.Response&lt;T&gt;**，它除了能够描述请求是否被正确的执行，还能向客户端返回数据。

这两个类型还有静态方法，用于封装了对异常的操作。

使用示例:

```csharp

// 在服务器上执行一个不需要返回数据的操作
public Response Action1()
{
    return Response.Do(()=>
    {
        // some code here;
    });
}

// 在服务器上执行一个需要返回数据的操作
public Response Action2()
{
    return Response<SomeResult>.Return(()=>
    {
        SomeResult result;
        // some code here;
        return result;
    });
}

```