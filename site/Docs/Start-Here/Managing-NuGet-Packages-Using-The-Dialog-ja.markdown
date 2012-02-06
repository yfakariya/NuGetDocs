<!-- Revision: b883cd649475dc9407c70998d9c5e655ca83a7a4  2011/11/19 10:23:36 -->
# Manage NuGet Packages Dialog ボックスの使用

このトピックは、**Manage NuGet Packages** ダイアログボックスを使用した NuGet パッケージの検索、インストール、削除、更新の方法を説明します。このダイアログボックスを使用するには、Visual Studio でソリューションが開かれていなければなりません。または、PowerShell のコマンドを使用してパッケージをインストールできます。さらなる情報については、[Package Manager Console を使用した NuGet パッケージの検索とインストール](Using-the-Package-Manager-Console)を参照してください。

なお、ソリューション内の複数のプロジェクトで同時にパッケージを管理する方法については、[ソリューション用のパッケージの管理](#Managing_Packages_for_the_Solution)のセクションまで進んでください。

## パッケージの検索

**ソリューション エクスプローラー** で、**参照** ノードを右クリックし、**Manage NuGet Packages** をクリックします。（注：Web サイトプロジェクトでは、**Bin** ノードを右クリックします。）

![Manage NuGet Packages メニュオプション](images/manage-nuget-packages-menu-option.png)

**Online** タブをクリックして、使用可能なパッケージの一覧を表示します。

![Manage NuGet Packages ダイアログの Online タブ](images/manage-nuget-packages-online-tab.png)

一覧を順に見るか、またはダイアログボックスの右上の検索ボックスを使用してパッケージを検索します。たとえば、ELMAH という名前のロギングのパッケージを検索するには、「elmah」や「logging」と入力します。

一覧が長い場合には複数のページに分割されます。別のページに移動するには、下にあるページ移動用リンクを使用してください。

パッケージを選択すると、**Install** ボタンが表示され、さらに右側のペインに説明が表示されます。

## パッケージのソース

NuGet は複数のパッケージ ソースにあるパッケージを表示できます。パッケージ ソースを追加するには、ダイアログの **Settings** ボタンをクリックして、**Options** ダイアログを表示します。そのダイアログで、**Package Sources** ノードを選択します。

![Package Sources ダイアログ](images/package-sources.png)

ソースの名前と、URL またはフォルダーのパス（NuGet パッケージファイルを含むフォルダーは妥当なパッケージ ソースとなります）を入力し、**Add** ボタンをクリックします。

![カスタム ソースを使用する場合の Package Sources ダイアログ](images/package-sources-with-custom-source.png)

パッケージ ソースは **All** ノードの下に一覧表示されます。そのソースのパッケージを見るには、そのパッケージ ソースをクリックします。**All** ノードは、すべてのパッケージ ソースのパッケージの集約ビューを表示します。

![カスタム ソースを使用する場合の Manage NuGet Packages ダイアログ](images/manage-nuget-packagse-with-custom-source.png)

あるパッケージ ソースを一時的に無効化したい場合には、ダイアログでそのパッケージ ソースのチェックを外すだけで無効にできます。これはパッケージ ソースが何らかの理由で一時的にダウンしており、かつ集約フィードには含まれた状態にしておく必要がある場合に役に立ちます。

![無効なソースのある Package Sources ダイアログ](images/package-source-with-disabled-source.png)

## パッケージのインストール

パッケージをインストールするには、そのパッケージを選択して、**Install** をクリックします。NuGet は選択されたパッケージと、その依存先のパッケージをインストールします。ソリューションにファイルがコピーされ、場合によってはプロジェクトに参照が追加されたり、プロジェクトの *app.config* ファイルや *web.config* ファイルが更新されたりといった具合です。

ライセンス条項への同意を求められることもあります。

![License Acceptance ダイアログボックス](images/License-acceptance.png)

インストールが完了すると、**Install** ボタンが、パッケージが正しくインストールされたことを示す緑色のチェックマークに変わります。

![elmah がインストールされたことを示す緑色のチェックマーク](images/elmah-installed.png)

**ソリューション エクスプローラー** で、インストール済みのライブラリが Visual Studio の参照として追加されていることが確認できます。

![ソリューション エクスプローラーでの elmah への参照](images/elmah-reference-in-solution-explorer.png)

*app.config* または *web.config* ファイルの変更が必要な場合、変更が適用されています。次の例は ELMAH 用の変更を示しています。

![elmah 用の Web.config の変更](images/elmah-web.config-changes.png)

*packages* という名前の新しいフォルダーがプロジェクト フォルダーに作成されます。（ソリューションにソリューション フォルダーがない場合には、*packages* フォルダーがプロジェクト フォルダー内に作成されます。）

![packages フォルダー](images/packages-folder.png)

*packages* フォルダーにはインストールされたパッケージごとにサブフォルダーがあります。このサブフォルダーにはパッケージによってインストールされたファイルがあります。さらに、パッケージファイルそのもの（*.nupkg* ファイル、これはパッケージに含まれるすべてのファイルを含む *.zip* ファイルです）もあります。

![packages フォルダーの中にある elmah フォルダー](images/elmah-folder-in-packages-folder.png)

これで、プロジェクトでライブラリを使用できるようになりました。コードを記述すると IntelliSense が動作し、プロジェクトを実行すると ELMAH のロギング情報ページのようなライブラリの機能が動作します。

![elmah の IntelliSense](images/elmah-intellisense.png)

![elmah のエラーログページ](images/elmah-errorr-log-page.png)

## パッケージの削除

**Manage NuGet Packages** ダイアログを開き、インストール済みのパッケージの一覧を表示するために、**Installed Packages** タブを選択します。

![インストール済みパッケージを表示する Manage NuGet Packages ダイアログ](images/manage-nuget-packages-installed.png)

アンインストールしたいパッケージを選択し、**Uninstall** をクリックしてパッケージを削除します。

次のパッケージ要素が削除されます。

* プロジェクト内の参照。**ソリューション エクスプローラー** の *参照* フォルダーや *bin* フォルダーからライブラリがなくなります。（*bin* フォルダーから削除されたかどうかを確認するには、プロジェクトをビルドする必要があるかもしれません。）
* ソリューション フォルダー内のファイル。削除したパッケージ用のフォルダーが *packages* フォルダーから削除されます。（インストールしたパッケージが他にない場合には、*packages* フォルダーも削除されます。）
* *app.config* または *web.config* ファイルへに行った変更が元に戻されます。

削除したパッケージが依存しているために別のパッケージがインストールされていた場合、それらも削除したほうがよいでしょう。

パッケージに依存先がある場合、NuGet は依存先パッケージも削除するかどうか尋ねてきます。

![依存先のパッケージの削除](../Start-Here/images/remove-dependent-packages.png)

## パッケージの更新

**Manage NuGet Packages** ダイアログを開き、新しいバージョンが使用可能なパッケージの一覧を表示するために、**Updates** タブを選択します。

使用可能な更新のあるパッケージがある場合には、中央のペインに一覧表示されます。次のスクリーン ショットは jQuery の新しいバージョンが使用可能であることを示しています。

![更新が使用可能であることを示す Manage NuGet Packages ダイアログ](images/manage-nuget-packages-showing-updates.png)

更新したいパッケージを選択し、**Update** をクリックして、最新のバージョンにパッケージを更新します。

[リリースノートを含むパッケージ](../reference/nuspec-reference)の場合、右ペインの説明の表示場所にリリースノードが表示されます。

# ソリューション用のパッケージの管理

前のセクションでは、単一のプロジェクト用のパッケージの管理について見ました。NuGet 1.4 以降では、複数のプロジェクトで同時にパッケージインストール/アンインストールできるように、**Manage NuGet Packages** ダイアログをソリューション レベルで起動することもできます。

ソリューションを右クリックして、**Manage NuGet Packages** を選択するだけです。次のメニュから起動することもできます。

![Manage NuGet Packages ソリューション レベル メニュ](images/manage-nuget-packages-solution-menu.png)

ダイアログボックスはプロジェクトから起動した場合と同じですが、タイトル バーはダイアログの範囲がソリューションであることを示していることに注意してください。

![ソリューション用の Manage NuGet Packages ダイアログ](images/manage-nuget-packages-solution-dialog.png)

重要な違いは、各操作について、適用対象のプロジェクトを選択できることです。

## オンライン パッケージのインストール

**Online** タブを選択して、**Install** ボタンをクリックすると、パッケージをインストールするプロジェクトのセットを選択できます。

![Manage NuGet Packages のプロジェクトの選択](images/manage-nuget-packages-project-selection.png)

## インストール済みパッケージの管理

**Installed** タブを選択して、**Manage** ボタンをクリックすると、そのパッケージを各プロジェクトでインストールするかどうかを切り替えることができます。

![Manage NuGet Packages のプロジェクトの選択](images/manage-nuget-packages-install-project-selection.png)

## パッケージの更新

**Updates** タブを選択して、**Update** ボタンをクリックすると、更新をインストールするプロジェクトのセットを選択できます。

![Manage NuGet Packages のプロジェクトの選択](images/manage-nuget-packages-update-project-selection.png)