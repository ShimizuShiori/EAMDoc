# Token

Token 是使用 WebApi 时的身份标识。
它会在成功登录后，响应给客户端。

## 前端

客户端应当在每个请求中都以 token= 的方式加在 url 后。

```javascript
$.get("api/controller/action?token=abcdefg", function(rlt{
    // some code here
});
```

## 后端

### 认证

对待需要认证身份的请求，我们都需要校验它的 Token 是否正确。

我们可以使用过滤器 **Dexi.EAM.WebApi.Filters.TokenCheckerAttribute** 来完成这项工作。

* 将 **Dexi.EAM.WebApi.Filters.TokenCheckerAttribute** 定义在 Action 上可以指定对该 Action 进行 Token 校验。
* 将 **Dexi.EAM.WebApi.Filters.TokenCheckerAttribute** 定义在 Controller 上可以指定对该 Controller 中的所有 Action 进行 Token 校验。

### 获取 Token 对应的人

若一个控制器中，只有个 Action 法需要获取 Token 对应的人。
首先，我们在这些 Action 上添加 **Dexi.EAM.WebApi.Filters.TokenCheckerAttribute** , 然后在参数位上添加 **Dexi.EAM.App.SSO.UserAuthSession** 作为入参即可

```csharp
public class SomeController : ApiController
{
    [TokenChecker]
    [HttpGet]
    [Router("router")]
    public Response DoSomething(UserAuthSession user)
    {
        // some code here;
    }
}
```

若我们的控制中的每个方法都需要访问当前登录人，则可以通过修改基类就实现了。
此时，不需要再添加过滤器了。
```csharp
public class SomeController : TokenCheckedApiController
{
    [Router("router")]
    public Response DoSomething()
    {
        var userId = this.CurrentUser.Id;
    }

    [Router("router2")]
    public Response DoSomething2()
    {
        var userId = this.CurrentUser.Id;
    }
}
```
