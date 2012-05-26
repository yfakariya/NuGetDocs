<!-- 11 18 03:43:14 2011 1c15ab3b01cce3b4a6a13755b0d74e9b51c8f6b6 -->
# 自前の NuGet フィードのホスト

会社によっては、開発者が使用できるサードパーティのライブラリを制限しています。そういう会社は公式の NuGet フィードにあるすべてのパッケージに開発者をアクセスさせたくないでしょう。または、公式のフィードに追加できるようにしたくないプロプライエタリなライブラリを持っているかもしれません。

これらのシナリオでは、カスタムの NuGet フィードをセットアップし、そのフィードを公式のフィードの代わりにするか、または公式のフィードに追加するように、Visual Studio を構成できます。フィードはローカル（ローカルマシン上のフォルダーまたはネットワーク共有フォルダー）かリモート（インターネットの URL）にできます。

## ローカルフィードの作成

まずは、カスタムフィードを含むパッケージを作成、または取得し、それからあるフォルダーにそれらすべてを配置します。次の例では、フォルダーはローカルの *c:* ドライブに作成されています。
そのフォルダーは単一のパッケージ（.nupkg ファイル）を含んでいます。

![ローカルの NuGet フィードフォルダー](images/LocalNuGetFeed-folder.png)

次に、そのフォルダーを NuGet フィードの場所として指定します。Visual Studio の **ツール**（**Tools**）メニュから **ライブラリ パッケージ マネージャー**（**Library Package Manager**）を選択し、**パッケージ マネージャーの設定**（**Package Manager Settings**）をクリックします。

![メニュでの Package Manager の設定](images/Package-Manager-Settings-in-menu.png)

**オプション**（**Options**）ダイアログボックスが表示されます。

![オプションダイアログボックス](images/Options-dialog-box.png)

**名前**（**Name**）テキストボックスには、フィードの名前を入力します。
**ソース**（**Source**）テキストボックスには、パッケージのあるフォルダーのパスを入力します。

![新しいフィードのない利用可能なパッケージ ソース ダイアログボックス](images/Available-Package-Sources-without-new-feed.png)

**追加**（**Add**）をクリックします。ローカルのフォルダーがもう一つの NuGet フィードソースになります。

![新しいフィードのある利用可能なパッケージ ソース ダイアログボックス](images/Available-Package-Sources-with-new-feed.png)

この新しいフィードを使用してパッケージをインストールするには、**パッケージ マネージャー コンソール**（**Package Manager Console**）ウィンドウで、**パッケージ ソース**（**Package source**）リストで新しいフィードを選択します。

![パッケージ マネージャー コンソールでのローカルフィードの選択](images/Selecting-local-feed-in-Package-Manager-Console.png)

**NuGet パッケージの管理**（**Manage NuGet Packages**）ダイアログボックスの**オンライン**（**Online**）タブで新しいフィードを選択することもできます。

![NuGet パッケージの管理ダイアログでのローカルフィードの選択](images/Selecting-local-feed-in-Add-Library-Package-Reference.png)

## リモートフィードの作成

IIS を実行しているサーバー上でリモート（または内部）フィードをホストすることもできます。

<p class="caution">NuGet 1.4 以降のバージョンを実行しているか確認してください！</p>

### ステップ 1：Visual Studio での新しい空の Web アプリケーションを作成する

**ファイル** | **新規作成** | **プロジェクト** メニュオプションを選択し（または Ctrl + Shift + N を押し）、新規プロジェクトの作成ダイアログを表示させ、次のスクリーンショットのように **ASP.NET 空の Web アプリケーション** を選択します。

![新しいプロジェクト ダイアログボックス](images/New-Project-dialog-box.png)

これによって完全に空のプロジェクトのテンプレートが作成されます。

![ソリューション エクスプローラーでの新しいプロジェクト](images/New-project-in-Solution-Explorer.png)

### ステップ 2：NuGet.Server Package をインストールする

ここで、**参照** ノードを右クリックして **NuGet パッケージの管理**（**Manage NuGet Packages**）を選択し、NuGet のダイアログを起動します（またはパッケージ マネージャー コンソール（Package Manager Console）を使用して、 `Install-Package NuGet.Server` と入力します）。

**オンライン**（**Online**）タブをクリックしてから、右上の検索ボックスに **NuGet.Server** と入力します。
次の画像のように表示される **NuGet.Server** パッケージの **Install** をクリックします。

![NuGet.Server パッケージ](images/NuGet.Server-package.png)

### ステップ 3：Packages フォルダーを構成する

`NuGet.Server` 1.5 以降では、パッケージを含めるフォルダーを構成できます。web.config ファイルに、`packagesPath` という名前の新しい `appSetting` が含まれています。このキーのエントリが省略されたり、値が空のままの場合、パッケージフォルダーは既定の ~/Packages になります。値として、**絶対パス** か **仮想パス** のいずれかを指定できます。

    <appSettings>
        <!-- カスタムパッケージフォルダーを指定するための値を設定します。 -->
        <add key="packagesPath" value="C:\MyPackages" />
    </appSettings>


### ステップ 4：パッケージフォルダーにパッケージを追加する

文字通りです！　**NuGet.Server** パッケージが空の Web サイトを OData パッケージフィードを提供できるサイトに変換します。
パッケージフォルダーにパッケージを追加するだけで、パッケージが表示されるようになります。

次のスクリーンショットでは、既定の **Packages** フォルダーにいくつかのパッケージが手動で追加されていることがわかると思います。

![Packages フォルダーへのパッケージの追加](images/Adding-packages-to-the-packages-folder.png)

<p class="info">このパッケージを発行したい（たとえば、アプリケーションのメニュの ビルド -> 発行 から）場合には、ソリューションエクスプローラーで .nupkg ファイルを選択して、ビルド アクションプロパティを「コンテンツ」に変更する必要もあります。</p>

`NuGet.Server` 1.4 以降では、NuGet.exe を使用して、軽量フィードに対するパッケージの追加または削除もできます。パッケージをインストールすると、web.config ファイルには `apiKey` という名前の新しい `appSetting` が含まれています。このキーのエントリが省略されたり、値が空のままの場合、フィードに対するパッケージのプッシュは無効になります。apiKey に値を設定する（理想的には強力なパスワードにします）と、NuGet.exe を使用したパッケージのプッシュが有効になります。

    <appSettings>
        <!-- ここで値を設定すると、ユーザーがサーバーに対するパッケージのプッシュとデリートを実行できるようになります。
             注：この値（パスワード）はすべてのユーザーで共有されます。 -->
        <add key="apiKey" value="" />
    </appSettings>


### ステップ 5：新しいパッケージフィードを配置して実行します！

Ctrl + F5 を押してサイトを実行できます。そうすると、次に何をするのかを示す情報が表示されます。

![パッケージフィードのホームーページ](images/Package-feed-home-page.png)

「here」をクリックすると、パッケージが Atom 形式の OData フィードで表示されます。

![Atom パッケージフィード形式の OData](images/OData-over-ATOM-package-feed.png)

ここで、他のサイトと同じように Web サイトを配置するために必要なことが終わりました。次のスクリーンショットのように、（Visual Studio の NuGet のダイアログで）Settings ボタンをクリックして、このフィードをパッケージソースのセットに追加できます。


![パッケージソースへの新しいフィードの追加](images/Adding-new-feed-to-package-sources.png)

入力する必要のある <a href="http://yourdomain/nuget/">http://yourdomain/nuget/</a> という URL は、サイトの配置方法に依存して変わることに注意してください。

はい、簡単でしたね！　このフィードは「読み取り専用」であり、NuGet.exe コマンドラインツールによる発行をサポートしていないことに注意してください。代わりに、**Packages** フォルダーにパッケージを追加することで、自動的にシンジケートされます。
