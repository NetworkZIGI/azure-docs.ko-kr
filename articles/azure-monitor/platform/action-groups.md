---
title: Azure Portal에서 작업 그룹 만들기 및 관리
description: Azure Portal에서 작업 그룹을 만들고 관리하는 방법에 대해 알아봅니다.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 1/29/2019
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 5717074d53e267018b7697aeed21ea00d246592e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252220"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure Portal에서 작업 그룹 만들기 및 관리
## <a name="overview"></a>개요 ##
작업 그룹은 Azure 구독 소유자가 정의한 알림 기본 설정 컬렉션입니다. Azure Monitor 및 Service Health 경고는 작업 그룹을 사용하여 경고가 트리거되었음을 사용자에게 알립니다. 사용자의 요구 사항에 따라 다양한 경고가 동일한 작업 그룹을 사용할 수도 있고 서로 다른 작업 그룹을 사용할 수도 있습니다. 구독에서는 작업 그룹을 2,000개까지 구성할 수 있습니다.

작업이 메일 또는 SMS로 개인에게 알리도록 구성된 경우 개인은 작업 그룹에 추가되었음을 나타내는 확인을 수신합니다.

이 문서에서는 Azure Portal에서 작업 그룹을 만들고 관리하는 방법을 보여 줍니다.

각 작업은 다음과 같은 속성으로 구성됩니다.

* **이름**: 작업 그룹 내의 고유 식별자입니다.  
* **작업 유형**: 수행할 작업입니다. 음성 통화, SMS, 이메일 보내기나, 여러 자동화된 작업 유형 트리거를 예로 들 수 있습니다. 이 문서 뒷부분에 나오는 유형을 참조하세요. 
* **세부 정보**: *작업 유형*에 따라 달라지는 해당 세부 정보입니다. 

Azure 리소스 관리자 템플릿을 사용하여 작업 그룹을 구성하는 방법에 대한 자세한 내용은 [작업 그룹 리소스 관리자 템플릿](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)을 참조하세요.

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure Portal을 사용하여 작업 그룹 만들기 ##
1. [포털](https://portal.azure.com)에서 **모니터**를 선택합니다. **모니터** 블레이드는 모든 모니터링 설정 및 데이터를 하나의 뷰에 통합합니다.

    ![“모니터링” 서비스](./media/action-groups/home-monitor.png)
1. **경고**, **작업 그룹 관리**를 차례로 선택합니다.

    ![작업 그룹 관리 단추](./media/action-groups/manage-action-groups.png)
1. **작업 그룹 추가**를 선택하고 필드를 입력합니다.

    ![“작업 그룹 추가” 명령](./media/action-groups/add-action-group.png)
1. **작업 그룹 이름** 상자에 이름을 입력하고 **약식 이름** 상자에 이름을 입력합니다. 약식 이름은 이 그룹을 사용하여 알림을 보내는 경우 전체 작업 그룹 이름 대신 사용됩니다.

      ![“작업 그룹 추가” 대화 상자](./media/action-groups/action-group-define.png)

1. **구독** 상자가 현재 구독으로 자동으로 채워집니다. 이 구독은 작업 그룹이 저장되는 위치입니다.

1. 작업 그룹이 저장되는 **리소스 그룹**을 선택합니다.

1. 각 작업에 대해 다음 정보를 제공하여 작업 목록을 정의합니다.

    a. **이름**: 이 작업에 대한 고유 식별자를 입력합니다.

    b. **작업 유형**: 메일/SMS/푸시/음성, 논리 앱, 웹후크, ITSM 또는 Automation Runbook을 선택합니다.

    다. **세부 정보**: 작업 유형에 따라 전화 번호, 메일 주소, 웹후크 URI, Azure 앱, ITSM 연결 또는 Automation Runbook을 입력합니다. ITSM 작업의 경우 **작업 항목** 및 ITSM 도구에 필요한 다른 필드를 추가로 지정합니다.

1. **확인**을 선택하여 작업 그룹을 만듭니다.

## <a name="manage-your-action-groups"></a>작업 그룹 관리 ##
작업 그룹을 만들면 **모니터** 블레이드의 **작업 그룹** 섹션에 표시됩니다. 관리하려는 작업 그룹을 선택합니다.

* 작업을 추가, 편집 또는 제거합니다.
* 작업 그룹을 삭제합니다.

## <a name="action-specific-information"></a>작업별 정보
**Azure 앱 푸시** - 하나의 작업 그룹에 최대 10개의 Azure 앱 작업이 포함될 수 있습니다. 현재, Azure 앱 작업은 ServiceHealth 알림만 지원합니다. 다른 경고 시간은 무시됩니다. [서비스 상태 알림이 게시될 때마다 경고 구성](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)을 참조하세요.

**이메일** - 다음 이메일 주소에서 이메일이 전송됩니다. 이메일 필터링이 적절하게 구성되었는지 확인합니다.
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

한 작업 그룹에 최대 1000개의 이메일 작업이 허용됩니다. [속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 문서를 참조하세요.

**ITSM** - ITSM 작업이 ITSM 연결을 요구하는 한 작업 그룹에 최대 10개의 ITSM 작업이 있을 수 있습니다. [ITSM 연결](../../azure-monitor/platform/itsmc-overview.md)을 만드는 방법에 대해 알아봅니다.

**논리 앱** - 한 작업 그룹에 최대 10개의 논리 앱 작업이 포함될 수 있습니다.

**함수 앱** - 작업으로 구성된 함수 앱의 함수 키는 Functions API를 통해 읽으며, 현재는 v2 함수 앱에서 "AzureWebJobsSecretStorageType" 앱 설정을 "파일"로 구성해야 합니다. 자세한 내용은 [Functions V2에서 키 관리 변경]( https://aka.ms/funcsecrets)을 참조하세요.

**Runbook** - 한 작업 그룹에 최대 10개의 Runbook이 작업이 있을 수 있습니다. Runbook 페이로드 제한은 [Azure 구독 서비스 제한](../../azure-subscription-service-limits.md)을 참조하세요.

**SMS** - 한 작업 그룹에 최대 10개의 SMS 작업이 있을 수 있습니다. [속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 문서를 참조하세요. [SMS 경고 동작](../../azure-monitor/platform/alerts-sms-behavior.md) 문서를 참조하세요.

**음성** - 하나의 작업 그룹에 최대 10개의 음성 작업이 포함될 수 있습니다.</dd>
[속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 문서를 참조하세요.</dd>

**웹후크** - 하나의 작업 그룹에 최대 10개의 웹후크 작업이 포함될 수 있습니다.
다시 시도 논리 - 응답에 대한 제한 시간은 10초입니다. 웹후크 호출은 HTTP 상태 코드 408, 429, 503, 504가 반환되거나 HTTP 엔드포인트가 응답하지 않으면 최대 2번 다시 시도됩니다. 10초 후에 첫 번째 다시 시도가 발생합니다. 두 번째 다시 시도는 100초 후에 진행됩니다. 두 시도가 모두 실패하면 어떤 작업 그룹에서도 해당 엔드포인트를 30분 동안 호출하지 않습니다.

원본 IP 주소 범위
    - 13.72.19.232
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196
    - 52.244.68.117

이러한 IP 주소에 대한 변경 내용에 대한 업데이트를 받으려면 작업 그룹 서비스에 대한 정보 알림을 모니터링하는 [서비스 상태 경고](./../../azure-monitor/platform/service-notifications.md)를 구성하는 것이 좋습니다.


## <a name="next-steps"></a>다음 단계 ##
* [SMS 경고 동작](../../azure-monitor/platform/alerts-sms-behavior.md)에 대해 자세히 알아보세요.  
* [활동 로그 경고 웹후크 스키마의 이해](../../azure-monitor/platform/activity-log-alerts-webhook.md)를 확인해 보세요.  
* [ITSM 커넥터](../../azure-monitor/platform/itsmc-overview.md)에 대해 자세히 알아보세요.
* 경고의 [속도 제한](../../azure-monitor/platform/alerts-rate-limiting.md)에 대해 자세히 알아보세요.
* [활동 로그 경고의 개요](../../azure-monitor/platform/alerts-overview.md)를 확인하고 경고를 받는 방법에 대해 알아보세요.  
* [서비스 상태 알림이 게시될 때마다 경고를 구성](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)하는 방법을 알아보세요.

