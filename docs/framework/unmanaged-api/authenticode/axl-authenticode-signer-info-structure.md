---
title: AXL_AUTHENTICODE_SIGNER_INFO 構造体
ms.date: 03/30/2017
ms.assetid: 81c0f8b4-ce35-4716-8651-b642d40648a2
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 024837870aade6b0beb76fe758a571b15fd14d59
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61948978"
---
# <a name="axlauthenticodesignerinfo-structure"></a>AXL_AUTHENTICODE_SIGNER_INFO 構造体
Authenticode の署名者情報を定義します。  
  
## <a name="syntax"></a>構文  
  
```  
typedef struct _AXL_AUTHENTICODE_SIGNER_INFO {  
    DWORD cbSize;  
    HRESULT dwError;  
    ALG_ID algHash;  
    LPCWSTR pwszHash  
    LPCWSTR pwszDescription;  
    LPCWSTR pwszDescriptionUrl;  
    PCCERT_CHAIN_CONTEXT pChainContext  
} AXL_AUTHENTICODE_SIGNER_INFO, * PAXL_AUTHENTICODE_SIGNER_INFO;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`cbSize`|この構造のサイズ。|  
|`dwError`|エラー コード。|  
|`algHash`|ハッシュアルゴリズム。|  
|`pwszHash`|ハッシュ。|  
|`pwszDescription`|説明。|  
|`pwszDescriptionUrl`|説明の URL。|  
|`pChainContext`|署名者のチェーン コンテキスト。 参照してください、 [CERT_CONTEXT](/windows/desktop/api/wincrypt/ns-wincrypt-_cert_context)構造体。|  
  
## <a name="see-also"></a>関連項目

- [Authenticode](../../../../docs/framework/unmanaged-api/authenticode/index.md)
