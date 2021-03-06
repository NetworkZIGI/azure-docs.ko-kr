---
title: Azure Event Grid Container Registry 이벤트 스키마
description: Azure Event Grid에서 Container Reigstry 이벤트에 대해 제공되는 속성 설명
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/13/2019
ms.author: spelluru
ms.openlocfilehash: 6f00d4f249543ece0eb8db4a8e040300d55b2de8
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462847"
---
# <a name="azure-event-grid-event-schema-for-container-registry"></a>Container Reigstry에 대한 Azure Event Grid 이벤트 스키마

이 문서에서는 Container Reigstry 이벤트에 대한 속성 및 스키마를 제공합니다. 이벤트 스키마에 대한 소개는 [Azure Event Grid 이벤트 스키마](event-schema.md)를 참조하세요.

## <a name="available-event-types"></a>사용할 수 있는 이벤트 유형

Blob Storage는 다음과 같은 이벤트 유형을 내보냅니다.

| 이벤트 유형 | 설명 |
| ---------- | ----------- |
| Microsoft.ContainerRegistry.ImagePushed | 이미지를 푸시할 때 발생합니다. |
| Microsoft.ContainerRegistry.ImageDeleted | 이미지를 삭제할 때 발생합니다. |

## <a name="example-event"></a>예제 이벤트

다음 예제는 이미지가 푸시된 이벤트의 스키마를 보여줍니다. 

```json
[{
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld:v1",
  "eventType": "ImagePushed",
  "eventTime": "2018-04-25T21:39:47.6549614Z",
  "data": {
    "id": "31c51664-e5bd-416a-a5df-e5206bc47ed0",
    "timestamp": "2018-04-25T21:39:47.276585742Z",
    "action": "push",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 3023,
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "length": 3023,
      "repository": "aci-helloworld",
      "tag": "v1"
    },
    "request": {
      "id": "7c66f28b-de19-40a4-821c-6f5f6c0003a4",
      "host": "demo.azurecr.io",
      "method": "PUT",
      "useragent": "docker/18.03.0-ce go/go1.9.4 git-commit/0520e24 os/windows arch/amd64 UpstreamClient(Docker-Client/18.03.0-ce \\\\(windows\\\\))"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

이미지가 삭제된 이벤트의 스키마는 다음과 유사합니다.

```json
[{
  "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld",
  "eventType": "ImageDeleted",
  "eventTime": "2018-04-26T17:56:01.8211268Z",
  "data": {
    "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
    "timestamp": "2018-04-26T17:56:00.996603117Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "repository": "aci-helloworld"
    },
    "request": {
      "id": "aeda5b99-4197-409f-b8a8-ff539edb7de2",
      "host": "demo.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>이벤트 속성

이벤트에는 다음과 같은 최상위 데이터가 있습니다.

| 자산 | type | 설명 |
| -------- | ---- | ----------- |
| 토픽 | string | 이벤트 원본에 대한 전체 리소스 경로입니다. 이 필드는 쓸 수 없습니다. Event Grid는 이 값을 제공합니다. |
| 제목 | string | 게시자가 정의한 이벤트 주체에 대한 경로입니다. |
| eventType | string | 이 이벤트 원본에 대해 등록된 이벤트 유형 중 하나입니다. |
| eventTime | string | 공급자의 UTC 시간을 기준으로 이벤트가 생성되는 시간입니다. |
| id | string | 이벤트에 대한 고유 식별자입니다. |
| 데이터 | object | Blob Storage 이벤트 데이터입니다. |
| dataVersion | string | 데이터 개체의 스키마 버전입니다. 게시자가 스키마 버전을 정의합니다. |
| metadataVersion | string | 이벤트 메타데이터의 스키마 버전입니다. Event Grid는 최상위 속성의 스키마를 정의합니다. Event Grid는 이 값을 제공합니다. |

데이터 개체의 속성은 다음과 같습니다.

| 자산 | type | 설명 |
| -------- | ---- | ----------- |
| id | string | 이벤트 ID입니다. |
| timestamp | string | 이벤트가 발생한 시간입니다. |
| action | string | 제공된 이벤트를 포함하는 동작입니다. |
| 대상 | object | 이벤트의 대상입니다. |
| request | object | 이벤트를 생성한 요청입니다. |

대상 개체의 속성은 다음과 같습니다.

| 자산 | type | 설명 |
| -------- | ---- | ----------- |
| mediaType | string | 참조된 개체의 MIME 형식입니다. |
| size | 정수 | 콘텐츠의 바이트 수입니다. 길이 필드와 동일합니다. |
| digest | string | 콘텐츠의 다이제스트로, 레지스트리 V2 HTTP API 사양에 따라 정의됩니다. |
| length | 정수 | 콘텐츠의 바이트 수입니다. 크기 필드와 동일합니다. |
| 리포지토리 | string | 리포지토리 이름입니다. |
| tag | string | 태그 이름입니다. |

요청 개체의 속성은 다음과 같습니다.

| 자산 | type | 설명 |
| -------- | ---- | ----------- |
| id | string | 이벤트를 시작한 요청의 ID입니다. |
| addr | string | IP 또는 호스트 이름 및 가능한 경우 이벤트를 시작한 클라이언트 연결의 포트입니다. 이 값은 표준 http 요청에서 온 RemoteAddr입니다. |
| host | string | 외부에서 액세스할 수 있는 레지스트리 인스턴스의 호스트 이름으로, 들어오는 요청의 http 호스트 헤더를 통해 지정됩니다. |
| 메서드 | string | 이벤트를 생성한 요청 메서드입니다. |
| useragent | string | 요청의 사용자 에이전트 헤더입니다. |

## <a name="next-steps"></a>다음 단계

* Azure Event Grid에 대한 소개는 [Event Grid란?](overview.md)을 참조하세요.
* Azure Event Grid 구독을 만드는 방법에 대한 자세한 내용은 [Event Grid 구독 스키마](subscription-creation-schema.md)를 참조하세요.
