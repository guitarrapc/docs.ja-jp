---
title: インターネット インフォメーション サービスでのホスティング
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF], IIS
ms.assetid: ddae14e8-143c-442d-b660-2046809b2d43
ms.openlocfilehash: d881a75e95bc3c3eef1df651b380210ff51ea3ce
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2019
ms.locfileid: "65877503"
---
# <a name="hosting-in-internet-information-services"></a>インターネット インフォメーション サービスでのホスティング
Windows Communication Foundation (WCF) サービスをホストするための 1 つは、インターネット インフォメーション サービス (IIS) アプリケーション内でです。 このホスティング モデルは、ASP.NET と ASP.NET Web サービス (ASMX) Web サービスによって使用されるモデルに似ています。  
  
## <a name="versions-of-iis"></a>IIS バージョン  
 WCF は、次のオペレーティング システムで次のバージョンの IIS でホストできます。  
  
- [!INCLUDE[wxpsp2](../../../../includes/wxpsp2-md.md)] 上で IIS 5.1 を使用。 この環境は、後で [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] などのサーバー オペレーティング システムに展開される、IIS ホスト型アプリケーションの設計と開発に役立ちます。  
  
- [!INCLUDE[iis601](../../../../includes/iis601-md.md)] に対する [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]。 [!INCLUDE[iis601](../../../../includes/iis601-md.md)] は、スケーラビリティと信頼性を向上し、アプリケーションの分離を実現する高度なプロセス モデルを提供します。 この環境では、HTTP 通信のみを使用する WCF サービスの運用環境のデプロイに適しています。  
  
- [!INCLUDE[wv](../../../../includes/wv-md.md)] および [!INCLUDE[lserver](../../../../includes/lserver-md.md)] 上で IIS 7.0 を使用。 IIS 7.0 は、[!INCLUDE[iis601](../../../../includes/iis601-md.md)] と同じ高度なプロセス モデルを提供しますが、Windows プロセス アクティブ化サービス (WAS) を使用して、HTTP 以外のプロトコル経由でのアクティブ化とネットワーク通信を可能にします。 この環境では、WCF (HTTP、net.tcp、net.pipe、net.msmq など) でサポートされている任意のネットワーク プロトコル経由で通信する WCF サービスの開発に適しています。 WAS の詳細については、次を参照してください。 [Windows プロセス アクティブ化サービスでのホスティング](../../../../docs/framework/wcf/feature-details/hosting-in-windows-process-activation-service.md)します。  
  
- [Windows Server AppFabric](https://go.microsoft.com/fwlink/?LinkId=196496)連携[!INCLUDE[iisver](../../../../includes/iisver-md.md)]NET4 WCF および WF サービス用に環境をホストしている豊富なアプリケーションを提供する Windows プロセス アクティブ化サービス (WAS)。 この利点には、プロセス ライフサイクル管理、プロセス リサイクル、共有ホスティング、迅速な障害保護、プロセスの孤立化、オンデマンド アクティブ化、状態監視などがあります。 詳細については、次を参照してください。 [AppFabric のホスティング機能](https://go.microsoft.com/fwlink/?LinkId=196494)と[AppFabric ホスティングの概念](https://go.microsoft.com/fwlink/?LinkId=196495)します。  
  
## <a name="benefits-of-iis-hosting"></a>IIS ホスティングの利点  
 IIS で WCF サービスをホストすると、いくつかの利点があります。  
  
- IIS でホストされる WCF サービスが展開されているし、IIS のアプリケーション、ASP.NET アプリケーションや ASMX などの他の任意の型と同様に管理します。  
  
- IIS はプロセスのアクティブ化、状態管理、リサイクル機能を提供し、ホストされるアプリケーションの信頼性を向上します。  
  
- ASP.NET のように ASP.NET でホストされる WCF サービスを利用できます ASP.NET の共有ホスティング モデルの改良されたサーバー密度とスケーラビリティのための共通のワーカー プロセスで複数のアプリケーションが存在します。  
  
- IIS でホストされる WCF サービスと同様の動的なコンパイル モデルを使用して、[!INCLUDE[vstecasplong](../../../../includes/vstecasplong-md.md)]の展開には、サービスがホストされている、開発が簡単になります。  
  
 注意してくださいは IIS で WCF サービスをホストする場合は、その IIS 5.1 と[!INCLUDE[iis601](../../../../includes/iis601-md.md)]は HTTP 通信のみに制限されています。 ホスト環境の選択の詳細については、次を参照してください。[ホスティング サービス](../../../../docs/framework/wcf/hosting-services.md)します。  
  
## <a name="deploying-an-iis-hosted-wcf-service"></a>IIS にホストされた WCF サービスの展開  
 IIS でホストされる WCF サービスの展開の開発とは、次のタスクで構成されます。  
  
- ある IIS、ASP.NET、WCF、および WCF HTTP アクティブ化コンポーネントが正しくインストールされ、登録されていることを確認します。  
  
- 新しい IIS アプリケーションを作成または既存の ASP.NET アプリケーションを再利用します。  
  
- WCF サービスの .svc ファイルを作成します。  
  
- IIS アプリケーションにサービス実装を展開します。  
  
- WCF サービスを構成します。  
  
 これらの各タスクの詳細については、次を参照してください。[インターネット WCF サービスの展開](../../../../docs/framework/wcf/feature-details/deploying-an-internet-information-services-hosted-wcf-service.md)します。  
  
## <a name="wcf-services-and-aspnet"></a>WCF サービスと ASP.NET  
 WCF サービスは、いずれかのサイド ASP.NET を使用した、またはサービスが、ASP.NET Web アプリケーション プラットフォームで提供される機能の活用を実行できる ASP.NET 互換モードでホストされています。 これらの機能の詳細については、次を参照してください。 [WCF サービスと ASP.NET](../../../../docs/framework/wcf/feature-details/wcf-services-and-aspnet.md)します。  
  
## <a name="see-also"></a>関連項目

- [ServiceHostFactory を使用したホストの拡張](../../../../docs/framework/wcf/extending/extending-hosting-using-servicehostfactory.md)
- [インターネット インフォメーション サービスでホストされる WCF サービスの配置](../../../../docs/framework/wcf/feature-details/deploying-an-internet-information-services-hosted-wcf-service.md)
- [WCF サービスと ASP.NET](../../../../docs/framework/wcf/feature-details/wcf-services-and-aspnet.md)
- [インターネット インフォメーション サービス ホスティングのベスト プラクティス](../../../../docs/framework/wcf/feature-details/internet-information-services-hosting-best-practices.md)
- [Windows Communication Foundation での Internet Information Services 7.0 の構成](../../../../docs/framework/wcf/feature-details/configuring-iis-for-wcf.md)
- [AppFabric のホスティング機能](https://go.microsoft.com/fwlink/?LinkId=201276)
