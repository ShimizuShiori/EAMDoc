# 路由标准

路由标准可以帮助我们更容易的看懂一个路由表达的含义。
甚至可以让我们在只知道资源别名的情况下，快速的在路由定义上达成一致，减少了查阅的时间。

## 资源名

不同的资源应当具备不同的资源名，如：

* Repair 维修单
* Equipment 设备
* User 人员

注意
* 资源名使用单数
* 资源名不可以重复

所有的 Router 尽可能使用 *资源名* 或 *api/资源名* 开头。如：
* api/Repair
* Repair

## 谓词

| Verb | Usage | Example |
|------|-------|---------|
| GET | 获取某个资源 | GET : Repair <br/> GET : Repair/1 |
| POST | 创建某个新的资源，通过 FormData 上传创建数据用的键值对 | POST : Repair |
| PUT | 更新某个资源，使用 FormData 上传数据 | PUT : Repair/1 |
| DELETE | 删除某个资源 | DELETE : Repair/1 |

POST 还可以用来执行对像上的某个操作，如：
```javascript
// 将编号为1的维修单 完成，[完成] 是一个操作
$.post("/Repair/1/Completed");
```

## 明细

资源下面往往挂载有明细数据，如维修单下面会挂载工时明细，对这些工时明细的操作，见下面的例子。

```javascript

// 获取维修单为1的所有工时明细，注意，这些的明细使用资源的复数形式
$.get("/Repair/1/Workloads");

// 创建一个新的工时到编号为1的维修单下
$.post("/Repair/1/Workloads", workload);

// 更新维修单1下面工时编号为2的数据
$.put("/Repair/1/Workloads/2", workload);

// 删除维修单1下面工时编号为2的数据
$.delete("/Repair/1/Workloads/2);

```