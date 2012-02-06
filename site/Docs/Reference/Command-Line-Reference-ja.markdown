<!-- Revision: 6bf8ca8e0121d58c3735cb260b433c10caf4e0c7 2011/11/19 5:55:14 -->
# コマンドラインリファレンス

[ここ](http://nuget.codeplex.com/releases/view/58939)からコマンドラインツールをダウンロードできます。

## Delete コマンド

サーバーからパッケージを削除します。

### 使用法

    nuget delete <パッケージの ID> <パッケージのバージョン> [API キー] [オプション]

サーバーから削除するパッケージの ID とバージョンを指定します。

### オプション

<table>
    <tr>
        <td>Source</td>
        <td>サーバーの URL を指定します。</td>
    </tr>
    <tr>
        <td>NoPrompt</td>
        <td>削除するときに確認を行いません。</td>
    </tr>
    <tr>
        <td>ApiKey</td>
        <td>サーバー用の API キー。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget delete MyPackage 1.0
        
    nuget delete MyPackage 1.0 -NoPrompt





## Help コマンド

一般的なヘルプ情報と、他のコマンドについてのヘルプ情報を表示します。

### 使用法

    nuget help [コマンド]

コマンドについてのヘルプ情報を表示するには、コマンドの名前を渡します。

### オプション

<table>
    <tr>
        <td>All</td>
        <td>使用可能なすべてのコマンドに対する詳細なヘルプを出力します。</td>
    </tr>
    <tr>
        <td>Markdown</td>
        <td>markdown 形式で詳細なすべてのヘルプを出力します。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget help
    
    nuget help push
    
    nuget ?
    
    nuget push -?





##  Install コマンド

指定したソースを使用してパッケージをインストールします。ソースが指定されない場合、`%AppData%\NuGet\NuGet.config` で定義されているすべてのソースが使用されます。`NuGet.config` でソースが何も指定されていない場合、既定の NuGet フィードを使用します。

### 使用法

    nuget install <パッケージ ID | packages.config へのパス> [オプション]

インストールするパッケージの ID を指定し、オプションでバージョンも指定します。ID ではなく `packages.config` ファイルへのパスが使用された場合、ファイルに含まれるすべてのパッケージがインストールされます。

### オプション

<table>
    <tr>
        <td>Source</td>
        <td>インストールに使用するパッケージソースのリスト。</td>
    </tr>
    <tr>
        <td>OutputDirectory</td>
        <td>パッケージがインストールされるディレクトリを指定します。何も指定しない場合、現在のディレクトリを使用します。</td>
    </tr>
    <tr>
        <td>Version</td>
        <td>インストールするパッケージのバージョン。</td>
    </tr>
    <tr>
        <td>ExcludeVersion</td>
        <td>指定した場合、出力先のフォルダーにはパッケージ名のみが含まれ、バージョン番号は含まれません。</td>
    </tr>
    <tr>
        <td>Prerelease</td>
        <td>プレリリースパッケージのインストールを許可します。`packages.config` からインストールしてパッケージを復元する場合には、このフラグは不要です。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget install elmah
    
    nuget install packages.config
    
    nuget install ninject -o c:\foo





## List コマンド

指定したソースからのパッケージの一覧を表示します。ソースが指定されない場合、`%AppData%\NuGet\NuGet.config` で定義されているすべてのソースが使用されます。`NuGet.config` でソースが何も指定されていない場合、既定の NuGet フィードを使用します。

### 使用法

    nuget list [検索用語] [オプション]

オプションの検索用語を指定します。

### オプション

<table>
    <tr>
        <td>Source</td>
        <td>検索するパッケージソースのリスト。</td>
    </tr>
    <tr>
        <td>Verbose</td>
        <td>各パッケージについて詳細な情報の一覧を表示します。</td>
    </tr>
    <tr>
        <td>AllVersions</td>
        <td>パッケージのすべてのバージョンを一覧表示します。既定では、最新のパッケージバージョンのみが表示されます。</td>
    </tr>
    <tr>
        <td>Prerelease</td>
        <td>プレリリースパッケージの表示を許可します。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget list
    
    nuget list -verbose -allversions





## Pack コマンド

指定された NuSpec またはプロジェクトファイルに基づいて、NuGet パッケージを作成します。

### 使用法

    nuget pack <NuSpec | プロジェクト> [オプション]

パッケージの作成用の NuSpec またはプロジェクトファイルの位置を指定します。

### オプション

<table>
    <tr>
        <td>OutputDirectory</td>
        <td>作成される NuGet パッケージファイル用のディレクトリを指定します。指定されない場合、現在のディレクトリを使用します。</td>
    </tr>
    <tr>
        <td>BasePath</td>
        <td>NuSpec ファイルで定義されるファイルの基準となるパスです。</td>
    </tr>
    <tr>
        <td>Verbose</td>
        <td>パッケージのビルドに対して詳細な出力を表示します。</td>
    </tr>
    <tr>
        <td>Version</td>
        <td>NuSpec ファイルのバージョン番号を上書きします。</td>
    </tr>
    <tr>
        <td>Exclude</td>
        <td>パッケージの作成時に除外する 1 つ以上のワイルドカードパターンを指定します。</td>
    </tr>
    <tr>
        <td>Symbols</td>
        <td>ソースとシンボルを含むパッケージを作成すべきかどうかを決定します。NuSpec と一緒に指定した場合、通常の NuGet パッケージファイルと対応するシンボルパッケージを作成します。</td>
    </tr>
    <tr>
        <td>Tool</td>
        <td>プロジェクトの出力ファイルを `tool` フォルダーに配置すべきかどうかを決定します。</td>
    </tr>
    <tr>
        <td>Build</td>
        <td>パッケージのビルド前にプロジェクトをビルドすべきかどうかを決定します。</td>
    </tr>
    <tr>
        <td>NoDefaultExcludes</td>
        <td>.既定の動作である NuGet パッケージファイルと、svn のようなドットで始まるファイルとフォルダーの除外を無効化します。</td>
    </tr>
    <tr>
        <td>NoPackageAnalysis</td>
        <td>パッケージのビルド後にコマンドがパッケージの分析を実行すべきでない場合に指定します。</td>
    </tr>
    <tr>
        <td>Properties</td>
        <td>パッケージの作成時に、セミコロン「;」区切りのプロパティのリストを指定する機能を提供します。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget pack
    
    nuget pack foo.nuspec
    
    nuget pack foo.csproj
    
    nuget pack foo.csproj -Build -Symbols -Properties Configuration=Release
    
    nuget pack foo.nuspec -Version 2.1.0





## Publish コマンド

サーバーにアップロード済みですが、フィードに追加されていないパッケージを公開します。

### 使用法

    nuget publish <パッケージ ID> <パッケージバージョン> <API キー> [オプション]

フィードに公開されるパッケージの ID とバージョンを指定します。

### オプション

<table>
    <tr>
        <td>Source</td>
        <td>サーバーの URL を指定します。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
    
    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s http://example.com/nuget-publish-endpoint
    
    nuget push foo.nupkg
    
    nuget push foo.nupkg.symbols





## Push コマンド

サーバーにパッケージをプッシュし、オプションで公開します。

### 使用法

    nuget push <パッケージのパス> [API キー] [オプション]

パッケージへのパスと、サーバーにパッケージをプッシュするための API キーを指定します。

### オプション

<table>
    <tr>
        <td>CreateOnly</td>
        <td>パッケージを作成してサーバーにアップロードすべきですが、サーバーに公開すべきでない場合に指定します。既定では false です。</td>
    </tr>
    <tr>
        <td>Source</td>
        <td>サーバーの URL を指定します。</td>
    </tr>
    <tr>
        <td>ApiKey</td>
        <td>サーバー用の API キー。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
    
    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s http://customsource/
    
    nuget push foo.nupkg
    
    nuget push foo.nupkg.symbols





## Setapikey コマンド

指定したサーバー URL 用の API キーを保存します。URL が指定されない場合、API キーは NuGet ギャラリー用として保存されます。

### 使用法

    nuget setapikey <API キー> [オプション]

保存する API キーと、オプションで API キーの提供元のサーバーの URL を指定します。

### オプション

<table>
    <tr>
        <td>Source</td>
        <td>API キーが使用可能なサーバーの URL。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
    
    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source http://example.com/nugetfeed





## Sources コマンド

`%AppData%\NuGet\NuGet.config` にあるソースのリストを管理する機能を提供します。

### 使用法

    nuget sources <List|Add|Remove|Enable|Disable> -Name [名前] -Source [ソース]

### オプション

<table>
    <tr>
        <td>Name</td>
        <td>ソースの名前</td>
    </tr>
    <tr>
        <td>Source</td>
        <td>パッケージ（単数または複数）のソースへのパス。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>





## Spec コマンド

新しいパッケージ用の NuSpec を生成します。プロジェクトファイル（.csproj、.vbproj、.fsproj）と同じフォルダーでこのコマンドが実行された場合、トークン化された NuSpec ファイルを作成します。

### 使用法

    nuget spec [パッケージ ID]

### オプション

<table>
    <tr>
        <td>AssemblyPath</td>
        <td>メタデータとして使用するアセンブリ。</td>
    </tr>
    <tr>
        <td>Force</td>
        <td>存在する場合に既存の NuSpec ファイルを上書きします。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget spec
    
    nuget spec MyPackage
    
    nuget spec -a MyAssembly.dll





## Update コマンド

パッケージを最新の使用可能なバージョンに更新します。このコマンドは NuGet.exe 自身も更新します。

### 使用法

    nuget update <packages.config|ソリューション>

### オプション

<table>
    <tr>
        <td>Source</td>
        <td>更新を検索するパッケージソースの一覧。</td>
    </tr>
    <tr>
        <td>Id</td>
        <td>更新するパッケージの ID（複数）。</td>
    </tr>
    <tr>
        <td>RepositoryPath</td>
        <td>ローカルのパッケージフォルダー（パッケージがインストールされる場所）へのパス。</td>
    </tr>
    <tr>
        <td>Safe</td>
        <td>インストールされたパッケージと同じメジャーバージョンとマイナーバージョンの範囲内で使用可能な、最も番号の高いバージョンを使用して更新を期待します。</td>
    </tr>
    <tr>
        <td>Self</td>
        <td>サーバーから使用可能な最新のバージョンを取得して、実行中の NuGet.exe を更新します。</td>
    </tr>
    <tr>
        <td>Verbose</td>
        <td>更新中に詳細な出力を表示します。</td>
    </tr>
    <tr>
        <td>Prerelease</td>
        <td>プレリリースバージョンの更新を許可します。既にインストールされているプレリリースパッケージの更新時には、このフラグは不要です。</td>
    </tr>
    <tr>
        <td>Help</td>
        <td>ヘルプ</td>
    </tr>
</table>

### 例

    nuget update
        
    nuget update -Safe
    
    nuget update -Self
