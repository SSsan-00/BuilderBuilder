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
- 型とパラメータ名を別々の入力欄で管理
- 初期パラメータは `string name`、`int num`、`HashMap data`
- 入力内容を `localStorage` に保存
- 確認後に入力を初期状態へ戻すクリア機能
- 生成スニペットをクリップボードへコピー

## 使い方

1. ブラウザで `BB.html` を開く。
2. 対象の関数名を入力する。
3. 型とパラメータ名を入力する。
4. `スニペットをコピー` を押す。
5. 既存の MSTest テストクラス内に貼り付ける。

入力内容はブラウザの `localStorage` に保存されます。`クリア` を押して確認ダイアログで OK を選ぶと、初期状態に戻ります。

生成スニペットは、C# ファイル側で次の `using` が利用できる前提です。

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Reflection;
```

## DisplayName

`DisplayName` は、関数名とパラメータ値から自動生成されます。

```text
CalculatePrice: amount=0, taxRate=0, roundingMode=ToEven
```

## 生成されるコード

生成スニペットには次の要素が含まれます。

- `public static IEnumerable<object[]> GetXxxTestCase()`
- `public static string GetXxxDisplayName(MethodInfo _, object[] data)`
- `[TestMethod]` と `[DynamicData(...)]`
- `private sealed record XxxTestCase`
- `private sealed class XxxTestCaseBuilder`

## ライセンス

ライセンスは未設定です。
