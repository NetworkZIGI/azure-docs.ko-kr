---
title: Fluxx Labs와 Azure Active Directory 통합 | Microsoft Docs
description: Azure Active Directory 및 Fluxx Labs 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 367310527619d4bdb5f84a80c567a9d83698846e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433775"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>자습서:Fluxx Labs와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Fluxx Labs를 통합하는 방법에 대해 알아봅니다.

Fluxx Labs를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- Fluxx Labs에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Fluxx Labs에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Fluxx Labs와의 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Fluxx Labs Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Fluxx Labs 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-fluxx-labs-from-the-gallery"></a>갤러리에서 Fluxx Labs 추가
Fluxx Labs의 Azure AD 통합을 구성하려면 갤러리의 Fluxx Labs를 관리하는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Fluxx Labs를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Azure Active Directory 단추][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![엔터프라이즈 애플리케이션 블레이드][2]

1. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추][3]

1. 검색 상자에 **Fluxx Labs**를 입력하고 결과 패널에서 **Fluxx Labs**를 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![결과 목록의 Fluxx Labs](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 Fluxx Labs에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Fluxx Labs 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Fluxx Labs의 관련 사용자 간에 연결 관계가 형성되어야 합니다.

Fluxx Labs에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

Fluxx Labs에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
1. **[Fluxx Labs 테스트 사용자 만들기](#create-a-fluxx-labs-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Fluxx Labs에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Fluxx Labs 애플리케이션에서 Single Sign-On을 구성합니다.

**Fluxx Labs에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Fluxx Labs** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-On 구성 링크][4]

1. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 대화 상자](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

1. **Fluxx Labs 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Fluxx Labs 도메인 및 URL Single Sign-On 정보](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. **식별자** 텍스트 상자에서 다음 패턴을 사용하여 URL을 입력합니다.

    | Environment | URL 패턴|
    |-------------|------------|
    | 프로덕션 | `https://<subdomain>.fluxx.io` |
    | 사전 프로덕션 | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    나. **회신 URL** 텍스트 상자에 다음 패턴으로 URL을 입력합니다.

    | Environment | URL 패턴|
    |-------------|------------|
    | 프로덕션 | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | 사전 프로덕션 | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 실제 식별자 및 회신 URL로 해당 값을 업데이트합니다. 이러한 값을 얻으려면 [Fluxx Labs 클라이언트 지원팀](mailto:travis@fluxxlabs.com)에 문의하세요.

1. **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

1. **저장** 단추를 클릭합니다.

    ![Single Sign-On 구성 저장 단추](./media/fluxxlabs-tutorial/tutorial_general_400.png)

1. **Fluxx Labs 구성** 섹션에서 **Fluxx Labs 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

1. 다른 웹 브라우저 창에서 Fluxx Labs 회사 사이트에 관리자로 로그인합니다.

1. **설정** 섹션 아래에서 **관리**를 선택합니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/config1.png)

1. 관리 패널에서 **플러그 인** > **통합**을 차례로 선택한 다음, **SAML SSO - (사용 안 함)** 을 선택합니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/config2.png)

1. 특성 섹션에서 다음 단계를 수행합니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/config3.png)

    a. **SAML SSO** 확인란을 선택합니다.

    나. **요청 경로** 텍스트 상자에 **/auth/saml**을 입력합니다.

    다. **콜백 경로** 텍스트 상자에 **/auth/saml/callback**을 입력합니다.

    d. Azure Portal에서 입력한 **회신 URL** 값을 **Assertion Consumer Service URL(Single Sign-On URL)** 텍스트 상자에 입력합니다.

    e. Azure Portal에서 입력한 **식별자** 값을 **Audience(SP Entity ID)**(대상 그룹(SP 엔터티 ID)) 텍스트 상자에 입력합니다.

    f. Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL** 값을 **Identity Provider SSO Target URL**(ID 공급자 SSO 대상 URL) 텍스트 상자에 붙여넣습니다.

    g. Base 64로 인코딩된 인증서를 메모장에서 열고, 내용을 클립보드에 복사한 다음 **ID 공급자 인증서** 텍스트 상자에 붙여넣습니다.

    h. **이름 식별자 형식** 텍스트 상자에 `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress` 값을 입력합니다.

    i. **저장**을 클릭합니다.

    > [!NOTE]
    > 콘텐츠가 저장되면 보안을 위해 해당 필드가 빈 상태로 표시되지만 값은 구성에 저장되었습니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 클릭합니다.

    ![Azure Active Directory 단추](./media/fluxxlabs-tutorial/create_aaduser_01.png)

1. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/fluxxlabs-tutorial/create_aaduser_02.png)

1. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![추가 단추](./media/fluxxlabs-tutorial/create_aaduser_03.png)

1. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    다. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
  
### <a name="create-a-fluxx-labs-test-user"></a>Fluxx Labs 테스트 사용자 만들기

Azure AD 사용자가 Fluxx Labs에 로그인할 수 있도록 하려면 Fluxx Labs로 프로비전되어야 합니다. Fluxx Labs의 경우 프로비전은 수동 작업입니다.

**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. Fluxx Labs 회사 사이트에 관리자 권한으로 로그인합니다.

1. 아래 표시된 **아이콘**을 클릭합니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/config6.png)

1. 대시보드에서 아래 표시된 아이콘을 클릭하여 **새 사람** 카드를 엽니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/config4.png)

1. **새 사람** 섹션에서 다음 단계를 수행합니다.

    ![Fluxx Labs 구성](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs에서는 SSO 로그인에 대해 고유 식별자로 이메일을 사용합니다. **SSO UID** 필드를 사용자의 이메일 주소로 채웁니다. 해당 주소는SSO를 사용하여 로그인할 때 사용한 이메일 주소와 일치해야 합니다.

    나. **저장**을 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Fluxx Labs에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200]

**Britta Simon을 Fluxx Labs에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 애플리케이션 목록에서 **Fluxx Labs**를 선택합니다.

    ![애플리케이션 목록의 Fluxx Labs 링크](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    !["사용자 및 그룹" 링크][202]

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Fluxx Labs 타일을 클릭하면 Fluxx Labs 애플리케이션에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/fluxxlabs-tutorial/tutorial_general_203.png
