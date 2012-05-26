<!-- Revision: 9c9849ea53f0dd154493f4a6d7c0765e77ad8260 2011/10/28 4:55:14 -->

# NuGet のインストール

NuGet は Visual Studio の **拡張機能マネージャー** を使用してインストール、および更新できます。インストール済みの Visual Studio に既に NuGet 拡張機能がインストールされているかどうか調べるには、Visual Studio の **ツール** メニューに **ライブラリ パッケージ マネージャー**（**Library Package Manager**）があるかどうかを見てください。

![メニュ](images/Menu.png)

Visual Studio に**ライブラリ パッケージ マネージャー**（**Library Package Manager**）（NuGet）拡張機能がまだインストールされていないならば、**拡張機能マネージャー** を使用してインストールできます。

## 拡張機能マネージャーの使用

Visual Studio で、**ツール* メニュをクリックして、**拡張機能マネージャー** をクリックします。

**拡張機能マネージャー** ダイアログボックスで、**オンライン ギャラリー** タブを選択して、検索ボックスに「nuget」と入力すると、**NuGet Package Manager** 拡張機能が検索されます。

**NuGet Package Manager** を選択し、**ダウンロード** をクリックします。

![拡張機能マネージャーで NuGet を表示する](images/extension-manager-with-nuget.png)

**インストーラー** ダイアログボックスで、**インストール** をクリックします。

![Visual Studio 拡張機能インストーラー](images/visual-studio-extension-installer.png)

インストールが完了したら、Visual Studio を閉じて、もう一度起動してください。

![Visual Studio 拡張機能インストーラーの完了](images/visual-studio-extension-installer-complete.png)

これで NuGet が使用できるようになりました。

## Visual Studio Gallery の使用

[vsg]:http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c

[Visual Studio Gallery][vsg] から拡張機能インストーラーをダウンロードして実行して、NuGet をインストールすることもできます。

![MSDN の Visual Studio Gallery の NuGet のスクリーンショット](images/Visual-Studio-Gallery-Download.PNG)

VSIX ファイルをダウンロードして、実行します。ライセンス条項に同意しなければなりません。同意したら、インストールが完了するまで、お待ちください。それから、開いている Visual Studio のインスタンスをすべて閉じ、もう一度起動します。

これで NuGet が使用できるようになりました。

# NuGet の更新

Visual Studio の **拡張機能マネージャー** を使用して NuGet を更新できます。更新があるかどうか調べるには、 **拡張機能マネージャー** に移動して、**更新** タブをクリックします。

NuGet の新しいバージョンがあるならば、**使用できる更新プログラム** の一覧に NuGet が表示されます。

![拡張機能マネージャーでの NuGet の新しいバージョンが使用できることの表示](images/visual-studio-extension-update-check.png)

一覧から NuGet を選択して、**更新** をクリックします。更新が完了したら、Visual Studio のすべてのインスタンスを閉じて、もう一度起動します。

# CI ビルドのインストール

NuGet のまだリリースされていない本当の最新版を実行したい場合には、[CI（Continuous Integration：継続的統合）マシンからインストール](http://ci.nuget.org:8080/guestAuth/repository/download/bt4/.lastSuccessful/VisualStudioAddIn/NuGet.Tools.vsix)できます。

**重要事項**：公式の NuGet ビルドは署名されていますが、CI マシンから取得したものは署名されていません。そのため、公式のビルドがインストールされている場合に、Visual Studio は CI ビルドのインストールを許可しません。その場合、次のようなエラーになります。

*'NuGet Package Manager' の署名付きのバージョンがインストールされていますが、更新バージョンは署名されていません。そのため、拡張機能マネージャーは更新をインストールできません。*

この問題を避けるには、CI ビルドをインストールする前に、（Visual Studio の拡張機能マネージャーからインストールした）公式ビルドをアンインストールする必要があります。同様に、公式ビルドに戻す前には、CI ビルドを案インストールしてください。ただし、CI ビルドをより新しい CI ビルドに更新しようとしている場合には、アンインストールする必要はありません。Visual Studio が拡張機能のアンインストールを許可しない（**アンインストール** ボタンが無効になっている場合）には、「管理者として起動」を使用して Visual Studio を再起動する必要があるかもしれません。
