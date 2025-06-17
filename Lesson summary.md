  # Flutter 全まとめ集1

## プロジェクトのファイル構成

**プロジェクトのフォルダ類**

| フォルダ名             | 説明                                                       |
| :--------------------- | :--------------------------------------------------------- |
| `[Dart_tool]`フォルダ  | Dart言語が自動生成するファイル類を保管するところ。         |
| `[idea]`フォルダ       | IntelliJ IDEA開発ツールの設定情報。                        |
| `[build]`フォルダ      | ビルドで生成されるファイル類。                             |
| `[Android]`フォルダ    | Androidアプリ生成に必要なファイル類（プラットフォームにAndroidを選択した場合）。 |
| `[ios]`フォルダ        | iOSアプリ生成に必要なファイル類（プラットフォームにiOSを選択した場合）。     |
| `[Linux]`フォルダ      | Linuxアプリ生成に必要なファイル類（プラットフォームにLinuxを選択した場合）。 |
| `[macOS]`フォルダ      | macOSアプリ生成に必要なファイル類（プラットフォームにmacOSを選択した場合）。 |
| `[windows]`フォルダ    | Windowsアプリ生成に必要なファイル類（プラットフォームにWindowsを選択した場合）。 |
| `[web]`フォルダ        | Webアプリ生成に必要なファイル類（プラットフォームにWebを選択した場合）。     |
| `[lib]`フォルダ        | ここにDartのスクリプトが保存される。                       |
| `[test]`フォルダ       | ユニットテスト関連のファイル類。                           |

**プロジェクトのファイル類**

| ファイル名                 | 説明                                   |
| :------------------------- | :------------------------------------- |
| `.gitignore`               | Gitで利用するファイル。                |
| `.metadata`                | Flutterツールが利用するファイル。      |
| `.packages`                | 利用しているパッケージ情報。           |
| `analysis_options.yaml`    | Dartの分析に関するファイル。           |
| `flutter_app.iml`          | モジュール定義ファイル。               |
| `pubspec.lock`             | Pub（Dartのパッケージマネージャ）が利用するファイル。 |
| `pubspec.yaml`             | Pubが利用するファイル。                |
| `README.md`                | リードミーファイル                     |

## アプリ画面とウィジェットツリー

Flutterにおけるアプリ画面は、私たちが普段目にする様々な要素（テキスト、画像、ボタン、入力欄など）が、すべて**「ウィジェット」**として構成されています。これらは、まるでレゴブロックのように、それぞれが独立した部品として存在し、組み合わせることで複雑なUIを簡単に構築できます。

### 1. アプリ画面は「ウィジェットの集合体」

**宣言的UI:** Flutterは**「宣言的UI」**を採用しています。これは、UIの状態（データ）をコードで宣言すれば、Flutterが自動的にその状態を反映したUIを描画してくれるという考え方です。例えば、ユーザー名が表示されるテキストウィジェットがあるとして、ユーザー名を変更すれば、自動的にそのテキストウィジェットの表示も更新されます。

**状態（State）の管理:** アプリの画面が動的に変化するためには、ウィジェットが**「状態（State）」**を持つ必要があります。静的な見た目だけを扱うウィジェット（例: `Text`, `Icon`）は**`StatelessWidget`**、状態を持ち、それが変化することで再描画されるウィジェット（例: `Checkbox`, `TextField`）は**`StatefulWidget`**として実装されます。この違いを理解することが、FlutterでのUI構築の鍵です。

### 2. ウィジェットツリーは「アプリの設計図」

アプリ画面を構成するウィジェットたちは、ただバラバラに並んでいるわけではありません。これらは親子の関係を持ち、**「ウィジェットツリー」**と呼ばれる木構造を形成しています。このツリーが、アプリのUIがどのように構成されているかを示す「設計図」のようなものです。

**階層構造の重要性:**
* **レイアウトの基礎:** `Row`や`Column`のようなレイアウトウィジェットが親となり、その中に複数の子ウィジェットを配置することで、複雑なレイアウトを実現します。例えば、`AppBar`の中に`Text`や`Icon`があるように、それぞれのウィジェットはツリーの中で適切な位置に配置されます。
* **データの伝達:** ツリーの上位（親）にあるウィジェットから、下位（子）のウィジェットへデータが渡される（プロパティを通じて）ことで、ウィジェット間で連携が図られます。
* **パフォーマンスの最適化:** Flutterは、このウィジェットツリーを効率的に再構築することで、高速なUI描画を実現しています。状態が変化した部分だけを効率的に更新するため、無駄な再描画を防ぎ、スムーズなユーザー体験を提供します。
* **`build`メソッドの役割:** 各ウィジェットには**`build`メソッド**があり、このメソッドがそのウィジェットが持つ子ウィジェット（または自身）を返します。この`build`メソッドが連鎖的に呼び出されることで、最終的なウィジェットツリーが構築され、画面に描画されるのです。

---

## アプリ実行の仕組み

Flutterアプリがどのように動作し、画面が表示されるまでの流れを理解することは、トラブルシューティングやより高度な開発を行う上で不可欠です。

### 1. `main`関数からアプリの開始へ

すべてのFlutterアプリは、Dart言語の**`main`関数**から実行が開始されます。これはC言語やJavaなどのエントリーポイントと同じ役割を果たします。

```dart
void main() {
  runApp(ウィジェット); 
}
```
`main`という関数は、アプリを起動する際に呼び出される処理です。アプリのプログラミングでは、このmain関数に記述をします。

ここでは、`runApp`という関数を実行していますね。これが、アプリを起動する処理です。つまりアプリのプログラムというのは、「`main`関数で、`runApp`でアプリを起動する」というだけのシンプルなものなのです。

## StatelessWidgetクラスについて

### StatelessWidgetとは何か？

`StatelessWidget`は、**「状態（State）を持たないウィジェット」**と定義されます。これは、一度描画されると、そのウィジェット自身のプロパティ（データ）が外部から変更されない限り、見た目が自動的に変化することはないという意味です。静的なコンテンツ、つまりアプリの実行中に内容が変わることがないUI要素を表示するのに最適です。

例えるなら、一度印刷されたポスターや看板のようなものです。内容が固定されており、それ自体が時間と共に変化することはありません。

### 1. StatelessWidgetの基本的な特徴

* **不変性（Immutable）:** `StatelessWidget`は、コンストラクタを通じて受け取ったプロパティ（データ）を内部で変更することはありません。すべてのデータは最終的（`final`）として定義されることが一般的です。
* **シンプルな使用法:** 状態管理のメカニズムを持たないため、`StatefulWidget`に比べてコードがシンプルになり、理解しやすいのが特徴です。
* **再構築のトリガー:** `StatelessWidget`の`build`メソッドが再呼び出しされ、再構築されるのは、主に以下のケースです。
    * ウィジェットが初めて画面に表示される時。
    * 親ウィジェットが再構築され、この`StatelessWidget`に新しいプロパティが渡された時。
    * このウィジェットが依存する上位の`InheritedWidget`が変更された時。

```dart
class クラス名 extends StatelessWidget {
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(・・・・・略・・・・・);
  }
}
```      
StatelessWidgetクラスは、`build`というメソッドが用意されます。これは、その名の通りウィジェットが生成される際に呼び出されるものです。

ここでは、returnで`MaterialApp`というクラスのインスタンスが返されています。これはマテリアルデザインのアプリを管理するクラスです。StatelessWidgetクラスは、ステートのないウィジェットというだけで、表示されるデザインなどは特に扱っていません。このStatelessWidgetでMaterialAppインスタンスを`return`することで、マテリアルデザインによるアプリが表示されるようになるのです。

## MaterialAppクラスについて

### MaterialAppとは何か？

`MaterialApp`は、Flutterアプリケーションの**ルート（最上位）ウィジェット**として使用されることが最も一般的です。これを使用することで、以下のようなマテリアルデザイン特有の機能や設定を簡単に利用できるようになります。

* **ナビゲーション（画面遷移）システム**：ルーティング（経路）を管理し、複数の画面間を移動する機能を提供します。
* **テーマ設定**：アプリ全体のカラー、フォント、タイポグラフィなどのデザインを一元的に管理できます。
* **ローカライゼーション（国際化）**：複数の言語に対応するための設定が可能です。
* **マテリアルデザインのウィジェット**：`Scaffold`、`AppBar`、`FloatingActionButton`など、マテリアルデザインに準拠した豊富なウィジェット群を正しく機能させるためのコンテキストを提供します。

例えるなら、アプリという「家」を建てる際の、デザインコンセプトや骨組み、そして各部屋への動線を決める「設計図」のようなものです。

### 1. MaterialAppの基本的な使い方

```dart
return MaterialApp(
  title: 'Flutter Demo',
  home: Text(
    'Hello, Flutter World!!',
    style: TextStyle(fontSize:32.0),
  ),
);
```
これは、よく見るとたった１つの文しか書かれてない、ということがわかるでしょう
か？ MaterialAppクラスのインスタンスを作成しreturnするもので、以下のような文を
改行してわかりやすく書いているだけなのです。
```dart
return MaterialApp( title: OO, home: OO );
```
ここでは、`title`と`home`という２つの名前付き引数が用意されています。`title`は、その
名のとおり、**アプリケーションのタイトル**を示します。`home`というのが、このアプリケー
ションに組み込まれる**ウィジェット**を示すものです。ここに設定されたウィジェットが、
このMaterialAppの表示となります。

### ScaffoldとAppBarのまとめ

Flutterにおいて、`Scaffold`と`AppBar`はマテリアルデザインに準拠したアプリケーションの基本的なレイアウトを構成する上で不可欠なウィジェットです。

#### 1. Scaffoldウィジェット

`Scaffold`は、マテリアルデザインの視覚的なレイアウト構造を実装するための基本的なウィジェットです。アプリケーションの最も上位のウィジェットとして機能し、一般的に以下の要素を配置するコンテナとして使用されます。

* **目的:** アプリケーションの骨格を定義し、一貫したUI要素（ヘッダー、フッター、ドロワーなど）を簡単に配置できるようにします。
* **主なプロパティ:**

    * `appBar`: アプリケーションの上部に表示されるバー（通常は`AppBar`ウィジェット）。
    * `body`: `AppBar`の下に表示される主要なコンテンツ領域。画面の中心部分にあたります。
    * `floatingActionButton`: 画面コンテンツ上にフローティング表示されるアクションボタン（例: 新規作成ボタン）。
    * `drawer`: 画面の左端からスライドして表示されるサイドメニュー。
    * `endDrawer`: 画面の右端からスライドして表示されるサイドメニュー。
    * `bottomNavigationBar`: 画面下部に表示されるナビゲーションバー。
    * `bottomSheet`: 画面下部からスライドして表示される一時的なシート。
    * `backgroundColor`: `Scaffold`の背景色。
    * `resizeToAvoidBottomInset`: キーボードが表示された際に`body`のサイズを調整するかどうか（デフォルトは`true`）。

* **使用例:**

    ```dart
    import 'package:flutter/material.dart';

    void main() {
      runApp(MyApp());
    }

    class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Flutter Demo',
          home: Scaffold(
            appBar: AppBar(
              title: const Text('Hello, Flutter!'),
            ),
            body: Text('Hello, Flutter World!!',
            style: TextStyle(fontsize:32.0),
            floatingActionButton: 
            ),
          ),
        );
      }
    }  
    ```

#### 2. AppBarウィジェット

`AppBar`は、`Scaffold`ウィジェットの`appBar`プロパティに設定することで、画面の上部に表示されるマテリアルデザインのバーです。アプリケーションの現在の画面に関する情報やアクションを提供します。

* **目的:** アプリケーションのタイトル表示、ナビゲーション、主要なアクションの提供、ブランドの表示などを行います。
* **主なプロパティ:**

    * `title`: バーの中央または左に表示されるタイトル（通常は`Text`ウィジェット）。
    * `leading`: タイトルの前に表示されるウィジェット（例: 戻るボタン、ドロワーアイコン）。通常は`IconButton`や`Icon`。
        * `Scaffold`に`drawer`が設定されている場合、自動的にドロワーを開くアイコン（ハンバーガーメニュー）が表示されます。
    * `actions`: タイトルの後に表示される一連のウィジェット（例: 検索アイコン、設定アイコン）。通常は`IconButton`のリスト。
    * `bottom`: `AppBar`の下部に表示されるウィジェット（例: `TabBar`）。
    * `backgroundColor`: `AppBar`の背景色。
    * `elevation`: `AppBar`の下に表示される影の深さ。
    * `centerTitle`: `title`ウィジェットを中央に配置するかどうか（デフォルトはプラットフォームによって異なる）。
    * `toolbarHeight`: ツールバーの高さ。

* **使用例:**

    ```dart
    import 'package:flutter/material.dart';

    class MyAppBarExample extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            // タイトル
            title: const Text('My App Title'),
            // タイトルの前に表示されるウィジェット
            leading: IconButton(
              icon: const Icon(Icons.menu),
              onPressed: () {
                // ドロワーを開くなど
                Scaffold.of(context).openDrawer(); // ScaffoldのDrawerを開く
              },
            ),
            // タイトルの後に表示されるアクションウィジェットのリスト
            actions: <Widget>[
              IconButton(
                icon: const Icon(Icons.search),
                onPressed: () {
                  // 検索アクション
                },
              ),
              IconButton(
                icon: const Icon(Icons.settings),
                onPressed: () {
                  // 設定アクション
                },
              ),
            ],
            // AppBarの背景色
            backgroundColor: Colors.teal,
            // 影の高さ
            elevation: 8.0,
            // タイトルを中央に配置
            centerTitle: true,
            // AppBarの下にTabBarを配置
            bottom: const TabBar(
              tabs: [
                Tab(icon: Icon(Icons.home)),
                Tab(icon: Icon(Icons.favorite)),
                Tab(icon: Icon(Icons.person)),
              ],
            ),
          ),
          body: const Center(
            child: Text('AppBar Example Content'),
          ),
          drawer: const Drawer(), // ドロワーを有効にするため
        );
      }
    }
    ```
