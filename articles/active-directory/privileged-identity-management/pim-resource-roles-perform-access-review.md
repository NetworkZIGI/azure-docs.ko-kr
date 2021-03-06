---
title: PIM에서 Azure 리소스 역할에 대한 액세스 검토 수행 | Microsoft Docs
description: Azure AD PIM(Privileged Identity Management)에서 Azure 리소스 역할에 대한 액세스 검토를 수행하는 방법을 알아봅니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3b9c998351432a591ece0a61d60256aea89a6f8f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180894"
---
# <a name="perform-an-access-review-of-my-azure-resource-roles-in-pim"></a>PIM에서 Azure 리소스 역할에 대한 액세스 검토 수행
Azure 리소스에 대한 PIM(Privileged Identity Management)에서는 기업이 Azure에서 리소스에 대해 권한 있는 액세스를 관리하는 방법을 간소화합니다. 

관리 역할에 할당된 경우 조직의 권한 있는 역할 관리자가 작업에 해당 역할이 여전히 필요한지 정기적으로 확인하도록 요청할 수 있습니다. 링크가 포함된 메일을 받거나 [Azure 포털](https://portal.azure.com)로 바로 이동할 수 있습니다. 할당된 역할을 자체 검토하려면 이 문서의 단계를 따릅니다.

액세스 검토에 관심이 있는 권한 있는 역할 관리자인 경우 [액세스 검토를 시작하는 방법](pim-resource-roles-start-access-review.md)에서 자세한 내용을 참조하세요.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management 애플리케이션 추가
[Azure Portal](https://portal.azure.com/)에서 Azure AD(Active Directory) PIM 애플리케이션을 사용하여 검토를 수행할 수 있습니다. 포털에 애플리케이션이 없는 경우 다음 단계에 따라 시작하세요.

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. Azure Portal의 오른쪽 위에서 사용자 이름을 선택하고 작동할 디렉터리를 선택합니다.
3. **모든 서비스**를 선택하고 **필터** 상자를 사용하여 *Azure AD Privileged Identity Management*를 검색합니다.
4. **대시보드에 고정**을 선택하고 **만들기**를 선택합니다. PIM 애플리케이션이 열립니다.

## <a name="approve-or-deny-access"></a>액세스 승인 또는 거부
액세스를 승인하거나 거부할 때, 단지 검토자에게 이 역할을 사용할지를 알리는 것입니다. 역할을 유지하려면 **승인**을, 또는 더 이상 액세스를 필요로 하지 않는 경우 **거부**를 선택합니다. 검토자가 결과를 적용하는 경우에만 상태가 변경됩니다.

액세스 검토를 찾아 완료하려면 다음 단계를 수행합니다.
1. Azure AD PIM 애플리케이션을 찾습니다.
2. **액세스 검토** 블레이드를 선택합니다.

   ![액세스 검토 블레이드를 선택한 PIM 애플리케이션의 스크린샷](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. 완료할 검토를 선택합니다. 
4. **승인** 또는 **거부**를 선택합니다. **이유 설명** 상자에 결정 이유를 입력해야 할 수도 있습니다.

   ![세부 정보 검토 페이지의 스크린샷](media/azure-pim-resource-rbac/rbac-access-review-choice.png)

## <a name="next-steps"></a>다음 단계

- [PIM에서 Azure AD 디렉터리 역할에 대한 액세스 검토 수행](pim-how-to-perform-security-review.md)
