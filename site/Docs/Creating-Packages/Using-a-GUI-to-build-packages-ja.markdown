<!-- 10 24 05:55:09 2011 18c624d00d31e023241ae8e94fe4531f9d1c1e6d -->
# パッケージのビルドへの GUI（Package Explorer）の使用

全員がコマンドライン大好き人間なわけではありません。実際、人によっては、パッケージの作成のようなタスクをこなすのに、Windows GUI アプリケーションのほうが快適だと思っています。NuGet Package Explorer という Click-Once アプリケーションによって、パッケージの作成が非常に簡単になります。このアプリケーションはパッケージの調査やパッケージの構成方法を学ぶ優れた手段でもあります。

ビルドシステムにパッケージのビルドを統合したいならば、[パッケージの作成と発行に NuGet.exe を使用する](~/docs/creating-packages/Creating-and-Publishing-a-Package)のほうが優れています。

## インストール

Package Explorer のインストールは簡単です。[ここをクリックするだけです](http://nuget.codeplex.com/releases/59864/clickOnce/NuGetPackageExplorer.application)！

Package Explorer は Click-Once アプリケーションなので、起動するたびに、更新があるかをチェックし、アプリケーションを最新にしつづけることができます。

![Package Explorer の更新が利用可能](images/package-explorer-update-available.png)

## パッケージの作成

パッケージを作成するには、Package Explorer を起動し、**File** > **New** メニュオプションを選択します（または、Ctrl + N を押します）。

![新しいパッケージの作成](images/package-explorer-file-new.png)

それから、**Edit** > **Edit Package Metadata** メニュオプションを選択し（または Ctrl + K を押し）、パッケージのメタデータを編集します。

![Package Explorer でのメタデータの編集](images/package-explorer-metadata.png)

メタデータエディターは、nuspec ファイルの編集用の GUI エディターです。フィールドの詳細については、[NuSpec のリファレンス](~/docs/reference/nuspec-reference) を参照してください。

最後のステップは、パッケージのコンテンツを **Packages contents** ペインにドラッグすることです。Package Explorer はパッケージの正しいディレクトリに配置するために、コンテンツが属する先について推論しようとし、確認してきます。

たとえば、Packages contents ウィンドウにアセンブリをドラッグすると、Package Explorer はアセンブリを **lib** フォルダーに配置するかどうか確認してきます。

![Package Explorer によるコンテンツの場所の推論](images/package-explorer-content-inference.png)

**OK** をクリックすると、ファイルが適切なフォルダーに配置されます。

![Package Explorer の lib フォルダー](images/package-explorer-lib-folder.png)

**Content** メニュ経由で、特殊なフォルダーに明示的に追加することもできます。

![Package Explorer の Content](images/package-explorer-content.png)

**File** > **Save** メニュオプション（または Ctrl + S）でパッケージを保存することを忘れないようにしてください。

## パッケージの発行

パッケージの作成と保存が終わったら、**File** > **Publish** メニュオプション（Ctrl + P）を行って、パッケージを発行します。

すると、**Publish Package** ダイアログが表示されます。

![Publish Package ダイアログ](images/package-explorer-publish.png)

事前に取得した API キーを入力し、**Publish** をクリックしてパッケージを NuGet パッケージフィードに発行します。アカウントをまだ持っていないならば、[NuGet Gallery](http://nuget.org/) に行って、アカウントを登録してください。

登録してログインし、**[My Account**](http://nuget.org/Contribute/MyAccount) リンクをクリックして API アクセスキーを確認してください。
