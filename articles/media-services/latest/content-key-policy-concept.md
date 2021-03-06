---
title: Media Services의 콘텐츠 키 정책 - Azure | Microsoft Docs
description: 이 문서에서는 콘텐츠 키 정책의 개념과 Azure Media Services에서 이러한 정책을 사용하는 방법을 설명합니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f12632b20d516c81e21a50cfdda7e40d4163afc1
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742221"
---
# <a name="content-key-policies"></a>콘텐츠 키 정책

Azure Media Services를 사용하여 컴퓨터를 떠날 때부터 저장, 처리 및 배달에 이르는 과정 내내 미디어를 보호할 수 있습니다. Media Services를 사용하면 Advanced Encryption Standard(AES-128) 또는 Microsoft PlayReady, Google Widevine 및 Apple FairPlay 등 세 가지 주요 DRM(디지털 권한 관리) 시스템 중 하나로 동적 암호화된 라이브 콘텐츠 및 주문형 콘텐츠를 제공할 수 있습니다. 또한 Media Services는 인증된 클라이언트에게 AES 키 및DRM(PlayReady, Widevine 및 FairPlay) 라이선스를 배달하는 서비스를 제공합니다.

Azure Media Services v3에서 [콘텐츠 키 정책](https://docs.microsoft.com/rest/api/media/contentkeypolicies)을 사용하면 콘텐츠 키가 Media Services 키 전송 구성 요소를 통해 최종 클라이언트에 전송되는 방법을 지정할 수 있습니다. 자세한 내용은 [콘텐츠 보호 개요](content-protection-overview.md)를 참조하세요.

모든 자산에 대해 동일한 ContentKeyPolicy를 재사용하는 것이 좋습니다. ContentKeyPolicies를 업데이트할 수 있으므로 키 회전을 수행하려는 경우 새 키를 사용하여 토큰 제한으로 기존 ContentKeyPolicy에 새 ContentKeyPolicyOption을 추가할 수 있습니다. 또는 기본 확인 키와 기존 정책 및 옵션에서 대체 확인 키 목록을 업데이트할 수 있습니다. 키 배달 캐시를 업데이트하고 업데이트된 정책을 선택하는 데 최대 15분까지 걸릴 수 있습니다.

## <a name="contentkeypolicy-definition"></a>ContentKeyPolicy 정의

다음 표에는 ContentKeyPolicy의 속성 및 해당 정의가 나와 있습니다.

|이름|설명|
|---|---|
|id|리소스에 대한 정규화된 리소스 ID입니다.|
|이름|리소스의 이름입니다.|
|properties.created |정책을 만든 날짜입니다.|
|properties.description |정책에 대한 설명입니다.|
|properties.lastModified|정책을 마지막으로 수정한 날짜입니다.|
|properties.options |키 정책 옵션입니다.|
|properties.policyId|레거시 정책 ID입니다.|
|형식|리소스 형식입니다.|

전체 정의는 [콘텐츠 키 정책](https://docs.microsoft.com/rest/api/media/contentkeypolicies)을 참조하세요.

## <a name="filtering-ordering-paging"></a>필터링, 정렬, 페이징

Media Services에서 지원하는 ContentKeyPolicies에 대한 OData 쿼리 옵션은 다음과 같습니다. 

* $filter 
* $orderby 
* $top 
* $skiptoken 

연산자 설명:

* Eq = 같음
* Ne = 같지 않음
* Ge = 크거나 같음
* Le = 작거나 같음
* Gt = 보다 큼
* Lt = 보다 작음

### <a name="filteringordering"></a>필터링/순서

다음 표에는 이러한 옵션을 ContentKeyPolicies 속성에 적용하는 방법이 나와 있습니다. 

|이름|Filter|순서|
|---|---|---|
|id|||
|이름|Eq, ne, ge, le, gt, lt|오름차순 및 내림차순|
|properties.created |Eq, ne, ge, le, gt, lt|오름차순 및 내림차순|
|properties.description |Eq, ne, ge, le, gt, lt||
|properties.lastModified|Eq, ne, ge, le, gt, lt|오름차순 및 내림차순|
|properties.options |||
|properties.policyId|Eq, ne||
|형식|||

### <a name="pagination"></a>페이지 매김

네 개의 활성화된 정렬 순서 각각에 대해 페이지 매김이 지원됩니다. 현재 페이지 크기는 10입니다.

> [!TIP]
> 항상 다음 링크를 사용하여 컬렉션을 열거하고, 특정 페이지 크기에 따라 달라지지 않아야 합니다.

쿼리 응답에 많은 항목이 포함된 경우 서비스에서 "\@odata.nextLink" 속성을 반환하여 결과의 다음 페이지를 가져옵니다. 전체 결과 집합을 통해 페이지에 사용할 수 있습니다. 페이지 크기는 구성할 수 없습니다. 

컬렉션을 통해 페이징하는 동안 ContentKeyPolicies가 생성되거나 삭제되면 변경 내용이 반환된 결과에 반영됩니다(해당 변경 내용이 다운로드되지 않은 컬렉션의 일부인 경우). 

다음 C# 예제에서는 계정의 모든 ContentKeyPolicies를 열거하는 방법을 보여 줍니다.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

REST 예제는 [콘텐츠 키 정책 - List](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)를 참조하세요.

## <a name="next-steps"></a>다음 단계

[AES-128 동적 암호화 및 키 전달 서비스 사용](protect-with-aes128.md)

[DRM 동적 암호화 및 라이선스 배달 서비스 사용](protect-with-drm.md)
