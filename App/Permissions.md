# 授权

EAM 系统中，使用 Permission(授权) 作为权限控制的最小粒度。
用户组则是 Permission 的集合。
而最终用户与用户组进行关联，从而得到所有的授权。

授权的本质是一个关键字。

## 创建授权

授权不通过UI进行创建。
因为授权会与服务端代码深度耦合。
因此授权是通过代码创建的。

创建方法
1. 创建一个类型，实现 **Dexi.EAM.App.Permissions.IPermissionProvider** 接口。
2. 通过接口中的 **Provide** 方法，向参数中添加需要创建的授权即可

```csharp
public class MyPermissionProvider : IPermissionProvider
{
    public void Provide(IList<Permission> permissions)
    {
        permissions.Add(new Permission("TestPermission","测试授权的名称","测试授权的描述"));
    }
}
```

> 所有的授权提供器一律使用 PermissionProvider 结尾

## 使用授权

目前，我们对授权的审核和过滤，都能过在 MVC 或 WebApi 的 Action 上添加 Filter 来实现的。

### MVC 项目

* Dexi.EAM.Mvc.Filters.HasAllPermissions 表示请求该接口时，用户必须具备所有的授权
* Dexi.EAM.Mvc.Filters.HasAnyPermission 表示请求该接口时，用户只要具备指定的授权中的一任何一个就能通过
* Dexi.EAM.Mvc.Filters.HasPermission 表示请求该接口时，用户具备指定的一个授权

### WebApi 项目

尚未实现相应的 Filter