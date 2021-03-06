---
title: '자습서: ThousandEyes와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 ThousandEyes 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 06de55fd2bd1064e93e38fe0ff0cfb13fdc1463a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175437"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>자습서: ThousandEyes와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 ThousandEyes를 통합하는 방법에 대해 알아봅니다.

ThousandEyes를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- ThousandEyes에 액세스할 수 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 자신의 Azure AD 계정으로 ThousandEyes에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

ThousandEyes와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- ThousandEyes Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [평가판 제품](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 ThousandEyes 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-thousandeyes-from-the-gallery"></a>갤러리에서 ThousandEyes 추가
ThousandEyes가 Azure AD에 통합되도록 구성하려면 갤러리의 ThousandEyes를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 ThousandEyes를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![애플리케이션][2]
    
1. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![애플리케이션][3]

1. 검색 상자에 **ThousandEyes**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/thousandeyes-tutorial/tutorial_thousandeyes_search.png)

1. 결과 패널에서 **ThousandEyes**를 선택하고 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 사용하여 ThousandEyes에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 ThousandEyes 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 ThousandEyes의 관련 사용자 간에 연결 관계를 설정해야 합니다.

ThousandEyes에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 연결 관계를 설정합니다.

ThousandEyes에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
1. **[ThousandEyes 테스트 사용자 만들기](#creating-a-thousandeyes-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 ThousandEyes에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 ThousandEyes 애플리케이션에서 Single Sign-On을 구성합니다.

**ThousandEyes에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **ThousandEyes** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

1. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Configure Single Sign-On](./media/thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

1. **ThousandEyes 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    **로그온 URL** 텍스트 상자에 URL을 입력합니다. `https://app.thousandeyes.com/login/sso`

1. **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![Configure Single Sign-On](./media/thousandeyes-tutorial/tutorial_thousandeyes_certificate.png)

1. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/thousandeyes-tutorial/tutorial_general_400.png)

1. **ThousandEyes 구성** 섹션에서 **ThousandEyes 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **로그아웃 URL, SAML 엔터티 ID 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Configure Single Sign-On](./media/thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

1. 다른 웹 브라우저 창에서 **ThousandEyes** 회사 사이트에 관리자로 로그인합니다.

1. 위쪽의 메뉴에서 **설정**을 클릭합니다.

    ![설정](./media/thousandeyes-tutorial/ic790066.png "설정")

1. 페이지 맨 아래에 있는 **계정**

    ![계정](./media/thousandeyes-tutorial/ic790067.png "계정")

1. **보안 및 인증** 탭을 클릭합니다.

    ![보안 및 인증](./media/thousandeyes-tutorial/ic790068.png "보안 및 인증")

1. **Single Sign-On 설정** 섹션에서 다음 단계를 수행합니다.

    ![Single Sign-On 설정](./media/thousandeyes-tutorial/ic790069.png "Single Sign-On 설정")

    a. **Single Sign-On 사용**을 선택합니다.

    b. Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL**을 **로그인 페이지 URL** 텍스트 상자에 붙여넣습니다.

    다. Azure Portal에서 복사한 **로그아웃 URL**을 **로그아웃 페이지 URL** 텍스트 상자에 붙여넣습니다.

    d. Azure Portal에서 복사한 **SAML 엔터티 ID**를 **ID 공급자 발급자** 텍스트 상자에 붙여넣습니다.

    e. **확인 인증서**에서 **파일 선택**을 클릭하고 Azure Portal에서 다운로드한 인증서를 업로드합니다.

    f. **저장**을 클릭합니다.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/thousandeyes-tutorial/create_aaduser_01.png) 

1. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/thousandeyes-tutorial/create_aaduser_02.png) 

1. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/thousandeyes-tutorial/create_aaduser_03.png)

1. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.

    ![Azure AD 테스트 사용자 만들기](./media/thousandeyes-tutorial/create_aaduser_04.png)

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="creating-a-thousandeyes-test-user"></a>ThousandEyes 테스트 사용자 만들기

이 섹션은 ThousandEyes에서 Britta Simon이라는 사용자를 만들기 위한 것입니다. ThousandEyes는 자동 사용자 프로비전을 지원하며 기본적으로 사용하도록 설정되어 있습니다. 자동 사용자 프로비전 구성 방법에 대한 자세한 내용은 [여기](thousandeyes-provisioning-tutorial.md)에서 제공합니다.

**사용자를 수동으로 만들어야 할 경우 다음 단계를 수행합니다.**

1. ThousandEyes 회사 사이트에 관리자 권한으로 로그인합니다.

1. **설정**을 클릭합니다.

    ![설정](./media/thousandeyes-tutorial/IC790066.png "설정")

1. **계정**을 클릭합니다.

    ![계정](./media/thousandeyes-tutorial/IC790067.png "계정")

1. **계정 및 사용자** 탭을 클릭합니다.

    ![계정 및 사용자](./media/thousandeyes-tutorial/IC790073.png "계정 및 사용자")

1. **사용자 및 계정 추가** 섹션에서 다음 단계를 수행합니다.

    ![사용자 계정 추가](./media/thousandeyes-tutorial/IC790074.png "사용자 계정 추가")

    a. **이름** 텍스트 상자에 사용자 이름(예: **Britta Simon**)을 입력합니다.

    b. **메일** 텍스트 사용자에 사용자 메일(예: **brittasimon@contoso.com**)을 입력합니다.

    b. **계정에 새 사용자 추가**를 클릭합니다.

    > [!NOTE]
    > Azure Active Directory 계정 보유자는 계정을 확인하고 활성화하기 위한 링크가 포함된 메일을 받습니다.

> [!NOTE]
> 다른 ThousandEyes 사용자 계정 생성 도구 또는 ThousandEyes가 제공한 API를 사용하여 Azure Active Directory 사용자 계정을 프로비전할 수 있습니다.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 ThousandEyes에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 ThousandEyes에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 애플리케이션 목록에서 **ThousandEyes**를 선택합니다.

    ![Configure Single Sign-On](./media/thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 ThousandEyes 타일을 클릭하면 ThousandEyes 애플리케이션에 자동으로 로그온됩니다.

액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)
* [사용자 프로비저닝 구성](thousandeyes-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/thousandeyes-tutorial/tutorial_general_203.png
