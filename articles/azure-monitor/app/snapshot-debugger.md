---
title: .NET 앱용 Azure Application Insights 스냅숏 디버거 | Microsoft Docs
description: 프로덕션 .NET 앱에서 예외가 throw되면 디버그 스냅숏이 자동으로 수집됨
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/08/2018
ms.reviewer: pharring
ms.author: mbullwin
ms.openlocfilehash: 6ada9fad7640086a174f09d39d23487d2776345e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119716"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>.NET 앱의 예외에 대한 디버그 스냅숏

예외가 발생할 때 라이브 웹 애플리케이션에서 자동으로 디버그 스냅숏을 수집할 수 있습니다. 스냅숏은 예외가 throw되었을 때의 소스 코드 및 변수의 상태를 보여 줍니다. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)의 스냅숏 디버거(미리 보기)는 웹앱에서 예외 원격 분석을 모니터링합니다. 프로덕션에서 문제를 진단하는 데 필요한 정보를 유지하도록 많이 throw되는 예외에 대한 스냅숏을 수집합니다. [스냅숏 수집기 NuGet 패키지](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector)를 애플리케이션에 포함하고 필요에 따라, [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)에서 컬렉션 매개 변수를 구성합니다. 스냅숏은 Application Insights 포털의 [예외](../../azure-monitor/app/asp-net-exceptions.md)에 표시됩니다.

포털에서 디버그 스냅숏을 확인하여 호출 스택을 보고 각 호출 스택 프레임에서 변수를 검사할 수 있습니다. 소스 코드가 있는 좀 더 강력한 디버깅 환경을 구현하려면 Visual Studio 2017 Enterprise에서 스냅숏을 엽니다. 또한 Visual Studio에서 예외를 기다리지 않고 [snappoint에서 대화형으로 스냅숏을 만들도록 설정](https://aka.ms/snappoint)할 수도 있습니다.

디버그 스냅숏은 7일 동안 저장됩니다. 이 보존 정책은 응용 프로그램 단위로 설정됩니다. 이 값을 늘려야 하는 경우 Azure Portal에서 지원 사례를 열어 증가를 요청할 수 있습니다.

스냅숏 컬렉션을 다음에 사용할 수 있습니다.
* .NET Framework 및 .NET Framework 4.5 이상을 실행하는 ASP.NET 애플리케이션
* .NET Core 2.0 및 Windows에서 실행되는 ASP.NET Core 2.0 애플리케이션

다음 환경이 지원됩니다.
* Azure App Service
* OS 제품군 4 이상을 실행하는 Azure Cloud Service
* Windows Server 2012 R2 이상을 실행하는 Azure Service Fabric 서비스
* Windows Server 2012 R2 이상을 실행하는 Azure Virtual Machines
* Windows Server 2012 R2 이상을 실행하는 온-프레미스 가상 또는 물리적 컴퓨터

> [!NOTE]
> 클라이언트 애플리케이션(예를 들어, WPF, Windows Forms 또는 UWP)은 지원되지 않습니다.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>ASP.NET 애플리케이션에 대한 스냅숏 컬렉션 구성

1. 이 작업을 아직 수행하지 않은 경우 [웹앱에서 Application Insights를 사용하도록 설정](../../azure-monitor/app/asp-net.md)합니다.

2. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet 패키지를 앱에 포함합니다.

3. 패키지가 [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)에 추가한 기본 옵션을 검토합니다.

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. 스냅숏은 Application Insights에 보고되는 예외에 대해서만 수집됩니다. 경우에 따라(예: 이전 버전의 .NET 플랫폼) 포털에서 스냅숏 예외를 보려면 [예외 컬렉션을 구성](../../azure-monitor/app/asp-net-exceptions.md#exceptions)해야 할 수 있습니다.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>ASP.NET Core 2.0 애플리케이션에 대한 스냅숏 컬렉션 구성

1. 이 작업을 아직 수행하지 않은 경우 [ASP.NET Core 웹앱에서 Application Insights를 사용하도록 설정](../../azure-monitor/app/asp-net-core.md)합니다.

    > [!NOTE]
    > 애플리케이션이 Microsoft.ApplicationInsights.AspNetCore 패키지의 버전 2.1.1 이상을 참조하는지 확인하세요.

2. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet 패키지를 앱에 포함합니다.

3. 애플리케이션의 `Startup` 클래스를 수정하여 스냅숏 수집기의 원격 분석 프로세서를 추가하고 구성합니다.

    명령문을 사용하여 다음 항목을 `Startup.cs`에 추가합니다.

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   다음 `SnapshotCollectorTelemetryProcessorFactory` 클래스를 `Startup` 클래스에 추가합니다.

   ```csharp
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }
       ...
    ```
    `SnapshotCollectorConfiguration` 및 `SnapshotCollectorTelemetryProcessorFactory` 서비스를 시작 파이프라인에 추가합니다.

    ```csharp
       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. appsettings.json에 SnapshotCollectorConfiguration 섹션을 추가하여 스냅숏 수집기를 구성합니다. 예: 

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>다른 .NET 애플리케이션에 대한 스냅숏 컬렉션 구성

1. 애플리케이션이 아직 Application Insights로 계측되지 않는 경우 먼저 [Application Insights를 사용하도록 설정하고 계측 키를 설정](../../azure-monitor/app/windows-desktop.md)합니다.

2. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet 패키지를 앱에 추가합니다.

3. 스냅숏은 Application Insights에 보고되는 예외에 대해서만 수집됩니다. 예외를 보고하도록 코드를 수정해야 할 수도 있습니다. 예외 처리 코드는 애플리케이션의 구조에 따라 달라집니다. 예제는 다음과 같습니다.
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```

## <a name="grant-permissions"></a>권한 부여

스냅숏에 대한 액세스는 RBAC(역할 기반 액세스 제어)로 보호됩니다. 스냅숏을 검사하려면 먼저 구독 소유자가 사용자를 필요한 역할에 추가해야 합니다.

> [!NOTE]
> 소유자 및 참가자는 이 역할을 자동으로 소유하지는 않습니다. 스냅숏을 보려면 해당 스냅숏을 역할에 추가해야 합니다.

구독 소유자는 스냅숏을 검사할 사용자에게 `Application Insights Snapshot Debugger` 역할을 할당해야 합니다. 대상 Application Insights 리소스 또는 리소스 그룹이나 구독에 대한 구독 소유자가 개별 사용자 또는 그룹에 이 역할을 할당할 수 있습니다.

1. Azure Portal에서 Application Insights 리소스로 이동합니다.
1. **액세스 제어(IAM)** 를 클릭합니다.
1. **+역할 할당 추가** 단추를 클릭합니다.
1. **역할** 드롭다운 목록에서 **Application Insights 스냅숏 디버거**를 선택합니다.
1. 추가할 사용자의 이름을 검색하고 입력합니다.
1. **저장** 단추를 클릭하여 역할에 사용자를 추가합니다.


> [!IMPORTANT]
> 스냅숏은 변수 및 매개 변수 값의 개인 정보 및 기타 중요한 정보를 포함할 수 있습니다.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Application Insights 포털에서 스냅숏 디버그

지정된 예외 또는 문제 ID에 대해 스냅숏을 사용할 수 있으면 Application Insights 포털의 [예외](../../azure-monitor/app/asp-net-exceptions.md)에 **디버그 스냅숏 열기** 단추가 표시됩니다.

![예외에서 디버그 스냅숏 열기 단추](./media/snapshot-debugger/snapshot-on-exception.png)

디버그 스냅숏 보기에는 호출 스택 및 변수 창이 표시됩니다. 호출 스택 창에서 호출 스택의 프레임을 선택하면 변수 창에 해당 함수 호출에 대한 지역 변수 및 매개 변수가 표시됩니다.

![포털에서 디버그 스냅숏 보기](./media/snapshot-debugger/open-snapshot-portal.png)

중요한 정보가 스냅숏에 포함될 수 있으며 기본적으로 표시되지 않습니다. 스냅숏을 보려면 `Application Insights Snapshot Debugger` 역할이 할당되어 있어야 합니다.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Visual Studio 2017 Enterprise에서 스냅숏 디버그
1. **스냅숏 다운로드** 단추를 클릭하여 Visual Studio 2017 Enterprise에서 열 수 있는 `.diagsession` 파일을 다운로드합니다.

2. `.diagsession` 파일을 열려면 스냅숏 디버거 VS 구성 요소를 설치해야 합니다. 스냅숏 디버거 구성 요소는 VS에서 ASP.net 워크로드의 필수 구성 요소이며 VS 설치 관리자의 개별 구성 요소 목록에서 선택할 수 있습니다. 15.5 이전 버전의 Visual Studio를 사용하는 경우 [VS 마켓플레이스](https://aka.ms/snapshotdebugger)에서 확장을 설치해야 합니다.

3. 스냅숏 파일을 연 후에 Visual Studio에서 미니덤프 디버깅 페이지가 표시됩니다. **관리 코드 디버그**를 클릭하여 스냅숏을 디버깅하기 시작합니다. 예외가 throw되는 코드 줄에 스냅숏이 열리고 프로세스의 현재 상태를 디버그할 수 있습니다.

    ![Visual Studio에서 디버그 스냅숏 보기](./media/snapshot-debugger/open-snapshot-visualstudio.png)

다운로드한 스냅숏에는 웹 애플리케이션 서버에 있는 모든 기호 파일이 포함되어 있습니다. 이러한 기호 파일은 소스 코드에 스냅숏 데이터를 연결하는 데 필요합니다. App Service 앱의 경우 웹앱을 게시할 때 기호 배포를 사용할 수 있는지를 확인합니다.

## <a name="how-snapshots-work"></a>스냅숏 작동 방식

스냅숏 수집기는 [Application Insights 원격 분석 프로세서](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-processors-aspnet)로 구현됩니다. 애플리케이션이 실행되면 스냅숏 수집기 원격 분석 프로세서가 애플리케이션의 원격 분석 파이프라인에 추가됩니다.
애플리케이션에서 [TrackException](../../azure-monitor/app/asp-net-exceptions.md#exceptions)을 호출할 때마다 스냅숏 수집기는 throw된 예외의 형식과 throw하는 메서드에서 문제 ID를 계산합니다.
애플리케이션에서 TrackException을 호출할 때마다 해당 문제 ID에 대한 카운터가 증가합니다. 카운터가 `ThresholdForSnapshotting` 값에 도달하면 문제 ID가 수집 계획에 추가됩니다.

또한 Snapshot Collector는 [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) 이벤트에 가입하여 예외가 throw되었을 때 이를 모니터링합니다. 해당 이벤트가 발생하면 예외의 문제 ID가 계산되어 수집 계획의 문제 ID와 비교됩니다.
일치하는 항목이 있으면 실행 중인 프로세스의 스냅숏이 만들어집니다. 스냅숏에는 고유 식별자가 할당되고, 예외는 해당 식별자로 스탬프 처리됩니다. FirstChanceException 처리기가 반환되면 throw된 예외는 정상으로 처리됩니다. 결국, 예외는 스냅숏 식별자와 함께 Application Insights에 보고되는 TrackException 메서드에 다시 도달합니다.

주 프로세스는 계속 실행되고 매우 짧은 중단을 통해 사용자에게 트래픽을 제공합니다. 한편 스냅숏은 스냅숏 업로더 프로세스에 전달됩니다. 스냅숏 업로더는 미니덤프를 만들고, 관련된 모든 기호(.pdb) 파일과 함께 이를 Application Insights에 업로드합니다.

> [!TIP]
> - 프로세스 스냅숏은 실행 중인 프로세스의 일시 중단된 복제본입니다.
> - 스냅숏을 만드는 데는 약 10~20 밀리초가 걸립니다.
> - `ThresholdForSnapshotting`의 기본값은 1이며, 최솟값이기도 합니다. 따라서 스냅숏을 만들려면 먼저 앱에서 동일한 예외를 **두 번** 트리거해야 합니다.
> - Visual Studio에서 디버그하는 동안 스냅숏을 생성하려면 `IsEnabledInDeveloperMode`를 true로 설정합니다.
> - 스냅숏을 만드는 속도는 `SnapshotsPerTenMinutesLimit` 설정으로 제한됩니다. 기본적으로 10분마다 하나의 스냅숏으로 제한됩니다.
> - 매일 최대 50개의 스냅숏을 업로드할 수 있습니다.

## <a name="current-limitations"></a>현재 제한 사항

### <a name="publish-symbols"></a>기호 게시
스냅숏 디버거를 사용하려면 Visual Studio에서 변수를 디코딩하고 디버깅 환경을 제공하기 위해 프로덕션 서버에 기호 파일이 있어야 합니다.
Visual Studio 2017의 15.2 버전 이상은 App Service에 게시할 때 기본적으로 릴리스 빌드에 대한 기호를 게시합니다. 이전 버전에서는 기호가 릴리스 모드에서 게시될 수 있게 게시 프로필 `.pubxml` 파일에 다음 줄을 추가해야 합니다.

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Azure Compute 및 기타 형식의 경우 기호 파일이 주 애플리케이션 .dll의 동일한 폴더(일반적으로 `wwwroot/bin`)에 있거나 현재 경로에서 사용할 수 있는지 확인합니다.

### <a name="optimized-builds"></a>최적화된 빌드
경우에 따라 JIT 컴파일러에서 적용한 최적화로 인해 릴리스 빌드에서 지역 변수가 표시되지 않습니다.
그러나 Azure App Services에서 스냅숏 수집기는 수집 계획에 속한 throw하는 메서드를 최적화 해제할 수 있습니다.

> [!TIP]
> 최적화 해제 지원을 받으려면 Application Insights 사이트 확장을 App Service에 설치합니다.

## <a name="troubleshooting"></a>문제 해결

이러한 팁은 스냅숏 디버거를 사용하여 문제를 해결하는 데 유용합니다.

### <a name="use-the-snapshot-health-check"></a>스냅숏 상태 확인 사용
몇 가지 일반적인 문제로 인해 [디버그 스냅숏 열기]가 표시되지 않습니다. 오래된 스냅숏 수집기를 사용했거나(예: 일일 업로드 제한에 도달), 스냅숏을 업로드하는 데 시간이 오래 걸렸을 수도 있습니다. [스냅숏 상태 확인]을 사용하여 일반적인 문제를 해결합니다.

종단 간 추적 보기의 예외 창에는 [스냅숏 상태 확인]으로 이동하는 링크가 있습니다.

![스냅숏 상태 확인 진입](./media/snapshot-debugger/enter-snapshot-health-check.png)

채팅 모양의 대화형 인터페이스는 일반적인 문제를 찾아 수정하도록 안내합니다.

![상태 확인](./media/snapshot-debugger/healthcheck.png)

아직도 문제가 해결되지 않으면 다음 수동 문제 해결 단계를 참조하세요.

### <a name="verify-the-instrumentation-key"></a>계측 키 확인

게시된 애플리케이션에서 올바른 계측 키를 사용하는 있는지 확인합니다. 일반적으로 계측 키는 ApplicationInsights.config 파일에서 읽습니다. 포털에 표시된 Application Insights 리소스에 대한 계측 키와 동일한 값인지 확인합니다.

### <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>최신 버전의 NuGet 패키지로 업그레이드

Visual Studio의 NuGet 패키지 관리자를 사용하여 최신 버전의 Microsoft.ApplicationInsights.SnapshotCollector를 사용하고 있는지 확인합니다. 릴리스 정보는 https://github.com/Microsoft/ApplicationInsights-Home/issues/167에 있습니다.

### <a name="check-the-uploader-logs"></a>업로더 로그 확인

스냅숏을 만들면 디스크에 미니덤프 파일(.dmp)이 생성됩니다. 별도의 업로더 프로세스에서 해당 미니덤프 파일을 만들어 관련 PDB와 함께 Application Insights 스냅숏 디버거 저장소에 업로드합니다. 미니덤프가 성공적으로 업로드되면 디스크에서 삭제됩니다. 업로더 프로세스에 대한 로그 파일은 디스크에 유지됩니다. App Service 환경에서는 `D:\Home\LogFiles`에서 이러한 로그를 찾을 수 있습니다. App Service에 대한 Kudu 관리 사이트를 사용하여 이러한 로그 파일을 찾을 수 있습니다.

1. Azure Portal에서 App Service 애플리케이션을 엽니다.
2. **고급 도구**를 클릭하거나 **Kudu**를 검색합니다.
3. **이동**을 클릭합니다.
4. **디버그 콘솔** 드롭다운 목록 상자에서 **CMD**를 선택합니다.
5. **LogFiles**를 클릭합니다.

이름이 `Uploader_` 또는 `SnapshotUploader_`로 시작하고 확장명이 `.log`인 파일이 하나 이상 있어야 합니다. 해당 아이콘을 클릭하여 모든 로그 파일을 다운로드하거나 브라우저에서 엽니다.
파일 이름에는 App Service 인스턴스를 식별하는 고유한 접미사가 포함됩니다. App Service 인스턴스가 둘 이상의 컴퓨터에서 호스팅되는 경우 각 컴퓨터에 대한 별도의 로그 파일이 있습니다. 업로더에서 새 미니덤프 파일을 검색하면 로그 파일에 기록됩니다. 성공적인 스냅숏 및 업로드의 예는 다음과 같습니다.

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> 위의 예제는 Microsoft.ApplicationInsights.SnapshotCollector NuGet 패키지 버전 1.2.0에 있습니다. 이전 버전에서 업로더 프로세스는 `MinidumpUploader.exe`라고 하고 로그는 덜 자세하게 설명됩니다.

위 예에서 계측 키는 `c12a605e73c44346a984e00000000000`입니다. 이 값은 애플리케이션의 계측 키와 일치해야 합니다.
미니덤프는 ID가 `139e411a23934dc0b9ea08a626db16c5`인 스냅숏에 연결됩니다. 나중에 이 ID를 사용하여 Application Insights Analytics에서 연결된 예외 원격 분석을 찾을 수 있습니다.

업로더는 약 15분에 한 번씩 새 PDB를 검색합니다. 예를 들면 다음과 같습니다.

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

App Service에서 호스팅되지 _않는_ 애플리케이션의 경우 업로더 로그는 미니덤프와 동일한 폴더 `%TEMP%\Dumps\<ikey>`(여기서 `<ikey>`는 계측 키)에 저장됩니다.

### <a name="troubleshooting-cloud-services"></a>Cloud Services 문제 해결
Cloud Services의 역할의 경우, 기본 임시 폴더가 너무 작아서 미니 덤프 파일을 저장할 수 없게 되어 스냅숏이 손실될 수 있습니다.
필요한 공간은 애플리케이션의 전체 작업 집합과 동시 스냅숏 수에 따라 다릅니다.
32비트 ASP.NET 웹 역할의 작업 집합은 일반적으로 200MB ~ 500MB 사이입니다.
둘 이상의 동시 스냅숏을 허용합니다.
예를 들어, 애플리케이션이 1GB의 전체 작업 집합을 사용하는 경우, 스냅숏 저장을 위해 2GB 이상의 디스크 공간이 있는지 확인해야 합니다.
스냅숏용 전용 로컬 리소스가 있는 클라우드 서비스 역할을 구성하려면 다음 단계를 수행합니다.

1. 클라우드 서비스 정의(.csdef) 파일을 편집하여 클라우드 서비스에 새 로컬 리소스를 추가합니다. 다음 예제에서는 크기가 5GB인 `SnapshotStore`라는 리소스를 정의합니다.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. 역할의 시작 코드를 수정하여 `SnapshotStore` 로컬 리소스를 가리키는 환경 변수를 추가합니다. 작업자 역할의 경우 역할의 `OnStart` 메서드에 코드를 추가해야 합니다.
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   웹 역할(ASP.NET)의 경우 웹 애플리케이션의 `Application_Start` 메서드에 코드를 추가해야 합니다.
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. 사용자 역할의 ApplicationInsights.config 파일을 업데이트하여 `SnapshotCollector`에서 사용되는 임시 폴더 위치를 재정의합니다.
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

### <a name="overriding-the-shadow-copy-folder"></a>섀도 복사본 폴더 재정의

Snapshot Collector가 시작되면 Snapshot Uploader 프로세스를 실행하기에 적합한 디스크에서 폴더를 찾으려고 시도합니다. 선택된 폴더는 섀도 복사본 폴더라고 합니다.

Snapshot Collector는 잘 알려진 위치 몇 곳에서 Snapshot Uploader 바이너리를 복사할 권한이 있는지 확인합니다. 다음 환경 변수가 사용됩니다.
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- TEMP

적당한 폴더를 찾을 수 없으면 Snapshot Collector는 _"Could not find a suitable shadow copy folder."_(적합한 섀도 복사본 폴더를 찾을 수 없습니다.)라는 오류를 보고합니다.

복사에 실패하면 Snapshot Collector는 `ShadowCopyFailed` 오류를 보고합니다.

업로더를 시작할 수 없으면 Snapshot Collector는 `UploaderCannotStartFromShadowCopy` 오류를 보고합니다. 메시지 본문에 `System.UnauthorizedAccessException`이 포함되는 경우가 많습니다. 일반적으로 이 오류는 권한이 축소된 계정으로 애플리케이션이 실행되기 때문에 발생합니다. 계정에 섀도 복사본 폴더에 쓸 수 있는 권한은 있지만 코드를 실행할 수 있는 권한이 없습니다.

이러한 오류는 일반적으로 시작하는 동안 발생하기 때문에 _"Uploader failed to start."_(업로드를 시작하지 못했습니다.)라는 `ExceptionDuringConnect` 오류 다음에 발생합니다.

이러한 오류를 해결하려면 `ShadowCopyFolder` 구성을 통해 섀도 복사본 폴더를 수동으로 지정하면 됩니다. ApplicationInsights.config를 사용하는 예:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

또는 .NET Core 애플리케이션과 appsettings.json을 사용하는 경우:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Application Insights 검색을 사용하여 스냅숏 예외 찾기

스냅숏이 생성될 때 throw되는 예외에는 스냅숏 ID로 태그가 지정됩니다. 예외 원격 분석이 Application Insights에 보고될 때 이 스냅숏 ID가 사용자 지정 속성으로 포함됩니다. Application Insights에서 **검색**을 사용하여 `ai.snapshot.id` 사용자 지정 속성으로 모든 원격 분석을 찾을 수 있습니다.

1. Azure Portal에서 Application Insights 리소스로 이동합니다.
2. **검색**을 클릭합니다.
3. 검색 텍스트 상자에 `ai.snapshot.id`를 입력하고 Enter 키를 누릅니다.

![포털에서 스냅숏 ID로 원격 분석 검색](./media/snapshot-debugger/search-snapshot-portal.png)

이 검색에서 반환되는 결과가 없으면 선택한 시간 범위에서 애플리케이션에 대해 Application Insights에 보고된 스냅숏이 없는 것입니다.

업로더 로그에서 특정 스냅숏 ID를 검색하려면 검색 상자에 해당 ID를 입력합니다. 업로드된 것을 알고 있는 스냅숏에 대한 원격 분석을 찾을 수 없는 경우 다음 단계를 따릅니다.

1. 계측 키를 확인하여 올바른 Application Insights 리소스가 표시되어 있는지 다시 확인합니다.

2. 업로더 로그에서 타임스탬프를 사용하여 해당 시간 범위를 포함하도록 검색의 시간 범위 필터를 조정합니다.

해당 스냅숏 ID가 포함된 예외가 여전히 보이지 않는 경우 Application Insights에 예외 원격 분석이 보고되지 않은 것입니다. 스냅숏을 만든 후 예외 원격 분석을 보고하기 전에 애플리케이션의 작동이 중단된 경우에 이 상황이 발생할 수 있습니다. 이 경우 `Diagnose and solve problems`에서 App Service 로그를 검사하여 예기치 않은 다시 시작 또는 처리되지 않은 예외가 있는지 확인합니다.

### <a name="edit-network-proxy-or-firewall-rules"></a>네트워크 프록시 또는 방화벽 규칙 편집

애플리케이션에서 프록시 또는 방화벽을 통해 인터넷에 연결하는 경우 애플리케이션이 스냅숏 디버거 서비스와 통신할 수 있도록 규칙을 편집해야 할 수 있습니다. [스냅숏 디버거에서 사용하는 IP 주소 및 포트 목록](../../azure-monitor/app/ip-addresses.md#snapshot-debugger)이 있습니다.

## <a name="next-steps"></a>다음 단계

* 예외를 기다리지 않고 스냅숏을 가져오기 위해 [코드에서 snappoint를 설정](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications)합니다.
* [웹앱의 예외 진단](../../azure-monitor/app/asp-net-exceptions.md)에서는 Application Insights에서 추가 예외를 표시하는 방법을 설명합니다.
* [스마트 검색](../../azure-monitor/app/proactive-diagnostics.md)은 성능 예외를 자동으로 검색합니다.
