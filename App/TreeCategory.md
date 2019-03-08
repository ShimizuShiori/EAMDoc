# 树形分类

EAM 系统中，对所有的树形分类进行了统一的管理。

所有的树形分类都存在数据表 **EAM_TreeCategory** 中。

用于区分不同的树的字段是 Type。

## 注册一个树形分类

EAM 系统未提供通过UI创建一个新的树形分类。
因此所有的树形分类是通过代码注册的。

注册方法是实现 **Dexi.EAM.App.Components.ITreeCategoryRegister** 接口

例
```csharp
public class AttachmentTypeTreeCategoryRegister : ITreeCategoryRegister
{
    // 在界面中的排序号
    public int Sn => 4;

    // 编码，即表中的 Type 字段
    public string Code => "AttachmentType";

    // 界面中的显示文本
    public string Name => "文档类型";
}
```