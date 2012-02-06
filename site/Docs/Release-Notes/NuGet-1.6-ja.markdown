<!-- Revision: b027708c6ae20288dfadf5462415beb2af235372 2011/12/13 12:11:59 -->
# NuGet 1.6 リリース ノート

## 既知のインストールに関する問題

Visual Studio 2010 SP1 を実行している場合、以前のバージョンの NuGet がインストールされている状況でアップグレードしようとすると、インストール エラーになる可能性があります。

回避策は、単に NuGet をアンインストールしてから Visual Studio 拡張機能ギャラリーからインストールすることです。さらなる情報については、<a href="http://support.microsoft.com/kb/2581019">http://support.microsoft.com/kb/2581019</a>（英語）を参照してください。

注：Visual Studio が拡張機能のアンインストールを許可しない（**Uninstall** ボタンが無効になっている）場合、「管理者として実行」を使用して Visual Studio を起動しなおす必要があるかもしれません。

## 新機能

### セマンティック バージョニングとプレリリース パッケージのサポート

NuGet 1.6 はセマンティック バージョニング（SemVer：Semantic Versioning）のサポートを取り入れます。SemVer の使用方法の詳細については、[バージョニングについてのドキュメント](../Reference/Versioning)を参照してください。

### パッケージのチェックインが不要な NuGet の使用方法（パッケージ復元）

NuGet 1.6 では、ソース コントロールに NuGet のパッケージを追加する必要をなくしつつも、ビルド時に足りないパッケージを復元するワークフローに対するファースト クラスのサポートが追加されました。さらなる詳細については、[ソース コントロールにパッケージをコミットしない NuGet の使用方法](../Workflows/Using-NuGet-without-committing-packages) のトピックを参照してください。

### NuGet パッケージをインストールする項目テンプレート

Visual Studio のプロジェクトテンプレートへの NuGet パッケージの事前インストールをサポートする作業を踏まえ、NuGet 1.6 では Visual Studio の項目テンプレートのサポートも追加されます。項目テンプレートは、テンプレートの実行時にインストールされる NuGet パッケージに関連付けることができます。

NuGet パッケージをインストールするためにプロジェクトテンプレートまたは項目テンプレートを変更する方法の詳細については、[Visual Studio テンプレートでのパッケージ](../Reference/Packages-in-Visual-Studio-Templates)のトピックを参照してください。

### パッケージ ソースの無効化のサポート

複数のパッケージ ソースを構成している場合、NuGet はパッケージとその依存先のインストール中に、各ソースを検索します。何らかの理由でダウンしているパッケージソースがあると、NuGet の動作が非常に重たくなる可能性があります。

NuGet の 1.6 より前のバージョンでは、パッケージ ソースを削除することはできましたが、そうすると追加し直すときのためにそのことを覚えておかなければなりませんでした。

NuGet 1.6 では、パッケージ ソースを無効化するためにチェックを外しつつも、残しておくことができます。

![パッケージの無効化](../Start-Here/images/package-source-with-disabled-source.png)

## バグ収支絵

NuGet 1.6 は合計 106 個の作業項目を修正しました。これらのうちの 95 個はバグに分類され、10 個は新機能に分類されました。

NuGet 1.6 で修正された作業項目の完全な一覧については、[このリリースについての NuGet の Issue Tracker](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%201.6&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0)をご覧ください。

