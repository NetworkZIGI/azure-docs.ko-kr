---
title: '자습서: Azure Active Directory로 자동 사용자 프로비전을 위한 Samanage 구성 | Microsoft Docs'
description: 사용자 계정을 Samanage로 자동으로 프로비전 및 프로비전 해제하도록 Azure Active Directory를 구성하는 방법을 알아봅니다.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2018
ms.author: v-wingf-msft
ms.openlocfilehash: d3442710e1e1327dcafc1b4ed6617aeb7ff1bf0f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322433"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로비전을 위한 Samanage 구성

이 자습서에서는 사용자 및/또는 그룹을 Samanage로 자동으로 프로비전 및 프로비전 해제하도록 Azure AD(Azure Active Directory)를 구성하기 위해 Samanage 및 Azure AD에서 수행하는 단계를 보여 줍니다.

> [!NOTE]
> 이 자습서에서는 Azure AD 사용자 프로비저닝 서비스에 기반하여 구축된 커넥터에 대해 설명합니다. 이 서비스의 기능, 작동 방법 및 질문과 대답에 대한 중요한 내용은 [Azure Active Directory를 사용하여 SaaS 응용 프로그램의 사용자를 자동으로 프로비저닝 및 프로비저닝 해제](../manage-apps/user-provisioning.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 자습서에서 설명한 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

*   Azure AD 테넌트
*   Professional 패키지 포함 [Samanage 테넌트](https://www.samanage.com/pricing/)
*   관리자 권한이 있는 Samanage의 사용자 계정

> [!NOTE]
> Azure AD 프로비전 통합에서는 [Samanage REST API](https://www.samanage.com/api/)를 사용하며, Professional 패키지가 포함된 계정의 Samanage 개발자에게 제공됩니다. 

## <a name="adding-samanage-from-the-gallery"></a>갤러리에서 Samanage 추가
Azure AD를 사용하여 사용자를 자동으로 프로비전하도록 Samanage를 구성하기 전에, Samanage를 Azure AD 응용 프로그램 갤러리에서 관리되는 SaaS 응용 프로그램 목록으로 추가해야 합니다.

**Azure AD 응용 프로그램 갤러리에서 Samanage를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 패널에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 응용 프로그램** > **모든 응용 프로그램**으로 이동합니다.

    ![엔터프라이즈 응용 프로그램 섹션][2]

3. Samanage를 추가하려면 대화 상자의 위쪽에서 **새 응용 프로그램** 단추를 클릭합니다.

    ![새 응용 프로그램 단추][3]

4. 검색 상자에 **Samanage**를 입력합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/AppSearch.png)

5. 결과 패널에서 **Samanage**를 선택한 다음, **추가** 단추를 클릭하여 SaaS 응용 프로그램의 목록에 Samanage를 추가합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/AppSearchResults.png)

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-samanage"></a>Samanage에 사용자 할당

Azure Active Directory는 "할당"이라는 개념을 사용하여 어떤 사용자가 선택한 앱에 대한 액세스를 받아야 하는지를 판단합니다. 자동 사용자 프로비저닝의 컨텍스트에서 Azure AD의 응용 프로그램에 "할당된" 사용자 및/또는 그룹만 동기화됩니다.

자동 사용자 프로비전을 구성하고 사용하도록 설정하기 전에 Samanage에 액세스해야 하는 Azure AD의 사용자 및/또는 그룹을 결정해야 합니다. 일단 결정되면 다음 지침에 따라 이러한 사용자 및/또는 그룹을 Samanage에 할당할 수 있습니다.

*   [엔터프라이즈 앱에 사용자 또는 그룹 할당](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Samanage에 사용자를 할당하기 위한 주요 팁

*    Samanage 역할은 현재 Azure Portal UI에 자동으로 동적으로 채워집니다. 사용자에게 Samanage 역할을 할당하기 전에 Samanage 테넌트에서 최신 역할을 검색하기 위해 Samanage에 대해 초기 동기화가 완료되었는지 확인합니다.

*    단일 Azure AD 사용자를 Samanage에 할당하여 초기 자동 사용자 프로비전 구성을 테스트하는 것이 좋습니다. 테스트가 성공적으로 완료되면 추가 사용자 및/또는 그룹이 나중에 할당될 수 있습니다.

*   사용자를 Samanage에 할당할 때 할당 대화 상자에서 유효한 응용 프로그램별 역할(사용 가능한 경우)을 선택해야 합니다. **기본 액세스** 역할이 있는 사용자는 프로비전에서 제외됩니다.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Samanage에 자동 사용자 프로비전 구성

이 섹션에서는 Azure AD의 사용자 및/또는 그룹 할당에 따라 Samanage에서 사용자 및/또는 그룹을 만들고, 업데이트하고, 사용 해제하도록 Azure AD 프로비전 서비스를 구성하는 단계를 안내합니다.

> [!TIP]
> [Samanage Single Sign-On 자습서](samanage-tutorial.md)에서 제공한 지침에 따라 Samanage에 SAML 기반 Single Sign-On을 사용하도록 선택할 수도 있습니다. Single Sign-On과 자동 사용자 프로비저닝은 서로 보완적이지만, 별개로 구성할 수 있습니다.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Azure AD에서 Samanage에 대한 자동 사용자 프로비전을 구성하려면 다음을 수행합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인하고, **Azure Active Directory > 엔터프라이즈 응용 프로그램 > 모든 응용 프로그램**으로 차례로 이동합니다.

2. SaaS 응용 프로그램 목록에서 Samanage를 선택합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/AppInstanceSearch.png)

3. **프로비전** 탭을 선택합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. **프로비전 모드**를 **자동**으로 설정합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. **관리자 자격 증명** 섹션에 Samanage 계정의 **관리자 사용자 이름** 및 **관리자 암호**를 입력합니다. 이러한 값의 예는 다음과 같습니다.

    *   **관리자 사용자 이름** 필드에서 Samanage 테넌트의 관리자 계정에 대한 사용자 이름을 채웁니다. 예: admin@contoso.com.

    *   **관리자 암호** 필드에서 관리자 사용자 이름에 해당하는 관리자 계정의 암호를 채웁니다.

6. 5단계에 표시된 필드를 채우면 **연결 테스트**를 클릭하여 Azure AD에서 Samanage에 연결할 수 있는지 확인합니다. 연결에 실패하면 Samanage 계정에 관리자 권한이 있는지 확인하고 다시 시도합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/TestConnection.png)

7. **알림 이메일** 필드에 프로비전 오류 알림을 받을 개인 또는 그룹의 이메일 주소를 입력하고, **오류가 발생할 경우 이메일 알림 보내기** 확인란을 선택합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. **저장**을 클릭합니다.

9. **매핑** 섹션에서 **Azure Active Directory 사용자를 Samanage에 동기화**를 선택합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/UserMappings.png)

10. **특성 매핑** 섹션에서 Azure AD에서 Samanage로 동기화되는 사용자 특성을 검토합니다. **일치** 속성으로 선택한 특성은 업데이트 작업 시 Samanage의 사용자 계정을 일치시키는 데 사용됩니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. 그룹 매핑을 사용하려면 **매핑** 섹션에서 **Azure Active Directory 그룹을 Samanage에 동기화**를 선택합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. **사용**을 **예**로 설정하여 그룹을 동기화합니다. **특성 매핑** 섹션에서 Azure AD에서 Samanage로 동기화되는 그룹 특성을 검토합니다. **일치** 속성으로 선택한 특성은 업데이트 작업 시 Samanage의 사용자 계정을 일치시키는 데 사용됩니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. 범위 지정 필터를 구성하려면 [범위 지정 필터 자습서](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)에서 제공하는 다음 지침을 참조합니다.

14. Samanage에 대한 Azure AD 프로비전 서비스를 사용하도록 설정하려면 **설정** 섹션에서 **프로비전 상태**를 **켜기**로 변경합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. **설정**의 **범위** 섹션에서 원하는 값을 선택하여 Samanage에 프로비전하려는 사용자 및/또는 그룹을 정의합니다. **모든 사용자 및 그룹 동기화** 옵션 선택 시 아래의 **커넥터 제한 사항** 섹션에 설명된 대로 제한 사항을 고려하세요.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. 프로비전할 준비가 되면 **저장**을 클릭합니다.

    ![Samanage 프로비전](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


이 작업은 **설정**의 **범위** 섹션에 정의된 모든 사용자 및/또는 그룹의 초기 동기화를 시작합니다. 초기 동기화는 Azure AD 프로비전 서비스가 실행되는 동안 약 40분마다 발생하는 후속 동기화보다 더 많은 시간이 걸립니다. **동기화 세부 정보** 섹션을 사용하여 진행 상태를 모니터링하고, Samanage의 Azure AD 프로비전 서비스에서 수행한 모든 작업을 설명하는 프로비전 활동 보고서에 대한 링크를 따를 수 있습니다.

Azure AD 프로비저닝 로그를 읽는 방법에 대한 자세한 내용은 [자동 사용자 계정 프로비저닝에 대한 보고](../manage-apps/check-status-user-account-provisioning.md)를 참조하세요.

## <a name="connector-limitations"></a>커넥터 제한 사항

* **모든 사용자 및 그룹 동기화** 옵션을 선택하고 기본값이 Samanage **역할** 특성에 대해 구성된 경우 **Null인 경우 기본값(선택 사항임)** 필드에서 원하는 값이 다음 형식 **{"displayName":"역할"}**(여기 역할은 원하는 기본값임)으로 표현되었는지 확인합니다.

## <a name="additional-resources"></a>추가 리소스

* [엔터프라이즈 앱에 대한 사용자 계정 프로비전 관리](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>다음 단계

* [프로비전 활동에 대한 로그를 검토하고 보고서를 받아 보는 방법을 살펴봅니다](../manage-apps/check-status-user-account-provisioning.md).

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
