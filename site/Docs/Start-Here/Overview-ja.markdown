<!-- Revision: 18c624d00d31e023241ae8e94fe4531f9d1c1e6d 2011/10/24 5:55:09 -->
# NuGet の概要

NuGet は .NET Framework を使用する Visual Studio プロジェクトでの、ライブラリやツールの追加、削除、更新を簡単にする Visual Studio 拡張です。他の開発者と共有したいライブラリやツールを開発するときには、NuGet パッケージを作成して、NuGet リポジトリにパッケージを保存します。他の誰かが開発したライブラリやツールを使用したいときには、リポジトリからパッケージを取得して、Visual Studio のプロジェクト、またはソリューションにインストールします。

パッケージのインストール時に、NuGet はソリューションにファイルをコピーし、さらに参照の追加や `app.config` や `web.config` の変更のような必要な変更の一切を自動的に実行します。ライブラリを削除することにした場合、NuGet はファイルを削除し、残骸が残らないようにプロジェクトへの一切の変更を元に戻します。

## NuGet パッケージ

ライブラリやツールのインストールに必要なものはすべて 1 つのパッケージ（`.nupkg` ファイル）にバンドルされます。パッケージには、プロジェクトにコピーするファイルと、パッケージの内容とライブラリの追加や削除に必要なものを記述するマニフェストファイルが含まれます。パッケージは、使用可能なパッケージの一覧を表示するために Visual Studio がアクセスするフィードにバンドルされます。NuGet の既定のソースとなる公式のフィードがあります。そのフィードにコントリビュートすることも、自前のフィードを作成することもできます。

## Visual Studio の NuGet ユーザーインターフェイス

NuGet は Visual Studio 2010、Visual Web Developer 2010、Windows Phone Developer Tools 7.1 で動作します。**Manage NuGet Packages** ダイアログボックスを使用するか、Visual Studio のウィンドウでのみ使用できる **Package Manager Console** 内の PowerShell コマンドラインコマンドを使用して、パッケージの検索、インストール、削除を実行できます。
両方とも Visual Studio のメインメニュからアクセスできます。ソリューション エクスプローラーのコンテキストメニュからダイアログボックスを開くこともできます。

### Manage NuGet Packages ダイアログボックス

以下の図は **Manage NuGet Packages** ダイアログボックスを示しています。公式のフィードで使用可能なパッケージを見るには、**Online** タブをクリックしてください。

![Manage NuGet Packages ダイアログ](images/Manage-NuGet-Packages-Dialog.png)

**Manage NuGet Packages** ダイアログボックスの使用方法については、[Manage NuGet Packages Dialog ボックスの使用](Managing-NuGet-Packages-Using-The-Dialog-ja)
を参照してください。

## Package Manager Console ウィンドウ

以下のイラストは **Package Manager Console** ウィンドウを示しています。

![Package Manager Console](images/package-console.png)

Package Manager Console の使用方法については、[Package Manager Console を使用した NuGet パッケージの検索とインストール](Using-the-Package-Manager-Console-ja) を参照してください。

## サポートされるオペレーティングシステム

PowerShell コマンドレットには PowerShell 2.0 が必要です。したがって、NuGet では以下のオペレーティングシステムのいずれかが必要になります。

* Windows 7
* Windows Vista SP1
* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2003 SP2
* Windows XP SP3

## ビデオ

<iframe width="640" height="390" src="http://www.youtube.com/embed/PboPfoptU2c?hd=1" frameborder="0" allowfullscreen></iframe>
