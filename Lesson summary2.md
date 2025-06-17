 # Flutter 全まとめ集2

 ## statefulWidgetについて

#### 1. StatefulWidgetとは

* **定義:** 状態を持つ（statefulな）ウィジェットであり、その状態が変化すると、自動的にUIが再構築（再描画）されます。
* **特徴:**
    * ウィジェット自身は不変（immutable）ですが、それに関連付けられた`State`オブジェクトが可変（mutable）な状態を保持します。
    * 状態が変化したときに、`setState()`メソッドを呼び出すことで、ウィジェットツリーを再構築し、UIを更新します。
* **用途:** ユーザーインタラクション（ボタンタップ、テキスト入力など）、データフェッチの結果、アニメーションの進行状況など、時間とともに変化するデータを扱うUI要素に適しています。

#### 2. StatefulWidgetの構造

`StatefulWidget`は、常に2つのクラスで構成されます。

1.  **`StatefulWidget`クラス:**
    * 不変（immutable）なウィジェットの定義部分です。
    * `createState()`メソッドをオーバーライドし、対応する`State`オブジェクトを返します。
    * ウィジェットが受け取る初期データや、親ウィジェットから渡されるデータ（`final`フィールドとして定義）を保持します。

2.  **`State`クラス:**
    * 可変（mutable）な状態データを保持し、その状態に基づいてUIを構築する部分です。
    * `build()`メソッドをオーバーライドし、現在の状態に基づいてUIツリーを構築します。
    * `setState()`メソッドを呼び出すことで、状態の変更をフレームワークに通知し、`build()`メソッドを再実行させます。

#### 3. StatefulWidgetのライフサイクル

`StatefulWidget`の`State`オブジェクトには、いくつかの重要なライフサイクルメソッドがあります。これらのメソッドは、ウィジェットがツリーに挿入されたり、更新されたり、削除されたりする際に特定の処理を実行するために使用されます。

* **`createState()`:**
    * `StatefulWidget`が初めてツリーに挿入されるときに一度だけ呼び出されます。
    * ウィジェットに対応する`State`オブジェクトを作成して返します。
* **`initState()`:**
    * `State`オブジェクトが作成された直後、かつ`build()`が初めて呼び出される前に一度だけ呼び出されます。
    * サブスクリプションの開始、アニメーションコントローラーの初期化、初期データ（API呼び出しなど）のフェッチといった、状態の初期化処理に適しています。
    * `super.initState()`を必ず呼び出す必要があります。
* **`didChangeDependencies()`:**
    * `State`オブジェクトの依存関係が変更されたときに呼び出されます。
    * `InheritedWidget`を介して取得したデータが変更された場合などにトリガーされます。
    * `initState()`の直後にも一度だけ呼び出されます。
* **`build(BuildContext context)`:**
    * UIを構築するために呼び出されます。
    * `setState()`が呼び出された時、依存関係が変更された時、または親ウィジェットが再構築された時などに再実行されます。
    * 現在の状態に基づいて新しいウィジェットツリーを返します。
    * **このメソッドは純粋であるべきであり、副作用（状態の変更など）を持つべきではありません。**
* **`didUpdateWidget(covariant T oldWidget)`:**
    * 親ウィジェットが、このウィジェットの新しいインスタンスを提供したときに呼び出されます。
    * 新しいウィジェットのプロパティと古いウィジェットのプロパティを比較し、それに応じて状態を更新する場合に利用します。
    * `super.didUpdateWidget(oldWidget)`を必ず呼び出す必要があります。
* **`setState(VoidCallback fn)`:**
    * 状態が変更されたことをフレームワークに通知し、UIの再構築（`build()`メソッドの再実行）をスケジュールします。
    * 状態を変更する際には、**必ずこのメソッドの中で変更を行う必要があります**。
* **`deactivate()`:**
    * `State`オブジェクトがツリーから削除されようとしているときに呼び出されます。
    * 別の部分に移動される可能性があります（例: `ListView`でウィジェットがスクロールアウトされるが、後で再度表示される可能性がある場合）。
    * ツリーから完全に削除される前に、リソースの解放を行うことができます。
* **`dispose()`:**
    * `State`オブジェクトがツリーから完全に削除され、二度とビルドされないときに呼び出されます。
    * `initState()`で確立したサブスクリプションのキャンセル、アニメーションコントローラーの破棄、リソースの解放など、クリーンアップ処理を行うのに適しています。
    * `super.dispose()`を必ず呼び出す必要があります。

#### 4. StatefulWidgetクラスの基本形

```dart
class ウィジェットクラス extends StatefulWidget {

  @override
  ステートクラス createState() => ステートクラス();

}
```
#### 5. Stateクラスの基本形

```dart
class ステートクラス extends State<ウィジェットクラス> {

  // ......略......

  @override
  Widget build(BuildContext context) {
    // ......略......
  }

}
```
## FloatingActionButtonクラスについて

`FloatingActionButton` は、Flutter の Material Design における主要なアクションを表現するために使用される、円形のボタンウィジェットです。通常、アプリケーションのUIの右下隅に配置され、画面上の主要なアクションをユーザーに促します。

#### 主な特徴

* **視覚的な強調**: アプリケーションのメインアクションを目立たせるためにデザインされています。
* **Material Design**: Material Design のガイドラインに準拠しており、タップ時のアニメーションや影の表現が特徴です。
* **配置**: 通常 `Scaffold` ウィジェットの `floatingActionButton` プロパティに配置されます。

## main.dartのデフォルト

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

#  ウィジェットの基本レイアウトまとめ

Flutter におけるUI構築の核となるのは「ウィジェットツリー」であり、すべてのUI要素はウィジェットによって表現されます。レイアウトもまたウィジェットであり、他のウィジェットをどのように配置するかを決定します。

## 1. レイアウトの基本的な考え方

* **すべてはウィジェット**: テキスト、画像、ボタンだけでなく、パディング、マージン、行、列などもすべてウィジェットです。
* **コンポジション**: 小さなウィジェットを組み合わせて、より複雑なUIを構築します。
* **制約ベースのレイアウト**: 親ウィジェットが子ウィジェットに制約（最大幅、最大高さなど）を与え、子ウィジェットはその制約内で自身のサイズと位置を決定します。

## 2. レイアウトウィジェットの分類

大きく分けて以下の2種類があります。

1.  **単一の子ウィジェットを持つレイアウトウィジェット**: 自身の子ウィジェットに対して、何らかの視覚的・空間的な特性を付与するウィジェットです。
2.  **複数の子ウィジェットを持つレイアウトウィジェット**: 複数の子ウィジェットを、特定の規則（行、列、重ね合わせなど）に従って配置するウィジェットです。

## 3. 主要な単一の子ウィジェットを持つレイアウトウィジェット

### 3.1. `Container`
* **概要**: 最も汎用的なレイアウトウィジェットの一つで、単一の子ウィジェットを保持し、その周囲にパディング、マージン、ボーダー、背景色、変形などを適用できます。
* **主なプロパティ**:
    * `child`: 配置するウィジェット。
    * `padding`: 子ウィジェットの内側の余白。
    * `margin`: コンテナの外側の余白。
    * `color`: 背景色。
    * `decoration`: 背景画像やグラデーション、ボーダー、角丸など、より複雑な装飾。
    * `alignment`: 子ウィジェットの配置（`Alignment` オブジェクトを使用）。
    * `width`, `height`: コンテナの幅と高さ。
    * `constraints`: 幅と高さの最小/最大制約。
* **使用例**:
    ```dart
    Container(
      padding: EdgeInsets.all(20.0),
      margin: EdgeInsets.only(bottom: 10.0),
      color: Colors.blue[100],
      child: Text('Hello Container'),
    )
    ```

### 3.2. `Padding`
* **概要**: 指定されたパディング（余白）を子ウィジェットの周囲に追加します。
* **主なプロパティ**:
    * `padding`: `EdgeInsets` オブジェクトでパディングの量を指定。
    * `child`: パディングを適用するウィジェット。
* **使用例**:
    ```dart
    Padding(
      padding: EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
      child: Text('With Padding'),
    )
    ```

### 3.3. `Center`
* **概要**: 子ウィジェットを親ウィジェットの中央に配置します。
* **主なプロパティ**:
    * `child`: 中央に配置するウィジェット。
* **使用例**:
    ```dart
    Center(
      child: Text('Centered Text'),
    )
    ```

### 3.4. `Align`
* **概要**: 子ウィジェットを親ウィジェット内の指定された位置に配置します。`Center` よりも柔軟な配置が可能です。
* **主なプロパティ**:
    * `alignment`: `Alignment` オブジェクト（例: `Alignment.topLeft`, `Alignment.bottomRight`）で配置を指定。
    * `child`: 配置するウィジェット。
* **使用例**:
    ```dart
    Align(
      alignment: Alignment.bottomRight,
      child: Icon(Icons.favorite),
    )
    ```

### 3.5. `Expanded`
* **概要**: `Row` や `Column` の子として使用され、利用可能なスペースを埋めるように子ウィジェットを拡張します。
* **主なプロパティ**:
    * `flex`: 利用可能なスペースを分配する比率（デフォルトは1）。
    * `child`: 拡張するウィジェット。
* **使用例**:
    ```dart
    Row(
      children: [
        Expanded(
          flex: 2,
          child: Container(color: Colors.red, height: 50),
        ),
        Expanded(
          flex: 1,
          child: Container(color: Colors.blue, height: 50),
        ),
      ],
    )
    ```

### 3.6. `Flexible`
* **概要**: `Expanded` と同様に `Row` や `Column` の子として使用されますが、子ウィジェットが自身の内容に必要なスペースを維持しつつ、残りのスペースを柔軟に埋めることができます。`Expanded` は必ず利用可能なスペースを埋めますが、`Flexible` はそうではありません。
* **主なプロパティ**:
    * `flex`: 利用可能なスペースを分配する比率（デフォルトは1）。
    * `fit`: `FlexFit.tight` (Expandedと同じ動作) または `FlexFit.loose` (子のサイズに従う) を指定。
    * `child`: 柔軟に配置するウィジェット。
* **使用例**:
    ```dart
    Row(
      children: [
        Flexible(
          flex: 1,
          child: Text('これは長いテキストです。折り返されるかもしれません。', softWrap: true),
        ),
        Container(color: Colors.green, width: 50, height: 50),
      ],
    )
    ```

### 3.7. `SizedBox`
* **概要**: 特定の幅または高さを強制するボックスを作成します。スペースを空ける目的でよく使用されます。
* **主なプロパティ**:
    * `width`: 幅。
    * `height`: 高さ。
    * `child`: 配置するウィジェット（省略可能）。
* **使用例**:
    ```dart
    Column(
      children: [
        Text('上部のテキスト'),
        SizedBox(height: 20.0), // 縦に20ピクセルのスペース
        Text('下部のテキスト'),
      ],
    )
    ```

## 4. 主要な複数の子ウィジェットを持つレイアウトウィジェット

### 4.1. `Row`
* **概要**: 子ウィジェットを水平方向（行）に配置します。
* **主なプロパティ**:
    * `children`: 配置するウィジェットのリスト。
    * `mainAxisAlignment`: 主軸（水平方向）に沿った子ウィジェットの配置（`MainAxisAlignment.start`, `end`, `center`, `spaceBetween`, `spaceAround`, `spaceEvenly`）。
    * `crossAxisAlignment`: 副軸（垂直方向）に沿った子ウィジェットの配置（`CrossAxisAlignment.start`, `end`, `center`, `stretch`, `baseline`）。
    * `mainAxisSize`: 主軸のサイズ（`MainAxisSize.max` (デフォルト) または `min`）。
* **使用例**:
    ```dart
    Row(
      mainAxisAlignment: MainAxisAlignment.spaceAround,
      children: [
        Icon(Icons.home),
        Icon(Icons.search),
        Icon(Icons.settings),
      ],
    )
    ```

### 4.2. `Column`
* **概要**: 子ウィジェットを垂直方向（列）に配置します。
* **主なプロパティ**: (`Row` と同様ですが、主軸が垂直、副軸が水平になります)
    * `children`: 配置するウィジェットのリスト。
    * `mainAxisAlignment`: 主軸（垂直方向）に沿った子ウィジェットの配置。
    * `crossAxisAlignment`: 副軸（水平方向）に沿った子ウィジェットの配置。
    * `mainAxisSize`: 主軸のサイズ。
* **使用例**:
    ```dart
    Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text('First item'),
        Text('Second item'),
        Text('Third item'),
      ],
    )
    ```

### 4.3. `Stack`
* **概要**: 子ウィジェットをZ軸方向（手前と奥）に重ねて配置します。
* **主なプロパティ**:
    * `children`: 重ねるウィジェットのリスト（リストの後ろにあるものが手前に表示される）。
    * `alignment`: 子ウィジェットの初期配置（デフォルトは `AlignmentDirectional.topStart`）。
    * `fit`: `StackFit.loose` (子のサイズに従う) または `StackFit.expand` (親のサイズに拡張)。
    * `overflow`: 親の境界からはみ出した子