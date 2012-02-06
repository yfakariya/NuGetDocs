<!-- Revision: 0d2451255cd9a57f47bb55d5ed8a348a3dba9666 2011/12/30 2:47:18 -->
# パッケージ規約

このページの目的はパッケージ規約の徹底的な解説を始めることです。これらの規約は、パッケージのフォルダー構造や複数の .NET Framework バージョンのサポートといった、パッケージの作成時に NuGet によって必要とされる「強制的な」規約とは異なるものです。強制的な規約については、[パッケージの作成](Creating-and-Publishing-a-Symbol-Package-ja)のドキュメントを参照してください。

## パッケージ ID 規約

**名前空間と同じ名前付け：** パッケージ ID は .NET の名前空間と似たパターンに従うべきです。たとえば、 `Ninject-Mvc3` ではなく `NInject.Mvc3` とします。

**サンプルパッケージ：** あなたのパッケージ用のサンプルコードを提供するパッケージについては、 「`.Sample`」サフィックスを使用します。これによって他の人の取っ掛かりが良くなります。たとえば、パッケージ名が `Clay` ならば、`Clay` の使用方法のサンプルは `Clay.Sample` になります。同様に、 `content` フォルダーの中では、ルートとなる `/Sample/PackageId` フォルダー構造内にサンプルを配置します。たとえば、 `Clay.Sample` パッケージは `/Samples/Clay` フォルダーを持つことになります。

## パッケージのコンテンツの規約

**App\_Start フォルダー：** `[WebActivator](http://nuget.org/List/Packages/WebActivator)` パッケージを使用する場合、パッケージの `Content` フォルダー直下の `App\_Start` フォルダーにアプリケーションのすべてのスタートアップコードを配置します。さらなる詳細についてはこのブログ記事を参照してください。

**アセンブリ：** 一般的に、アセンブリごとに 1 つのパッケージにするとよいでしょう。場合によっては、ライブラリ内ではない別のコンテキストでは意味のないアセンブリがライブラリに含まれている場合があります。その場合、それらのアセンブリをパッケージに含めるとよいでしょう。たとえば、`Bar.dll` に依存する `Foo.dll` があり、かつ他の人が `Bar.dll` に依存する可能性があると思うならば、2 つのパッケージを作成します。ところが、 `Foo.dll` と `Foo.resources.dll` の場合、2 つのパッケージにする意味はありません。素直に単一のパッケージに両方を入れてください。

## パッケージのバージョニングの規約

**バージョニングのガイドライン：** NuGet のバージョニングを理解するためには、以下の 3 部作の連載が非常に重要です（かつ手軽でもあります！） [NuGet のバージョニング 第 1 部：DLL ヘルについての見解](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)<!--(~/docs/extern/nuget-versioning-part-1-taking-on-dll-ja.markdown)-->、[NuGet のバージョニング 第 2 部：コアアルゴリズム](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)<!--(~/docs/extern/nuget-versioning-part-2-core-algorithm-ja.markdown)-->、[NuGet のバージョニング 第 3 部：バインディングリダイレクトによる一元化](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)<!--(~/docs/extern/nuget-versioning-part-3-unification-via-ja.markdown)-->。

**バージョンの選択：** 一般的に、ライブラリのバージョンにパッケージのバージョンを対応させるとよいでしょう。ただし、ライブラリがあまり一般的なバージョニングスキームを持っていない場合には、NuGet が使用するバージョニングルールを使用しないでください。一般的にパッケージのバージョンをライブラリのバージョンに一致させることを推奨しますが、必須ではありません。