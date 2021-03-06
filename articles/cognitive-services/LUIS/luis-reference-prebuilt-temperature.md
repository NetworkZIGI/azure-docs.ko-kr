---
title: 미리 빌드된 온도 엔터티
titleSuffix: Azure
description: 이 아티클에는 LUIS(Language Understanding)의 미리 빌드된 온도 엔터티가 포함됩니다.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 921f2bb2c03a24d5ed269452cdc7f14e7a40c73b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207992"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>LUIS 앱용 미리 빌드된 온도 엔터티
온도는 다양한 온도 형식을 추출합니다. 이 엔터티를 이미 학습했기 때문에 온도를 포함하는 예제 발언을 애플리케이션에 추가할 필요가 없습니다. 온도 엔터티는 [여러 문화권](luis-reference-prebuilt-entities.md)에서 지원됩니다. 

## <a name="types-of-temperature"></a>온도 형식
온도는 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) GitHub 리포지토리에서 관리됩니다.

## <a name="resolution-for-prebuilt-temperature-entity"></a>미리 빌드된 온도 엔터티의 해결 방법
다음 예제에서는 **builtin.temperature** 엔터티의 해결 방법을 보여줍니다.

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>다음 단계

[percentage](luis-reference-prebuilt-percentage.md), [number](luis-reference-prebuilt-number.md) 및 [age](luis-reference-prebuilt-age.md) 엔터티에 대해 알아봅니다. 
