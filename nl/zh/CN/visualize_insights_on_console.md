---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, charts, visualize data, analytics console

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 在控制台上直观显示洞察
{: #visualize_insights_on_console}

要直观显示通过从应用程序捕获和发送的分析数据获取的洞察，您必须通过单击 Mobile Foundation Operations Console 的导航中的 **Analytics 控制台**选项来启动 Mobile Analytics 控制台。

Mobile Analytics 控制台可采用以下两种模式运行：
  - **演示模式开启**，仅用于演示目的，并使用模拟数据订阅源显示不同的分析视图（图表和表）。
  - **演示模式关闭**，根据来自[针对 Mobile Analytics 检测](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app)的应用程序的实时数据订阅源显示各种分析视图。

所有分析视图都可通过应用有关*应用程序名称*、*版本*、*设备操作系统*和*时间段*的过滤器进行修剪，从而让您获得不同角度的洞察。

要直观显示应用程序的洞察，请确保满足以下条件：
  - 应用程序已进行检测，以捕获相关分析数据并将其发送到 Mobile Analytics 服务。
  - 您已关闭 Analytics 控制台的演示模式。
  - 应用正确的过滤器。例如，应用程序已在现场部署并由用户积极使用时，请确保选择时间段。

Mobile Analytics 控制台提供针对移动应用程序使用情况和性能的不同类型分析，如 Analytics 控制台的导航窗格中所分类那样。以下各部分对不同的分析视图进行了详细说明：


## 用户数
{: #reports_visualized_users}
此视图可帮助您获取针对*用户上线模式*的洞察，例如指定日期范围内已使用应用程序的活跃用户数，以及新用户数与重新使用应用程序的现有用户数的比较。此视图中的图表可以按*应用程序名称*、*操作系统*或*操作系统版本*进行过滤。

## 会话数
{: #reports_visualized_sessions}
此视图可帮助您根据指定日期范围的*应用程序会话数*获取针对应用程序的*使用模式*的洞察。在应用程序进入设备前台时会记录一次会话。您将获取关于一天中使用应用程序最频繁及最稀少的时间的洞察，此信息可带来很有用的商业洞察。此视图中的图表可以按*应用程序名称*、*操作系统*或*操作系统版本*进行过滤。

## 网络请求数
{: #reports_visualized_network_requests}
此视图可帮助您获取关于应用程序向后端系统发出 API 调用的情况的洞察。此视图提供表和图表，可让您一窥后端系统中使用最频繁的功能、这些功能的响应时间和稳定性情况，以及您是否必须考虑重新均衡后端支持系统。

此视图包含针对以下项绘制的图表：选中的数据范围、应用程序的出站 API 调用的“平均往返时间”、每个 API 调用发出的请求数、按照响应代码分组的成功请求数与失败请求数。此视图中的图表可以按应用程序名称、操作系统或操作系统版本进行过滤。

## 崩溃次数
{: #reports_visualized_crashes}
此视图可帮助您获取关于应用程序在所选时间段内的稳定性情况的洞察，并帮助您决定是否必须修订应用程序设计或实施。此视图提供了图表来比较崩溃次数与使用总数以及总体崩溃率。此视图中的图表可以按*应用程序名称*、*操作系统*或*操作系统版本*进行过滤。


## 故障诊断
{: #reports_visualized_troubleshooting}
此视图提供了应用程序开发者对应用程序进行故障诊断时可能需要的所有必要信息。此视图提供了更详细的应用程序崩溃分析，涉及受影响的设备、主机操作系统、崩溃的具体时间、崩溃时的堆栈跟踪，以及可下载以获取更详细分析的崩溃日志。  

崩溃日志是通过查找在 FATAL 级别记录的应用程序日志来收集的。用于 Android 和 iOS 本机的 Analytics 客户机 SDK 会处理未捕获的异常并将其详细信息记录为 FATAL 级别日志消息。但是，对于 Cordova，JavaScript 层的任何崩溃都需要由开发者处理，并且需要将崩溃日志发送到 Mobile Analytics 服务，以在 Mobile Analytics 控制台中查看和分析。
{: note}


## 用户反馈
{: #reports_visualized_userfeedback}
此视图可提供针对用户在使用应用程序时经历的实际交互体验以及用户感受的洞察。

* **应用程序所有者**可获取富含上下文的详细错误视图以及**用户和测试者**发送的在运行应用程序时所记录的其他反馈。
* **开发者**可收到准确的应用程序上下文，以诊断并修订错误或功能缺口。
* **应用程序所有者**和**开发者**还可使用此视图管理针对所收到反馈的操作，例如记录对错误跟踪系统中所创建问题的注释或链接。还可对每个反馈设置总体复查状态，以帮助汇总对用户反馈执行的操作。

## 定制图表
此视图将 Mobile Analytics 扩展到定制情形中，其中**应用程序所有者**和**开发者**希望构建他们自己的特定于应用程序的分析。使用此工具，您可以围绕客户机 SDK 捕获的标准分析数据以及记录的定制数据或特定于应用程序的数据构建自己的分析视图（图表和表等）。有关此扩展分析工具的更多信息，请参阅[此处](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts)。
