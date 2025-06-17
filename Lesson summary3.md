# Flutter 全まとめ集3

## ボタン・ウィジェットについて

Flutter では、ユーザーインタラクションの基本的な要素として様々なボタンウィジェットが提供されています。これらのボタンは Material Design ガイドラインに準拠しており、タップ時の視覚的なフィードバックやアクセシビリティが考慮されています。

## ボタンに共通する基本的なプロパティ

多くのボタンウィジェットに共通して存在する重要なプロパティは以下の通りです。

* **`onPressed` (必須)**:
    * **型**: `VoidCallback?` (`Function()`)
    * **説明**: ボタンがタップされたときに実行されるコールバック関数です。このプロパティが `null` の場合、ボタンは無効化され、タップできなくなります（視覚的にもグレーアウトするなどして表示される）。
* **`child`**:
    * **型**: `Widget`
    * **説明**: ボタンの内部に表示されるウィジェットです。通常は `Text` (ボタンのテキスト) や `Icon` (アイコンボタン) が使用されます。
* **`tooltip`**:
    * **型**: `String?`
    * **説明**: ボタンを長押ししたときに表示されるツールチップ（ヒントテキスト）です。アクセシビリティにも役立ちます。
* **`style`**:
    * **型**: `ButtonStyle?`
    * **説明**: ボタンの見た目（色、形状、パディング、テキストスタイルなど）を細かくカスタマイズするためのプロパティです。多くのボタンで利用可能です。
* **`onLongPress`**:
    * **型**: `VoidCallback?`
    * **説明**: ボタンが長押しされたときに実行されるコールバック関数です。

## 主要なボタンウィジェットの種類

### 1. `ElevatedButton`

* **概要**: Material Design の「強調されたボタン」に相当し、背景色を持ち、影（Elevation）が付いているため、平面から浮き上がって見えるボタンです。主要なアクションや、ユーザーに目立たせたいアクションに使用されます。
* **特徴**:
    * デフォルトで背景色があり、影を持つ。
    * 無効化されると影がなくなり、背景色も薄くなる。
* **使用例**:
    ```dart
    ElevatedButton(
      onPressed: () {
        print('ElevatedButton pressed!');
      },
      child: const Text('保存'),
      style: ElevatedButton.styleFrom(
        backgroundColor: Colors.blue, // 背景色
        foregroundColor: Colors.white, // テキスト/アイコンの色
        padding: EdgeInsets.symmetric(horizontal: 30, vertical: 15),
        textStyle: TextStyle(fontSize: 18),
      ),
    )
    ```

### 2. `TextButton`

* **概要**: テキストのみで構成されるボタンで、背景色や影を持ちません。強調が少なく、サブアクションや、テキストリンクのような表示に適しています。
* **特徴**:
    * 背景色が透明で、影を持たない。
    * タップ時に波紋（Splash）エフェクトが表示される。
* **使用例**:
    ```dart
    TextButton(
      onPressed: () {
        print('TextButton pressed!');
      },
      child: const Text('詳細を見る'),
      style: TextButton.styleFrom(
        foregroundColor: Colors.deepPurple, // テキストの色
      ),
    )
    ```

### 3. `OutlinedButton`

* **概要**: 輪郭線（アウトライン）を持つボタンです。`ElevatedButton` と `TextButton` の中間的な強調度を持ち、代替アクションや二次的なアクションに適しています。
* **特徴**:
    * 背景色は透明で、輪郭線が表示される。
    * タップ時に輪郭線が強調され、背景に波紋エフェクトが表示される。
* **使用例**:
    ```dart
    OutlinedButton(
      onPressed: () {
        print('OutlinedButton pressed!');
      },
      child: const Text('キャンセル'),
      style: OutlinedButton.styleFrom(
        foregroundColor: Colors.red, // テキスト/アイコンの色
        side: BorderSide(color: Colors.red), // 輪郭線の色と太さ
      ),
    )
    ```

### 4. `IconButton`

* **概要**: アイコンのみを表示するボタンです。通常、アプリバーやツールバー内のアクション、またはアイコンで機能を表現したい場合に利用されます。
* **特徴**:
    * 背景は透明で、タップ時にアイコンを中心に波紋エフェクトが表示される。
    * サイズはアイコンのサイズに依存するが、タップ可能な領域はMaterial Designの規定に従って確保される。
* **使用例**:
    ```dart
    IconButton(
      icon: const Icon(Icons.menu),
      onPressed: () {
        print('Menu button pressed!');
      },
      tooltip: 'メニュー',
      color: Colors.blueGrey, // アイコンの色
      iconSize: 30.0, // アイコンのサイズ
    )
    ```

### 5. `FloatingActionButton`

* **概要**: アプリケーションのUIの右下隅に配置されることが多く、主要なアクションや「新規作成」などの目立った操作を促すために使用される円形のボタンです。
* **特徴**:
    * 常に画面の最前面に浮き上がって表示される。
    * Material Design のガイドラインで推奨される主要アクション。
* **詳細**: 別のまとめで詳しく記述しています。
* **使用例**:
    ```dart
    // Scaffold の floatingActionButton プロパティに配置
    Scaffold(
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          print('FAB pressed!');
        },
        child: const Icon(Icons.add),
        tooltip: '新規追加',
      ),
      // ...
    )
    ```

### 6. `DropdownButton`

* **概要**: ドロップダウンメニューを表示するボタンです。複数の選択肢の中から一つを選ばせる場合に便利です。
* **特徴**:
    * タップすると選択肢のリストが展開される。
    * 選択された値がボタン上に表示される。
* **主なプロパティ**:
    * `items`: `DropdownMenuItem` のリスト。
    * `onChanged`: 選択されたアイテムが変更されたときに呼び出されるコールバック。
    * `value`: 現在選択されている値。
* **使用例**:
    ```dart
    String? _selectedFruit; // 選択されたフルーツを保持する変数

    // Stateful Widget の build メソッド内で使用
    DropdownButton<String>(
      value: _selectedFruit,
      hint: const Text('フルーツを選択'),
      onChanged: (String? newValue) {
        setState(() {
          _selectedFruit = newValue;
          print('Selected: $_selectedFruit');
        });
      },
      items: const <DropdownMenuItem<String>>[
        DropdownMenuItem(
          value: 'Apple',
          child: Text('Apple'),
        ),
        DropdownMenuItem(
          value: 'Banana',
          child: Text('Banana'),
        ),
        DropdownMenuItem(
          value: 'Cherry',
          child: Text('Cherry'),
        ),
      ],
    )
    ```

## ボタンのスタイル設定 (`ButtonStyle`)

`ElevatedButton`, `TextButton`, `OutlinedButton` などは、`style` プロパティを通じて `ButtonStyle` オブジェクトを渡すことで、詳細なスタイル設定が可能です。

* `ButtonStyle.styleFrom()`: 最も手軽にスタイルを設定できる静的メソッドです。
* `ButtonStyle()`: より細かいカスタマイズが必要な場合に直接インスタンス化します。
* `ThemeData` から取得: アプリケーション全体のテーマでボタンのスタイルを統一することも可能です。

**`ButtonStyle.styleFrom()` で設定できる主なプロパティ**:

* `backgroundColor`: ボタンの背景色
* `foregroundColor`: ボタンのコンテンツ（テキストやアイコン）の色
* `shadowColor`: 影の色
* `elevation`: 影の深さ
* `padding`: ボタンの内部パディング
* `textStyle`: ボタンのテキストスタイル
* `shape`: ボタンの形状（`RoundedRectangleBorder` など）
* `side`: `OutlinedButton` の輪郭線のスタイル
* `minimumSize`, `fixedSize`, `maximumSize`: ボタンの最小/固定/最大サイズ

## ボタン・ウィジェットについてのまとめ

Flutter のボタンウィジェットは、Material Design の原則に基づいて設計されており、直感的で美しいUIを構築するのに役立ちます。各ボタンの特性と適切な使用場面を理解し、`onPressed` コールバックや `style` プロパティを使いこなすことで、ユーザーにとって使いやすいアプリケーションを提供できます。

## 複雑な構造のUIウィジェットについて

Flutterでは、単純なボタンやテキストだけでなく、より複雑なアプリケーションのレイアウトやユーザーインタラクションを構築するための高レベルなUIウィジェットが多数提供されています。これらは、複数の基本ウィジェットを組み合わせて、特定のUIパターンや機能を提供するために設計されています。

## 1. `Scaffold`

* **概要**: Material Design アプリケーションの基本的な視覚構造を実装するためのウィジェットです。アプリケーションの最も基本的な「骨格」を提供します。
* **主な役割**:
    * `appBar`: アプリケーションバー（ヘッダー）を設定します。
    * `body`: 画面の主要なコンテンツ領域を設定します。
    * `floatingActionButton`: フローティングアクションボタンを設定します。
    * `drawer`: ドロワー（サイドメニュー）を設定します。
    * `bottomNavigationBar`: 下部のナビゲーションバーを設定します。
    * `bottomSheet`: 画面下部からスライドアップするシートを設定します。
    * `snackBar`: 一時的なメッセージ表示（スナックバー）を管理します。
* **使用例**:
    ```dart
    Scaffold(
      appBar: AppBar(
        title: const Text('My App'),
      ),
      body: const Center(
        child: Text('Welcome to my app!'),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.add),
      ),
      drawer: Drawer(
        child: ListView(
          children: const <Widget>[
            DrawerHeader(child: Text('メニュー')),
            ListTile(title: Text('設定')),
          ],
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: 'ホーム'),
          BottomNavigationBarItem(icon: Icon(Icons.settings), label: '設定'),
        ],
      ),
    )
    ```

## 2. `AppBar`

* **概要**: アプリケーションの上部に表示されるバー（ヘッダー）です。タイトル、アクションボタン、検索バーなど、様々な要素を配置できます。`Scaffold` の `appBar` プロパティに設定されます。
* **主なプロパティ**:
    * `title`: アプリバーのタイトル。
    * `leading`: タイトルの前（通常は左端）に表示されるウィジェット（例: 戻るボタン、ハンバーガーアイコン）。
    * `actions`: タイトルの後（通常は右端）に表示されるウィジェットのリスト（例: 検索アイコン、設定アイコン）。
    * `bottom`: アプリバーの下部に表示されるウィジェット（例: `TabBar`）。
    * `elevation`: アプリバーの影の深さ。
    * `backgroundColor`: アプリバーの背景色。
* **使用例**:
    ```dart
    AppBar(
      title: const Text('商品リスト'),
      leading: IconButton(
        icon: const Icon(Icons.arrow_back),
        onPressed: () {},
      ),
      actions: <Widget>[
        IconButton(
          icon: const Icon(Icons.search),
          onPressed: () {},
        ),
        IconButton(
          icon: const Icon(Icons.shopping_cart),
          onPressed: () {},
        ),
      ],
      bottom: TabBar( // 例：タブバーを配置する場合
        tabs: const <Widget>[
          Tab(text: 'すべて'),
          Tab(text: 'カテゴリA'),
        ],
      ),
    )
    ```

## 3. `BottomNavigationBar`

* **概要**: 画面下部に配置され、複数のビュー間を切り替えるためのナビゲーションを提供します。通常、3～5個のアイテムを配置します。`Scaffold` の `bottomNavigationBar` プロパティに設定されます。
* **主なプロパティ**:
    * `items`: `BottomNavigationBarItem` のリスト。各アイテムはアイコンとラベルを持ちます。
    * `onTap`: アイテムがタップされたときに呼び出されるコールバック。
    * `currentIndex`: 現在選択されているアイテムのインデックス。
* **使用例**:
    ```dart
    BottomNavigationBar(
      items: const <BottomNavigationBarItem>[
        BottomNavigationBarItem(
          icon: Icon(Icons.home),
          label: 'ホーム',
        ),
        BottomNavigationBarItem(
          icon: Icon(Icons.business),
          label: 'ビジネス',
        ),
        BottomNavigationBarItem(
          icon: Icon(Icons.school),
          label: '学校',
        ),
      ],
      currentIndex: _selectedIndex, // State管理される変数
      selectedItemColor: Colors.amber[800],
      onTap: (int index) {
        setState(() {
          _selectedIndex = index;
        });
      },
    )
    ```

## 4. `Drawer`

* **概要**: 画面の端（通常は左端）からスライドして表示されるサイドパネル（ドロワーメニュー）です。アプリケーションの主要なナビゲーションや設定項目などを配置するのに適しています。`Scaffold` の `drawer` プロパティに設定されます。
* **特徴**:
    * 通常、`AppBar` のハンバーガーアイコンをタップするか、画面の端からスワイプすることで開閉する。
    * 内部には `ListView` を用いてメニュー項目を配置することが多い。
* **使用例**:
    ```dart
    Drawer(
      child: ListView(
        padding: EdgeInsets.zero, // 上部のパディングを削除
        children: const <Widget>[
          DrawerHeader(
            decoration: BoxDecoration(
              color: Colors.blue,
            ),
            child: Text(
              'Drawer Header',
              style: TextStyle(
                color: Colors.white,
                fontSize: 24,
              ),
            ),
          ),
          ListTile(
            leading: Icon(Icons.message),
            title: Text('メッセージ'),
          ),
          ListTile(
            leading: Icon(Icons.account_circle),
            title: Text('プロフィール'),
          ),
          ListTile(
            leading: Icon(Icons.settings),
            title: Text('設定'),
          ),
        ],
      ),
    )
    ```

## 5. `TabBar` と `TabBarView`

* **概要**: タブによるナビゲーションを実装するためのウィジェットです。`TabBar` はタブのヘッダーを表示し、`TabBarView` は各タブに対応するコンテンツを表示します。通常は `DefaultTabController` と組み合わせて使用されます。
* **`TabBar` の主なプロパティ**:
    * `tabs`: `Tab` ウィジェットのリスト。
    * `controller`: `TabController`。
* **`TabBarView` の主なプロパティ**:
    * `children`: 各タブに対応するウィジェットのリスト。
    * `controller`: `TabController`。
* **使用例**:
    ```dart
    // DefaultTabController で TabController を提供
    DefaultTabController(
      length: 3, // タブの数
      child: Scaffold(
        appBar: AppBar(
          title: const Text('タブの例'),
          bottom: const TabBar( // AppBar の bottom に TabBar を配置
            tabs: <Widget>[
              Tab(icon: Icon(Icons.cloud), text: 'クラウド'),
              Tab(icon: Icon(Icons.beach_access), text: 'ビーチ'),
              Tab(icon: Icon(Icons.brightness_5), text: '太陽'),
            ],
          ),
        ),
        body: const TabBarView( // TabBarView に各タブのコンテンツを配置
          children: <Widget>[
            Center(child: Text('クラウドのコンテンツ')),
            Center(child: Text('ビーチのコンテンツ')),
            Center(child: Text('太陽のコンテンツ')),
          ],
        ),
      ),
    )
    ```

## 6. `Card`

* **概要**: Material Design の「カード」を表現するウィジェットです。角が丸く、影が付いた長方形の領域で、関連する情報やアクションをグループ化して表示するのに適しています。
* **主なプロパティ**:
    * `child`: カードの内部に表示されるウィジェット。
    * `elevation`: カードの影の深さ。
    * `color`: カードの背景色。
    * `shape`: カードの形状（`RoundedRectangleBorder` など）。
    * `margin`: カードの外側の余白。
* **使用例**:
    ```dart
    Card(
      elevation: 4.0, // 影の深さ
      margin: const EdgeInsets.all(16.0),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisSize: MainAxisSize.min, // コンテンツに合わせてサイズを調整
          children: const <Widget>[
            ListTile(
              leading: Icon(Icons.album),
              title: Text('アルバムタイトル'),
              subtitle: Text('アーティスト名'),
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.end,
              children: <Widget>[
                TextButton(
                  onPressed: () {},
                  child: Text('購入'),
                ),
                SizedBox(width: 8),
                TextButton(
                  onPressed: () {},
                  child: Text('聴く'),
                ),
              ],
            ),
          ],
        ),
      ),
    )
    ```

## 7. `AlertDialog` と `SimpleDialog`

* **概要**: ユーザーに重要な情報や選択肢を提示するためのポップアップダイアログです。
* **`AlertDialog`**:
    * タイトル、コンテンツ、アクションボタンを持つ汎用的なダイアログ。
    * `showDialog` 関数を使って表示する。
* **`SimpleDialog`**:
    * タイトルと、リスト形式の選択肢（通常は `SimpleDialogOption`）を表示するダイアログ。
* **使用例 (AlertDialog)**:
    ```dart
    // ボタンが押されたときにダイアログを表示する例
    ElevatedButton(
      onPressed: () {
        showDialog(
          context: context,
          builder: (BuildContext context) {
            return AlertDialog(
              title: const Text('確認'),
              content: const Text('本当に削除しますか？'),
              actions: <Widget>[
                TextButton(
                  onPressed: () {
                    Navigator.of(context).pop(); // ダイアログを閉じる
                  },
                  child: const Text('キャンセル'),
                ),
                TextButton(
                  onPressed: () {
                    Navigator.of(context).pop(); // ダイアログを閉じる
                    print('削除を実行');
                  },
                  child: const Text('削除'),
                ),
              ],
            );
          },
        );
      },
      child: const Text('ダイアログを表示'),
    )
    ```

## 複雑な構造のUlウィジェットについてのまとめ

これらの複雑な構造のUIウィジェットは、Flutter アプリケーションの見た目と機能性を大幅に向上させます。`Scaffold` がアプリケーションの骨格を定義し、`AppBar`、`BottomNavigationBar`、`Drawer` が主要なナビゲーションパターンを提供します。`TabBar`/`TabBarView` はタブベースのコンテンツを、`Card` は情報のグループ化を助を助け、`AlertDialog` はユーザーとのインタラクションを可能にします。これらのウィジェットを適切に組み合わせることで、Material Design のベストプラクティスに従った、魅力的で使いやすいアプリケーションを構築できます。