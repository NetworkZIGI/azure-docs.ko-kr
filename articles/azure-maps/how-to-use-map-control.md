---
title: Azure Maps 맵 컨트롤을 사용하는 방법 | Microsoft Docs
description: Azure Maps 맵 컨트롤 클라이언트 쪽 Javascript 라이브러리를 사용하는 방법을 알아봅니다.
author: dsk-2015
ms.author: dkshir
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 850f9b28c112c11fd98a8abc81a1811cd26d81cc
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166034"
---
# <a name="use-the-azure-maps-map-control"></a>Azure Maps 맵 컨트롤 사용

맵 컨트롤 클라이언트 쪽 Javascript 라이브러리를 사용하면 맵 및 포함된 Azure Maps 기능을 웹 또는 모바일 애플리케이션에 렌더링할 수 있습니다.

## <a name="create-a-new-map-in-a-web-page"></a>웹 페이지에 새 맵 만들기

맵 컨트롤 클라이언트 쪽 Javascript 라이브러리를 사용하여 웹 페이지에 지도를 포함할 수 있습니다.

1. 새 파일을 만들고, 이름을 **MapSearch.html**로 지정합니다.

2. 파일의 `<head>` 요소에 Azure Maps 스타일시트 및 스크립트 소스 참조를 추가합니다.

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```

3. 브라우저에서 새 맵을 렌더링하려면 `<style>` 요소에 **#map** 참조를 추가합니다.

    ```html
    <style>
        #map {
            width: 100%;
            height: 100%;
        }
    </style>
    ```

4. 맵 컨트롤을 초기화하려면 HTML 본문에 새 섹션을 정의하고 스크립트를 만듭니다. 스크립트에서 자체 Azure Maps 계정 키를 사용합니다. 계정을 만들거나 키를 찾아야 하는 경우 [Azure Maps 계정과 키를 관리하는 방법](how-to-manage-account-keys.md)을 참조하세요. **setLanguage** 메서드는 맵 레이블 및 컨트롤에 사용할 언어를 지정합니다. 지원되는 언어에 대한 자세한 내용은 [지원되는 언어](https://docs.microsoft.com/azure/azure-maps/supported-languages)를 참조하세요.

    ```html
    <div id="map">
        <script>
            atlas.setSubscriptionKey("<_your account key_>");
            atlas.setLanguage("en");
            var map = new atlas.Map("map", {
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```

5. 웹 브라우저에서 파일을 열고 렌더링된 맵을 봅니다.

## <a name="next-steps"></a>다음 단계

전체 예제를 사용하여 맵을 만드는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [맵 만들기](map-create.md)

맵 스타일을 지정하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [지도 스타일 선택](choose-map-style.md)
