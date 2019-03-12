[TOC]

# 设备对话框

设备选择对话框使用场景较多。
且条件各不相同。
本页将介绍以下内容

-   构建不同查询条件的设备选择对话框
-   在多次选择时，排除已选择的设备
-   添加新的可用查询条件

## 1 查询条件

所有可能涉及到的查询条件，均定义在 **Dexi.EAM.App.Request.QueryEquipmentLedgerReq** 中。

## 2 查询执行

查询执行，就是使用 **Dexi.EAM.App.Request.QueryEquipmentLedgerReq** 进行设备数据的分页查询的方法。
位于 **Dexi.EAM.App.EquipmentLedgerApp.Load(QueryEquipmentLedgerReq request)** 中。

注意

1. **Dexi.EAM.App.Request.QueryEquipmentLedgerReq** 中的所有可能涉及的属性，都应当在 **Load** 中实现
2. **Dexi.EAM.App.Request.QueryEquipmentLedgerReq** 中，不是每个属性都会提供，因此在查询前，需要先判断属性的值是否提供
3. 先对 **Dexi.EAM.App.Request.QueryEquipmentLedgerReq** 中的属性是否提供进行判断，再构建 Linq 或 Lambda 表达式

## 3 绘制查询条件的控件

查询对话的页面文件位于 MVC 项目的 **View/EquipmentLedger/SelectEQ.cshtml** 中

其中 **#searchForm** 部门就是查询条件区域。

### 3.1 手动绘制控件

由于不同的调用方需要显示不同的控件，因此在手动绘制控件时，需要使用 @if(ShowFilter("PropertyName")) 来判断是否需要显示此控件。

```html
@if (ShowFilter("Type")) 
{
    <div class="layui-col-md3 layui-col-lg3 layui-col-xs3 ">
        <label class="layui-form-label">设备类型</label>
        <div class="layui-input-block">
            <div id="retrTypeBtn">
                <input
                    type="text"
                    id="retrType"
                    name="Type"
                    v-model="Type"
                    placeholder="设备类型"
                    autocomplete="off"
                    class="layui-input"
                    readonly
                    lay-search=""
                    style="cursor:pointer;"
                />
                <i class="layui-icon pull-icon-right">&#xe615;</i>
            </div>
        </div>
    </div>
}
```

### 3.2 自动绘制控件

通过对 **Dexi.EAM.App.Request.QueryEquipmentLedgerReq** 中的属性加上 Attribute ，可以实现一些常见控件的自动绘制。

1. 在属性上加上 [DisplayName] 用于定义显示名，如 设备类型、存放地点等
2. 在属性上加上 [XXXXInput] 用于定义以哪种方式显示控件，目前具备的标记有
    - [TextInput] 文本
    - [HiddenInput] 隐藏域

## 4 打开对话框

打开对话框时，可以做以下配置

-   定义需要显示的过滤控件
-   传递已选中的设备 ID 集合
-   定义一些默认条件的值

方法

1. 通过 layui 引用 dialogs 组件
2. 通过 dialogs.eq2 方法打开对话框

示例

```javascript
dialogs
    .eq2({
        filter: ["Location", "AssetNo", "Keeper"],
        width: 1000,
        height: 600,
        callback: function(array) {
            // 此处的 array 是选中的设备
            // 通过 return false，可以阻止对话框关闭
        }
    })
    .then(w => {
        // 此处是对话框打开后的事件
        if(typeof w.onShown === "function"){
            // 通过 onShown 方法可以传递一些默认值
            // 这些值的属性名原则上应当于 QueryEquipmentLedgerReq 相同
            w.onShown({
                Status : -1,
                SelectedEqIdList : ["1","2"]
            });
        }
    });
```
