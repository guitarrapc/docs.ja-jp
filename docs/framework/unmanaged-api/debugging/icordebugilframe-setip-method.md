---
title: ICorDebugILFrame::SetIP メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugILFrame.SetIP
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugILFrame::SetIP
helpviewer_keywords:
- SetIP method, ICorDebugILFrame interface [.NET Framework debugging]
- ICorDebugILFrame::SetIP method [.NET Framework debugging]
ms.assetid: 23f38dc1-85e4-4263-9235-2d05bbb6a833
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: a25e52c6b858aaa602ffade0e407b1aaf6e5c67e
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61995588"
---
# <a name="icordebugilframesetip-method"></a>ICorDebugILFrame::SetIP メソッド
命令ポインターを Microsoft intermediate language (MSIL) コードで指定されたオフセット位置に設定します。  
  
## <a name="syntax"></a>構文  
  
```  
HRESULT SetIP (  
    [in] ULONG32 nOffset  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `nOffset`  
 MSIL コード内のオフセット位置。  
  
## <a name="remarks"></a>Remarks  
 呼び出す`SetIP`すぐにすべてのフレームと現在のスレッドのチェーンが無効にします。 デバッガーには、呼び出しの後にフレーム情報が必要がある場合`SetIP`、新しいスタック トレースを実行する必要があります。  
  
 [ICorDebug](../../../../docs/framework/unmanaged-api/debugging/icordebug-interface.md)有効な状態でスタック フレームを保持します。 ただし、場合でも、フレームは、有効な状態では、まだある可能性があります初期化されていないローカル変数などの問題。 呼び出し元は、実行中のプログラムの一貫性を保証します。  
  
 64 ビットのプラットフォームでのうち、命令ポインターを移動できません、`catch`または`finally`ブロックします。 場合`SetIP`というはエラーを示す HRESULT を返す、64 ビット プラットフォームで、このような移動するために、します。  
  
## <a name="requirements"></a>必要条件  
 **プラットフォーム:**[システム要件](../../../../docs/framework/get-started/system-requirements.md)に関するページを参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
