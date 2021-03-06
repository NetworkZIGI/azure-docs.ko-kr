---
title: Azure에서 Windows VM의 OS 드라이브 확장 | Microsoft Docs
description: Resource Manager 배포 모델에서 Azure Powershell을 사용하여 가상 머신의 OS 드라이브 크기를 확장합니다.
services: virtual-machines-windows
documentationcenter: ''
author: kirpasingh
manager: roshar
editor: ''
tags: azure-resource-manager
ms.assetid: d9edfd9f-482f-4c0b-956c-0d2c2c30026c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: kirpas
ms.subservice: disks
ms.openlocfilehash: 3d5d0d4b17bcdc5e0461b977c4c832762a46b99b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456190"
---
# <a name="how-to-expand-the-os-drive-of-a-virtual-machine"></a>가상 머신의 OS 드라이브 확장 방법

[Azure Marketplace](https://azure.microsoft.com/marketplace/)에서 이미지를 배포하여 리소스 그룹에 새 VM(가상 머신)을 만드는 경우 기본 OS 드라이브는 종종 127GB입니다(일부 이미지의 경우 OS 디스크 크기가 기본적으로 더 작음). VM에 데이터 디스크를 추가할 수 있고(선택한 SKU에 따라 추가할 수 있는 양이 달라짐) 애플리케이션 및 CPU 사용량이 많은 워크로드는 이러한 추가 디스크에 설치하는 것이 좋지만, 고객이 다음과 같은 특정 시나리오를 지원하기 위해 OS 드라이브를 확장해야 하는 경우가 자주 있습니다.

- OS 드라이브에 구성 요소를 설치하는 기존 애플리케이션을 지원하려 합니다.
- OS 드라이브가 더 큰 온-프레미스로 실제 PC 또는 가상 머신을 마이그레이션하려 합니다.


> [!IMPORTANT]
> Azure Virtual Machine의 OS 디스크 크기를 조정하면 다시 시작됩니다.
>
> 디스크를 확장한 후 더 큰 디스크를 활용하려면 [OS 내에서 볼륨](#expand-the-volume-within-the-os)을 확장해야 합니다.
> 

## <a name="resize-a-managed-disk"></a>관리 디스크 크기 조정

관리 모드에서 Powershell ISE 또는 Powershell 창을 열고 다음 단계를 수행합니다.

1. 리소스 관리 모드에서 Microsoft Azure 계정에 로그인하고 다음과 같이 구독을 선택합니다.
   
   ```powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. 다음과 같이 리소스 그룹 이름 및 VM 이름을 설정합니다.
   
   ```powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. 다음과 같이 VM에 대한 참조를 얻습니다.
   
   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. 다음과 같이 VM을 중지한 후 디스크의 크기를 조정합니다.
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. 관리되는 OS 디스크에 대한 참조를 가져옵니다. 다음과 같이 관리되는 OS 디스크의 크기를 원하는 값으로 설정하고 디스크를 업데이트합니다.
   
   ```Powershell
   $disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > 새 크기가 기존 디스크 크기보다 커야 합니다. OS 디스크에 허용되는 최대 크기는 2,048GB입니다. (VHD Blob은 이 크기 이상으로 확장할 수 있지만, OS가 처음 2,048GB의 공간에서만 작동할 수 있습니다.)
   > 
   > 
6. VM이 업데이트될 때까지 몇 초 정도 걸릴 수 있습니다. 명령 실행이 완료되면 다음과 같이 VM을 다시 시작합니다.
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

이것으로 끝입니다! 이제 VM에 RDP를 실행하고, 컴퓨터 관리(또는 디스크 관리)를 열고, 새로 할당된 공간을 사용하여 드라이브를 확장합니다.

## <a name="resize-an-unmanaged-disk"></a>관리되지 않는 디스크 크기 조정

관리 모드에서 Powershell ISE 또는 Powershell 창을 열고 다음 단계를 수행합니다.

1. 리소스 관리 모드에서 Microsoft Azure 계정에 로그인하고 다음과 같이 구독을 선택합니다.
   
   ```Powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. 다음과 같이 리소스 그룹 이름 및 VM 이름을 설정합니다.
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. 다음과 같이 VM에 대한 참조를 얻습니다.
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. 다음과 같이 VM을 중지한 후 디스크의 크기를 조정합니다.
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. 다음과 같이 관리되지 않는 OS 디스크의 크기를 원하는 값으로 설정하고 VM을 업데이트합니다.
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > 새 크기가 기존 디스크 크기보다 커야 합니다. OS 디스크에 허용되는 최대 크기는 2,048GB입니다. (VHD Blob은 이 크기 이상으로 확장할 수 있지만, OS가 처음 2,048GB의 공간에서만 작동할 수 있습니다.)
   > 
   > 
   
6. VM이 업데이트될 때까지 몇 초 정도 걸릴 수 있습니다. 명령 실행이 완료되면 다음과 같이 VM을 다시 시작합니다.
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```


## <a name="scripts-for-os-disk"></a>OS 디스크용 스크립트

관리 디스크 및 관리되지 않는 디스크에 대한 참조용 전체 스크립트는 다음과 같습니다.


**관리되는 디스크**

```Powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

**관리되지 않는 디스크**

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="resizing-data-disks"></a>데이터 디스크 크기 조정

이 문서는 주로 VM의 OS 디스크를 확장하는 데 중점을 두지만 스크립트는 VM에 연결된 데이터 디스크를 확장하는 데 사용할 수도 있습니다. 예를 들어 VM에 연결된 첫 번째 데이터 디스크를 확장하려면 아래와 같이 `StorageProfile`의 `OSDisk` 개체를 `DataDisks` 배열로 바꾸고 숫자 인덱스를 사용하여 첫 번째 연결된 데이터 디스크의 참조를 가져옵니다.

**관리 디스크**

```powershell
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```


**관리되지 않는 디스크**

```powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```



마찬가지로 위와 같이 인덱스를 사용하거나 디스크의 **Name** 속성을 사용하여 VM에 연결된 다른 데이터 디스크를 참조할 수 있습니다.


**관리 디스크**

```powershell
(Get-AzureRmDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

**관리되지 않는 디스크**

```powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```

## <a name="expand-the-volume-within-the-os"></a>OS 내에서 볼륨 확장

VM의 디스크를 확장한 후에는 OS로 이동하고 볼륨을 확장하여 새 공간을 포함해야 합니다. 파티션을 확장하는 여러 방법이 있습니다. 이 섹션에서는 RDP 연결을 사용하여 VM을 연결하고 **DiskPart**를 사용하여 파티션을 확장하는 방법을 설명합니다.

1. VM에 대한 RDP 연결을 엽니다.

2.  명령 프롬프트를 열고 **diskpart**를 입력합니다.

2.  **DISKPART** 프롬프트에 `list volume`을 입력합니다. 확장할 볼륨을 기록해 둡니다.

3.  **DISKPART** 프롬프트에 `select volume <volumenumber>`를 입력합니다. 동일한 디스크의 인접한 빈 공간으로 확장하려는 볼륨 *volumenumber*가 선택됩니다.

4.  **DISKPART** 프롬프트에 `extend [size=<size>]`를 입력합니다. 선택된 볼륨이 *size*(MB)만큼 확장됩니다.


## <a name="next-steps"></a>다음 단계

[Azure Portal](attach-managed-disk-portal.md)을 사용하여 디스크를 연결할 수도 있습니다.
