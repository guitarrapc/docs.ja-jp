---
title: 補間文字列 (Visual Basic)
ms.date: 10/31/2017
ms.openlocfilehash: 408f3232258b3b4c7fe6ec160149f8ac70b84b03
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62024561"
---
# <a name="interpolated-strings-visual-basic-reference"></a>補間文字列 (Visual Basic リファレンス)

文字列の作成に使用されます。  挿入文字列は、*挿入式*が含まれているテンプレート文字列のように見えます。  挿入文字列は、含まれる挿入式をその文字列表現に置き換えた文字列を返します。 この機能は、Visual Basic 14 以降のバージョンで使用できます。

挿入文字列の引数は、[複合書式指定文字列](../../../../standard/base-types/composite-formatting.md#composite-format-string)よりもわかりやすいものです。  たとえば、挿入文字列には、

```vb
Console.WriteLine($"Name = {name}, hours = {hours:hh}")
```

2 つの挿入式、"{name}" と "{hours:hh}" が含まれています。 同等の複合書式指定文字列は次のとおりです。

```vb
Console.WriteLine("Name = {0}, hours = {1:hh}", name, hours);
```

挿入文字列の構造は次のとおりです。

```vb
$"<text> {<interpolated-expression> [,<field-width>] [:<format-string>] } <text> ..."
```

ここで、

- *field-width* はフィールドの文字数を示す符号付き整数です。 これが正である場合、フィールドは右揃えとなり、負である場合は左揃えとなります。

- *format-string* は、書式設定されるオブジェクトの種類に適した書式指定文字列です。 たとえば、<xref:System.DateTime>値である可能性があります、[標準の日付と時刻の書式指定文字列](~/docs/standard/base-types/standard-date-and-time-format-strings.md)"D"または"d"など。

> [!IMPORTANT]
> `$` と文字列を開始する `"` の間に空白を入れることはできません。 そうと、コンパイラ エラーが発生します。

文字列リテラルを使用できる場所であればどこでも補間文字列を使用できます。  この挿入文字列は、挿入文字列を含むコードが実行されるたびに評価されます。 これにより、挿入文字列の定義と評価を分離することができます。

挿入文字列に中かっこ "{" または "}" を含めるには、2 つの中かっこ "{{" または "}}" を使用します。  詳細については、「暗黙の型変換」を参照してください。

挿入文字列には、引用符 (")、コロン (:)、コンマ (,) など、特別な意味を持つその他の文字が含まれることがあります。これらの文字がリテラル テキストに出現する場合は、エスケープする必要があります。また、これらの文字が言語要素として挿入式に含まれる場合は、かっこで区切られた式に含める必要があります。 次の例では、引用符をエスケープして、結果文字列に引用符を含めています。さらに、かっこを使用して式 `(age == 1 ? "" : "s")` を区切り、コロンが書式指定文字列の先頭として解釈されないようにしています。

[!code-vb[interpolated-strings](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings4.vb)]

## <a name="implicit-conversions"></a>暗黙の型変換

挿入文字列から暗黙の型変換を行う方法は 3 種類あります。

1. 挿入文字列から <xref:System.String> への変換。 次の例では、挿入文字列式を文字列表現で置き換えた文字列が返されます。 例:

   [!code-vb[interpolated-strings1](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings1.vb)]

   これが、文字列解釈の最終的な結果です。 二重中かっこ ("{{" および "}}") のすべての発生箇所は、単一の中かっこに変換されます。

2. 挿入文字列から <xref:System.IFormattable> 変数への変換。これは、単一の <xref:System.IFormattable> インスタンスから、カルチャ固有のコンテンツを持った複数の結果文字列の作成を可能にするものです。 これは、個々のカルチャに適切な数値書式や日付形式などの情報を含めるのに便利です。  二重中かっこ ("{{" および "}}") のすべての出現箇所は、明示的または暗黙的に <xref:System.Object.ToString> メソッドを呼び出して文字列を書式指定するまで、二重中かっこのままです。  すべての含まれる補間式に変換されます{0}、{1}など。

   次の例では、リフレクションを使用することで、挿入文字列から作成された <xref:System.IFormattable> 変数のフィールドおよびプロパティ値だけでなく、メンバーも表示しています。 また、<xref:System.IFormattable> 変数を <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> メソッドに渡しています。

   [!code-vb[interpolated-strings2](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings2.vb)]

   挿入文字列の検査には、リフレクションを使用する必要があることに注意してください。 挿入文字列が <xref:System.Console.WriteLine(System.String)> などの文字列書式指定メソッドに渡されると、書式指定項目が解決され、結果文字列が返されます。

3. 挿入文字列からの変換、<xref:System.FormattableString>複合書式指定文字列を表す変数です。 たとえば、複合書式指定文字列を検査し、それが結果文字列としてどのように表示されるかを検査すると、クエリを構築する場合にインジェクション攻撃を防ぐことができます。 A<xref:System.FormattableString>も含まれています。

      - <xref:System.Globalization.CultureInfo.CurrentCulture> の結果文字列を生成する <xref:System.FormattableString.ToString> オーバーロード。

      - A<xref:System.FormattableString.Invariant%2A>の文字列を生成するメソッド、<xref:System.Globalization.CultureInfo.InvariantCulture>します。

      - 特定のカルチャの結果文字列を生成する <xref:System.FormattableString.ToString(System.IFormatProvider)> メソッド。

    出現するすべての二重中かっこ ("{{"と"}}") 書式を設定するまで二重中かっこのままです。  すべての含まれる補間式に変換されます{0}、{1}など。

   [!code-vb[interpolated-strings3](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings3.vb)]

## <a name="see-also"></a>関連項目

- <xref:System.IFormattable?displayProperty=nameWithType>
- <xref:System.FormattableString?displayProperty=nameWithType>
- [Visual Basic の言語リファレンス](index.md)
