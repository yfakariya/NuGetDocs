<!-- Revision: 18c624d00d31e023241ae8e94fe4531f9d1c1e6d 2011/10/24 05:55:09 -->
# Package Visualizer

NuGet 1.4 で追加された新機能では、ソリューションにあるすべてのプロジェクトとそれらのパッケージの依存関係を簡単にビジュアル化できます。

**ツール** -> **ライブラリ パッケージ マネージャー**（**Library Package Manager**） -> **Package Visualizer** メニュ オプションをクリックして、Package Visualizer を起動します。

_**重要な注意事項:** この機能は Visual Studio の DGML サポートを利用しています。ビジュアル化は Visual Studio Ultimate でのみサポートされています。 DGML ダイアグラムの閲覧は Visual Studio Premium 以上でのみサポートされています。_

この機能によって、ソリューション内のプロジェクト全体のパッケージの依存関係付きの図が表示されます。

![Package Visualizer の図](Images/package-visualizer.png)

図を表示しているときには、新しいツールバーを使用して、さまざまな方法で依存関係をビジュアル化できます。

![Package Visualizer のツールバー](Images/package-visualizer-toolbar.png)

たとえば、次の図はパッケージの依存関係のクラスタービューです。

![Package Visualizer クラスタービュー](Images/package-visualizer-cluster.png)

依存関係マトリックスを生成することもできます。

![Package Visualizer の依存関係マトリックス](Images/package-visualizer-matrix.png)

## Package Visualizer のフォーマット

Package Visualizer は、有向グラフの生成に [DGML 形式](http://en.wikipedia.org/wiki/DGML) を使用します。、DGML ファイルに読み取りのサポートは Visual Studio 2010 の Premium 以上に組み込まれており、DGML ファイルのサポートは Visual Studio 2010 の Ultimate に組み込まれています。

Package Visualizer は `Packages.dgml` という名前のファイルをソリューションのルートに作成します。