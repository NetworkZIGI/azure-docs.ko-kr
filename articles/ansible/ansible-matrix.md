---
title: Azure용 Ansible 모듈 및 버전 매트릭스
description: Azure용 Ansible 모듈 및 버전 매트릭스
ms.service: ansible
keywords: Ansible, 역할, 매트릭스, 버전, Azure, DevOps
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 09/22/2018
ms.topic: article
ms.openlocfilehash: 5265b6f6ebf779c83792ab2569c1b613d11070da
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051529"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible 모듈 및 버전 매트릭스

## <a name="ansible-modules-for-azure"></a>Azure용 Ansible 모듈
Ansible은 원격 호스트에서 직접 또는 플레이북을 통해 실행할 수 있는 여러 모듈과 함께 제공됩니다.
이 문서에서는 가상 머신, 네트워킹 및 컨테이너 서비스와 같은 Azure 클라우드 리소스를 프로비전할 수 있는 Azure용 Ansible 모듈을 나열합니다. Ansible의 공식 릴리스 또는 Microsoft에서 게시한 다음 플레이북에서 이러한 모듈을 가져올 수 있습니다.

| Azure용 Ansible 모듈                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | [Ansible 역할](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                          |                            |                                | 
| azure_rm_availabilityset                    | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_availabilityset_facts              | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_deployment                         | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_resource                           | -            | -                           | 예          | 예          | 예                                 | 
| azure_rm_resource_facts                     | -            | -                           | 예          | 예          | 예                                 | 
| azure_rm_virtualmachine_scaleset_facts      | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_virtualmachineimage_facts          | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_resourcegroup                      | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_resourcegroup_facts                | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_virtualmachine                     | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_virtualmachine_facts               | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_virtualmachine_extension           | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_virtualmachine_scaleset            | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_image                              |              | 예                         | 예          | 예          | 예                                 | 
| **네트워킹**                    |           |                          |                          |                             |                               | 
| azure_rm_virtualnetwork                     | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_virtualnetwork_facts               | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_subnet                             | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_networkinterface                   | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_networkinterface_facts             | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_publicipaddress                    | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_publicipaddress_facts              | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_dnsrecordset                       | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_dnsrecordset_facts                 | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_dnszone                            | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_dnszone_facts                      | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_loadbalancer                       | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_loadbalancer_facts                 | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_appgateway                         | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | 예                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | 예                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | 예                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | 예                                 |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | 예                                 | 
| azure_rm_securitygroup                      | 예          | 예                         | 예          | 예          | 예                                 |
| azure_rm_route                              | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_routetable                         | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_routetable_facts                   | -            | -                           | -            | 예          | 예                                 | 
| **Storage**                    |           |                          |                          |                             |                               | 
| azure_rm_storageaccount                     | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_storageaccount_facts               | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_storageblob                        | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_managed_disk                       | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_managed_disk_facts                 | 예          | 예                         | 예          | 예          | 예                                 | 
| **컨테이너**                    |           |                          |                          |                            |                                | 
| azure_rm_aks                                | -            | -                           | 예          | 예          | 예                                 | 
| azure_rm_aks_facts                          | -            | -                           | 예          | 예          | 예                                 | 
| azure_rm_acs                                | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_containerinstance                  | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -              | -            | 예                                 | 
| azure_rm_containerregistry                  | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | 예                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | 예                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | 예                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | 예                                 | 
| **Azure Functions**                    |           |                          |                          |                            |                                | 
| azure_rm_functionapp                        | 예          | 예                         | 예          | 예          | 예                                 | 
| azure_rm_functionapp_facts                  | 예          | 예                         | 예          | 예          | 예                                 | 
| **데이터베이스**                    |           |                          |                          |                             |                               | 
| azure_rm_sqlserver                          | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_sqlserver_facts                    | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_sqldatabase                        | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | 예                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | 예                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | 예                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | 예                                 | 
| azure_rm_mysqlserver                        | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_mysqldatabase                      | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | 예                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | 예                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | 예                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | 예                                 | 
| azure_rm_postgresqlserver                   | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_postgresqldatabase                 | -            | 예                         | 예          | 예          | 예                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | 예          | 예                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | 예                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | 예                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | 예                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | 예                                 | 
| **Key Vault**                    |           |                          |                          |                             |                               | 
| azure_rm_keyvault                           | -            | 예                         | 예          | 예          | 예                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -              | 예                               |
| azure_rm_keyvaultkey                        | -            | 예                         | 예          | 예          | 예                                 |
| azure_rm_keyvaultsecret                     | -            | 예                         | 예          | 예          | 예                                 |
| **Web Apps**                    |           |                          |                          |                             |                               | 
| azure_rm_appserviceplan                          | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_appserviceplan_facts                    | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_webapp                                  | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_webapp_facts                            | -            | -                         | -          | 예          | 예                                 | 
| **Traffic Manager**                    |           |                          |                          |                             |                               | 
| azure_rm_trafficmanagerendpoint                  | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_trafficmanagerendpoint_facts            | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_trafficmanagerprofile                   | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_trafficmanagerprofile_facts             | -            | -                         | -          | 예          | 예                                 | 
| **AutoScale**                    |           |                          |                          |                             |                               | 
| azure_rm_autoscale                  | -            | -                         | -          | 예          | 예                                 | 
| azure_rm_autoscale_facts            | -            | -                         | -          | 예          | 예                                 | 

## <a name="introduction-to-playbook-role-for-azure"></a>Azure에 대한 플레이북 역할 소개
[azure_preview_module 플레이북 역할](https://galaxy.ansible.com/Azure/azure_preview_modules/)은 가장 완벽한 역할로써 최신 Azure 모듈을 모두 포함합니다. 공식 Ansible 릴리스보다 업데이트 및 버그 수정이 더 적절하게 수행됩니다. Azure 리소스를 프로비전할 목적으로 Ansible를 사용하는 경우 azure_preview_module 플레이북 역할을 설치하는 것이 좋습니다.

azure_preview_module 플레이북 역할은 3주마다 릴리스됩니다.

## <a name="next-steps"></a>다음 단계
플레이북 역할과 관련된 자세한 내용은 [다시 사용할 수 있는 플레이북 만들기](https://docs.ansible.com/ansible/latest/playbooks_reuse.html)를 참조하세요. 
