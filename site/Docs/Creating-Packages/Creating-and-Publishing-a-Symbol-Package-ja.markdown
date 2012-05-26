<!-- Revision: 18c624d00d31e023241ae8e94fe4531f9d1c1e6d 2011/10/24 5:55:09 -->
# シンボルパッケージの作成と公開

## はじめに

ライブラリ/コンテンツのパッケージのビルドと [NuGet.org](http://nuget.org) への公開とは別に、NuGet はシンボル/ソースのパッケージのビルドと [SymbolSource.org](http://symbolsource.org) への公開もサポートします。パッケージが両方のリポジトリに公開されている場合、インストール済みのパッケージに関連付けられた PDB ファイルを自動的にダウンロードし、開発者が Visual Studio からオンデマンドにデバッガーでソースファイルにステップインできるように Visual Studio を構成できます。これは IDE の組み込みの機能であり、[Microsoft Reference Source](http://referencesource.microsoft.com/) サーバーを使用する .NET Framework のコードのデバッグにも使用できる機能です。必要なことは、デバッガーの設定で次の新しいシンボル ソースを追加するだけです（詳細な方法については [ここ] (http://www.symbolsource.org/Public/Home/VisualStudio) を参照してください）。

	http://srv.symbolsource.org/pdb/Public

## シンボルパッケージの作成

シンボルパッケージを作成して名前を付けるときには、以下の規約に従う必要があります。

* ライブラリ/コンテンツ パッケージは `MyPackage.nupkg` という名前にし、PDB ファイル **以外** のすべてのファイルを含めるべきです。
* シンボル/ソース パッケージは `MyPacakge.symbols.nupkg` という名前にし、DLL、PDB、XML ドキュメント、およびソースファイルのみを含めるべきです。

`-Symbols` オプションを使用して、両方のパッケージを作成することができます。次のように nuspec ファイルを指定するときにオプションを指定します。

    NuGet Pack MyPackage.nuspec -Symbols

または、プロジェクトファイルを指定するときにオプションを指定します。

    NuGet Pack MyProject.csproj -Symbols

## シンボルパッケージの構造

シンボルパッケージは、ライブラリパッケージと同じ方法で、複数のターゲットフレームワークを対象にできます。そのため、 `lib` フォルダーの構造は [パッケージの作成と公開](~/docs/creating-packages/Creating-and-Publishing-a-Package-ja) で説明されているようにすべきです。さらに、DLL の横に PDB ファイルを含めるようにすべきです。たとえば、.NET 4.0 と Silverlight 4 をターゲットにするシンボルパッケージは、次のような構造を持ちます。
	
    \lib
        \net40
            \MyAssembly.dll
            \MyAssembly.pdb
        \sl40
            \MyAssembly.dll
            \MyAssembly.pdb

ソースファイルは `src` という独立した特別なフォルダーに配置されます。このフォルダーはソースrepositoryの相対的な構造に従わなければなりません。PDB には対応する DLL をコンパイルするために使用されたソースファイルの絶対パスを含めなければならず、かつ [SymbolSource.org](http://symbolsource.org) に公開するときに見つかる場所にある必要があるためです。ベースパス（パスの共通のプリフィックス）は取り除くことができます。例として、以下のファイルからビルドされたライブラリを考えます。

    C:\Projects
        \MyProject
            \Common
                \MyClass.cs
            \Full
                \Properties
                    \AssemblyInfo.cs
                \MyAssembly.csproj (\lib\net40\MyAssembly.dll を生成します)
            \Silverlight
                \Properties
                    \AssemblyInfo.cs
                \MySilverlightExtensions.cs
                \MyAssembly.csproj (\lib\sl4\MyAssembly.dll を生成します)

`lib` フォルダーとは別に、シンボルパッケージでは次のレイアウトを含む必要があります。

    \src
        \Common
            \MyClass.cs
        \Full
            \Properties
                \AssemblyInfo.cs
            \Silverlight
                \Properties
                    \AssemblyInfo.cs
                \MySilverlightExtensions.cs

## シンボルパッケージの内容を指定する

シンボルパッケージは、前のセクションで説明した方法で構造化されたフォルダーから規約ベースで構築できます。そうではなく、`files` セクションを使用してコンテンツを指定することもできます。先ほど説明したパッケージ例をビルドしたい場合には、nuspec ファイルに次のように記述します。

    <files>
        <file src="Full\bin\Debug\*.dll" target="lib\net40" /> 
        <file src="Full\bin\Debug\*.pdb" target="lib\net40" /> 
        <file src="Silverlight\bin\Debug\*.dll" target="lib\sl40" /> 
        <file src="Silverlight\bin\Debug\*.pdb" target="lib\sl40" /> 
        <file src="**\*.cs" target="src" />
    </files>

## シンボルパッケージの公開

後で楽なように、いつもようにまず API キーを保存します。

    NuGet SetApiKey Your-API-Key

これによって [NuGet.org](http://nuget.org) と [SymbolSource.org](http://symbolsource.org) の両方に対する API キーを保存することになります。SymbolSource に公開すると、SymbolSource は NuGet Gallery に問い合わせ、あなたがプロジェクトのオーナーであるかどうかを検証します。

シンボルパッケージは個別にプッシュすることができます。この場合、ターゲットとして [SymbolSource.org](http://symbolsource.org) が自動的に選択されます。

    NuGet Push MyPackage.symbols.nupkg

`-Source` オプションを使用して、シンボル用のリポジトリをオーバーライドしたり、名前付け規約に従っていないシンボルパッケージをプッシュしたりもできます。

    NuGet Push MyPackage.symbols.nupkg -Source http://nuget.gw.symbolsource.org/Public/NuGet

同時に両方のパッケージを両方のリポジトリにプッシュすることもできます。

    NuGet Push MyPackage.nupkg

`.symbols.nupkg` パッケージがあることが検出されると、自動的に [SymbolSource.org](http://symbolsource.org) にプッシュされます。


