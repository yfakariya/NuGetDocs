<!-- Revision: 280628f091e13c62cba8f55438af947ed084b1e8  2012/01/06 3:44:02 -->
# Nuspec リファレンス

.nuspec ファイルは、パッケージを記述するための、XML を使用するマニフェスト（目録）です。このマニフェストはパッケージのビルドに使用されるだけでなく、パッケージがビルドされるとパッケージ内に保存されます。パッケージのビルド方法については、[パッケージの作成](~/docs/creating-packages/Creating-and-Publishing-a-Package-ja.markdown) を参照してください。

.nuspec ファイルにおいて、トップレベルの **package** 要素には、パッケージとその依存先を記述する **metadata** 要素が含まれます。オプションで、**package** 要素はパッケージに含まれるファイルを指定する **files** 要素を含むことができます。**files** 要素が省略されている場合、.nuspec ファイルと同じフォルダーにあるすべてのファイルとフォルダーがパッケージに含まれます。.nuspec ファイルはパッケージのビルド後にパッケージに含められます（ただし、ファイルをリストアップする要素が含まれていた場合、その要素は除外されます）。

## メタデータセクション

<table class="reference">
<tbody>
    <tr>
        <th>要素</th> <th>説明</th>
    </tr>
    <tr>
        <td><strong>id</strong></td>
        <td>
            パッケージの一意な識別子です。これは、Package Manager Console を使用してパッケージを一覧表示したときに表示されるパッケージ名です。パッケージ ID は Package Manager Console で Install-Package コマンドを使用してパッケージをインストールするときにも使用されます。この ID には、一切の空白文字、および URL として不正な文字を含めることはできません。おおまかには、.NET の名前空間と同じ規則に従います。そのため、<code>Foo.Bar</code> は正しい ID ですが、<code>Foo!</code> や <code>Foo Bar</code> は正しくありません。
        </td>
    </tr>
    <tr>
        <td><strong>version</strong></td>
        <td>パッケージのバージョンです。形式は <code>1.2.3</code> のような形式です。</td>
    </tr>
    <tr>
        <td>title</td>
        <td>
            Manage NuGet Packages ダイアログで表示される、パッケージの人間が読むことができるタイトルです。何も指定しない場合には、ID が使用されます。
        </td>
    </tr>
    <tr>
        <td><strong>authors</strong></td>
        <td>パッケージコードの記述者のカンマ区切りのリストです。</td>
    </tr>
    <tr>
        <td>owners</td>
        <td>
            パッケージの作成者のカンマ区切りのリストです。これは authors と同じリストとなることがよくあります。NuGet.org のギャラリーにパッケージをアップロードする際、この値は無視されます。
        </td>
    </tr>
    <tr>
        <td><strong>description</strong></td>
        <td>
            パッケージの長い説明です。これは Add Package ダイアログの右ペインに表示されるほか、<code>Get-Package</code> コマンドを使用して Package Manager Console でパッケージの一覧表示を行う際にも表示されます。
        </td>
    </tr>
    <tr>
        <td>releaseNotes</td>
        <td>
            パッケージの各リリースで行われた変更の説明です。このフィールドは _Updates_ タブが選択され、かつそのパッケージが以前インストールされたパッケージに対する更新である場合にのみ表示されます。この値は、普段は description が表示される場所に表示されます。
        </td>
    </tr>
    <tr>
        <td>summary</td>
        <td>
            パッケージの短い説明です。指定されると、Add Package ダイアログの中央ペインに表示されます。指定されない場合、description の後ろのほうが切り捨てられたものが使用されます。
        </td>
    </tr>
    <tr>
        <td>language</td>
        <td>en-us のようなパッケージのロケール ID です。</td>
    </tr>
    <tr>
        <td>projectUrl</td>
        <td>パッケージのホームページの URL です。</td>
    </tr>
    <tr>
        <td>iconUrl</td>
        <td><strong>Manage NuGet Packages</strong> ダイアログボックスでパッケージ用のアイコンとして使用される画像の URL です。これは、背景が透過になっている、32×32 ピクセルの <em>.png</em> ファイルにすべきです。</td>
    </tr>
    <tr>
        <td>licenseUrl</td>
        <td>パッケージに適用するライセンス条項へのリンクです。</td>
    </tr>
    <tr>
        <td>copyright</td>
        <td>（v<em>1.5</em>）パッケージの著作権に関する詳細事項です。</td>
    </tr>
    <tr>
        <td>requireLicenseAcceptance</td>
        <td>パッケージをインストールする前に、ユーザーがパッケージのライセンス（licenseUrl に記述されたものです）を受け入れる必要があるかどうかを示す真偽値です。</td>
    </tr>
    <tr>
        <td>dependencies</td>
        <td>パッケージの依存先のリストです。さらなる情報については、このドキュメントで述べる <a href="#依存先の指定">依存先の指定</a>を参照してください。</td>
    </tr>
    <tr>
        <td>references</td>
        <td>(v<em>1.5</em>) プロジェクト参照として追加される、lib の下のアセンブリの名前です。指定されない場合、lib 内のすべてのアセンブリに対する参照がプロジェクトに追加されます。参照を指定する場合には、名前のみを指定し、パッケージ内のパスを指定しないでください。</td>
    </tr>
    <tr>
        <td>frameworkAssemblies</td>
        <td>(v<em>1.2+</em>) パッケージが必要とする .NET Framework のアセンブリへの参照のリストです。参照は .NET Framework に存在するアセンブリへの参照で、かつどのマシンでも GAC に存在するものにすべきです。フレームワークのアセンブリへの参照を指定することで、パッケージのインストール時にそれらのアセンブリへの参照が追加されるようになります。</td>
    </tr>
    <tr>
        <td>tags</td>
        <td>パッケージを説明する、空白区切りのタグとキーワードのリストです。この情報は、ユーザーが <strong>Add Package Reference</strong> ダイアログボックスの検索機能を使用してパッケージを検索できるようにしたり、<strong>Package Manager Console</strong> ウィンドウでフィルター処理を行えるようにしたりするために使用されます。</td>
    </tr>
</tbody>
</table>

### 置換トークン

置換トークンは、[プロジェクトファイルからパッケージを作成するとき](../creating-packages/creating-and-publishing-a-package#From_a_project) に NuSpec ファイルの Metadata セクションの値を置き換えるために使用できます。

たとえば、次のコマンドを使用する場合、

    nuget pack MyProject.csproj

`MyProject.csproj` の隣にある `MyProject.nuspec` ファイルは、プロジェクト内の値を使用して生成される、次の置換トークンを含むことができます。

<table class="reference">
    <tr><th>トークン</th><th>ソース</th></tr>
    <tr>
        <td>$id$</td>
        <td>アセンブリ名</td>
    </tr>
    <tr>
        <td>$version$</td>
        <td>アセンブリの <code>AssemblyVersionAttribute</code> で指定されるアセンブリ バージョン。</td>
    </tr>
    <tr>
        <td>$author$</td>
        <td><code>AssemblyCompanyAttribute</code> で指定される企業名。</td>
    </tr>
    <tr>
        <td>$description$</td>
        <td><code>AssemblyDescriptionAttribute</code> で指定される説明。</td> 
    </tr>
    <tr>
        <td>$references$</td>
        <td>
            この要素は <code>&lt;reference&gt;</code> のセットを含み、各要素はプロジェクトによって参照されるアセンブリを指定します。
            この要素が存在する場合、 <code>lib</code> フォルダーにあるすべてのアセンブリを参照するという規約がオーバーライドされます。
            詳細については後述します。
        </td>
    </tr>
</table>

## 依存関係の指定

dependencies 要素は metadata 要素の子要素で、 dependency 要素のセットを含みます。
各 dependency 要素はこのパッケージが依存する別のパッケージへの参照です。

    <dependencies>
      <dependency id="RouteMagic" version="1.1.0" />
      <dependency id="RouteDebugger" version="1.0.0" />
    </dependencies>

次の表は dependency 要素の属性の一覧です。

<table class="reference">
    <tr><th>属性</th><th>説明</th></tr>
    <tr>
        <td><code>id</code></td>
        <td>このパッケージが依存するパッケージのパッケージ ID。dependency 要素の id 属性で指定されるパッケージは、このパッケージにとって必須のパッケージであり、NuGet はこのパッケージのインストール時に依存先のパッケージをインストールします。</td>
    </tr>
    <tr>
        <td><code>version</code></td>
        <td>依存先として受け入れられるバージョンの範囲です。一般的に、最小のバージョンを表すバージョン番号のみになります。とはいえ、より明示的な<a href="versioning#.nuspecファイルでのバージョン範囲の指定">バージョン範囲構文</a>がサポートされています。</td>
    </tr>
</table>

## 明示的なアセンブリ参照の指定

`<references />` 要素を使用して、対象のプロジェクトが参照すべきアセンブリを明示的に指定できます。

たとえば、次のように追加したとします。

    <references>
      <reference file="xunit.dll" />
      <reference file="xunit.extensions.dll" />
    </references>

`lib` フォルダーからは、たとえそのフォルダーに他にもアセンブリがあったとしても、
_xunit.dll_ と _xunit.extensions.dll_ のみが適切な [framework/profile サブディレクトリ](../creating-packages/creating-and-publishing-a-package#Grouping_Assemblies_by_Framework_Version) から参照されます。

この要素が省略されている場合、通常の動作が適用され、 lib フォルダー内のすべてのアセンブリが参照されます。

__この機能は何に使うのか？__

この機能はデザイン時のみのアセンブリをサポートします。たとえば、コード コントラクトを使用する場合、Visual Studio が検索できるように、コントラクト アセンブリは対象となる実行時アセンブリの隣にある必要があります。
ところが、コントラクト アセンブリが実際にプロジェクトから参照されるべきではありませんし、 bin フォルダーにコピーされるべきでもありません。

同様に、この機能は、実行時アセンブリの隣にツール アセンブリを配置する必要があるけれども、プロジェクト参照からは除外する必要のある、XUnit のような単体テストフレームワークでも使用可能です。

## フレームワークアセンブリ（GAC）の参照の指定

場合によっては、パッケージは .NET Framework にあるアセンブリに依存する可能性があります。厳密にいうと、パッケージの使用者から .NET Framework のアセンブリへの参照が常に必要となるとは限りません。ただし、たとえば開発者がパッケージを使用するために .NET Framework のアセンブリにある型を使用するコードが必要となるような場合、アセンブリへの参照が重要になります。metadata 要素の子要素である `<frameworkAssemblies>` 要素を使用して、GAC にある .NET Framework のアセンブリを指す frameworkAssembly 要素のセットを指定できます。
.NET Framework のアセンブリとわざわざ言っていることに注意してください。これらのアセンブリは、.NET Framework の一部としてすべてのマシンに存在するものとみなされ、パッケージには含まれません。

    <frameworkAssemblies>
      <frameworkAssembly assemblyName="System.ServiceModel" targetFramework="net40" />
      <frameworkAssembly assemblyName="System.SomethingElse"  />
    </frameworkAssemblies>

次の表は frameworkAssembly 要素の属性の一覧です。
<table class="reference">
    <tr>
        <th>属性</th>
        <th>説明</th>
    </tr>
    <tr>
        <td>assemblyName</td>
        <td>
            <em>必須</em> 完全修飾アセンブリ名です。
        </td>
    </tr>
    <tr>
        <td>targetFramework</td>
        <td>
            <em>省略可能。</em>指定された場合、この参照の適用先となる特定のターゲットフレームワークになります。
            たとえば、参照が .NET 4 でのみ適用されるならば、値を "net40" にします。
            参照が *すべての* フレームワークに適用されるならば、この属性を省略します。
        </td>
    </tr>
</table>

## パッケージに含まれるファイルの指定

パッケージの作成で説明した規約に従うならば、.nuspec ファイルにファイルの一覧を明示的に指定しなくてもかまいません。ファイルを指定すると、規約は無視され、.nuspec ファイルにリストアップされたファイルのみがパッケージに含まれるようになることに注意してください。

files 要素は package 要素の省略可能な子要素で、file 要素のセットを含みます。
各 file 要素はパッケージに含むファイルのコピー元を示す `src` 属性とコピー先を示す `target` 属性を指定します。

<table class="reference">
    <tr>
        <th>属性</th>
        <th>説明</th>
    </tr>
    <tr>
        <td>src</td>
        <td>
            含めるファイル（単数または複数）の場所。パスは NuSpec ファイルに対する相対パスか、絶対パスを指定します。
            ワイルドカード文字 * を使用できます。
            2 文字のワイルドカード ** は、再帰的なディレクトリ検索を行うことを示します。
        </td>
    </tr>
    <tr>
        <td>target</td>
        <td>
            ソースファイルが配置されるパッケージ内のディレクトリへの相対パスです。
        </td>
    </tr>
    <tr>
        <td>exclude</td>
        <td>
            除外するファイル（単数または複数）です。この属性は一般的に `src` 属性でワイルドカードを使用する場合に組み合わせて使用されます。
            `exclude` 属性には、ファイルまたはファイル パターンの、セミコロン区切りのリストを含めることができます。
            2 文字のワイルドカード ** は、再帰的な除外パターンを示します。
        </td>
    </tr>
</table> 

`files` 要素の例は次のとおりです。

    <files>
      <file src="bin\Debug\*.dll" target="lib" /> 
      <file src="bin\Debug\*.pdb" target="lib" /> 
      <file src="tools\**\*.*" exclude="**\*.log" />
    </files>
    
<p class="caution">NuGet は '.resources.dll' で終わる DLL への参照を追加しません。<a href="http://nuget.codeplex.com/discussions/280566" target="_blank" title="Nuget .resources.dll discussion">http://nuget.codeplex.com/discussions/280566</a> にある Codeplex の議論を参照してください。</p> 

### file 要素の例

使用方法をより深く理解するために、この要素の使用例を見ていきましょう。

#### 単一のアセンブリ

パッケージの `lib` フォルダーに NuSpec ファイルと同じフォルダーにある単一のアセンブリをコピーします。

含まれるソース：`foo.dll`

NuSpec の file 要素：

    <file src="foo.dll" target="lib" />
    
パッケージの結果：`lib\foo.dll`

#### 深いパスにある単一のアセンブリ

.NET Framework 4 をターゲットにするプロジェクトにのみ適用されるように、パッケージの `lib\net40` フォルダーに単一のアセンブリをコピーします。

含まれるソース：`foo.dll`

    <file src="assemblies\net40\foo.dll" target="lib\net40" />

パッケージの結果：`lib\net40\foo.dll`

#### DLL のセット

`bin\release` フォルダーにあるアセンブリのセットをパッケージの `lib` フォルダーにコピーします。

含まれるソース：

* `bin\releases\MyLib.dll`
* `bin\releases\CoolLib.dll`

NuSpec の file 要素

    <file src="bin\release\*.dll" target="lib" />

パッケージの結果：
* `lib\MyLib.dll`
* `lib\CoolLib.dll`

#### 別々のフレームワーク用の DLL 群

含まれるソース：

* `lib\net40\foo.dll`
* `lib\net20\foo.dll`

NuSpec の file 要素：

    <file src="lib\**" target="lib" />

パッケージの結果：

* `lib\net40\foo.dll` 
* `lib\net20\foo.dll`

注：2 文字のワイルドカードはファイルのマッチングでソース内を再帰的に検索することを示します。

#### コンテンツファイル

含まれるソース：

* `css\mobile\style1.css`
* `css\mobile\style2.css`

NuSpec の file 要素：

    <file src="css\mobile\*.css" target="content\css\mobile" />

パッケージの結果：

* `Content\css\mobile\style1.css`
* `Content\css\mobile\style2.css`

#### ディレクトリ構造を持つコンテンツファイル

含まれるソース：
 
* `css\mobile\style.css`
* `css\mobile\wp7\style.css`
* `css\browser\style.css`

NuSpec の file 要素：

    <file src="css\**\*.css" target="content\css" />

パッケージの結果：

* `content\css\mobile\style.css` 
* `content\css\mobile\wp7\style.css`
* `content\css\browser\style.css`

#### 深いパスを持つコンテンツファイル

含まれるソース：`css\cool\style.css`

    <file src="css\cool\style.css" target="Content" />

パッケージの結果：`Content\style.css`

#### 名前にドットが含まれるフォルダーへのコンテンツファイルのコピー

    <file src="images\Neatpic.png" target="Content\images\foo.bar" />

パッケージの結果：`Content\images\foo.bar\Neatpick.png`

注：target の「拡張子」が src の拡張子と一致しないので、NuGet はディレクトリとして扱います。

#### 深いパスと深い target を持つコンテンツファイル

    <file src="css\cool\style.css" target="Content\css\cool" />

パッケージの結果：`Content\css\cool\style.css`

注：次の指定でも動作します。

    <file src="css\cool\style.css" target="Content\css\cool\style.css" />

この場合、src と target のファイルの拡張子が一致するため、target がディレクトリ名ではなくファイル名であるとみなされます。

#### コンテンツファイルのコピーとリネーム

    <file src="ie\css\style.css" target="Content\css\ie.css" />

パッケージの結果：`Content\css\ie.css`

### NuSpec でのファイルの除外

NuSpec ファイルの `<file>` 要素では、ワイルドカードを使用して、含める対象の特定のファイルやファイルのセットを指定できます。
ワイルドカードを使用する場合に、含まれるファイルの特定のサブセットを除外する方法はありません。
たとえば、あるディレクトリにある特定のファイル以外のすべてのテキストファイルが欲しいとしましょう。

    <files>
        <file src="docs\*.txt" target="content\docs" exclude="docs\admin.txt" />
    </files>

複数のファイルを指定するには、セミコロンを使用します。

    <files>
        <file src="*.txt" target="content\docs" exclude="admin.txt;log.txt" />
    </files>

すべてのバックアップファイルのような、あるファイルセットを除外するためにワイルドカードを使用します。

    <files>
        <file src="tools\*.*" target="tools" exclude="tools\*.bak" />
    </files>

または、ディレクトリツリーで再帰的にファイルのセットを除外するために、2 文字のワイルドカードを使用します。

    <files>
        <file src="tools\**\*.*" target="tools" exclude="**\*.log" />
    </files>

## .nuspec ファイルの例

次の例は、依存先やファイルを指定しない、単純な .nuspec ファイルの例を示しています。

    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
      <metadata>
        <id>sample</id>
        <version>1.2.3</version>
        <authors>Kim Abercrombie, Franck Halmaert</authors>
        <description>Sample exists only to show a sample .nuspec file.</description>
        <language>en-US</language>
        <projectUrl>http://xunit.codeplex.com/</projectUrl>
        <licenseUrl>http://xunit.codeplex.com/license</licenseUrl>
      </metadata>
    </package>

次の例は、依存関係を指定する .nuspec ファイルを示す例です。

    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">>
      <metadata>
        <id>sample</id>
        <version>1.0.0</version>
        <authors>Microsoft</authors>
        <dependencies>
          <dependency id="another-package" version="3.0.0" />
          <dependency id="yet-another-package"/>
        </dependencies> 
      </metadata>
    </package>

次の例は、ファイルを指定する .nuspec ファイルを示す例です。

    <?xml version="1.0"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
      <metadata>
        <id>routedebugger</id>
        <version>1.0.0</version>
        <authors>Jay Hamlin</authors>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Route Debugger is a little utility I wrote...</description>
      </metadata>
      <files>
        <file src="bin\Debug\*.dll" target="lib" />
      </files>
    </package>

次の例は、フレームワークアセンブリを指定する .nuspec ファイルを示す例です。

    <?xml version="1.0"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">>
      <metadata>
        <id>PackageWithGacReferences</id>
        <version>1.0</version>
        <authors>Author here</authors>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>
            A package that has framework assemblyReferences depending 
            on the target framework.
        </description>
        <frameworkAssemblies>
          <frameworkAssembly assemblyName="System.Web" targetFramework="net40" />
          <frameworkAssembly assemblyName="System.Net" targetFramework="net40-client, net40" />
          <frameworkAssembly assemblyName="Microsoft.Devices.Sensors" targetFramework="sl4-wp" />
          <frameworkAssembly assemblyName="System.Json" targetFramework="sl3" />
        </frameworkAssemblies>
      </metadata>
    </package>

上記の例で、特定のプロジェクト ターゲットに対して何がインストールされるのかを示すと、次のようになります。

* .NET4 をターゲットにするプロジェクト -> System.Web、System.Net  
* .NET4 Client Profile をターゲットにするプロジェクト -> System.Net  
* Silverlight 3 をターゲットにするプロジェクト -> System.Json  
* Silverlight 4 をターゲットにするプロジェクト -> System.Windows.Controls.DomainServices  
* WindowsPhone をターゲットにするプロジェクト -> Microsoft.Devices.Sensors  
