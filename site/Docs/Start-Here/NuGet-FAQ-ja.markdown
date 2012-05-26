<!-- Revision: 1c15ab3b01cce3b4a6a13755b0d74e9b51c8f6b6 2011/11/18 3:39:41 -->
# NuGet のよくある質問

## NuGet を実行するための要件は何ですか？

NuGet には Visual Studio 2010 か Visual Web Developer Express 2010 が必要です。
NuGet Package Manager Console では [PowerShell 2.0](http://support.microsoft.com/kb/968929) がインストールされている必要があります。
以下のオペレーティングシステムでは、PowerShell 2.0 があらかじめインストールされています。

* Windows 7 
* Windows Server 2008 R2 

以下のオペレーティングシステムでは、[Powershell 2.0 を手動でインストール](http://support.microsoft.com/kb/968929/ja-jp)しなければなりません。

* Windows XP SP3 
* Windows Server 2003 SP2 
* Windows Vista SP1 
* Windows Server 2008 

## NuGet を取得するために ASP.NET MVC 3 をインストールしなければならないのでしょうか？

**そのようなことはまったくありません。**MVC 3 なしで NuGet をインストールする方法については、[NuGet のインストール](Installing-NuGet)を参照してください。

## パッケージ マネージャー コンソール（Package Manager Console）や NuGet パッケージの管理（Manage NuGet Packages）ダイアログボックスがクラッシュしたり、例外を表示したりするのはなぜですか？

[既知の問題](../Reference/Known-Issues-ja)ページをチェックしてください。一般的に、問題は Reflector アドインの旧式のバージョンがインストールされているか、PowerShell 2.0 がインストールされていない（Windows XP の場合）ことが原因です。

## フィードにパッケージを登録するにはどうすればいいですか？

[パッケージの作成と公開](../creating-packages/creating-and-publishing-a-package-ja)のページを参照してください。

## プロジェクトレベルのパッケージとソリューションレベルのパッケージの違いは何ですか？

ソリューションレベルのパッケージはソリューションに一度だけインストールされ、ソリューション内のすべてのパッケージで使用可能になります。
プロジェクトレベルのパッケージは、使用したいプロジェクトそれぞれで独立してインストールしなければなりません。
ソリューションレベルのパッケージでは、NuGet はプロジェクト内を一切変更しませんが、プロジェクトレベルのパッケージでは変更を行います。
一般的に、ソリューションレベルのパッケージは**パッケージ マネージャー コンソール**（**Package Manager Console**）ウィンドウから呼び出すことができる新しいコマンドをインストールします。

## .NET Framework の異なるバージョンをターゲットにするライブラリの複数のバージョンがあります。これをサポートする単一のパッケージをビルドするにはどうすればいいですか？

[パッケージの作成と公開ページの .NET Framework の複数のバージョンとプロファイルをサポートする](../creating-packages/creating-and-publishing-a-package-ja#supporting-multiple-framework-versions)のセクションを参照してください。

## インストールされている NuGet の正確なバージョンをチェックするにはどうすればいいですか？

Visual Studio で、_Help_ > _Microsoft Visual Studio について_ メニュを開き、NuGet Package Manager を調べます。そのエントリの次にバージョンが表示されます。

![Visual Studio の拡張機能マネージャーでの NuGet のバージョン](images/nuget-version.png)

代わりに、**パッケージ マネージャー コンソール**（**Package Manager Console**）を起動して、バージョンを含む NuGet Powershell ホストの情報を出力するように `$host` と入力することもできます。

## Package Manager Console で DTE オブジェクトにアクセスするにはどうすればいいですか？

コンソールは `$DTE` という名前の変数を提供しており、これは `DTE` オブジェクトを返します。[パッケージ マネージャー コンソール（Package Manager Console） Powershell リファレンス](../Reference/Package-Manager-Console-PowerShell-Reference-ja)で `Get-Project` コマンドを参照してください。

## リモートの依存先を持つローカルパッケージのインストール中に「Unable to resolve dependency error」と出る理由は何ですか？

**ローカルパッケージのインストール時に *All* ノードを選択してください。** *all* ノードはフィード全体を集約します。
Online タブの下から特定のリポジトリを選択すると、そのノードのみにインストールが制限されます。
この動作の理由は、多くのケースについて、企業のポリシーのために、ローカルリポジトリのユーザーはリモートのパッケージをうっかりインストールしたくないためです。

## $DTE 変数を DTE2 型にキャストしようとしましたが、「Cannot convert the "EnvDTE.DTEClass" value of type "EnvDTE.DTEClass" to type "EnvDTE80.DTE2"」というエラーになります。何が間違っているのでしょうか？

これは PowerShell と COM オブジェクトの相互作用の方法に関する既知の問題です。次の方法を試してください。
    $dte2 = Get-Interface $dte ([EnvDTE80.DTE2])
`Get-Interface` は NuGet PowerShell ホストによって追加されるヘルパー関数です。

## ローカルリポジトリやフィードをセットアップするにはどうすればいいですか？

[自前の NuGet フィードのホスト](../Creating-Packages/Hosting-Your-Own-NuGet-Feeds-ja)を参照してください。

## NuGet は Nu Gems と連携しますか？ それとも、Nu Gems の後継となるものですか？

**申し訳ありませんが、連携しません。ただし、これから述べる内容をお読みください。** NuGet は Nu Gems と直接連携しません。NuGet は自前のパッケージ形式（OPC ベース）を使用するので、*.gemspec* ファイルを直接読むことができません。基本的に、NuGet は Nu Gems のバージョン 2 であり、Visual Studio と .NET Framework プラットフォーム世のパッケージ管理の革命と考えることができます。
パッケージの取り扱いについて Nu の動作を気に入っている場合、NuGet.exe の動作は Visual Studio 外で NuGet を使用したい人が求めているものに非常に近い動作をすることをお忘れなく。

## NuGet は Mono をサポートしますか？

コマンドラインアプリケーション（*nuget.exe*）は Mono でビルドおよび実行でき、Mono でパッケージを作成できます。

## NuGet 用のコマンドラインツールはありますか？

**ええ、あります！** David Ebbo のブログの [Installing NuGet Packages directly from the command line](http://blog.davidebbo.com/2011/01/installing-nuget-packages-directly-from.html) というそのものずばりな記事を参照してください。

NuGet の目標が、Visual Studio のプロジェクトに対してプロジェクトの変更と参照の追加を可能にすることであることをお忘れなく。
コマンドラインツールである NuGet.exe は、パッケージのダウンロードとアンパックを行いますが、Visual Studio の自動化を行わずとプロジェクトファイルを変更しません。
Visual Studio には、NuGet 用のクライアントが 2 つあります。
PowerShell ベースの**パッケージ マネージャー コンソール**（**Package Manager Console**）と、**NuGet パッケージの管理**（**Manage NuGet Package**）ダイアログボックスです。
両方とも NuGet API のラッパーであり、この API はマネージコードで記述されています。
NuGet.exe はパッケージの作成と公開にも使用されます。

## NuGet によってサポートされる言語は何ですか？

NuGet は現在 C#、F#、Visual Basic のプロジェクトをサポートしています。

## Visual Studio の外部で NuGet を使用できますか？

**もちろん！** NuGet 用のコマンドラインツールの質問で説明したように、NuGet の主要な対象は Visual Studio ですが、コア NuGet API は Visual Studio に依存しません。
Visual Studio の外部で完全に動作する NuGet クライアントが複数存在します。

* [SharpDevelop アルファ版](http://community.sharpdevelop.net/blogs/mattward/archive/2011/01/23/NuGetSupportInSharpDevelop.aspx)。（[Phil Haack の MvcConf での講演](http://bit.ly/fzrJDa)でのデモを参照してください。）
* WebMatrix での ASP.NET Web ページ。（[Phil Haack の MvcConf での講演](http://bit.ly/fzrJDa)でのデモを参照してください。）
* [NuGet.exe](http://blog.davidebbo.com/2011/01/installing-nuget-packages-directly-from.html) 