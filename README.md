# BuilderBuilder

BuilderBuilder は、C# / MSTest の `DataRow` が横に長くなって読みにくくなったときに、`DynamicData` と Builder パターンへリファクタリングするためのコードスニペットジェネレーターです。

単体の `BB.html` として動作します。生成されるコードは、既存の MSTest テストクラス内に貼り付ける前提です。`namespace` や外側のクラス宣言は生成しません。

## 機能

- MSTest の `DynamicData` 用メンバーを生成
- テストケース用 record と Builder クラスを生成
- テストメソッドの引数を生成ケースオブジェクト 1 つに整理
- テストメソッド本体は空で生成
- `メソッド名: name=value` 形式の `DisplayName` を生成
- `HashMap` と `HashMap<T>` の既定値は `new()` で生成
- 型、パラメータ名、初期値を別々の入力欄で管理
- 初期パラメータは `string name`、`int num`、`bool flg`、`HashMap data`
- ドラッグ&ドロップでパラメータを並び替え
- 入力内容を `localStorage` に保存
- 確認後に入力を初期状態へ戻すクリア機能
- 生成スニペットをクリップボードへコピー

## 使い方

1. ブラウザで `BB.html` を開く。
2. 対象の関数名を入力する。
3. 型、パラメータ名、初期値を入力する。
4. 必要に応じてパラメータをドラッグ&ドロップで並び替える。
5. `スニペットをコピー` を押す。
6. 既存の MSTest テストクラス内に貼り付ける。

入力内容はブラウザの `localStorage` に保存されます。`クリア` を押して確認ダイアログで OK を選ぶと、初期状態に戻ります。

初期値は C# の式としてそのまま出力されます。空欄の場合は型から既定値を生成します。

```text
"test"
1
false
new()
```

生成スニペットは、C# ファイル側で次の `using` が利用できる前提です。

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Reflection;
```

## DisplayName

`DisplayName` は、関数名とパラメータ値から自動生成されます。

```text
CalculatePrice: name=Alice, num=1, flg=True, data=...
```

`WithDisplayName(...)` を使った場合は、指定した値の次の行に自動生成された `DisplayName` が続きます。

```text
指定した表示名
CalculatePrice: name=Alice, num=1, flg=True, data=...
```

## 生成されるコード

生成スニペットには次の要素が含まれます。

- `public static IEnumerable<object[]> GetXxxTestCase()`
- `public static string GetXxxDisplayName(MethodInfo _, object[] data)`
- `[TestMethod]` と `[DynamicData(...)]`
- `public sealed record XxxTestCase`
- `public sealed class XxxTestCaseBuilder`

## ライセンス

ライセンスは未設定です。
