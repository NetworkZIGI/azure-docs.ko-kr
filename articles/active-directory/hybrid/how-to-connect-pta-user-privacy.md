---
title: 사용자 개인 정보 및 Azure Active Directory 통과 인증 | Microsoft Docs
description: 이 문서에서는 Azure AD(Azure Active Directory) 통과 인증 및 GDPR 준수에 대해 다룹니다.
services: active-directory
keywords: Azure AD Connect 통과 인증, GDPR, Azure AD에 대한 필수 구성 요소, SSO, Single Sign-On
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 2247eb8e82e959b3c95778c154842b5f98708001
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496281"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>사용자 개인 정보 및 Azure Active Directory 통과 인증


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>개요

Azure AD 통과 인증은 개인 데이터를 포함할 수 있는 다음 로그 형식을 만듭니다.

- Azure AD Connect 추적 로그 파일.
- 인증 에이전트 추적 로그 파일.
- Windows 이벤트 로그.

다음과 같은 두 가지 방법으로 통과 인증에 대한 사용자 개인 정보 보호 수준을 높일 수 있습니다.

1.  요청 시 사람에 대한 데이터를 추출하고 그 사람의 데이터를 설치에서 제거합니다.
2.  데이터는 48시간 이상 데이터가 보존하지 않도록 합니다.

구현 및 유지 관리가 훨씬 편한 두 번째 옵션을 강력하게 권장합니다. 다음은 각 로그 형식에 대한 지침입니다.

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Connect 추적 로그 파일을 삭제합니다.

Azure AD Connect를 설치 또는 업그레이드하거나 통과 인증 구성을 수정한 지 48시간 이내에 **%ProgramData%\AADConnect** 폴더의 콘텐츠를 확인하고 이 폴더의 추적 로그 콘텐츠(**trace-\*.log** 파일)를 삭제합니다. 이 작업으로 인해 GDPR에서 다루는 데이터가 생성될 수 있기 때문입니다.

>[!IMPORTANT]
>이 폴더의 **PersistedState.xml** 파일은 삭제하지 마세요. 이 파일은 이전에 설치한 Azure AD Connect의 상태를 유지하고 업그레이드 설치를 수행할 때 사용됩니다. 이 파일에는 사람에 대한 데이터가 포함되지 않으므로 삭제해서는 안됩니다.

Windows 탐색기를 사용하여 이러한 추적 로그 파일을 검토한 후 삭제할 수 있고, 다음과 같은 PowerShell 스크립트를 사용하여 필요한 작업을 수행할 수도 있습니다.

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

확장명이 ".PS1"인 파일에 스크립트를 저장합니다. 필요에 따라 이 스크립트를 실행합니다.

관련 Azure AD Connect GDPR 요구 사항에 대한 자세한 내용은 [이 문서](reference-connect-user-privacy.md)를 참조하세요.

### <a name="delete-authentication-agent-event-logs"></a>인증 에이전트 이벤트 로그 삭제

이 제품은 **Windows 이벤트 로그**를 생성할 수도 있습니다. 자세한 내용은 [이 문서](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx)를 참조하세요.

통과 인증 에이전트와 관련된 로그를 보려면 서버에서 **이벤트 뷰어** 애플리케이션을 열고 **Application and Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**에서 확인합니다.

### <a name="delete-authentication-agent-trace-log-files"></a>인증 에이전트 추적 로그 파일 삭제

<strong>%ProgramData%\Microsoft\Azure AD Connect Authentication Agent\Trace\</strong>의 콘텐츠를 주기적으로 확인하면서 이 폴더의 콘텐츠를 48시간마다 삭제해야 합니다. 

>[!IMPORTANT]
>인증 에이전트 서비스가 실행 중이면 폴더에서 현재 로그 파일을 삭제할 수 없습니다. 서비스를 중지한 후 다시 시도하세요. 사용자 로그인 오류를 방지하려면 [고가용성](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)에 대해 통과 인증이 구성되어 있어야 합니다.

Windows 탐색기를 사용하여 이러한 파일을 검토하고 삭제하거나 다음 스크립트를 사용하여 필요한 작업을 수행할 수 있습니다.

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

이 스크립트가 48시간마다 실행되도록 예약하려면 다음 단계를 수행합니다.

1.  확장명이 ".PS1"인 파일에 스크립트를 저장합니다.
2.  **제어판**을 열고 **시스템 및 보안**을 클릭합니다.
3.  **관리 도구** 제목 아래에서 “**예약 작업**”을 클릭합니다.
4.  **작업 스케줄러**에서 **예약 작업 라이브러리**를 마우스 오른쪽 단추로 클릭하고 “**기본 작업 만들기…**”를 클릭합니다.
5.  새 작업의 이름을 입력하고 **다음**을 클릭합니다.
6.  **작업 트리거**로 “**매일**”을 선택하고 **다음**을 클릭합니다.
7.  되풀이를 이틀로 설정하고 **다음**을 클릭합니다.
8.  “**프로그램 시작**”을 작업으로 선택하고 **다음**을 클릭합니다.
9.  프로그램/스크립트 상자에 “**PowerShell**”을 입력하고 “**인수 추가(선택 사항)**” 상자에 앞서 만든 스크립트의 전체 경로를 입력한 다음, **다음**을 클릭합니다.
10. 다음 화면에는 작성하려는 작업의 요약이 표시됩니다. 값을 확인하고 **마침**을 클릭하여 작업을 만듭니다.
 
### <a name="note-about-domain-controller-logs"></a>도메인 컨트롤러 로그에 대한 참고 사항

감사 로깅이 설정된 경우 해당 제품은 도메인 컨트롤러에 대한 보안 로그를 생성할 수 있습니다. 감사 정책 구성에 대한 자세한 내용은 이 [문서](https://technet.microsoft.com/library/dd277403.aspx)를 참조하세요.

## <a name="next-steps"></a>다음 단계
* [보안 센터에서 Microsoft 개인 정보 취급 방침을 검토합니다.](https://www.microsoft.com/trustcenter)
- [**문제 해결**](tshoot-connect-pass-through-authentication.md) - 기능과 관련된 일반적인 문제를 해결하는 방법에 대해 알아봅니다.
