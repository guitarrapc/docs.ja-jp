---
title: '方法: ディレクトリをコピーする'
ms.date: 12/27/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- directory copying
- I/O [.NET Framework], copying directories
- subdirectory copying
- copying directories
- directories [.NET Framework], copying
ms.assetid: 5a969765-e5f8-4b4e-977e-90e2b0a1fe3c
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 2a7fa901d887701e0fa41a0887b363adec07dba2
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65644694"
---
# <a name="how-to-copy-directories"></a>方法: ディレクトリをコピーする
このトピックでは、I/O クラスを使用して、別の場所にディレクトリの内容を同期的にコピーする方法について説明します。 

ファイルを非同期的にコピーする例については、「[非同期ファイル I/O](../../../docs/standard/io/asynchronous-file-i-o.md)」を参照してください。 

この例では、`DirectoryCopy` メソッドの `copySubDirs` を `true` に設定することでサブディレクトリをコピーします。 `DirectoryCopy` メソッドは各サブディレクトリでそれ自体を呼び出すことで、コピーするサブディレクトリがなくなるまでサブディレクトリを再帰的にコピーします。  
  
## <a name="example"></a>例  
 [!code-csharp[System.IO.Directory_Copy#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Directory_Copy/cs/program.cs#1)]
 [!code-vb[System.IO.Directory_Copy#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Directory_Copy/vb/Program.vb#1)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.IO.FileInfo>
- <xref:System.IO.DirectoryInfo>
- <xref:System.IO.FileStream>
- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
- [共通 I/O タスク](../../../docs/standard/io/common-i-o-tasks.md)
- [非同期ファイル I/O](../../../docs/standard/io/asynchronous-file-i-o.md)
