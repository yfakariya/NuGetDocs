<!-- 4 05 06:07:00 2012 f0b366aa39c95cc852cb3089c61c618851b4673c -->
# NuGet 1.7 リリースノード

## 既知のインストールに関する問題

Visual Studio 2010 SP1 を使用している場合には、古いバージョンがインストールされていると、NuGet のアップグレードするときにインストールエラーが発生するかもしれません。

回避策は、単に NuGet をアンインストールしてから VS Extension Gallery 経由でインストールすることです。詳細情報については、<a href="http://support.microsoft.com/kb/2581019">http://support.microsoft.com/kb/2581019</a> を参照してください。

注：Visual Studio が拡張機能のアンインストールを許可しない（アンインストールボタンが無効になっている）場合には、「管理者として実行」を使用して Visual Studio を再起動する必要があるかもしれません。

## 新機能

### インストール後の readme.txt のオープンのサポート

1.7 から、パッケージのルートに <b>readme.txt</b> ファイルが含まれている場合、NuGet はパッケージのインストールの完了後に Readme ファイルを自動的に開くようになります。

### Manage NuGet packages ダイアログでのプリリリースパッケージの表示

Manage NuGet Packages ダイアログに、プリリリースパッケージを表示するかどうかの選択を行うためのドロップダウンが含まれるようになりました。

![Showing prerelease packages](images/prerelease-dropdown.png)

### パッケージのファイルがない場合の Show Package Restore ボタン

Package Manager Console または Manage NuGet Packages ダイアログを開いたときに、NuGet は現在のソリューションでパッケージ復元が有効かどうか、および <b>packages</b> フォルダーに足りないパッケージのファイルがあるかを調べます。これら 2 つの条件に当てはまる場合、NuGet はユーザーに通知し、すぐに対処できるように Restore ボタンを表示します。Restore ボタンをクリックすると、NuGet は足りないパッケージをすべて復元します。

![Package restore button on dialog](images/packagerestore-dialog.png)

![Package restore button on console](images/packagerestore-console.png)

### ソリューションレベルの packages.config ファイルの追加

これまでのバージョンの NuGet では、各プロジェクトには <b>packages.config</b> ファイルがあり、このファイルはそのプロジェクトにインストールされている NuGet パッケージを追跡していました。ところが、ソリューションレベルのパッケージを追跡するための、ソリューションレベルのファイルといったものは存在しませんでした。その結果、ソリューションレベルのパッケージを復元する方法がありませんでした。
NuGet 1.7 では、ソリューションレベルの package.config ファイル機能が実装されました。このファイルはソリューションのルートの <b>.nuget</b> フォルダーに配置され、ソリューションレベルのパッケージのみを保存します。

### New-Package コマンドの削除

使用率が低いので、New-Package コマンドは削除されました。開発者は、パッケージの作成に nuget.exe または手軽な NuGet Package Explorer を使用することを推奨します。

### バグ修正

NuGet 1.7 はパッケージ復元のワークフローとネットワーク/ソースコントロールのシナリオに関する、数多くのバグを修正しています。

NuGet 1.7 で修正されたワークアイテムの完全な一覧については、[このリリースについての NuGet Issue Tracker](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%201.7&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0) をご覧ください。
