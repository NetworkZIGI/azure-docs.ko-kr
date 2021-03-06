---
title: Custom Speech Service를 사용한 어쿠스틱 모델 만들기 자습서 - Microsoft Cognitive Services | Microsoft Docs
description: 이 자습서에서는 Microsoft Cognitive Services에서 Custom Speech Service를 사용하여 어쿠스틱 모델을 만드는 방법을 알아봅니다.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ms.openlocfilehash: 0e4c21a064cdb0a60aef49482eee4b768112b899
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216424"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>자습서: 사용자 지정 어쿠스틱 모델 만들기

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

이 자습서에서는 애플리케이션에서 인식하려는 음성 데이터에 대한 사용자 지정 어쿠스틱 모델을 만듭니다. 애플리케이션이 시끄러운 공장 등의 특정 환경 또는 특정 사용자 집단에서 사용하도록 설계된 경우에 사용자 지정 어쿠스틱 모델을 만드는 것이 유용합니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.
> [!div class="checklist"]
> * 데이터 준비
> * 어쿠스틱 데이터 집합 가져오기
> * 사용자 지정 어쿠스틱 모델 만들기

Cognitive Services 계정이 아직 없는 경우 시작하기 전에 [무료 계정](https://cris.ai)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

[Cognitive Services 구독](https://cris.ai/Subscriptions) 페이지를 열어 Cognitive Services 계정이 구독에 연결되어 있는지 확인합니다.

구독이 목록에 없으면 **무료 구독 가져오기** 단추를 클릭하여 Cognitive Services가 사용자를 위한 계정을 만들게 할 수 있습니다. 또는 **기존 구독 연결** 단추를 클릭하여 Azure Portal에서 만든 사용자 지정 Search Service 구독에 연결할 수 있습니다.

Azure Portal에서 사용자 지정 Search Service 구독을 만드는 방법에 대한 정보는 [Azure Portal에서 Cognitive Services API 계정 만들기](../../cognitive-services-apis-create-account.md)를 참조합니다.

## <a name="prepare-the-data"></a>데이터 준비

특정 도메인에 어쿠스틱 모델을 사용자 지정하려면 음성 데이터의 컬렉션이 필요합니다. 이 컬렉션은 음성 데이터의 오디오 파일 집합과 각 오디오 파일의 전사에 대한 텍스트 파일로 구성됩니다. 오디오 데이터는 인식기를 사용하고자 하는 시나리오를 대표해야 합니다.

예: 

*   시끄러운 공장 환경에서 음성을 더 잘 식별하도록 하려면 오디오 파일은 잡음이 있는 공장에서 시끄러운 공장에서 말하는 사람으로 구성되어야 합니다.
<a name="Preparing data to customize the acoustic model"></a>
*   가령 FDR의 Fireside Chats을 모두 전사하려는 경우와 같이 단일 스피커의 성능 최적화에 관심이 있으면 오디오 파일은 해당 스피커의 많은 예제로만 구성되어야 합니다.

어쿠스틱 모델을 사용자 지정하는 어쿠스틱 데이터 세트는 (1) 음성 데이터를 포함하는 오디오 파일 세트 및 (2) 모든 오디오 파일의 전사를 포함하는 파일로 구성됩니다.

### <a name="audio-data-recommendations"></a>오디오 데이터 권장 사항

*   데이터 집합의 모든 오디오 파일은 WAV(RIFF) 오디오 형식으로 저장되어야 합니다.
*   오디오 샘플링 속도는 8kHz 또는 16kHz이고 샘플 값은 압축되지 않은 PCM 16비트 부호 있는 정수(Short) 값으로 저장되어야 합니다.
*   단일 채널(mono) 오디오 파일만 지원됩니다.
*   오디오 파일 길이는 100밀리초 ~ 1분 범위여야 합니다. 각 오디오 파일은 이상적으로 100ms의 휴지가 있게 시작 및 종료되어야 하며 500ms ~ 1초 범위가 일반적입니다.
*   데이터에 배경 소음이 있는 경우 데이터에서 음성 콘텐츠 앞뒤 또는 앞이나 뒤에 몇 초처럼 휴지 세그먼트가 더 긴 예제가 있는 것이 좋습니다. 
*   각 오디오 파일은 단일 발언(예: 단일 받아쓰기 문장, 단일 쿼리 또는 대화 시스템의 한 전환)으로 구성되어야 합니다.
*   데이터 집합의 각 오디오 파일에는 고유한 파일 이름 및 “wav” 확장명이 있어야 합니다.
*   오디오 파일 집합은 하위 디렉터리 없이 단일 폴더 안에 있어야 하며 오디오 파일의 전체 집합은 단일 Zip 파일 보관으로 패키징되어야 합니다.

> [!NOTE]
> 웹 포털을 통한 데이터 가져오기는 현재 2GB로 제한되므로 이것이 어쿠스틱 데이터 집합의 최대 크기가 됩니다. 이것은 대략 16kHz로 기록된 오디오 17시간 또는 8kHz로 기록된 오디오 34시간에 해당합니다.  오디오 데이터에 대한 기본 요구 사항이 다음 표에 요약되어 있습니다.
>

| 속성 | 값 |
|---------- |----------|
| 파일 형식 | RIFF(WAV) |
| 샘플링 레이트 | 8000Hz 또는 16000Hz |
| 채널 | 1(mono) |
| 샘플 형식 | PCM, 16비트 정수 |
| 파일 지속 기간 | 0.1초 < 기간 < 60초 |
| 무음 목걸이 | 0.1초 초과 |
| 보관 형식 | Zip |
| 최대 보관 크기 | 2GB |

### <a name="transcriptions"></a>전사

모든 WAV 파일에 대한 전사는 단일 일반 텍스트 파일에 포함되어야 합니다. 전사 파일의 각 줄은 오디오 파일 중 하나의 이름을 갖고 그 뒤에 해당 전사가 와야 합니다. 파일 이름과 전사는 탭(\t)으로 구분 해야 합니다.

  예: 
```
  speech01.wav  speech recognition is awesome

  speech02.wav  the quick brown fox jumped all over the place

  speech03.wav  the lazy dog was not amused
```

시스템에서 처리할 수 있도록 전사는 텍스트로 정규화됩니다. 그러나 데이터를 사용자 지정 음성 서비스에 업로드하기 _전에_ 사용자가 수행해야 하는 몇몇 중요한 정규화 작업이 있습니다. 전사 준비에 적합한 언어는 [전사 지침](cognitive-services-custom-speech-transcription-guidelines.md)의 섹션을 참조하세요.

다음 단계는 [Custom Speech Service 포털](https://cris.ai)을 통해 수행됩니다. 

## <a name="import-the-acoustic-data-set"></a>어쿠스틱 데이터 집합 가져오기

오디오 파일 및 전사가 준비되면 서비스 웹 포털로 가져올 수 있습니다.

이를 위해 먼저 [Custom Speech Service 포털](https://cris.ai)에 로그인했는지 확인합니다. 그럼 다음, 위쪽 리본에서 "사용자 지정 음성" 드롭 다운 메뉴를 클릭하고 "적응 데이터"를 선택합니다. Custom Speech Service에 처음으로 데이터를 업로드하는 경우 "데이터 세트"라는 빈 테이블이 표시됩니다. 

"음향 데이터 세트" 행에서 "가져오기" 단추를 클릭하면 해당 사이트는 새 데이터 세트를 업로드하기 위한 페이지를 표시합니다.

![다음을 시도해 보세요.](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-import.png)

적합한 텍스트 상자에 _이름_과 _설명_을 입력합니다. 이는 업로드하는 다양한 데이터 집합을 추적하는 데 유용합니다. 다음으로 "전사 파일"과 "Wav 파일"에 대해 "파일 선택"을 클릭하고 각각에 대해 일반 텍스트 전사 파일과 WAV 파일의 zip 보관을 선택합니다. 이 작업이 완료되면 “가져오기”를 클릭하여 데이터를 업로드합니다. 그러면 데이터가 업로드됩니다. 대규모 데이터 집합의 경우 몇 분 정도 걸릴 수 있습니다.

업로드가 완료되면 "어쿠스틱 데이터 세트" 테이블로 돌아가며 어큐스틱 데이터 세트에 해당하는 항목이 표시됩니다. 고유 ID(GUID)가 할당되었음에 유의합니다. 데이터에는 또한 현재 상태를 반영하는 상태가 있습니다. 해당 상태는 처리를 위해 큐 대기하는 동안 "대기 상태"이며, 유효성 검사를 통과하는 동안 "처리 상태"이고, 데이터가 사용할 준비가 되면 "완료 상태"에 있게 됩니다.

데이터 유효성에는 파일 형식, 길이 및 샘플링 주기를 확인하는 오디오 파일에 대한 검사와, 파일 형식을 확인하고 몇 가지 텍스트 정규화를 수행하는 전사 파일에 대한 검사가 포함됩니다.

상태가 “완료”이면 “세부 정보”를 클릭하여 어쿠스틱 데이터 확인 보고서를 확인할 수 있습니다. 확인을 통과하고 실패한 발언 수가 실패한 발언에 대한 세부 정보와 함께 표시됩니다. 아래 예제에서는 오디오 형식이 부적절하여 두 WAV 파일이 확인에 실패했습니다. 이 데이터 집합에서 하나는 샘플링 주기가 부적절하고 다른 하나는 파일 형식이 잘못되었습니다.

![다음을 시도해 보세요.](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-report.png)

특정 시점에 데이터 집합의 이름이나 설명을 변경하려면 "편집" 링크를 클릭하고 해당 항목을 변경할 수 있습니다. 오디오 파일이나 전사는 수정할 수 없습니다.

## <a name="create-a-custom-acoustic-model"></a>사용자 지정 어쿠스틱 모델 만들기

어쿠스틱 데이터 집합의 상태가 "완료"이면 사용자 지정 어쿠스틱 모델을 만드는 데 사용될 수 있습니다. 이렇게 하려면 "사용자 지정 음성" 드롭다운 메뉴에서 "어쿠스틱 모델"을 클릭합니다. 사용자 지정 어쿠스틱 모델을 모두 나열하는 "Your models" 테이블이 표시됩니다. 이 테이블은 처음 사용할 경우 비어 있습니다. 현재 로캘이 테이블 제목에 표시됩니다. 현재, 어쿠스틱 모델은 미국 영어에 대해서만 만들 수 있습니다.

새 모델을 만들려면 테이블 제목 아래의 "새로 만들기"를 클릭합니다. 앞에서와 같이, 이 모델을 식별할 수 있는 이름과 설명을 입력합니다. 예를 들어 모델을 만드는 데 사용한 시작 모델과 어쿠스틱 데이터 집합을 “설명” 필드에 기록할 수 있습니다. 그런 다음, 드롭다운 메뉴에서 "기본 어쿠스틱 모델"을 선택합니다. 기본 모델은 사용자 지정의 시작 지점인 모델입니다. 두 가지 기본 어쿠스틱 모델 중에 선택할 수 있습니다. _Microsoft 검색 및 받아쓰기 AM_은 명령, 검색 쿼리 또는 받아쓰기 같은 애플리케이션의 지시 음성에 적합합니다. _Microsoft 대화형 모델_은 대화 스타일로 통용되는 음성을 인식하는 데 적합합니다. 이런 유형의 음성은 일반적으로 다른 사람을 대상으로 지시하며 콜센터나 회의에서 사용됩니다. 대화형 모델의 부분 결과 대기 시간은 검색 및 받아쓰기 모델보다 깁니다.

다음으로 드롭다운 메뉴를 사용하여 사용자 지정을 수행하는 데 사용할 어쿠스틱 데이터를 선택합니다.

![다음을 시도해 보세요.](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-create2.png)

필요에 따라, 처리가 완료되면 새 모델의 오프라인 테스트 수행을 선택할 수 있습니다. 그러면 사용자 지정 어쿠스틱 모델을 사용하여 특정 어쿠스틱 데이터 집합에 대한 음성-텍스트 평가를 실행하고 결과를 보고합니다. 이 테스트를 수행하려면 "정확도 테스트" 확인란을 선택합니다. 그런 다음, 드롭다운 메뉴에서 언어 모델을 선택합니다. 사용자 지정 언어 모델을 전혀 만들지 않은 경우 기본 언어 모델만 드롭다운 목록에 표시됩니다. 가이드에서 기본 언어 모델의 [설명](cognitive-services-custom-speech-create-language-model.md)을 참조하고 가장 적합한 모델을 선택합니다.

마지막으로 사용자 지정 모델을 평가하는 데 사용하려는 어쿠스틱 데이터 집합을 선택합니다. 정확도 테스트를 수행할 경우 모델 성능을 현실적으로 파악할 수 있게 모델을 만드는 데 사용한 것과는 다른 어쿠스틱 데이터를 선택하는 것이 중요합니다. 오프라인 테스트는 발성 1000건으로 제한됩니다. 테스트를 위한 어쿠스틱 데이터 세트가 이보다 크면 처음 1000건만 평가됩니다.

사용자 지정 프로세스 실행을 시작할 준비가 되면 "만들기"를 누릅니다.

이제 이 새 모델에 해당하는 새 항목이 어쿠스틱 모델 테이블에 표시됩니다. 프로세스의 상태가 테이블에 반영됩니다. 상태는 "대기 중", "처리 중" 및 "완료"입니다.

![다음을 시도해 보세요.](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-creating.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 오디오 파일 및 스크립트에 사용할 사용자 지정 어쿠스틱 모델을 개발했습니다. 텍스트 파일에 사용할 사용자 지정 언어 파일을 만들려면 사용자 지정 언어 모델 만들기 자습서를 계속 진행하세요.

> [!div class="nextstepaction"]
> [사용자 지정 언어 모델 만들기](cognitive-services-custom-speech-create-language-model.md)
