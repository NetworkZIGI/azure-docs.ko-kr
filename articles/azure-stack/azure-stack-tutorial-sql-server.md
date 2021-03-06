---
title: Azure Stack 사용자에 게 SQL database를 사용할 수 있도록 | Microsoft Docs
description: SQL Server 리소스 공급자를 설치 하 고 만드는 자습서는 Azure Stack 사용자가 SQL 데이터베이스를 만들 수 있는 제공 합니다.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 11/05/2018
ms.custom: mvc
ms.openlocfilehash: 983e8b279261d3ff8e5d24c8e3a6f61c5a787e5b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55240814"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>자습서: SQL database에 사용할 Azure Stack 사용자

Azure Stack 클라우드 관리자에 사용자는 제품을 만들 수 있습니다 (테 넌 트)는 클라우드 네이티브 앱, 웹사이트 및 워크 로드를 사용 하 여 사용할 수 있는 SQL database를 만듭니다. 이러한 사용자 지정, 주문형, 클라우드 기반 데이터베이스를 사용자에 게 제공 하 여 저장할 수 시간 및 리소스입니다. 이 설정 하려면 다음을 수행 해야 합니다.

> [!div class="checklist"]
> * SQL Server 리소스 공급자 배포
> * 제품 만들기
> * 제품 테스트

## <a name="deploy-the-sql-server-resource-provider"></a>SQL Server 리소스 공급자 배포

배포 프로세스에서 자세히 설명 되어는 [Azure Stack 문서에서 사용 하 여 SQL database](azure-stack-sql-resource-provider-deploy.md), 다음과 같은 기본 단계로 구성 됩니다.

1. [SQL 리소스 공급자 배포](azure-stack-sql-resource-provider-deploy.md)합니다.
2. [배포 확인](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal)합니다.
3. 호스팅 SQL server에 연결 하 여 용량을 제공 합니다. 자세한 내용은 참조 하세요. [호스팅 서버 추가](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>제품 만들기

1.  [할당량을 설정할](azure-stack-setting-quotas.md) 하 고 이름을 *SQLServerQuota*합니다. 선택 **Microsoft.SQLAdapter** 에 대 한 합니다 **Namespace** 필드입니다.
2.  [계획을 만들려면](azure-stack-create-plan.md)합니다. 이름을 *TestSQLServerPlan*를 선택 합니다 **Microsoft.SQLAdapter** 서비스 하 고 **SQLServerQuota** 할당량.

    > [!NOTE]
    > 사용자가 다른 앱을 만들 수 있도록, 다른 서비스 계획에 필요할 수 있습니다. 예를 들어, Azure Functions 필요 합니다 **Microsoft.Storage** Wordpress 해야 하는 동안 계획에서 서비스 **Microsoft.MySQLAdapter**합니다.

3.  [제품을 만드는](azure-stack-create-offer.md), 이름을 **TestSQLServerOffer** 선택 합니다 **TestSQLServerPlan** 계획 합니다.

## <a name="test-the-offer"></a>제품 테스트

이제는 SQL Server 리소스 공급자를 배포 하 고 제품을 생성, 사용자로 로그인 하 고, 제품을 구독할 하 고, 데이터베이스를 만들 수 있습니다.

### <a name="subscribe-to-the-offer"></a>제품 구독

1. Azure Stack 포털에 로그인 (https://portal.local.azurestack.external) 테 넌 트입니다.
2. 선택 **구독** 누릅니다 **TestSQLServerSubscription** 아래 **표시 이름**합니다.
3. 선택 **제품을 선택** > **TestSQLServerOffer** > **만들기**합니다.
4. 선택 **모든 서비스** > **구독** > **TestSQLServerSubscription** > **리소스 공급자**합니다.
5. 선택 **등록할** 옆에 **Microsoft.SQLAdapter** 공급자입니다.

### <a name="create-a-sql-database"></a>SQL 데이터베이스 만들기

1. 선택 **+**  >  **데이터 + 저장소** > **SQL Database**합니다.
2. 기본값을 그대로 유지 하거나 이러한 예제를 사용 하 여 다음 필드에 대 한 합니다.
    - **데이터베이스 이름**: SQLdb
    - **최대 크기 (mb)**: 100
    - **구독**: TestSQLOffer
    - **리소스 그룹**: SQL-RG
3. 선택 **로그인 설정**데이터베이스에 대 한 자격 증명을 입력 하 고 선택한 **확인**합니다.
4. 선택 **SKU** > SQL 호스팅 서버에 대해 만든 SQL SKU 선택 > 선택한 후 **확인**합니다.
5. **만들기**를 선택합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * SQL Server 리소스 공급자 배포
> * 제품 만들기
> * 제품 테스트

에 대해 알아보려면 다음 자습서로 이동 하는 방법.

> [!div class="nextstepaction"]
> [웹, 모바일 및 API apps를 사용자에 게 사용할 수 있도록]( azure-stack-tutorial-app-service.md)