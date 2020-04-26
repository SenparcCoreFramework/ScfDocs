# Xscf 模块开发

SCF 底层支持库官方 Nuget 包源码

## Xscf 单功能执行模块开发

> 1.新建一个Dotnet Core的Class Library项目

![Image text](/start/images/create_dotnet_core_class_library.png)

> 2.输入Class Libray的名称,点击创建

![Image text](/start/images/input_dotnet_core_class_library_name.png)

> 3.建立Register.cs

![Image text](/start/images/create_register_and_functions_folder.png)

> 4.配置Register中的必要内容

|    名称     |    说明         |
|--------------|-----------------|
|  Name       |  模块名称
|  Uid  |  全球唯一码,最好使用工具生成
|  Version  |  模块版本号(更新版本靠它识别)
|  MenuName  |  安装到SCF中以后菜单中显示的名称
|  Icon  |  显示在菜单旁边的[字体图标](https://colorlib.com/polygon/gentelella/icons.html)
|  Description  |  模块的说明文字,安装前可以根据此说明了解模块的具体功能

![Image text](/start/images/register_content.png)

> 5.创建自定义方法类

![Image text](/start/images/create_functions_class_library.png)

> 6.完成自定义方法

    public class LaunchApp_Parameters : IFunctionParameter
    {
        [Required]
        [MaxLength(300)]
        [Description("自定义路径||文件名")]
        public string FilePath { get; set; }
    }

    //注意：Name 必须在单个 Xscf 模块中唯一！
    public override string Name => "应用程序";

    public override string Description => "启动所有的程序";

    public override Type FunctionParameterType => typeof(LaunchApp_Parameters);

    public LaunchApp(IServiceProvider serviceProvider) : base(serviceProvider)
    {
    }

    /// <summary>
    /// 运行
    /// </summary>
    /// <param name="param"></param>
    /// <returns></returns>
    public override FunctionResult Run(IFunctionParameter param)
    {
        var typeParam = param as LaunchApp_Parameters;

        FunctionResult result = new FunctionResult()
        {
            Success = true
        };

        StringBuilder sb = new StringBuilder();
        base.RecordLog(sb, "开始运行 LaunchApp");

        StartApp(typeParam.FilePath);

        //sb.AppendLine($"LaunchPath{typeParam.LaunchPath}");
        sb.AppendLine($"FilePath{typeParam.FilePath}");

        result.Log = sb.ToString();
        result.Message = "操作成功！";
        return result;
    }

> 7.在Register中注册自定义的方法类

![Image text](/start/images/register_add_functions.png)

> 8.发布Nuget,详细步骤到发布Nuget

## Xscf 自定义带页面功能模块开发

> 1.新建一个DotnetCore Class Library项目，输入项目名称

![Image text](/start/images/page_create_dotnet_core_class_library.png)

![Image text](/start/images/page_create_dotnet_core_class_library_input_name.png)

>> 1.1 目录名称如下

![Image text](/start/images/page_folder_struct.png)

> 设置项目支持RazorPage功能
> Senparc.Core.Models.DataBaseModel新建模型Test类
> Senparc.Core.Models.DataBaseModel新建TestDto类
> AutoMapperConfigs增加以下代码
> Senparc.Areas.Admin.Areas.Admin.Pages下简历页面，更改IndexModel继承为BaseAdminPageModel
> 增加Service类
> Senparc.Core.Models中SenparcEntities里面增加
> 在Senparc.Web下执行
> add-migration Xscf_AreaTemplate_Init2 -Context MySenparcEntities

## 欢迎贡献代码！