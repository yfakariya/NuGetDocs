<!-- Revision: 5baa8cd5ad9007583f321ba44469afdd172b7a95  2011/12/13 10:42:14 -->
# NuGet の既知の問題

## Reflector Visual Studio Add-In もインストールされている場合、Package Manager Console が例外をスローします。

Package Manager Console の実行時に、Reflector Visual Studio Add-In がインストールされていると、以下の例外メッセージに遭遇する可能性があります。

    The following error occurred while loading the extended type data file: 
    Microsoft.PowerShell.Core, C:\Windows\SysWOW64\WindowsPowerShell\v1.0\types.ps1xml(2950) : 
    Error in type "System.Security.AccessControl.ObjectSecurity": 
    Exception: Cannot convert the "Microsoft.PowerShell.Commands.SecurityDescriptorCommandsBase" 
    value of type "System.String" to type "System.Type".
    System.Management.Automation.ActionPreferenceStopException: 
    Command execution stopped because the preference variable "ErrorActionPreference" or common parameter 
    is set to Stop: Unable to find type

または

    System.Management.Automation.CmdletInvocationException: Could not load file or assembly 'Scripts\nuget.psm1' or one of its dependencies. <br />The parameter is incorrect. (Exception from HRESULT: 0x80070057 (E_INVALIDARG)) ---&gt; System.IO.FileLoadException: Could not load file or <br />assembly 'Scripts\nuget.psm1' or one of its dependencies. The parameter is incorrect. (Exception from HRESULT: 0x80070057 (E_INVALIDARG)) <br />---&gt; System.ArgumentException: Illegal characters in path.
       at System.IO.Path.CheckInvalidPathChars(String path)
       at System.IO.Path.Combine(String path1, String path2)
       at Microsoft.VisualStudio.Platform.VsAppDomainManager.&lt;AssemblyPaths&gt;d__1.MoveNext()
       at Microsoft.VisualStudio.Platform.VsAppDomainManager.InnerResolveHandler(String name)
       at Microsoft.VisualStudio.Platform.VsAppDomainManager.ResolveHandler(Object sender, ResolveEventArgs args)
       at System.AppDomain.OnAssemblyResolveEvent(RuntimeAssembly assembly, String assemblyFullName)
       --- End of inner exception stack trace ---
       at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadBinaryModule(Boolean trySnapInName, String moduleName, String fileName, <br />Assembly assemblyToLoad, String moduleBase, SessionState ss, String prefix, Boolean loadTypes, Boolean loadFormats, Boolean&amp; found)
       at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadModuleNamedInManifest(String moduleName, String moduleBase, <br />Boolean searchModulePath, <br />String prefix, SessionState ss, Boolean loadTypesFiles, Boolean loadFormatFiles, Boolean&amp; found)
       at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadModuleManifest(ExternalScriptInfo scriptInfo, ManifestProcessingFlags <br />manifestProcessingFlags, Version version)
       at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadModule(String fileName, String moduleBase, String prefix, SessionState ss, <br />Boolean&amp; found)
       at Microsoft.PowerShell.Commands.ImportModuleCommand.ProcessRecord()
       at System.Management.Automation.Cmdlet.DoProcessRecord()
       at System.Management.Automation.CommandProcessor.ProcessRecord()
       --- End of inner exception stack trace ---
       at System.Management.Automation.Runspaces.PipelineBase.Invoke(IEnumerable input)
       at System.Management.Automation.Runspaces.Pipeline.Invoke()
       at NuGetConsole.Host.PowerShell.Implementation.PowerShellHost.Invoke(String command, Object input, Boolean outputResults)
       at NuGetConsole.Host.PowerShell.Implementation.PowerShellHostExtensions.ImportModule(PowerShellHost host, String modulePath)
       at NuGetConsole.Host.PowerShell.Implementation.PowerShellHost.LoadStartupScripts()
       at NuGetConsole.Host.PowerShell.Implementation.PowerShellHost.Initialize()
       at NuGetConsole.Implementation.Console.ConsoleDispatcher.Start()
       at NuGetConsole.Implementation.PowerConsoleToolWindow.MoveFocus(FrameworkElement consolePane)

解決を希望するために、私たちはアドインの作者に連絡を取りました。

<p class="info">Update: Reflector の最新バージョンである 6.5 では、コンソールでこの例外が発生しなくなっていることを確認しました。</p>

## ソリューションに InstallShield Limited Edition のプロジェクトが含まれている場合、Add Package Library Reference ダイアログが例外をスローします。

ソリューションが 1 つ以上の InstallShield Limited Edition プロジェクトを含む場合、**Add Package Library Reference** ダイアログを開くときに例外がスローされることが確認されています。現在のところ、InstallShield プロジェクトの削除またはアンロード以外に回避方法がありません。

## 古いバージョンから最新の NuGet にアップグレードすると、署名の検証エラーが発生します。

Visual Studio 2010 SP1 を実行している場合、古いバージョンがインストールされているときに NuGet をアップグレードしようとすると次のエラーメッセージに遭遇するかもしれません。

![Visual Studio 拡張機能インストーラー](images/Visual-Studio-Extension-Installer.png)

ログを参照すると、`SignatureMismatchException` の存在が確認できるでしょう。

回避策は、単に NuGet をアンインストールして、Visual Studio 拡張機能ギャラリーから NuGet をインストールすることです。詳細情報については <a href="http://support.microsoft.com/kb/2581019">http://support.microsoft.com/kb/2581019</a> を参照してください。

## アンインストールボタンが無効になっていますか？　NuGet のインストールとアンインストールには管理者権限が必要です。

Visual Studio 拡張機能マネージャー経由で NuGet をアンインストールしようとすると、アンインストールボタンが無効になっていることに気付くことでしょう。NuGet ではインストールとアンインストールに管理者のアクセス権を必要とします。拡張機能をアンインストールするには、管理者権限で Visual Studio を再起動してください。
NuGet を使用するの管理者のアクセス権は必要ありません。

## Windows XP で開くと Package Manager Console がクラッシュします。何が悪いのでしょうか？

NuGet は PowerShell 2.0 のランタイムを必要とします。Windows XP は、既定では PowerShell 2.0 がありません。
PowerShell 2.0 ランタイムはこのリンク <a href="http://support.microsoft.com/kb/968929">http://support.microsoft.com/kb/968929</a> からダウンロードできます。
インストールしたら、Visual Studio を再起動すれば、Package Manager Console が開けるようになっているはずです。

## Package Manager Console が開いている状態で終了すると、Visual Studio 2010 SP1 のベータ版がクラッシュします。

Visual Studio 2010 SP1 のベータ版がインストールされている場合、Package Manager Console を開いたまま Visual Studio を閉じると、クラッシュすることに気付くことでしょう。これは Visual Studio の既知の問題であり、SP1 の RTM リリースでは修正されています。現在のところ、クラッシュを単に無視するか、可能ならば SP1 のベータ版をアンインストールしてください。

## 名前空間 'schemas.microsoft.com/packaging/2010/07/nuspec.xsd' の要素 'metadata' は不正な子要素を持つという例外が発生します。

NuGet のプレリリース版でビルドしたパッケージをインストールした場合、プロジェクトで NuGet の RTM 番を実行するとこのエラーメッセージに遭遇するかもしれません。パッケージをアンインストールしてから、NuGet の RTM 版を使用して各パッケージを再インストールする必要があります。

## インストールやアンインストールの試みが「Cannot create a file when that file already exists.」というエラーで終了します。

いくつかの理由から、Visual Studio 拡張機能は、VSIX 拡張をアンインストールされたが、いくつかのファイルが残ったままになるという奇妙な状態になる可能性があります。この問題の回避方法は以下のとおりです。

1. Visual Studio を終了します。
2. 以下のフォルダー（みなさんの環境ではドライブが異なるかもしれません）を開きます。

    <pre>C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\Extensions\Microsoft Corporation\NuGet Package Manager\&lt;version&gt;\</pre>

3. *.deleteme* という拡張子のファイルをすべて削除します。
4. Visual Studio をもう一度開きます。

これらの手順の後には、作業を継続できるはずです。

## 稀なケースにおいて、コード分析付きのコンパイルがエラーになります。

Package Manager console を使用して FluentNHibernate をインストールして、「コード分析」を有効にしてプロジェクトをコンパイルすると、以下のエラーになるかもしれません。

    Error 3 CA0058 : The referenced assembly 
    'NHibernate, Version=3.0.0.2001, Culture=neutral, PublicKeyToken=aa95f207798dfdb4' 
    could not be found. This assembly is required for analysis and was referenced by: 
    C:\temp\Scratch\src\MyProject.UnitTests\bin\Debug\MyProject.UnitTests.dll. 
    MyProject.UnitTests

<a href="http://davesbox.com/archive/2008/06/14/reference-resolutions-changes-in-code-analysis-and-fxcop-part-2.aspx">David Kean</a> はこの問題を説明する素晴らしいブログ記事を投稿しています。
既定では、FluentNHibernate は NHibernate 3.0.0.2001 を必要とします。ところが、仕様として、NuGet はプロジェクトに NHibernate 3.0.0.4000 をインストールし、かつパッケージが動作するように適切なバインディングリダイレクトを追加します。
コード分析が有効になっていない場合、プロジェクトは問題なくコンパイルされます。コンパイラとは異なり、コード分析ツールは 3.0.0.2001 の代わりに 3.0.0.4000 を使用するバインディングリダイレクトに正しく追随しません。
この問題を回避するには、NHibernate 3.0.0.2001 をインストールするか、または以下のようにしてコンパイラと同じようにコード分析ツールが動作するようにします。

1. *%PROGRAMFILES%\Microsoft Visual Studio 10.0\Team Tools\Static Analysis Tools\FxCop* に移動します。
2. `FxCopCmd.exe.config` を開き、`<code>AssemblyReferenceResolveMode</code>` を `<code>StrongName</code>` から `<code>StrongNameIgnoringVersion</code>` に変更します。
3. 変更を保存し、プロジェクトをリビルドします。

## install.ps1/uninstall.ps1/init.ps1 で Write-Error コマンドが動作しません。

これは既知の問題です。Write-Error を呼び出す代わりに、`throw` を呼び出してください。

    throw "My error message"

## Windows Server 2003 で制限付きアクセス権で NuGet をインストールすると、Visual Studio がクラッシュする可能性があります。

Visual Studio 拡張機能マネージャーを使用して NuGet をインストールしようとし、管理者として実行していない場合、「Run As」は「このプログラムを制限付きアクセス権で実行する」というラベルの付いたチェックボックスが、既定でチェックされた状態で表示されます。

![Run As Restricted Dialog](images/RunAsRestricted.png)

チェックされた状態で OK をクリックすると、Visual Studio がクラッシュします。NuGet のインストール前にこのオプションのチェックを外すようにしてください。

## Windows Phone Tools で NuGet をアンインストールできません。

Windows Phone Tools は Visual Studio 拡張機能マネージャーをサポートしていません。NuGet をアンインストールするには、次のコマンドを実行してください。

     vsixinstaller.exe /uninstall:NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5