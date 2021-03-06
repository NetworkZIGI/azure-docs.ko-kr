---
title: Azure SQL Data Warehouse 릴리스 정보 | Microsoft Docs
description: Azure SQL Data Warehouse에 대한 릴리스 정보입니다.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 7a131429d8f70255e47efaac0c4b5fa99a6df7d6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55460814"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Azure SQL Data Warehouse 릴리스 정보

Azure SQL DW(SQL Data Warehouse)는 MPP(대규모 병렬 처리)를 활용하여 페타바이트 단위의 데이터에서 복잡한 쿼리를 빠르게 실행하는 클라우드 기반 Enterprise Data Warehouse입니다. SQL Data Warehouse를 빅 데이터 솔루션의 주요 구성 요소로 사용합니다. 간단한 PolyBase T-SQL 쿼리를 사용하여 빅 데이터를 SQL Data Warehouse로 가져온 다음 MPP 기능을 사용하여 고성능 분석을 실행합니다. 통합하고 분석할 때 데이터 웨어하우스는 비즈니스가 정보에 대해 신뢰할 수 있는 단일 버전의 정보입니다.

최신 버전의 Azure SQL Data Warehouse에서 기대할 수 있는 새로운 기능과 향상된 기능에 대해 더 자세히 알아보려면 아래 링크를 클릭하세요. 이러한 서비스 업데이트는 식별된 유지 관리 일정 중에 받으실 수 있습니다.

- [SQL DW 버전 10.0.10106.0](./release-notes-10-0-10106-0.md)
- [2018년 12월](./release-notes-december-2018.md)
- [2018년 10월](./release-notes-october-2018.md)
- [2018년 9월](./release-notes-september-2018.md)
- [2018년 8월](./release-notes-august-2018.md)
- [2018년 7월](./release-notes-july-2018.md)
- [2018년 6월](./release-notes-june-2018.md)
- [2018년 5월](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>데이터 웨어하우스에 적용된 코드 버전 확인

데이터 웨어하우스에 적용된 릴리스를 확인할 수 있습니다. SSMS를 통해 데이터 웨어하우스에 연결하고 다음 구문을 실행하면 SQL Data Warehouse의 현재 버전을 반환합니다.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

예제 출력: ![SQL Data Warehouse 버전](./media/release-notes/dw-version.png)

식별된 버전을 사용하여 Azure SQL Data Warehouse에 적용된 릴리스를 확인하세요. 


## <a name="next-steps"></a>다음 단계
- 유지 관리 일정을 확인하는 방법을 [자세히 알아봅니다](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule). 
- 유지 관리 일정을 변경하는 방법을 [자세히 알아봅니다](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule).
- Azure Monitor를 사용하여 경고를 작성, 확인 및 관리하는 방법에 대해 [자세히 알아봅니다](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric).
- 로그 경고 규칙용 웹후크 작업에 대해 [자세히 알아봅니다](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook).
- Azure Service Health에 대해 [자세히 알아봅니다](https://docs.microsoft.com/azure/service-health/service-health-overview).
