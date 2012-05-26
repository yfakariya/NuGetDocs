<!-- 5 11 06:04:25 2012 8bad5a4d6c0d5f3091797b8a42738d361c938c8a -->
# Visual Studio テンプレートでのパッケージ

NuGet は、Visual Studio のプロジェクト テンプレートと項目テンプレートへの追加をサポートします。Visual Studio が NuGet パッケージを含むテンプレートを使用してプロジェクトを作成すると、パッケージがインストールされた状態でプロジェクトが作成されます。このため、この機能はよく「プリインストール済み NuGet パッケージ」機能と言われます。

この機能は、あるライブラリ（jQuery や EntityFramework）への参照をプロジェクトまたは項目テンプレートに含めたい場合に役立ちます。たとえば、ASP.NET MVC3 用のプロジェクトテンプレートには、NuGet パッケージとして jQUery、jQuery.Validation、Modernizer、他さまざまなライブラリが含まれています。

これによって、エンドユーザーに対し、テンプレートをリリースしてから長い時間が経った後にプロジェクトまたは項目テンプレートによってインストールされる NuGet パッケージの更新を簡単に行うことができるという、より優れたエクスペリエンスが提供されます。同時に、ライブラリで必要となるすべてのファイルを把握する代わりに、単一のライブラリ パッケージ ファイルを参照するだけで済むので、テンプレートの作成も簡単になります。

## Visual Studio テンプレートの作成

この記事では Visual Studio テンプレートの作成方法は説明しません。詳細情報については、[Visual Studio を直接使ってテンプレートを作成する方法](http://msdn.microsoft.com/ja-jp/library/s365byhx.aspx)や[Visual Studio SDK を使用してテンプレートを作成する方法](http://msdn.microsoft.com/en-us/library/ff527340.aspx)のリンクを参照してください。

## パッケージのテンプレートへの追加

プリインストール済みパッケージは[テンプレート ウィザード](http://msdn.microsoft.com/ja-jp/library/ms185301.aspx)を使用して動作します。テンプレートがインスタンス化されると、特別なウィザードが呼び出されます。このウィザードはインストールが必要なパッケージの一覧を読み込み、適切な NuGet API にそれらの情報を渡します。

テンプレートはパッケージの nupkg ファイルを探す場所を必要とします。現在、2 つのパッケージリポジトリがサポートされています。

1. VSIX パッケージ内に埋め込まれたパッケージ。
2. プロジェクトまたは項目テンプレートそのものの内部に埋め込まれたパッケージ。

プリインストール済みパッケージをプロジェクトまたは項目テンプレートに追加するには、次のことを行う必要があります。

1. .vstemplate ファイルを編集して [`WizardExtension`](http://msdn.microsoft.com/ja-jp/library/ms171411.aspx)要素を追加し、NuGet テンプレートウィザードへの参照を追加します。
    <pre><code>&lt;WizardExtension&gt;
        &lt;Assembly&gt;NuGet.VisualStudio.Interop, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a&lt;/Assembly&gt;
        &lt;FullClassName&gt;NuGet.VisualStudio.TemplateWizard&lt;/FullClassName&gt;
    &lt;/WizardExtension&gt;</code></pre>
    `NuGet.VisualStudio.Interop.dll` は `TemplateWizard` クラスのみを含む新しいアセンブリです。このクラスは `NuGet.VisualStudio.dll` にある実際の実装を呼び出す単純なラッパーです。NuGet のバージョンが新しくなってもプロジェクトまたは項目テンプレートが動作し続けるように、ラッパーを含むアセンブリのバージョンは決して変更されません。

2. プロジェクトにインストールするパッケージの一覧を追加します。
    <pre><code>&lt;WizardData&gt;
        &lt;packages&gt;
            &lt;package id="jQuery" version="1.6.2" /&gt;
        &lt;/packages&gt;
    &lt;/WizardData&gt;</code></pre>
    ウィザードは複数の `<package>` 要素をサポートします。`id` 属性と `version` 属性は両方とも必須です。このことから、重要なこととして、オンラインのパッケージフィードでより新しいバージョンが利用可能であったとしても、 _特定の_ バージョンのパッケージがインストールされます。
    このように動作する理由は、パッケージの将来のバージョンには、プロジェクトまたは項目テンプレートと互換性のない変更が入るかもしれないからです。NuGet を使用してパッケージを最新バージョンにアップグレードするかどうかの選択は、パッケージの最新バージョンへのアップグレードのリスクを想定するのに最も適した場所にいる開発者の手に残されます。

残った手順は、NuGet がパッケージ ファイルを検索するリポジトリの指定です。前述のように、以下の 2 つのリポジトリモードがサポートされます。

### VSIX パッケージ リポジトリ

推奨される Visual Studio のプロジェクトまたは項目テンプレートの配置方法は、VSIX パッケージによるものです（[VSIX 配置についての詳細情報はこちらを読んでください](http://msdn.microsoft.com/en-us/library/ff363239.aspx)）。複数のプロジェクトまたは項目テンプレートを一緒にパッケージ化でき、さらに VS 拡張機能マネージャーや Visual Studio Galallery を使用して開発者が簡単にテンプレートを発見できるようになるので、VSIX による方法のほうが優れています。この仕組みを使用する場合、[Visual Studio 拡張機能マネージャーの自動更新メカニズム](http://msdn.microsoft.com/en-us/library/dd997169.aspx) を使用して、ユーザーに対して簡単に更新をプッシュ通知できます。

1. パッケージリポジトリとして VSIX を指定するには、次のように `<package>` 要素を変更します。
    <pre><code>&lt;packages repository="extension"
              repositoryId="MyTemplateContainerExtensionId"&gt;
    ...
    &lt;/packages&gt;</code></pre>
    `repository` 属性にはリポジトリの種類（「extension」）を、`repositoryId` には VSIX の一意な識別子（つまり、拡張機能の vsixmanifest ファイルの [`ID` 属性](http://msdn.microsoft.com/en-us/library/dd393688.aspx) の値）を指定します。

2. [カスタム拡張機能のコンテンツ](http://msdn.microsoft.com/en-us/library/dd393737.aspx) として nupkg ファイルを追加します。
    <pre><code>&lt;CustomExtension Type="Moq.4.0.10827.nupkg"&gt;
              packages/Moq.4.0.10827.nupkg&lt;/CustomExtension&gt;
    ...
    </code></pre>
    これらがVSIX パッケージの `Packages` というフォルダーにそれらを配置するようにしてください。

nupkg ファイルは、プロジェクトテンプレートと同じ VSIX に配置できます。または、配置のシナリオとしてそちらのほうが有用であれば、別の VSIX にパッケージを配置できます（自分が管理していない VSIX を参照すべきではない、ということだけは注意しておきます。それらのパッケージが将来的に変更され、プロジェクトまたは項目テンプレートが動作しなくなる可能性があるためです）。

### テンプレートパッケージリポジトリ

複数のプロジェクトのパッケージ化があまり重要ではない場合（たとえば、単一のプロジェクトまたは項目テンプレートのみを配布する場合）、より単純でありながら制限の多い方法として、プロジェクトまたは項目テンプレートの zip ファイルそのものに nupkg ファイルを含める方法があります。

ただし、互いに関連するプロジェクトまたは項目テンプレートのセットをバンドルし、NuGet パッケージを共有している場合（たとえば、Razor、Web Forms、C#、VB.NET 用のバージョンを含むカスタム MVC プロジェクトテンプレートをリリースする場合）、各プロジェクトまたは項目テンプレートの zip ファイルへの NuGet パッケージの直接の追加は推奨しません。プロジェクトまたは項目テンプレートのバンドルのサイズが不必要に大きくなります。

1. パッケージリポジトリとしてプロジェクトまたは項目テンプレートを指定する場合には、 `<package>` 要素を次のように変更します。
    <pre><code>&lt;packages repository="template"&gt;
        ...
    &lt;/packages&gt;</code></pre>
    `repository` 属性は、「template」という値になり、`repositoryId` 属性は必須ではなくなりました。nupkg ファイルはプロジェクトまたは項目テンプレートの zip ファイルのルートディレクトリに配置されなければなりません。

## ベストプラクティス

1. VSIX が NuGet VSIX への依存関係を宣言するよう、VSIX マニフェストに参照を追加します。
    <pre><code>&lt;Reference Id="NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5" MinVersion="1.7.30402.9028"&gt;
    &lt;Name&gt;NuGet Package Manager&lt;/Name&gt;
    &lt;MoreInfoUrl&gt;http://docs.nuget.org/&lt;/MoreInfoUrl&gt;
    &lt;/Reference&gt;
    ....
    </code></pre>

2. .vstemplate ファイルの [`<PromptForSaveOnCreation>`](http://msdn.microsoft.com/ja-jp/library/twfxayz5.aspx) を設定することで、プロジェクトまたは項目テンプレートが作成時に保存されるように要求します。

## VS テンプレート内のパッケージのサンプルプロジェクト

とっかかりとなるサンプルプロジェクトが利用できます。ソースコードは[ここ](https://bitbucket.org/marcind/nugetinvstemplates) から利用できます。