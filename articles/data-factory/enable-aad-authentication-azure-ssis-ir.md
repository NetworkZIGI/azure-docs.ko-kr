---
title: Azure-SSIS Integration Runtime에 대한 Azure Active Directory 인증 활성화 | Microsoft Docs
description: 이 문서에서는 Azure Data Factory에 대해 관리 ID와 함께 Azure Active Directory 인증을 사용하도록 설정하여 Azure-SSIS Integration Runtime을 만드는 방법을 설명합니다.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 1/9/2019
ms.author: douglasl
ms.openlocfilehash: 5cc625e07f1c92c53491e83f4049bad12cd9d1a1
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158264"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime을 위한 Azure Active Directory 인증 활성화

이 문서에서는 ADF(Azure Data Factory)에 대한 관리 ID와 함께 Azure AD(Azure Active Directory) 인증을 사용하도록 설정하고, SQL 인증 대신 이를 사용하여 최종적으로 사용자를 대신하여 Azure SQL Database 서버/Managed Instance에 SSISDB(SSIS 카탈로그 데이터베이스)를 만드는 Azure-SSIS IR(Integration Runtime)을 만드는 방법을 보여줍니다.

ADF의 관리 ID에 대한 자세한 내용은 [Azure Data Factory 서비스 ID](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity)를 참조하세요.

> [!NOTE]
> SQL 인증을 사용하여 Azure-SSIS IR을 이미 만들었다면, 이번에는 PowerShell을 통해 Azure AD 인증을 사용하도록 IR를 다시 구성할 수 없습니다. 단, Azure Portal/ADF 앱에서는 수행할 수 있습니다. 

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL Database에서 Azure AD 활성화

Azure SQL Database 서버는 Azure AD 사용자로 데이터베이스 만들기를 지원합니다. 먼저 구성원으로 ADF에 대한 관리 ID를 사용하여 Azure AD 그룹을 만들어야 합니다. 다음으로, Azure SQL Database 서버에 대한 Active Directory 관리자로 Azure AD 사용자를 설정한 다음, SSMS(SQL Server Management Studio)에서 해당 사용자를 사용하여 연결합니다. 마지막으로, Azure-SSIS IR에서 ADF에 대한 관리 ID를 사용하여 사용자 대신 SSISDB를 만들도록 Azure AD 그룹을 나타내는 포함된 사용자를 만들어야 합니다.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>구성원으로 ADF에 대한 관리 ID를 사용하여 Azure AD 그룹을 만듭니다.

기존 Azure AD 그룹을 사용하거나 Azure AD PowerShell을 사용하여 새로 만들 수 있습니다.

1.  [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) 모듈을 설치합니다.

2.   `Connect-AzureAD`를 사용하여 로그인하고, 다음 cmdlet을 실행하여 그룹을 만들고, 변수에 저장합니다.

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    결과는 다음 예제와 같으며 변수 값을 표시합니다.

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  ADF의 관리 ID를 그룹에 추가합니다. [Azure Data Factory 서비스 ID](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) 문서에 따라 기본 서비스 ID를 가져올 수 있습니다(예: 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc. 이 용도로 서비스 ID 애플리케이션 ID를 사용하지 마세요).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    또한 나중에 그룹 구성원을 검사할 수도 있습니다.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Azure SQL Database 서버를 위한 Azure AD 인증 구성

다음 단계를 사용하여  [SQL을 통해 Azure AD 인증을 구성하고 관리](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)할 수 있습니다.

1.  Azure Portal의 왼쪽 탐색 영역에서  **모든 서비스** -> **SQL 서버** 를 선택합니다.

2.  Azure AD 인증을 사용하여 구성할 Azure SQL Database 서버를 선택합니다.

3.  블레이드의  **설정** 섹션에서  **Active Directory 관리자**를 선택합니다.

4.  명령 모음에서 **관리자 설정**을 선택합니다.

5.  서버의 관리자로 만들 Azure AD 사용자 계정을 선택한 다음,  **선택**을 선택합니다.

6.  명령 모음에서 **저장**을 선택합니다.

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Azure AD 그룹을 나타내는 Azure SQL Database 서버에 포함된 사용자 만들기

이 다음 단계를 위해 [Microsoft SSMS(SQL Server Management Studio)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)가 필요합니다.

1.  SSMS를 시작합니다.

2.   **서버에 연결** 대화 상자의  **서버 이름** 필드에 Azure SQL Database 서버 이름을 입력합니다.

3.   **인증** 필드에서  **Active Directory - MFA 지원을 통한 유니버설**을 선택합니다. (다른 두 개의 Active Directory 인증 유형을 사용할 수도 있습니다. [SQL을 통해 Azure AD 인증 구성 및 관리](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)를 참조하세요).

4.   **사용자 이름**  필드에 서버 관리자로 설정한 Azure AD 계정의 이름(예: testuser@xxxonline.com)을 입력합니다.

5.   **연결**을 선택하고 로그인 프로세스를 완료합니다.

6.   **개체 탐색기**에서 **데이터베이스** -> **시스템 데이터베이스** 폴더를 확장합니다.

7.  **마스터** 데이터베이스를 마우스 오른쪽 단추로 클릭하고  **새 쿼리**를 선택합니다.

8.  쿼리 창에서 다음 T-SQL 명령을 입력하고, 도구 모음에서  **실행** 을 선택합니다.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    명령이 성공적으로 완료되고 그룹을 나타내는 포함된 사용자가 만들어집니다.

9.  쿼리 창을 지우고 다음 T-SQL 명령을 입력한 후, 도구 모음에서 **실행**을 선택합니다.

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    명령이 성공적으로 완료되면 포함된 사용자에게 데이터베이스(SSISDB)를 만들 수 있는 기능이 부여됩니다.

10.  SSISDB가 SQL 인증을 사용하여 생성되었고 액세스하기 위해 Azure-SSIS IR에 Azure AD 인증을 사용하도록 전환하려는 경우 마우스 오른쪽 단추로 **SSISDB** 데이터베이스를 클릭하고  **새 쿼리**를 선택합니다.

11.  쿼리 창에서 다음 T-SQL 명령을 입력하고, 도구 모음에서  **실행** 을 선택합니다.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    명령이 성공적으로 완료되고 그룹을 나타내는 포함된 사용자가 만들어집니다.

12.  쿼리 창을 지우고 다음 T-SQL 명령을 입력한 후, 도구 모음에서 **실행**을 선택합니다.

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    명령이 성공적으로 완료되면 포함된 사용자에게 SSISDB에 액세스할 수 있는 기능이 부여됩니다.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure SQL Database Managed Instance에서 Azure AD 활성화

Azure SQL Database Managed Instance는 직접 ADF에 대한 관리 ID로 데이터베이스 만들기를 지원합니다. Azure AD 그룹에 ADF에 대한 관리 ID를 조인하거나 Managed Instance에서 해당 그룹을 나타내는 포함된 사용자를 만들 필요는 없습니다.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Azure SQL Database Managed Instance를 위한 Azure AD 인증 구성

1.   Azure Portal의 왼쪽 탐색 영역에서 **모든 서비스** -> **SQL 서버**를 선택합니다.

2.   Azure AD 인증을 사용하여 구성할 Managed Instance를 선택합니다.

3.   블레이드의 **설정** 섹션에서 **Active Directory 관리자**를 선택합니다.

4.   명령 모음에서 **관리자 설정**을 선택합니다.

5.   서버 관리자로 만들 Azure AD 사용자 계정을 선택한 다음, **선택**을 선택합니다.

6.   명령 모음에서 **저장**을 선택합니다.

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>ADF에 대한 관리 ID를 Azure SQL Database Managed Instance의 사용자로 추가

이 다음 단계를 위해 [Microsoft SSMS(SQL Server Management Studio)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)가 필요합니다.

1.  SSMS를 시작합니다.

2.  SQL/Active Directory 관리자 계정을 사용하여 Managed Instance에 연결합니다.

3.  **개체 탐색기**에서 **데이터베이스** -> **시스템 데이터베이스** 폴더를 확장합니다.

4.  **마스터** 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

5.  ADF에 대한 관리 ID를 가져옵니다. [Azure Data Factory 서비스 ID](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) 문서에 따라 기본 서비스 ID 애플리케이션 ID를 가져올 수 있습니다. (이 용도로 서비스 ID를 사용하지 마세요).

6.  쿼리 창에서 다음 T-SQL 스크립트를 실행하여 ADF에 대한 관리 ID를 이진 형식으로 변환합니다.

    ```sql
    DECLARE @applicationId uniqueidentifier = {your SERVICE IDENTITY APPLICATION ID}
    select CAST(@applicationId AS varbinary)
    ```
    
    명령이 성공적으로 완료되어 ADF에 대한 관리 ID가 이진으로 표시됩니다.

7.  쿼리 창을 지우고 다음 T-SQL 스크립트를 실행하여 ADF에 대한 관리 ID를 사용자로 추가합니다.

    ```sql
    CREATE LOGIN [{a name for the managed identity}] FROM EXTERNAL PROVIDER with SID ={your SERVICE IDENTITY APPLICATION ID as binary}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{the managed identity name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{the managed identity name}]
    ```
    
    명령이 성공적으로 완료되면 ADF에 대한 관리 ID에 데이터베이스(SSISDB)를 만들 수 있는 기능이 부여됩니다.

8.  SSISDB가 SQL 인증을 사용하여 생성되었고 액세스하기 위해 Azure-SSIS IR에 Azure AD 인증을 사용하도록 전환하려는 경우 마우스 오른쪽 단추로 **SSISDB** 데이터베이스를 클릭하고  **새 쿼리**를 선택합니다.

9.  쿼리 창에서 다음 T-SQL 명령을 입력하고, 도구 모음에서  **실행** 을 선택합니다.

    ```sql
    CREATE USER [{the managed identity name}] FOR LOGIN [{the managed identity name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{the managed identity name}]
    ```

    명령이 성공적으로 완료되면 ADF에 대한 관리 ID에 SSISDB에 액세스할 있는 기능이 부여됩니다.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Azure Portal/ADF 앱에서 Azure-SSIS IR 프로비전

Azure Portal/ADF 앱에서 Azure-SSIS IR을 프로비전하는 경우 **SQL 설정** 페이지에서 **ADF에 대한 관리 ID와 함께 AAD 인증 사용** 옵션을 선택합니다. 다음 스크린샷은 SSISDB를 호스트하는 Azure SQL Database 서버가 포함된 IR에 대한 설정을 표시합니다. SSISDB를 호스트하는 Managed Instance를 사용한 IR의 경우 다른 설정은 동일하나, **카탈로그 데이터베이스 서비스 계층** 및 **Azure 서비스 액세스 허용** 설정은 적용할 수 없습니다.

Azure-SSIS IR을 만드는 방법에 대한 자세한 정보는 [Azure Data Factory에서 Azure-SSIS 통합 런타임 만들기](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)를 참조하세요.

![Azure-SSIS 통합 런타임에 대한 설정](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>PowerShell을 사용하여 Azure-SSIS IR 프로비전

PowerShell을 사용하여 Azure-SSIS IR을 프로비전하려면 다음 작업을 수행합니다.

1.  [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018)  모듈을 설치합니다.

2.  스크립트에서 `CatalogAdminCredential` 매개 변수를 설정하지 마세요. 예: 

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
