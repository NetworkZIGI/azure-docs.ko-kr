---
title: Azure Data Box를 사용하여 핫/콜드/보관 블록 Blob 계층으로 데이터 전송 | Microsoft Docs(데이터)
description: Azure Data Box를 사용하여 핫/콜드/보관 등의 적절한 블록 Blob 스토리지 계층으로 데이터를 전송하는 방법을 설명합니다.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: 91cabd6601ca8d0c0eabfdb5726d4bd6b37492a6
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54216085"
---
# <a name="use-azure-data-box-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Azure Data Box를 사용하여 적절한 Azure Storage Blob 계층으로 데이터 전송

Azure Data Box는 대량의 데이터를 Azure로 이동할 수 있도록 고객 소유의 저장 장치를 배송합니다. 고객은 이 저장 장치에 데이터를 저장하여 반송합니다. Data Box의 데이터는 스토리지 계정에 연결된 기본 계층으로 업로드됩니다. 그러면 해당 데이터를 다른 스토리지 계층으로 이동할 수 있습니다.

이 문서에서는 Data Box에 업로드된 데이터를 핫/콜드/보관 Blob 계층으로 이동하는 방법을 설명합니다.  

## <a name="choose-the-correct-storage-tier-for-your-data"></a>데이터에 적합한 스토리지 계층 선택

Azure Storage에서는 세 가지 계층(핫/콜드/보관)에 가장 비용 효율적인 방식으로 데이터를 저장할 수 있습니다. 핫 스토리지 계층은 자주 액세스하는 데이터 저장용으로 최적화되어 있습니다. 핫 저장소는 쿨 및 보관 저장소보다 저장소 비용이 높지만 액세스 비용은 가장 낮습니다.

콜드 스토리지 계층은 30일 이상 저장해야 하는 액세스 빈도가 낮은 데이터용입니다. 콜드 계층의 스토리지 비용은 핫 스토리지 계층보다는 저렴하지만 데이터 액세스 요금은 핫 계층에 비해 비쌉니다.

Azure 보관 계층(오프라인)의 경우 스토리지 비용은 가장 저렴하지만 액세스 비용은 가장 비쌉니다. 이 계층은 180일 이상 보관 스토리지에 저장해 두려는 데이터용입니다. 이러한 각 계층과 가격 모델을 자세히 살펴보려면 [스토리지 계층 비교](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers#comparison-of-the-storage-tiers)를 참조하세요.

Data Box의 데이터는 스토리지 계정에 연결된 스토리지 계층으로 업로드됩니다. 스토리지 계정을 만들 때 액세스 계층을 핫 또는 콜드로 지정할 수 있습니다. 워크로드 액세스 패턴과 비용에 따라 이 데이터를 기본 계층에서 다른 스토리지 계층으로 이동할 수 있습니다.

Blob Storage 또는 범용 v2(GPv2) 계정에서만 개체 스토리지 데이터를 계층별로 저장할 수 있습니다. 범용 v1(GPv1) 계정은 계층화를 지원하지 않습니다. 데이터용으로 적합한 스토리지 계층을 선택하려면 [Azure Blob Storage: 프리미엄, 핫, 쿨 및 보관 스토리지 계층](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)에 자세히 나와 있는 고려 사항을 검토하세요.

## <a name="set-a-default-blob-tier"></a>기본 Blob 계층 설정

Azure Portal에서 스토리지 계정을 만들 때 기본 Blob 계층을 지정합니다. 스토리지 유형을 GPv2 또는 Blob 스토리지로 선택하고 나면 액세스 계층 특성을 지정할 수 있습니다. 기본적으로는 핫 계층이 선택됩니다.

Data Box 주문 시 새 계정을 만들려는 경우에는 계층을 지정할 수 없습니다. 계정을 만든 후 포털에서 계정을 수정하여 기본 액세스 계층을 설정할 수 있습니다.

지정된 액세스 계층 특성을 사용하여 스토리지 계층을 먼저 만들 수도 있습니다. 그런 후에 Data Box 주문을 진행할 때 기존 스토리지 계층을 선택합니다. 스토리지 계정을 만들면서 기본 Blob 계층을 설정하는 방법에 대한 자세한 내용은 [Azure Portal에서 스토리지 계정 만들기](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal)를 참조하세요.

## <a name="move-data-to-a-non-default-tier"></a>기본 계층이 아닌 계층으로 데이터 이동

Data Box의 데이터가 기본 계층에 업로드된 후 기본 계층이 아닌 계층으로 데이터를 이동하려는 경우 다음의 두 가지 방법으로 이 데이터를 기본 계층이 아닌 계층으로 이동할 수 있습니다.

- **Azure Blob Storage 수명 주기 관리** - 정책 기반 방식을 사용해 데이터를 계층별로 자동 저장하거나 수명 주기가 종료되면 데이터를 만료 처리할 수 있습니다. 자세한 내용은 [Azure Blob Storage 수명 주기 관리](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts)를 참조하세요.
- **스크립팅** - Azure PowerShell을 통한 스크립팅 방식을 사용하여 Blob 수준 계층화를 사용하도록 설정할 수 있습니다. `SetBlobTier` 작업을 호출해 Blob에서 계층을 설정할 수 있습니다.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Azure PowerShell을 사용하여 Blob 계층 설정

아래 단계에서는 Azure PowerShell 스크립트를 사용하여 Blob 계층을 보관으로 설정하는 방법을 설명합니다.

1. 관리자 권한 Windows PowerShell 세션을 열고 다음 명령을 입력하여 PowerShell 5.0 이상을 실행 중인지 확인합니다. 형식:

   `$PSVersionTable.PSVersion`     

2. Azure PowerShell에 로그인합니다. 

   `Login-AzureRmAccount`  

3. 스토리지 계정, 액세스 키, 컨테이너 및 스토리지 컨텍스트에 해당하는 변수를 정의합니다.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. 컨테이너의 모든 Blob을 표시합니다.

    `$blobs = Get-AzureStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. 컨테이너의 모든 Blob 계층을 Archive로 설정합니다.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    샘플 출력은 다음과 같습니다.

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzureRmAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzureStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
> [!TIP]
> 수집 시 데이터를 보관하려면 기본 계정 계층을 핫으로 설정합니다. 기본 계층이 콜드인데 데이터를 보관으로 즉시 이동하는 경우에는 30일 조기 삭제 페널티가 적용됩니다.

## <a name="next-steps"></a>다음 단계

-  [수명 주기 정책 규칙을 사용하여 일반적인 데이터 계층화 시나리오](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples)를 처리하는 방법을 알아봅니다.

