---
title: Azure 애플리케이션 기술 자산 만들기 | Microsoft Docs
description: Azure 애플리케이션 제품에 대한 기술 자산을 만듭니다.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 6050ad98c87dbe38516a6ee3c4862495ad868031
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53414353"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Azure 애플리케이션 기술 자산 준비

이 문서에서는 Azure 애플리케이션 제품의 기술 자산을 준비하는 데 필요한 리소스에 대해 설명합니다.

## <a name="before-you-begin"></a>시작하기 전에

빠른 시작, 자습서 및 샘플을 제공하는 다음 Azure 애플리케이션 설명서를 검토합니다.

- [Azure Resource Manager 템플릿 이해](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- 빠른 시작:

  - [Azure Quickstart 템플릿](https://azure.microsoft.com/documentation/templates/)
  - [GitHub Azure 빠른 시작 템플릿](https://github.com/azure/azure-quickstart-templates)
  - [애플리케이션 정의 게시](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
  - [서비스 카탈로그 앱 배포](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- 자습서:

  - [정의 파일 만들기](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [마켓플레이스 응용 프로그램 게시](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

 - 샘플:

   - [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
   - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
   - [관리되는 애플리케이션 솔루션](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>기본 기술 지식

이러한 자산을 설계, 구축 및 테스트하려면 시간이 걸리고 Azure 플랫폼과 제안을 작성하는 데 사용되는 기술에 대한 기술적 지식이 모두 필요합니다.

엔지니어링 팀에는 다음 Microsoft 기술에 대한 지식이 있어야 합니다.

- [Azure 서비스](https://azure.microsoft.com/services/)에 대한 기본적 이해
- [Azure 응용 프로그램을 디자인 및 설계](https://azure.microsoft.com/solutions/architecture/)하는 방법
- [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) 및 [Azure 네트워킹](https://azure.microsoft.com/services/?filter=networking)에 대한 실무 지식
- [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)에 대한 실무 지식
- [JSON](https://www.json.org/)에 대한 실무 지식

## <a name="suggested-tools"></a>권장되는 도구

Azure 애플리케이션을 관리하는 데 도움이 되는 다음 스크립팅 환경 중 하나 또는 둘 다를 선택합니다.

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure CLI](https://docs.microsoft.com/cli/azure)

개발 환경에 다음 도구를 추가하는 것이 좋습니다.

- [Azure Storage 탐색기](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/)(다음 확장 포함):

  - 확장: [Azure Resource Manager 도구](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - 확장: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - 확장: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

또한 [ Azure 개발자 도구 ](https://azure.microsoft.com/tools/) 페이지에서 사용 가능한 도구를 검토하고, Visual Studio를 사용하는 경우 [ Visual Studio Marketplace ](https://marketplace.visualstudio.com/)를 검토하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

[Azure 애플리케이션 제품 만들기](./cpp-create-offer.md)

