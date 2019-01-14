---
title: SqlStreamChars.SetLength(Int64) メソッド (System.Data.SqlTypes)
author: douglaslMS
ms.author: douglasl
ms.date: 12/20/2018
ms.technology:
- dotnet-data
topic_type:
- apiref
api_name:
- System.Data.SqlTypes.SqlStreamChars.SetLength
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: c7199cbd08e62b1f0391437a0c1864cb9036fe1f
ms.sourcegitcommit: a36cfc9dbbfc04bd88971f96e8a3f8e283c15d42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2019
ms.locfileid: "54222701"
---
# <a name="sqlstreamcharssetlengthint64-method"></a><span data-ttu-id="c85dc-102">SqlStreamChars.SetLength(Int64) メソッド</span><span class="sxs-lookup"><span data-stu-id="c85dc-102">SqlStreamChars.SetLength(Int64) Method</span></span>

<span data-ttu-id="c85dc-103">派生クラスでオーバーライドされると、ストリームで使用されるリソースを解放します。</span><span class="sxs-lookup"><span data-stu-id="c85dc-103">When overridden in a derived class, releases the resources used by the stream.</span></span> <span data-ttu-id="c85dc-104">このメソッドを含むアセンブリには、SQLAccess.dll で友人関係があります。</span><span class="sxs-lookup"><span data-stu-id="c85dc-104">The assembly that contains this method has a friend relationship with SQLAccess.dll.</span></span> <span data-ttu-id="c85dc-105">SQL Server で使用するものでは。</span><span class="sxs-lookup"><span data-stu-id="c85dc-105">It's intended for use by SQL Server.</span></span> <span data-ttu-id="c85dc-106">その他のデータベースには、そのデータベースによって提供されるホスティング メカニズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="c85dc-106">For other databases, use the hosting mechanism provided by that database.</span></span>

```csharp
public abstract void SetLength (long value);
```

## <a name="parameters"></a><span data-ttu-id="c85dc-107">パラメーター</span><span class="sxs-lookup"><span data-stu-id="c85dc-107">Parameters</span></span>

`value`\
<span data-ttu-id="c85dc-108">現在のストリームの希望の長さ (バイト数)。</span><span class="sxs-lookup"><span data-stu-id="c85dc-108">The desired length of the current stream in bytes.</span></span>

## <a name="remarks"></a><span data-ttu-id="c85dc-109">Remarks</span><span class="sxs-lookup"><span data-stu-id="c85dc-109">Remarks</span></span>

> [!WARNING]
> <span data-ttu-id="c85dc-110">`SqlStreamChars.SetLength`メソッドはプライベートであり、コード内で直接使用するものではありません。</span><span class="sxs-lookup"><span data-stu-id="c85dc-110">The `SqlStreamChars.SetLength` method is private and is not meant to be used directly in your code.</span></span>
>
> <span data-ttu-id="c85dc-111">Microsoft はいかなる運用アプリケーションでこのフィールドの使用をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="c85dc-111">Microsoft does not support the use of this field in a production application under any circumstance.</span></span>

## <a name="requirements"></a><span data-ttu-id="c85dc-112">必要条件</span><span class="sxs-lookup"><span data-stu-id="c85dc-112">Requirements</span></span>

<span data-ttu-id="c85dc-113">**名前空間:** <xref:System.Data.SqlTypes></span><span class="sxs-lookup"><span data-stu-id="c85dc-113">**Namespace:** <xref:System.Data.SqlTypes></span></span>

<span data-ttu-id="c85dc-114">**アセンブリ:** System.Data (system.data.dll 内)</span><span class="sxs-lookup"><span data-stu-id="c85dc-114">**Assembly:** System.Data (in System.Data.dll)</span></span>

<span data-ttu-id="c85dc-115">**.NET framework のバージョン:** 2.0 以降で使用可能です。</span><span class="sxs-lookup"><span data-stu-id="c85dc-115">**.NET Framework versions:** Available since 2.0.</span></span>