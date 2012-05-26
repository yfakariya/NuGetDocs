<!-- 4 17 06:52:34 2012 0d8830f8c2067c292c497cd65ac92afb0ab78dd9 -->
# バージョニング

NuGet パッケージの作成時には、`.nuspec` ファイルにパッケージの依存先を指定できます。依存先パッケージのどのバージョンが必要なのかも指定します。NuGet パッケージの作成と .nuspec ファイルの形式の情報については、[パッケージの作成と公開](../creating-packages/creating-and-publishing-a-package-ja)と[.nuspec ファイル形式の仕様](nuspec-reference-ja)を参照してください。

依存先のバージョンは `dependency` 要素の `version` 属性で指定されます。たとえば、次の `dependency` 要素は、`ExamplePackage` という名前のパッケージの、バージョン 1.3.2 以上に対する依存関係を指定しています。

    <dependency id="ExamplePackage" version="1.3.2" />

## .nuspec ファイルでバージョン範囲を指定する

NuGet はバージョンの範囲の指定に区間記法の使用をサポートします。NuGet の使用は Mave の Version Range Specification に触発されましたが、まったく同じではありません。バージョン範囲の指定方法を要約すると以下のとおりです。

    1.0	 = 1.0 ≤ x
    (,1.0]	= x ≤ 1.0
    (,1.0)	= x < 1.0
    [1.0] = x == 1.0
    (1.0) = invalid
    (1.0,) = 1.0 < x
    (1.0,2.0) = 1.0 < x < 2.0
    [1.0,2.0] = 1.0 ≤ x ≤ 2.0
    empty = latest version.

## 例

次の例は、`ExamplePackage` の 1 または 2 から始まるいずれかのバージョンに対する依存関係を指定します。
角かっこは 1 が範囲に含まれることを示しますが、丸かっこは 3 が含まれないことを示します。

    <dependency id="ExamplePackage" version="[1,3)" />

この例では、バージョン 1 とバージョン 2.9 は使用可能ですが、0.9 や 3.0 は使用できません。

次の例は `ExamplePackage` の 1.3.2 から、1.4 で始まる任意のバージョン番号への依存関係を指定します。角かっこはバージョン 1.3.2 が含まれることを示しますが、丸かっこは 1.5 が含まれないことを示します。

    <dependency id="ExamplePackage" version="[1.3.2,1.5)" />

この例では、バージョン 1.3.2.1 とバージョン 1.4.999 は使用可能ですが、バージョン 1.5 は使用できません。

## アドバイス

一般的に、ほとんどのケースでのアドバイスは、次の例のように、下限のみを指定し、上限はオープンなままにしておくことです。

    <dependency id="ExamplePackage" version="1.3.2" />

## プレリリース版

ほとんどの人は NuGet からパッケージをインストールする際に、パッケージの最新「安定版」リリースを要求します。
他の開発者は最先端で活動し、パッケージの最新のプレリリース版を取得したいかもしれません。

NuGet 1.6 から、NuGet は[セマンティックバージョン指定仕様（Semantic Versioning (SemVer) specification）](http://semver.org/)に則ったバージョン番号にプレリリース文字列を指定することによる、プレリリースパッケージの作成をサポートします。

### SemVer のとても簡単な紹介

[SemVer 仕様](http://semver.org/)は、SemVer の詳細な理解を得るために最適な場所です。これは、急いでいる人のための、SemVer の簡単な要約です。

SemVer は、バージョン番号に一定の意味がつけられている公開 API のバージョン指定の規約です。各バージョンは、_メジャー.マイナー.パッチ_ の 3 つの部分からなります。

手短に言うと、それらは以下のように対応します。
In brief, these correspond to:
* __メジャー__: 互換性のない変更。
* __マイナー__: 新機能、ただし完全に後方互換。
* __パッチ__: 後方互換性のあるバグフィックスのみ。

さらに、_パッチ_番号にハイフンで区切って任意の文字列を追加することで、API のプレリリース版を指定できます。たとえば、以下のとおりです。

* `1.0.1-alpha`
* `1.0.1-beta`
* `1.0.1-Fizzleshnizzle`

実際に追加される文字列は何でもよいことに注意してください。文字列が存在するならば、プレリリース版となります。

リリースの準備ができたならば、ハイフンとその後ろの文字列を除去すると、そのバージョンはそれまでのあらゆるプレリリース版よりも「大きい」とみなされます。たとえば、安定版の `1.0.1` は `1.0.1-rc` よりも大きくなります。

プレリリース版はアルファベット順（より技術的に言えば、ASCII ソート順での辞書順）の優先順位が与えられます。

したがって、パッケージの最も小さいバージョンから最も大きいバージョンまでの並びは次のようになります。

* `1.0.1-alpha`
* `1.0.1-alpha2`
* `1.0.1-beta`
* `1.0.1-rc`
* `1.0.1-zeeistalmostdone`
* `1.0.1`

SemVer は、日次ビルドや継続的ビルドを作成するためのビルド番号のコンセプトも導入します。これは公開されている NuGet.org のギャラリーではサポートされていません。

### プレリリース版パッケージを作成する

前述のように、プレリリース版を作成するには、単にプレリリース文字列を持つバージョンを与えます。
これを行う方法は 2 つあります。

1. NuSpec ファイルで、&lt;version/&gt; 要素でバージョンを指定します。

        <version>1.0.1-alpha</version>

2. プロジェクト（.csproj や .vbproj）からパッケージをビルドする場合には、バージョンの指定に `AssemblyInformationalVersionAttribute` を使用します。

        [assembly: AssemblyInformationalVersion("1.0.1-alpha")]

NuGet は `AssemblyVersion` 属性（この属性は SemVer をサポートせず、これが別の属性が必要になった理由です）で指定されたバージョン番号ではなく、`AssemblyInformationalVersion` 属性で指定された値を使用します。

### プレリリース版パッケージをインストールする

既定では、ダイアログやコンソールにプレリリース版のパッケージを表示しません。NuGet 1.6 では、_Manage NuGet Packages_ ダイアログを使用してプレリリース版のパッケージをインストールする方法はありません。これは将来的に変更される可能性があります。

プレリリース版パッケージのインストールを許可するには、_Package Manager Console_ を使用して、`-IncludePrerelease` フラグを指定します。

    Install-Package CoolStuff -IncludePrerelease

このコマンドはパッケージのセットにプレリリース版のパッケージを含めます。プレリリース版のパッケージが最新版ならば、それがインストールされます。安定板のパッケージが最新ならば、それがインストールされます。このフラグはプレリリース版のインストールを強制するものではありません。

プレリリース版のパッケージのインストール用に、`-Pre` エイリアスを使用してもかまいません。

    Install-Package CoolStuff -Pre

NuGet は、安定版のパッケージからプレリリース版のパッケージへの参照を許可しません。これによって、安定版のパッケージのみを対象としたい場合に、プレリリース版のパッケージがインストールされてしまう事故が避けられます。

### プレリリース版のパッケージをアップグレードする

`Update-Package` コマンドはすべてのパッケージを最新の安定版にアップデートします。前述のように、安定版かプレリリース版の最新版のより新しいほうのパッケージにアップグレードするには、`-IncludePrerelease` フラグを使用すします。

## 許可されたバージョンに対するアップグレードに制限する

既定では、パッケージに対して `Update-Package` コマンドを実行すると（あるいは、ダイアログを使用してパッケージをアップデートすると）、フィード内の最新のバージョンにアップデートされます。すべてのパッケージのアップデートに対する新しいサポートを使用して、パッケージを特定のバージョン範囲にロックしたい場合があります。たとえば、事前に、アプリケーションがパッケージの 2.* でのみ動作し、3.0 以上では動作しないことが分かっていることがあります。誤ってパッケージをバージョン 3 にアップデートしてしまうことを防ぐために、NuGet は、アップグレードできるパッケージのバージョンの範囲の制限をサポートします。これは、`package.config` ファイルを手動で編集して、`allowedVersions` 属性を使用して行います。

たとえば、以下の例は `SomePackage` パッケージをバージョン範囲 2.0～3.0（3.0 は含まない）にロックする方法を示しています。`allowedVersions` 属性は、前述のバージョン範囲形式を使用した値を受け取ります。

    <?xml version="1.0" encoding="utf-8"?>
    <packages>
        <package id="SomePackage" version="2.1.0" allowedVersions="[2,3)" />
    </packages>

現在、パッケージを特定のバージョン範囲にロックするには、`packages.config` ファイルを手動で編集する必要があります。

    PM> Enable-PackageRestore
    Attempting to resolve dependency 'NuGet.CommandLine (≥ 1.4)'.
    Successfully installed 'NuGet.CommandLine 1.4.20615.182'.
    Successfully installed 'NuGet.Build 0.16'.
 
    Copying nuget.exe and msbuild scripts to D:\Code\StarterApps\Mvc3Application\.nuget
    Successfully uninstalled 'NuGet.Build 0.16'.
    Successfully uninstalled 'NuGet.CommandLine 1.4.20615.182'.
 
    Don't forget to commit the .nuget folder
    Updated 'Mvc3Application' to use 'NuGet.targets'
    Enabled package restore for Mvc3Application

これで完了です！　基本的には、1 番目のコマンドはいくつかの有用なコマンドを含む NuGet パッケージをインストールし、2 番目のコマンドはそれらのコマンドを実行します。

この後、ソリューションの下に、nuget.exe と 1 組の MSBuild 用ターゲットファイルを含む新しい .nuget フォルダーがあることに気づくでしょう。ソースコード管理にこのフォルダーをコミットするようにしてください！　ビルド時に復元機能が起動されるように、csproj ファイルにちょっとした変更が行われていることにも気づくことでしょう。
