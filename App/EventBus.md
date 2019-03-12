# 事件总线

事件，是解耦信号来源和信号处理两方的方法。
也就是设计模式中的观测者模式。

在 **EAM** 系统中，我们可以通过事件总线的方式，拆解两个业务单元的信号来源和信息处理。

比如：
当维修单完成后，向 ITS 发起一个请求。

事件总线有三个概念

* 事件处理器类型
* 事件名称
* 事件参数

## 1 事件处理器定义

### 1.1 事件处理器

所有的事件处理器都应当是一个接口，并且继承于 IEventHandler

```csharp
// 与应用程序相关的事件处理器
public interface IApplicationEventHandler : IEventHandler
{

}
```

### 1.2 事件名称

一个事件处理器接口上可以定义多个相关的事件，比如我们可以在 IApplicationEventHandler 上定义以下事件名称

* OnStarted 应用程序启动后
* OnStopping 应用程序尝试关闭时
* OnError 应用程序出现异常时

```csharp
public interface IApplicationEventHandler : IEventHandler
{
    void OnStarted();
    void OnStopping();
    void OnError();
}
```

### 1.3 事件参数

事件除了名称，还需要一些参数。

EAM 系统中的事件总线允许每个事件具备一个参数。

```csharp
public interface IApplicationEventHandler : IEventHandler
{
    void OnStarted(ApplicationInformation appInfo);
    void OnStopping(StopInformation stopInfo);
    void OnError(Exception ex);
}
```

## 2 编写监听者

一个事件监听者，一定是实现了某个 IEventHandler 的接口。

```csharp
public class WriteLogWhenApplicationStarted : IApplicationEventHandler
{
    public void OnStarted(SomeEventArgs e)
    {
        // do some thing
    }
}
```

## 3 触发事件总线

触发事件总线依赖于 IEventBus 组件。
该组件需要通过 依赖注入 的方式来得到其实现类的实例。

IEventBus 只有一个成员，Trigger
它要求三个参数

* eventHandlerType, IEventHandler 的类型，比如 typeof(IApplicationEventHandler)
* action, 事件，比如 nameof(IApplicationEventHandler.OnStarted)
* eventArgs，事件的参数

```csharp
public class SomeClass : IDependency
{
    private readonly IEventBus eventBus;

    public SomeClass(IEventBus eventBus)
    {
        this.eventBus = eventBus;
    }

    public void DoSomeThing(SomeData data)
    {
        // some code here
        this.eventBus.Trigger(
            typeof(ISomeEventHandler),
            nameof(ISomeEventHandler.OnSomeThingSuccess),
            data
        );
    }
}
```