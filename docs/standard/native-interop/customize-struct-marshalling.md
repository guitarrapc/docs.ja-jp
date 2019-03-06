---
title: 構造体のマーシャリングのカスタマイズ - .NET
description: .NET で構造体をネイティブ表現にマーシャリングする方法をカスタマイズする手順について説明します。
author: jkoritzinsky
ms.author: jekoritz
ms.date: 01/18/2019
dev_langs:
- csharp
- cpp
ms.openlocfilehash: c4d2d84a59aebedda2d1e6380caeef170051c0a3
ms.sourcegitcommit: b56d59ad42140d277f2acbd003b74d655fdbc9f1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2019
ms.locfileid: "56411443"
---
# <a name="customizing-structure-marshalling"></a><span data-ttu-id="d2e5d-103">構造体のマーシャリングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="d2e5d-103">Customizing structure marshalling</span></span>

<span data-ttu-id="d2e5d-104">構造体の既定のマーシャリング規則が、必要な規則とは異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-104">Sometimes the default marshalling rules for structures aren't exactly what you need.</span></span> <span data-ttu-id="d2e5d-105">.NET ランタイムには、構造体のレイアウトやフィールドのマーシャリング方法をカスタマイズできる拡張ポイントがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-105">The .NET runtimes provide a few extension points for you to customize your structure's layout and how fields are marshaled.</span></span>

## <a name="customizing-structure-layout"></a><span data-ttu-id="d2e5d-106">構造体レイアウトのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="d2e5d-106">Customizing structure layout</span></span>

<span data-ttu-id="d2e5d-107">.NET には <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> 属性と <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> 列挙型が用意されており、フィールドをメモリに配置する方法をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-107">.NET provides the <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> attribute and the <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> enumeration to allow you to customize how fields are placed in memory.</span></span> <span data-ttu-id="d2e5d-108">次のガイダンスを使用すると、一般的な問題を回避できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-108">The following guidance will help you avoid common issues.</span></span>

<span data-ttu-id="d2e5d-109">**✔️ 推奨**: 可能な限り `LayoutKind.Sequential` を使用するようにします。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-109">**✔️ CONSIDER** using `LayoutKind.Sequential` whenever possible.</span></span>

<span data-ttu-id="d2e5d-110">**✔️ 実行**: ネイティブ構造体に共用体などの明示的なレイアウトもある場合は、マーシャリング時に `LayoutKind.Explicit` のみを使用します。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-110">**✔️ DO** only use `LayoutKind.Explicit` in marshalling when your native struct is also has an explicit layout, such as a union.</span></span>

<span data-ttu-id="d2e5d-111">**❌ 回避**: Windows 以外のプラットフォームで構造体をマーシャリングするときに、`LayoutKind.Explicit` を使用しないようにします。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-111">**❌ AVOID** using `LayoutKind.Explicit` when marshalling structures on non-Windows platforms.</span></span> <span data-ttu-id="d2e5d-112">.NET Core ランタイムは、Intel または AMD 64 ビットの Windows 以外のシステム上でネイティブ関数への値による明示的な構造体の受け渡しをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-112">The .NET Core runtime doesn't support passing explicit structures by value to native functions on Intel or AMD 64-bit non-Windows systems.</span></span> <span data-ttu-id="d2e5d-113">ただし、ランタイムはすべてのプラットフォーム上で明示的な構造体の参照渡しをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-113">However, the runtime supports passing explicit structures by reference on all platforms.</span></span>

## <a name="customizing-boolean-field-marshalling"></a><span data-ttu-id="d2e5d-114">ブール値フィールドのマーシャリングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="d2e5d-114">Customizing boolean field marshalling</span></span>

<span data-ttu-id="d2e5d-115">ネイティブ コードには、さまざまなブール表現があります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-115">Native code has many different boolean representations.</span></span> <span data-ttu-id="d2e5d-116">Windows だけでも、ブール値を表現する方法が 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-116">On Windows alone, there are three ways to represent boolean values.</span></span> <span data-ttu-id="d2e5d-117">ランタイムは構造体のネイティブ定義を認識しないので、ランタイムに可能な最善の処理は、ブール値のマーシャリング方法を推測することです。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-117">The runtime doesn't know the native definition of your structure, so the best it can do is make a guess on how to marshal your boolean values.</span></span> <span data-ttu-id="d2e5d-118">.NET ランタイムには、ブール値フィールドのマーシャリング方法を示す方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-118">The .NET runtime provides a way to indicate how to marshal your boolean field.</span></span> <span data-ttu-id="d2e5d-119">以下の例は、.NET `bool` をさまざまなネイティブ ブール型にマーシャリングする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-119">The following examples show how to marshal .NET `bool` to different native boolean types.</span></span>

<span data-ttu-id="d2e5d-120">次の例に示すように、ブール値は既定でネイティブの 4 バイト Win32 [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) 値としてマーシャリングされます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-120">Boolean values default to marshalling as a native 4-byte Win32 [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) value as shown in the following example:</span></span>

```csharp
public struct WinBool
{
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

<span data-ttu-id="d2e5d-121">明示的にする場合は、<xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> 値を使用して上記と同じ動作にすることができます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-121">If you want to be explicit, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> value to get the same behavior as above:</span></span>

```csharp
public struct WinBool
{
    [MarshalAs(UnmanagedType.Bool)]
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

<span data-ttu-id="d2e5d-122">以下の `UmanagedType.U1` 値または `UnmanagedType.I1` 値を使用して、`b` フィールドを 1 バイトのネイティブ `bool` 型としてマーシャリングするようにランタイムに指示できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-122">Using the `UmanagedType.U1` or `UnmanagedType.I1` values below, you can tell the runtime to marshal the `b` field as a 1-byte native `bool` type.</span></span>

```csharp
public struct CBool
{
    [MarshalAs(UnmanagedType.U1)]
    public bool b;
}
```

```cpp
struct CBool
{
    public bool b;
};
```

<span data-ttu-id="d2e5d-123">Windows では、<xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> 値を使用して、ブール値を 2 バイトの `VARIANT_BOOL` 値にマーシャリングするようにランタイムに指示できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-123">On Windows, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> value to tell the runtime to marshal your boolean value to a 2-byte `VARIANT_BOOL` value:</span></span>

```csharp
public struct VariantBool
{
    [MarshalAs(UnmanagedType.VariantBool)]
    public bool b;
}
```

```cpp
struct VariantBool
{
    public VARIANT_BOOL b;
};
```

> [!NOTE]
> <span data-ttu-id="d2e5d-124">`VARIANT_BOOL` は、`VARIANT_TRUE = -1` と `VARIANT_FALSE = 0` が他のほとんどのブール型とは異なります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-124">`VARIANT_BOOL` is different than most bool types in that `VARIANT_TRUE = -1` and `VARIANT_FALSE = 0`.</span></span> <span data-ttu-id="d2e5d-125">さらに、`VARIANT_TRUE` と等しくないすべての値は false と見なされます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-125">Additionally, all values that aren't equal to `VARIANT_TRUE` are considered false.</span></span>

## <a name="customizing-array-field-marshalling"></a><span data-ttu-id="d2e5d-126">配列フィールドのマーシャリングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="d2e5d-126">Customizing array field marshalling</span></span>

<span data-ttu-id="d2e5d-127">.NET には、配列のマーシャリングをカスタマイズする方法もいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-127">.NET also includes a few ways to customize array marshalling.</span></span>

<span data-ttu-id="d2e5d-128">既定では、.NET は要素の連続したリストへのポインターとして配列をマーシャリングします。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-128">By default, .NET marshals arrays as a pointer to a contiguous list of the elements:</span></span>

```csharp
public struct DefaultArray
{
    public int[] values;
}
```

```cpp
struct DefaultArray
{
    int* values;
};
```

<span data-ttu-id="d2e5d-129">COM API とやり取りする場合は、必要に応じて配列を `SAFEARRAY*` オブジェクトとしてマーシャリングします。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-129">If you're interfacing with COM APIs, you may have to marshal arrays as `SAFEARRAY*` objects.</span></span> <span data-ttu-id="d2e5d-130"><xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 値と <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> 値を使用して、配列を `SAFEARRAY*` としてマーシャリングするようにランタイムに指示できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-130">You can use the <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> and the <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> value to tell the runtime to marshal an array as a `SAFEARRAY*`:</span></span>

```csharp
public struct SafeArrayExample
{
    [MarshalAs(UnmanagedType.SafeArray)]
    public int[] values;
}
```

```cpp
struct SafeArrayExample
{
    SAFEARRAY* values;
};
```

<span data-ttu-id="d2e5d-131">`SAFEARRAY` に含まれる要素の型をカスタマイズする必要がある場合は、<xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> および <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> フィールドを使用して `SAFEARRAY` の正確な要素の種類をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-131">If you need to customize what type of element is in the `SAFEARRAY`, then you can use the <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> and <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> fields to customize the exact element type of the `SAFEARRAY`.</span></span>

<span data-ttu-id="d2e5d-132">インプレースで配列をマーシャリングする必要がある場合は、<xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> 値を使用して、インプレースで配列をマーシャリングするようマーシャラーに指示できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-132">If you need to marshal the array in-place, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> value to tell the marshaler to marshal the array in-place.</span></span> <span data-ttu-id="d2e5d-133">このマーシャリングを使用するときは、ランタイムが構造体の空間を適切に割り当てられるように、<xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> フィールドに配列内の要素数の値を指定する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-133">When you're using this marshalling, you also must supply a value to the <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> field  for the number of elements in the array so the runtime can correctly allocate space for the structure.</span></span>

```csharp
public struct InPlaceArray
{
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = 4)]
    public int[] values;
}
```

```cpp
struct InPlaceArray
{
    int values[4];
};
```

> [!NOTE]
> <span data-ttu-id="d2e5d-134">.NET は、C99 の柔軟な配列のメンバーとして可変長配列フィールドをマーシャリングすることをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-134">.NET doesn't support marshalling a variable length array field as a C99 Flexible Array Member.</span></span>

## <a name="customizing-string-field-marshalling"></a><span data-ttu-id="d2e5d-135">文字列フィールドのマーシャリングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="d2e5d-135">Customizing string field marshalling</span></span>

<span data-ttu-id="d2e5d-136">.NET には、文字列フィールドをマーシャリングするためのさまざまなカスタマイズも用意されています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-136">.NET also provides a wide variety of customizations for marshalling string fields.</span></span>

<span data-ttu-id="d2e5d-137">既定では、.NET は文字列を null で終わる文字列へのポインターとしてマーシャリングします。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-137">By default, .NET marshals a string as a pointer to a null-terminated string.</span></span> <span data-ttu-id="d2e5d-138">エンコードは、<xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> の <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> フィールドの値によって決まります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-138">The encoding depends on the value of the <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> field in the <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType>.</span></span> <span data-ttu-id="d2e5d-139">属性が指定されていない場合、エンコードは既定で ANSI エンコードになります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-139">If no attribute is specified, the encoding defaults to an ANSI encoding.</span></span>

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char* str;
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

<span data-ttu-id="d2e5d-140">フィールドごとに異なるエンコードを使用する必要がある場合、または単に構造体の定義内でより明示的にする場合は、<xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 属性に <xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> 値または <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> 値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-140">If you need to use different encodings for different fields or just prefer to be more explicit in your struct definition, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> or <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> values on a <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> attribute.</span></span>

```csharp
public struct AnsiString
{
    [MarshalAs(UnmanagedType.LPStr)]
    public string str;
}
```

```cpp
struct AnsiString
{
    char* str;
};
```

```csharp
public struct UnicodeString
{
    [MarshalAs(UnmanagedType.LPWStr)]
    public string str;
}
```

```cpp
struct UnicodeString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

<span data-ttu-id="d2e5d-141">UTF-8 エンコードを使用して文字列をマーシャリングする場合は、<xref:System.Runtime.InteropServices.MarshalAsAttribute> に <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> 値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-141">If you want to marshal your strings using the UTF-8 encoding, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> value in your <xref:System.Runtime.InteropServices.MarshalAsAttribute>.</span></span>


```csharp
public struct UTF8String
{
    [MarshalAs(UnmanagedType.LPUTF8Str)]
    public string str;
}
```

```cpp
struct UTF8String
{
    char* str;
};
```

> [!NOTE]
> <span data-ttu-id="d2e5d-142"><xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> を使用するには、.NET Framework 4.7 (またはそれ以降のバージョン) または .NET Core 1.1 (またはそれ以降のバージョン) のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-142">Using <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> requires either .NET Framework 4.7 (or later versions) or .NET Core 1.1 (or later versions).</span></span> <span data-ttu-id="d2e5d-143">.NET Standard 2.0 では使用できません。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-143">It isn't available in .NET Standard 2.0.</span></span>

<span data-ttu-id="d2e5d-144">COM API を使用している場合は、文字列を `BSTR` としてマーシャリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-144">If you're working with COM APIs, you may need to marshal a string as a `BSTR`.</span></span> <span data-ttu-id="d2e5d-145"><xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> 値を使用すると、文字列を `BSTR` としてマーシャリングできます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-145">Using the <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> value, you can marshal a string as a `BSTR`.</span></span>

```csharp
public struct BString
{
    [MarshalAs(UnmanagedType.BStr)]
    public string str;
}
```

```cpp
struct BString
{
    BSTR str;
};
```

<span data-ttu-id="d2e5d-146">WinRT ベースの API を使用している場合は、文字列を `HSTRING` としてマーシャリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-146">When using a WinRT-based API, you may need to marshal a string as an `HSTRING`.</span></span>  <span data-ttu-id="d2e5d-147"><xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> 値を使用すると、文字列を `HSTRING` としてマーシャリングできます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-147">Using the <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> value, you can marshal a string as a `HSTRING`.</span></span>

```csharp
public struct HString
{
    [MarshalAs(UnmanagedType.HString)]
    public string str;
}
```

```cpp
struct BString
{
    HSTRING str;
};
```

<span data-ttu-id="d2e5d-148">API で、構造体でインプレースで文字列を渡す必要がある場合は、<xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType> 値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-148">If your API requires you to pass the string in-place in the structure, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType> value.</span></span> <span data-ttu-id="d2e5d-149">`ByValTStr` でマーシャリングされた文字列のエンコードは、`CharSet` 属性によって決まる点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-149">Do note that the encoding for a string marshalled by `ByValTStr` is determined from the `CharSet` attribute.</span></span> <span data-ttu-id="d2e5d-150">さらに、文字列長を <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> フィールドで渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-150">Additionally, it requires that a string length is passed by the <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> field.</span></span>

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char str[4];
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t str[4]; // Could also be wchar_t[4] on Windows.
};
```

## <a name="customizing-decimal-field-marshalling"></a><span data-ttu-id="d2e5d-151">10 進数フィールドのマーシャリングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="d2e5d-151">Customizing decimal field marshalling</span></span>

<span data-ttu-id="d2e5d-152">Windows を使用している場合は、ネイティブの [`CY` または `CURRENCY`](/windows/desktop/api/wtypes/ns-wtypes-tagcy) 構造体を使用する API がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-152">If you're working on Windows, you might encounter some APIs that use the native [`CY` or `CURRENCY`](/windows/desktop/api/wtypes/ns-wtypes-tagcy) structure.</span></span> <span data-ttu-id="d2e5d-153">既定で、.NET の `decimal` 型はネイティブの [`DECIMAL`](/windows/desktop/api/wtypes/ns-wtypes-tagdec) 構造体にマーシャリングされます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-153">By default, the .NET `decimal` type marshals to the native [`DECIMAL`](/windows/desktop/api/wtypes/ns-wtypes-tagdec) structure.</span></span> <span data-ttu-id="d2e5d-154">ただし、値が <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> の <xref:System.Runtime.InteropServices.MarshalAsAttribute> を使用して、`decimal` 値をネイティブの `CY` 値に変換するようにマーシャラーに指示することができます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-154">However, you can use a <xref:System.Runtime.InteropServices.MarshalAsAttribute> with the <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> value to instruct the marshaler to convert a `decimal` value to a native `CY` value.</span></span>

```csharp
public struct Currency
{
    [MarshalAs(UnmanagedType.Currency)]
    public decimal dec;
}
```

```cpp
struct Currency
{
    CY dec;
};
```

## <a name="marshalling-systemobjects"></a><span data-ttu-id="d2e5d-155">`System.Object` のマーシャリング</span><span class="sxs-lookup"><span data-stu-id="d2e5d-155">Marshalling `System.Object`s</span></span>

<span data-ttu-id="d2e5d-156">Windows では、`object` 型のフィールドをネイティブ コードにマーシャリングできます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-156">On Windows, you can marshal `object`-typed fields to native code.</span></span> <span data-ttu-id="d2e5d-157">このようなフィールドは、次の 3 つの型のいずれかにマーシャリングできます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-157">You can marshal these fields to one of three types:</span></span>
- [`VARIANT`](/windows/desktop/api/oaidl/ns-oaidl-tagvariant)
- [`IUnknown*`](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
- <span data-ttu-id="d2e5d-158">[`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch).</span><span class="sxs-lookup"><span data-stu-id="d2e5d-158">[`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch).</span></span> 

<span data-ttu-id="d2e5d-159">既定では、`object` 型のフィールドはオブジェクトをラップする `IUnknown*` にマーシャリングされます。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-159">By default, an `object`-typed field will be marshalled to an `IUnknown*` that wraps the object.</span></span>

```csharp
public struct ObjectDefault
{
    public object obj;
}
```

```cpp
struct ObjectDefault
{
    IUnknown* obj;
};
```

<span data-ttu-id="d2e5d-160">オブジェクト フィールドを `IDispatch*` にマーシャリングする場合は、値が <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType> の <xref:System.Runtime.InteropServices.MarshalAsAttribute> を追加します。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-160">If you want to marshal an object field to an `IDispatch*`, add a <xref:System.Runtime.InteropServices.MarshalAsAttribute> with the <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType> value.</span></span>

```csharp
public struct ObjectDispatch
{
    [MarshalAs(UnmanagedType.IDispatch)]
    public object obj;
}
```

```cpp
struct ObjectDispatch
{
    IDispatch* obj;
};
```

<span data-ttu-id="d2e5d-161">`VARIANT` としてマーシャリングする場合は、値が <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> の <xref:System.Runtime.InteropServices.MarshalAsAttribute> を追加します。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-161">If you want to marshal it as a `VARIANT`, add a <xref:System.Runtime.InteropServices.MarshalAsAttribute> with the <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> value.</span></span>

```csharp
public struct ObjectVariant
{
    [MarshalAs(UnmanagedType.Struct)]
    public object obj;
}
```

```cpp
struct ObjectVariant
{
    VARIANT obj;
};
```

<span data-ttu-id="d2e5d-162">次の表は、`obj` フィールドのさまざまなランタイム型が `VARIANT` に格納されるさまざまな型にどのようにマップされるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="d2e5d-162">The following table describes how different runtime types of the `obj` field map to the various types stored in a `VARIANT`:</span></span>

| <span data-ttu-id="d2e5d-163">.NET 型</span><span class="sxs-lookup"><span data-stu-id="d2e5d-163">.NET Type</span></span> | <span data-ttu-id="d2e5d-164">バリアント型</span><span class="sxs-lookup"><span data-stu-id="d2e5d-164">VARIANT Type</span></span> | | <span data-ttu-id="d2e5d-165">.NET 型</span><span class="sxs-lookup"><span data-stu-id="d2e5d-165">.NET Type</span></span> | <span data-ttu-id="d2e5d-166">バリアント型</span><span class="sxs-lookup"><span data-stu-id="d2e5d-166">VARIANT Type</span></span> |
|------------|--------------|-|----------|--------------|
|  `byte`  | `VT_UI1` |     | `System.Runtime.InteropServices.BStrWrapper` | `VT_BSTR` |
| `sbyte`  | `VT_I1`  |     | `object`  | `VT_DISPATCH` |
| `short`  | `VT_I2`  |     | `System.Runtime.InteropServices.UnknownWrapper` | `VT_UNKNOWN` |
| `ushort` | `VT_UI2` |     | `System.Runtime.InteropServices.DispatchWrapper` | `VT_DISPATCH` |
| `int`    | `VT_I4`  |     | `System.Reflection.Missing` | `VT_ERROR` |
| `uint`   | `VT_UI4` |     | `(object)null` | `VT_EMPTY` |
| `long`   | `VT_I8`  |     | `bool` | `VT_BOOL` |
| `ulong`  | `VT_UI8` |     | `System.DateTime` | `VT_DATE` |
| `float`  | `VT_R4`  |     | `decimal` | `VT_DECIMAL` |
| `double` | `VT_R8`  |     | `System.Runtime.InteropServices.CurrencyWrapper` | `VT_CURRENCY` |
| `char`   | `VT_UI2` |     | `System.DBNull` | `VT_NULL` |
| `string` | `VT_BSTR`|