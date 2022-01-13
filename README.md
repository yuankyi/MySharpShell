## Shell Icon Overlay Handlers

1. 新建类库 `MySharpShell`

2. 使用`NuGet`添加`SharpShell`引用

3. 添加`ico`资源文件(图像在左下角最佳)

   建议包含16×16、24×24 、32×32、48×48、64×64、256×256这几种尺寸,且背景要做透明处理

   ![Image text](https://github.com/yuankyi/MySharpShell/blob/main/MySharpShell/img/image-shellicon-3.png)

4. 继承 `SharpIconOverlayHandler`,标记 `COMVisible `特性

   ```csharp
   [ComVisible(true)]
   public class ReadOnlyFileIconOverlayHandler : SharpIconOverlayHandler
   ```

   ```csharp
    /// <summary>
   /// The ReadOnlyFileIconOverlayHandler is an IconOverlayHandler that shows
   /// a padlock icon over files that are read only.
   /// </summary>
   public class ReadOnlyFileIconOverlayHandler : SharpIconOverlayHandler
   {
       protected override int GetPriority()
       {
           //This function must return the priority of the handler. 
           //The priority is used when there are two icon overlays for a single file 
           //- the shell will use the icon with the highest priority. 
           //Just to be confusing, the highest priority is 0 - the lowest priority is 100.
           throw new NotImplementedException();
       }
    
       protected override bool CanShowOverlay(string path, FILE_ATTRIBUTE attributes)
       {
           //Every time a shell item will be displayed, this function will be called. 
           //The path to the shell item is provided in the path parameter. 
           //The FILE_ATTRIBUTE flags are also provided (these are attributes like FILE_ATTRIBUTE_DIRECTORY). 
           //Return true if you want the icon overlay to be shown for the specified file.
           throw new NotImplementedException();
       }
    
       protected override System.Drawing.Icon GetOverlayIcon()
       {
           //This function must return a standard .NET System.Drawing.Icon object that is the overlay icon to use.
           throw new NotImplementedException();
       }
   }
   ```

5. 为程序集签名, 新建强名称密钥文件, 无需密码

   ![Image text](https://github.com/yuankyi/MySharpShell/blob/main/MySharpShell/img/image-shellicon-5.png)

6. 编译生成dll

   ```
   1>------ 已启动全部重新生成: 项目: MySharpShell, 配置: Debug Any CPU ------
   1>  MySharpShell -> D:\_MY\Code\_MyCode\MySharpShell\MySharpShell\bin\Debug\MySharpShell.dll
   ========== 全部重新生成: 成功 1 个，失败 0 个，跳过 0 个 ==========
   ```

7. 使用`RegAsm`注册dll到注册表(以管理员身份运行)

   C:\Windows\Microsoft.NET\Framework64\v4.0.30319\RegAsm.exe

   `/codebase`注册

   ```
   C:\Windows\Microsoft.NET\Framework64\v4.0.30319>RegAsm.exe D:\_MY\Code\_MyCode\MySharpShell\MySharpShell\bin\Debug\MySharpShell.dll /codebase
   Microsoft .NET Framework 程序集注册实用工具版本 4.8.3752.0
   (适用于 Microsoft .NET Framework 版本 4.8.3752.0)
   版权所有 (C) Microsoft Corporation。保留所有权利。
   
   成功注册了类型
   ```

   `/u`注销

   ```
   C:\Windows\Microsoft.NET\Framework64\v4.0.30319>RegAsm.exe D:\_MY\Code\_MyCode\MySharpShell\MySharpShell\bin\Debug\MySharpShell.dll /u
   Microsoft .NET Framework 程序集注册实用工具版本 4.8.3752.0
   (适用于 Microsoft .NET Framework 版本 4.8.3752.0)
   版权所有 (C) Microsoft Corporation。保留所有权利。
   
   成功注销了类型
   ```

8. 检查注册表顺序

   `\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers\ `

   `win10`只启用前15个, 可以在名称前加空格, 使其排名靠前

   ![Image text](https://github.com/yuankyi/MySharpShell/blob/main/MySharpShell/img/image-shellicon-8.png)

9. 重启`explorer`,查看效果(小图标效果不太好,可能是ico的问题)

   ![Image text](https://github.com/yuankyi/MySharpShell/blob/main/MySharpShell/img/image-shellicon-9.png)

