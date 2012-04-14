<!-- 4 12 02:57:02 2012 1f500d3709307f3ba4ffb87f93b69df3fca5bcf2 -->
# パッケージの作成と公開

## はじめに

1. [NuGet.exe をダウンロードします。](http://nuget.codeplex.com/releases/view/58939)
2. NuGet.exe へのパスが通っていることを確認します。

__GUI 好き__ の方は、パッケージの作成に [Package Explorer GUI](/docs/creating-packages/Using-a-Gui-to-Build-Packages-ja) を使用できます。

## パッケージの共有

NuGet.exe を最新版にアップグレードします（_または、[ここからダウンロードします]:(http://nuget.codeplex.com/releases/view/58939)_）。

    NuGet Update -self

[API キー](#api-key)を保存します。

    NuGet SetApiKey Your-API-Key

NuGet パッケージをビルドします。

    NuGet Pack YourPackage.nuspec

パッケージを発行します。

    NuGet Push YourPackage.nupkg

## NuGet.exe のインストール

1. [NuGet.exe をダウンロードします](http://nuget.codeplex.com/releases/view/58939)。
2. NuGet を既知の場所に配置します。たとえば、マシンの `c:\utils` に配置します。
3. NuGet.exe へのパスが通っていることを確認します。

NuGet のコマンドについて学習するには、<code>nuget help</code> を実行するか、[NuGet.exe コマンドラインリファレンス](~/docs/reference/command-line-reference) を参照してください。

## パッケージを作成する

パッケージを作成する方法はいくつかあります。ほとんどのパッケージは非常に単純で、単一のアセンブリを含むものです。そのような場合、非常に簡単なパッケージ作成方法があります。より込み入ったケースについては後で説明します。

### アセンブリからの作成

アセンブリがあるならば、アセンブリ内のメタデータから `nuspec` ファイルを生成して、パッケージを簡単に作成できます。

    nuget spec MyAssembly.dll

このコマンドは `NuSpec` ファイルを作成します。必要に応じて `NuSpec` ファイルを編集し、それから、次のコマンドを実行します。

    nuget pack MyAssembly.nuspec

### プロジェクトからの作成

単純なパッケージでは、 `csproj` または vbproj` ファイルからのパッケージの作成が便利です。たとえば、プロジェクトにインストールされた別のパッケージは、この方法でパッケージを作成するときに依存先として参照されます。

`csproj` ファイルのあるフォルダーで、次のコマンドを実行します。

    nuget spec

このコマンドはプロジェクトのメタデータに基づいて pack 時に置き換えられるトークンを持つ特別な `NuSpec` ファイルを作成します。たとえば、 `$version$` はアセンブリに適用された `AssemblyVersionAttribute` （一般的には `AssemblyInfo.cs` ファイルで指定されます）で置き換えられます。

サポートされる置換トークンの一覧は以下のとおりです。

<table class="reference">
    <tr><th>トークン</th><th>ソース</th></tr>
    <tr>
        <td>$id$</td>
        <td>アセンブリ名</td>
    </tr>
    <tr>
        <td>$version$</td>
        <td>アセンブリの `AssemblyVersionAttribute` で指定されたアセンブリバージョン。</td>
    </tr>
    <tr>
        <td>$author$</td>
        <td>`AssemblyCompanyAttribute` で指定された企業名。</td>
    </tr>
        <tr>
        <td>$description$</td>
        <td>`AssemblyDescriptionAttribute` で指定された説明。</td>
    </tr>
</table>

カスタマイズが必要ならば、この `NuSpec` ファイルを編集できます。たとえば、トークン置換が不要なフィールドがあるならば、 `NuSpec` にハードコードできます。

同様に、パッケージに追加のファイルを組み入れたいならば、 `Nuspec` で <files> ノードを使用できます。たとえば、パッケージに別の任意のフォルダーにあるすべてのファイルを追加したい場合、以下のようにします。

    <files>
        <file src="..\..\SomeRoot\**\*.*" target="" /> 
    </files>

`NuSpec` の準備ができたならば、次のコマンドを実行します。

    nuget pack MyProject.csproj

`NuSpec` そのものではなく、プロジェクトファイルに対して '`nuget pack`' を実行する必要がある点に注意してください。ただし、実際には `NuSpec` ファイルが使用されます。

既定では、NuGet はプロジェクトファイルで設定された既定のビルド構成（一般的には `Debug` ）を使用します。別のビルド構成（たとえば `Release` ）でファイルをパッケージングするには、次のようにします。

    nuget pack MyProject.csproj -Prop Configuration=Release

今後のパッケージング呼び出しにおけるプロジェクトの既定値を変更するには、プロジェクトファイルを直接変更してください。Visual Studio のプロジェクトのプロパティ用の GUI には、この設定を変更するための便利な方法はありません。Visual Studio でプロジェクトファイルを編集するには、まずプロジェクトを右クリックして、「プロジェクトのアンロード」を選択し、プロジェクトをアンロードしてください。

![Visual Studio でのプロジェクトのアンロード](images/Unloading-project.png)

プロジェクトファイルを編集するには、アンロードしたプロジェクトを右クリックして、「{プロジェクト名}.{cs|vb|etc.}proj の編集」を選択してください。

![Visual Studio でのアンロード済みプロジェクトファイルの編集](images/Editing-unloaded-project-file.png)

一般的なプロジェクトファイルでは、最初の `<Project><PropertyGroup>` に `<Configuration>` 要素が含まれています。この要素を、設定したいビルド構成（一般的には `Release` ）に変更します。

    <Configuration Condition=" '$(Configuration)' == '' ">Release</Configuration>

`-Symbols` フラグを使用して、シンボルパッケージを含めることもできます。シンボルパッケージによって、他の人がデバッガーでパッケージのコードにステップインできるようになります。

プロジェクトから NuGet パッケージを作成して公開する方法の詳細なウォークスルーについては、David Ebbo のブログの [The easy way to publish NuGet packages with sources](http://blog.davidebbo.com/2011/04/easy-way-to-publish-nuget-packages-with.html) を参照してください。

### 規約に基づいたワーキングディレクトリからの作成

パッケージによってはアセンブリ以外のものを含むことがあります。可能性として、次のものがあります。

1. 対象のプロジェクトに挿入されるコンテンツやソースコード。
2. [PowerShell スクリプト](#powershell)や実行可能ファイル。
3. [構成ファイルとソースコード変換](~/docs/creating-packages/Configuration-File-and-Source-Code-Transformations-ja)。

この方法でパッケージを作成するには、NuGet の規約に従うようにディレクトリ構造をレイアウトします。

* __tools__ - パッケージの _tools_ フォルダーは、Package Manager Console からアクセス可能な [PowerShell スクリプト](#powershell)とプログラムのためのものです。フォルダーがターゲットプロジェクトにコピーされた後、 `$env:Path` （`PATH` 環境変数）が追加されます。
* __lib__ - _lib_ フォルダー内のアセンブリ（_.dll_ ファイル）は、パッケージのインストール時にアセンブリ参照として追加されます。
* __content__ - _content_ フォルダー内のファイルは、パッケージのインストール時にアプリケーションのルートにコピーされます。

__*Content* フォルダーはターゲットアプリケーションのルートと考えてください。__ たとえば、パッケージでターゲットアプリケーションの _/images_ ディレクトリに画像を追加させたい場合、画像をパッケージの _Content/images_ フォルダーに画像を配置するようにします。

#### マニフェストを作成する

スクラッチで `NuSpec` ファイルを作成するには、次のコマンドを実行します。

    nuget spec

これによって `.nuspec` 拡張子がついた XML ファイルが作成されます。

    <?xml version="1.0"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
      <metadata>
        <id>MyPackageId</id>
        <version>1.0</version>
        <authors>philha</authors>
        <owners>philha</owners>
        <licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
        <projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
        <iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Package description</description>
        <tags>Tag1 Tag2</tags>
        <dependencies>
          <dependency id="SampleDependency" version="1.0" />
        </dependencies>
      </metadata>
    </package>

パッケージに対して適切な値でファイルの中身を埋めます。それから、パッケージで必要となるフォルダーを作成し、各フォルダーに適切なコンテンツをコピーします。

    mkdir lib
    mkdir tools
    mkdir content
    mkdir content\controllers

    copy ..\src\SomeController.cs content
    copy ..\src\MyLibrary lib
    copy ..\src\SomePowershellScript.ps1 tools

コンテンツのあるワーキングディレクトリで、パッケージを作成するために次のコマンドを実行します。

    nuget pack YourPackage.nuspec


## 発行

### NuGet.org でアカウントを作成する

<a name="api-key"></a>
http://nuget.org/ に移動し、アカウントを登録します。一度アカウントを取得したならば、あなた用に生成された API キーを「My Account」から確認できます。

コマンドコンソールで、次のコマンドを実行します。

    nuget setApiKey <発行されたAPIキー>

これによって API キーが保存されるので、このコマンドを実行したマシンでは、この手順を繰り返す必要はなくなります。

## パッケージ規約

パッケージの作成時に適用される規約は 2 種類あります。
このページの一覧に含まれる規約は、パッケージのビルド時に従わなければならない _強制的な規約_です。
_コミュニティによる（またはオプションの）規約_もあります。これはコミュニティによって定められたもので、他の人によるパッケージ全体の理解を簡単にし、すぐに使えるようにするためのものです。コミュニティによる規約の詳細情報については、[パッケージ規約](Package-Conventions-ja)のページを参照してください。このページは、新しい規約が定義されるたびに更新される予定です。

## 複数の .NET Framework のバージョンやプロファイルのサポート

多くのライブラリは、特定のバージョンの .NET Framework を対象とします。たとえば、あなたのライブラリには Silverlight 専用のライブラリと .NET Framework 4 の機能を活用した同じライブラリの別バージョンがあるかもしれません。
これらのバージョンそれぞれに対して別のパッケージを作成する必要はありません。NuGet は単一のパッケージに同じライブラリの複数のバージョンを含める機能をサポートしています。それらのライブラリはパッケージ内の別のフォルダーに配置されます。

### フレームワークのバージョンごとのフォルダー構造

NuGet は、パッケージからアセンブリをインストールするとき、パッケージを追加するプロジェクトのターゲット .NET Framework バージョンをチェックします。それから、NuGet は *lib* フォルダー内の適切なサブフォルダーを選択することで、パッケージ内の適切なバージョンのアセンブリを選択します。

NuGet がこのように動作できるようにするために、どのフレームワークバージョンにどのアセンブリを使用するのかを示すために、以下の命名規約に従います。

    lib\{フレームワークの名前}{バージョン}

以下の例は 4 つのバージョンのライブラリをサポートするフォルダー構造を示しています。

    \lib
        \net11
            \MyAssembly.dll
        \net20
            \MyAssembly.dll
        \net40
            \MyAssembly.dll
        \sl40
            \MyAssembly.dll

### フレームワークの名前

NuGet はフォルダー名を `< ahref="http://msdn.microsoft.com/ja-jp/library/system.runtime.versioning.frameworkname.aspx">FrameworkName</a>` オブジェクトにパースしようとします。名前は大文字小文字を区別せず、フレームワークの名前とバージョン番号には省略形を使用できます。

フレームワークの名前を省略した場合、 `.NET Framework` であるとみなされます。たとえば、以下のフォルダー構造は先ほどのものと等価になります。

    \lib
        \11
            \MyAssembly.dll
        \20
            \MyAssembly.dll
        \40
            \MyAssembly.dll
        \sl4
            \MyAssembly.dll

以下の表は使用可能なフレームワーク名とその省略形です。

<table class="reference">
    <tr><th>フレームワークの名前</th><th>省略形</th></tr>
    <tr><td>.NET Framework</td><td>net</td></tr>
    <tr><td>Silverlight</td><td>sl</td></tr>
    <tr><td>.NETMicroFramework</td><td>netmf</td></tr>
</table>

### フレームワークバージョンを限定しないアセンブリ

フレームワークの名前もバージョンも関連付けられていないアセンブリは `lib` フォルダー直下に保存されます。

### プロジェクトのターゲットフレームワークにアセンブリバージョンを一致させる

NuGet は、複数のアセンブリバージョンを持つパッケージをインストールするとき、プロジェクトのターゲットフレームワークと、アセンブリのフレームワーク名のマッチングを試みます。
一致するものが見つからないならば、NuGet はプロジェクトのターゲットフレームワーク以下のバージョン番号のうち、最大のバージョン番号を持つアセンブリをコピーします。
たとえば、.NET Framework 3.5 をターゲットにするプロジェクトで先ほどの例で示した *lib* フォルダー構造を持つパッケージをインストールすると、 *2* フォルダー内のアセンブリ（.NET Framework 2.0 用）が選択されます。

### フレームワークのバージョンでのアセンブリのグループ化

NuGet は単一のライブラリフォルダーからのみアセンブリをコピーします。たとえば、パッケージが以下のフォルダー構造を持っているとします。

    \lib
        \Net20
            \MyAssembly.dll (v1.0)
            \MyAssembly.Core.dll (v1.0)
        \Net40
            \MyAssembly.dll (v2.0)

パッケージが .NET Framework 4 をターゲットにするプロジェクトにインストールされると、 *MyAssembly.dll （v2.0）* アセンブリのみがインストールされます。*MyAssembly.Core.dll （v1.0）* はインストールされません。
（NuGet がこのように動作する理由の 1 つは、 *MyAssembly.Core* がバージョン 2.0 で *MyAssembly* にマージされているかもしれないからです）

この例では、.NET Framework 4 をターゲットにするプロジェクトに *MyAssembly.Core.dll* をインストールしたい場合、 *Net20* フォルダーと同じように *@Net40* フォルダーにも含めなければなりません。

1 つのフォルダーからのみアセンブリをコピーするという動作についての規則は、ルートの *lib* フォルダーにも適用されます。パッケージが以下のフォルダー構造を持っているとします。

    \lib
        \MyAssembly.dll (v1.0)
        \MyAssembly.Core.dll (v1.0)
        \Net40
            \MyAssembly.dll (v2.0)

.NET Framework 2.0 と .NET Framework 3.5 をターゲットにするプロジェクトでは、NuGet は *MyAssembly.dll* と *MyAssembly.Core.dll* の両方をコピーします。ところが、先ほどの例のように、.NET Framework 4 をターゲットとするプロジェクトでは、 *Net40* にある *MyAssembly.dll* のみがコピーされます。

先ほどの例のように、.NET Framework 4 をターゲットにするプロジェクトに *MyAssembly.Core.dll* をインストールしたいならば、 *Net40* フォルダーにアセンブリを含めなければなりません。

### フレームワークのプロファイルでのアセンブリのグループ化

NuGet は、フォルダー名の末尾にハイフンでプロファイル名をつなげることで、特定のフレームワークプロファイルをターゲットにする構成をサポートします。

    lib\{フレームワーク名}-{プロファイル}

たとえば、 Windows Phone プロファイルをターゲットにするには、 *sl3-wp* と言う名前のフォルダーにアセンブリを配置します。

NuGet によってサポートされるプロファイルは以下のとおりです。

* Client - クライアントプロファイル
* Full - フルプロファイル
* WP - Windows Phone

本ページの記述時点で、Windows Phone プロファイルをターゲットにするには、Silverlight 3 フレームワークを指定しなければなりません。将来的に、Silverlight のより新しいバージョンが Windows Phone でサポートされる可能性があります。

<table class="reference">
    <tr><th>プロファイルの名前</th><th>省略形</th></tr>
    <tr><td>Client</td><td>client</td></tr>
    <tr><td>WindowsPhone</td><td>wp</td></tr>
    <tr><td>CompactFramework</td><td>cf</td></tr>
    <tr><td>Full</td><td>full</td></tr>
</table>

一般的なフレームワークとプロファイルのターゲット指定の例

以下は一般的なターゲットの例です。

<table class="reference">
    <tr><th>ツール</th><th>ターゲット</th><th>備考</th></tr>
    <tr><td>.NET 4.0</td><td>net40</td><td>単に「40」とするだけでも動作します。</td></tr>
    <tr><td>.NET 4.0 Client Profile</td><td>net40-client</td><td></td></tr>
    <tr><td>.NET 4.0 Full Profile</td><td>net40-full</td><td>フル .NET プロファイルが必要です。</td></tr>
    <tr><td>.NET 4.0 Compact Framework</td><td>net40-cf</td><td>net40-compactframework でも動作します。</td></tr>
    <tr><td>Silverlight 4.0</td><td>sl4</td><td></td></tr>
    <tr><td>Windows Phone 7.0</td><td>sl3-wp</td><td></td></tr>
    <tr><td>Windows Phone 7.1 (Mango)</td><td>sl4-windowsphone71</td><td></td></tr>
</table>

## パッケージの削除

NuGet.org はパッケージの恒久的な削除をサポートしません。依存元の何かを動作不可能にしてしまうためです。これは、特にビルド時にパッケージを復元するワークフローを使用している場合に当てはまります。

代わりに、NuGet.org はパッケージを「一覧から削除（unlist）」する方法をサポートします。これは Web サイト上のパッケージ管理ページで実行できます。パッケージが一覧から削除されると、検索で表示されなくなり、かつ NuGet.org と NuGet Visual Studio 拡張（または nuget.exe）の両方のパッケージの一覧で表示されなくなります。ただし、正確なバージョンを指定することでパッケージをダウンロードし続けることはできるので、復元ワークフローを動作し続けさせることができます。

パッケージのいずれかを削除しなければならないと思われる例外的な状況に遭遇したならば、NuGet チームは手動でパッケージを削除できます。たとえば、著作権侵害の問題がある場合や、有害なコンテンツとなる可能性がある場合などが、パッケージを削除する正当な理由となるでしょう。

<a name="#powershell"></a>
## パッケージのインストール中と削除中の PowerShell スクリプトの自動実行

パッケージには、パッケージのインストール時または削除時に自動的に実行される PowerShell スクリプトを含めることができます。NuGet は、以下の規約を使用して、ファイル名に基づいてスクリプトを自動的に実行します。

* ***Init.ps1*** はソリューションで最初にパッケージがインストールされるときに実行されます。
    * ソリューションの別のプロジェクトに同じパッケージがインストールされている場合、パッケージのインストール中にこのスクリプトは実行されません。
    * スクリプトはソリューションを開くたびにも実行されます。たとえば、パッケージをインストールし、Visual Studio を閉じ、また Visual Studio を起動してソリューションを開くと、 *Init.ps1* スクリプトがもう一度実行されます。
* ***Install.ps1*** はパッケージがプロジェクトにインストールされるときに実行されます。
    * ソリューションで複数のプロジェクトに同じパッケージがインストールされている場合、このスクリプトはパッケージがインストールされるたびに実行されます。
    * *Install.ps1* を実行するためには、パッケージが *content* または *lib* フォルダーにファイルが含まれていなければなりません。 *tools* フォルダーにファイルがあるだけでは、このスクリプトは実行されません。
    * パッケージに *Init.ps1* もある場合、 *Install.ps1* は *Init.ps1* の **後** に実行されます。
* ***Uninstall.ps1*** はパッケージがアンインストールされるたびに毎回実行されます。
* これらのファイルはパッケージの `tools` ディレクトリに配置すべきです。
* ファイルの先頭に、次の行を追加します。**`param($installPath, $toolsPath, $package, $project)`**
    * **$installPath@** はパッケージがインストールされるフォルダーのパスです。
    * **`$toolsPath`** はパッケージがインストールされるフォルダー内の `tools` ディレクトリのパスです。
    * **`$package`** はパッケージオブジェクトへの参照です。
    * **`$project`** は `EnvDTE` プロジェクトオブジェクトへの参照であり、このオブジェクトはパッケージがインストールされるプロジェクトを表します。 *注:* この値は *Init.ps1* では `null` になります。ソリューションレベルで実行されるので、特定のプロジェクトへの参照がないためです。このオブジェクトのプロパティは[MSDN のドキュメント](http://msdn.microsoft.com/ja-jp/library/51h9a6ew.aspx)で定義されています。
* スクリプトの作成中にコンソールで **`$project`** をテストするときには、 *$project = Get-Project* で設定できます。

*実際の出力について勉強したい場合、 <a href="http://nuget.org/List/Packages/NuGetPSVariables">NuGetPSVariables</a> は一連のログファイルに情報を書きだすヘルパーパッケージです。次のように呼び出すことで、パッケージを使用できるようになります。*

    Install-Package NuGetPSVariables

`NuGetPSVariables` はログファイルを表示し、自身をアンインストールします。
