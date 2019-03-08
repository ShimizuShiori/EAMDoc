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