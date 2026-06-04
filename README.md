# BuilderBuilder

BuilderBuilder は、C# / MSTest の `DataRow` が横に長くなって読みにくくなったときに、`DynamicData` と Builder パターンへリファクタリングするためのコードスニペットジェネレーターです。

単体の `BB.html` として動作します。生成されるコードは、既存の MSTest テストクラス内に貼り付ける前提です。`namespace` や外側のクラス宣言は生成しません。

## 機能

- MSTest の `DynamicData` 用メンバーを生成
- テストケース用クラスと Builder クラスを生成
- テストメソッドの引数を生成ケースオブジェクト 1 つに整理
- テンプレートから `DisplayName` を生成
- 空の `DisplayName` も許可
- パラメータ名と型を別々の入力欄で管理
- 生成スニペットをクリップボードへコピー

## 使い方

1. ブラウザで `BB.html` を開く。
2. 対象の関数名を入力する。
3. `DisplayName` テンプレートを入力する。空欄でも可。
4. パラメータ名と型を入力する。
5. `スニペットをコピー` を押す。
6. 既存の MSTest テストクラス内に貼り付ける。

生成スニペットは、C# ファイル側で次の `using` が利用できる前提です。

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Reflection;
```

## DisplayName

`DisplayName` テンプレートでは、次のプレースホルダーを使えます。

- `{method}`: 対象の関数名
- `{parameters}`: 全パラメータを `name=value` 形式で展開
- `{amount}` のようなパラメータ名: そのパラメータの値

例:

```text
{method}: {parameters}
amount={amount}
```

テンプレートを空にした場合、生成される `CreateDisplayName()` は空文字を返します。

## 生成されるコード

生成スニペットには次の要素が含まれます。

- `public static IEnumerable<object[]> XxxData()`
- `public static string GetXxxDisplayName(MethodInfo methodInfo, object[] data)`
- `[TestMethod]` と `[DynamicData(...)]`
- `private static object[] Case(XxxCase testCase)`
- `private sealed class XxxCase`
- `private sealed class XxxCaseBuilder`

## ライセンス

ライセンスは未設定です。
