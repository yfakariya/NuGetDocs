<!-- 1 07 02:38:02 2012 d72bbda85a8906852cabbef5baf3279d43d74aff-->
# 構成ファイル変換とソースコード変換

## 概要

一般的に、パッケージの作成時には、一切変更されずに、対象となるソリューションの適切な場所に単にコピーされるだけのファイルをパッケージに含めます。ただし、場合によっては、インストール中にファイルを修正したり変換したりしたい場合があることでしょう。

### 構成ファイル変換

パッケージによっては、インストール先のプロジェクトの構成ファイル（web.config や app.config）をカスタマイズする必要があります。たとえば、[ELMAH（Error Logging Modules and Handlers for ASP.NET）](http://code.google.com/p/elmah/) は *web.config* ファイルの `httpModules` セクションにエントリを追加する必要があります。

*web.config.transform* または *app.config.transform* という名前のファイルを含めることで、プロジェクトの *app.config* または *web.config* ファイルに対する変更を指定できます。

### ソースコード変換

ソースコードのファイル名の末尾に *.pp* を追加することで、プロジェクトにコピーされるソースコードに対する変更を指定できます。そして、インストール中に対象のプロジェクトに対して適切な値で置き換えられる変数を、ソースコードに埋め込むことができます。たとえば、ソースコードのファイルに `namespace $rootnamespace$` を指定し、ファイル名の末尾に *.pp* を追加すると、ルート名前空間が `TargetProject` というプロジェクトにファイルがインストールされるときに、その部分は `namespace TargetProject` になります。対象のプロジェクトでは、ファイルの末尾の *.pp* 拡張子はなくなっています。

## 構成ファイル変換の指定

構成ファイル変換を適用するには、パッケージのコンテンツにファイルを追加し、さらに、そのファイルに対して、変換したいファイルと同じ名前に .transform 拡張子を追加した名前を付けます。たとえば、web.config ファイルを変換するには、web.config.transform ファイルを作成します。変換ファイルには web.config や app.config に似た XML が含まれますが、プロジェクトの構成ファイルにマージする必要のあるセクションしか含まれていません。

たとえば、あるパッケージでインストール先のプロジェクトの web.config ファイルの modules コレクションに項目を追加したいとします。そうするためには、パッケージに web.config.transform という名前のファイルを追加し、次に示す XML を記述します。

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyNuModule" type="Sample.MyNuModule" />
            </modules>
        <system.webServer>
    </configuration>

そして、パッケージがインストールされるプロジェクトには、次に示す web.config ファイルが含まれるとします。

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyModule" type="Sample.MyModule" />
            </modules>
        <system.webServer>
    </configuration>

NuGet がパッケージをインストールすると、web.config ファイルは次に示す例のようになります。

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyModule" type="Sample.MyModule" />
                <add name="MyNuModule" type="Sample.MyNuModule" />
            </modules>
        <system.webServer>
    </configuration>

NuGet が modules セクションを置き換えたのではなく、新しいエントリをマージしただけであることに注意してください。NuGet は変換ファイルをプロジェクトの構成ファイルにマージするときには、要素の追加か、構成ファイルにすでに存在する要素への属性の追加のみを行います。既存の要素や属性を変更する方法はありません。

先ほど述べたように、インストール中にプロジェクトの web.config ファイルを変更する必要のあるパッケージの例として、ELMAH（Error Logging Modules and Handlers for ASP.NET）があります。ELMAH は ELMAH の HTTP モジュールと HTTP ハンドラーを web.config ファイルに登録しなければなりません。ELMAH のパッケージには登録方法を示す web.config.transform という名前のファイルが含まれます。

![](images/web.config.transform.png)

web.config.transform ファイルに含まれる XML が次の例のようになっているとします。

    <configuration>
        <system.web>
            <httpModules>
                <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" />
            </httpModules>
            <httpHandlers>
                <add verb="POST,GET,HEAD" path="elmah.axd"
                  type="Elmah.ErrorLogPageFactory, Elmah" />
            </httpHandlers>
        </system.web>
        <system.webServer>
            <validation validateIntegratedModeConfiguration="false" />
            <modules>
                <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" />
            </modules>
            <handlers>
                <add name="Elmah" verb="POST,GET,HEAD" path="elmah.axd"
                  type="Elmah.ErrorLogPageFactory, Elmah" />
            </handlers>
        </system.webServer>
    </configuration>

さらに、次に示す web.config ファイルのあるプロジェクトに ELMAH パッケージをインストールするとします。

    <configuration> 
      <system.web> 
        <compilation debug="true" targetFramework="4.0"> 
          <assemblies> 
            <add assembly="System.Data.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /> 
            <add assembly="System.Web.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" /> 
          </assemblies> 
        </compilation> 
      </system.web> 
      <system.webServer> 
          <modules runAllManagedModulesForAllRequests="true"> 
              <add name="ErrorLog" /> 
          </modules> 
          <handlers> 
              <add name="Elmah" verb="HEAD" path="myOwnElmah.axd" type="My.Elmah, Elmah"/> 
          </handlers> 
      </system.webServer> 
    </configuration>

パッケージをインストールすると、**web.config** ファイルは次の例のようになります。

    <configuration> 
      <system.web> 
        <compilation debug="true" targetFramework="4.0"> 
          <assemblies> 
            <add assembly="System.Data.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /> 
            <add assembly="System.Web.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" /> 
          </assemblies> 
        </compilation> 
    <httpModules> 
          <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" /> 
        </httpModules> 
        <httpHandlers> 
          <add verb="POST,GET,HEAD" path="elmah.axd" type="Elmah.ErrorLogPageFactory, Elmah" /> 
        </httpHandlers> 
      </system.web> 
      <system.webServer> 
        <modules runAllManagedModulesForAllRequests="true"> 
          <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" /> 
        </modules> 
        <handlers> 
          <add name="Elmah" verb="HEAD" path="myOwnElmah.axd" type="My.Elmah, Elmah" /> 
          <add name="Elmah" verb="POST,GET,HEAD" path="elmah.axd" type="Elmah.ErrorLogPageFactory, Elmah" /> 
        </handlers> 
        <validation validateIntegratedModeConfiguration="false" /> 
      </system.webServer> 
    </configuration>

ここでは、次の変更が行われました。

* `<httpModules>` 要素と `<httpHandlers>` 要素が `<system.web>` 要素の子に追加されました。
* `<system.webserver/modules>` コレクションの ErrorLog 要素に type 属性が追加されました。NuGet は一致する属性を持つ要素があり、変換ファイルに新しいものが 1 つ存在することを見つけたので、既存の要素に属性を追加しました。パッケージが削除されるときに、NuGet にはこの要素が追加されたように見えるので、プロジェクトの web.config ファイルから要素全体が削除されることに注意してください。
* 新しい Elmah 要素が `<system.webserver/handlers>` コレクションに追加されました。NuGet は一致する要素が 1 つ あり、他に一致する要素がないことを見つけたので、既存の要素を変更するのではなく、新しい要素を追加しました。
* `<system.webserver>` 要素の子に `<validation>` 要素が追加されました。

## ソースコード変換の指定

NuGet は Visual Studio のプロジェクトテンプレートのように動作するソースコード変換もサポートします。ソースコード変換は、インストール先のプロジェクトに追加するソースコードがある NuGet パッケージで役に立ちます。たとえば、ライブラリの初期化に使用するソースコードを含めたいけれども、コードが対象のプロジェクトの名前空間に属すようにしたい場合があるでしょう。

コードに Visual Studio のプロジェクトのプロパティを含めることで、ソースコード変換を指定します。プロパティはドル記号（$）で区切られます。たとえば、namespace ステートメントに対象のプロジェクトのルート名前空間を含める用に指定したいとします。ぷろじぇくとに含めるソースコードは次の例のようになります。

    namespace $rootnamespace$.Models {
        public struct CategoryInfo {
            public string categoryid;
            public string description;
            public string htmlUrl;
            public string rssUrl;
            public string title;
        }
    }

ソースコードファイルでの変換処理を有効にするには、次の例で示すように、ファイル名に .pp 拡張子を追加します。

![](images/pp.files.png)

上記の図のソースコードファイルのインストール時に、NuGet はファイルを変換し、.pp 拡張子を取り除き、対象のプロジェクトの ~/Modules ディレクトリに追加します。

`$rootnamespace$` トークンは最もよく使われるプロジェクトのプロパティですが、パッケージのインストール時に使用可能な任意のプロジェクトプロパティを使用できます。（プロジェクトのプロパティによっては、プロジェクトの種類に固有かもしれません）。プロジェクトのプロパティの詳細情報については、[プロジェクトのプロパティに関する MSDN ドキュメント](http://msdn.microsoft.com/en-us/library/vslangproj.projectproperties_properties(VS.80).aspx) を参照してください。

対象のプロジェクト用のカスタマイズが必要かもしれないソースコードの例として、アプリケーションの開始時に実行する必要があるために、通常 **global.asax** に追加するようなコードがあります。**global.asax** を更新せずにこの効果を達成する方法については、[WebActivator](https://bitbucket.org/davidebbo/webactivator/wiki/Home) を参照してください。
