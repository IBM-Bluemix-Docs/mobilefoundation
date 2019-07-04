---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# 迁移 Mobile Foundation 服务套餐

使用不推荐的套餐创建的 Mobile Foundation 实例需要更新到新套餐。根据实例使用情况，还可能需要更新套餐。
{: shortdesc}

## 示例场景：从 Professional Per Device 套餐迁移到 Professional 1 Application 套餐

1. 在 IBM Cloud“仪表板”中，选择要迁移的 IBM Mobile Foundation 实例。
2. 在导航中，选择**套餐**。
   ![现有 Mobile Foundation 套餐](images/existing-plan.png)
3. 从列出的价格套餐中，选择 Professional 1 Application。
   ![新建 Mobile Foundation 套餐](images/new-plan.png)
4. 单击**保存**按钮，并确认套餐迁移。
     现在已完成迁移到 Professional 1 Application 的过程，并且保留了所有现有数据。计费已更改，并且没有停机时间。
5. 迁移套餐后，需要在服务仪表板中重新创建 Mobile Foundation 实例，以使正确配置生效。此更新需要短暂的停机时间。您需要计划停机时间。在导航中，选择**管理**，然后单击**重新创建**。

如果您使用的是其中一个不推荐的套餐，那么必须迁移到新套餐。
{: note}

## 支持的套餐迁移

* *Developer*（不推荐）套餐只能更新为新的 *Developer* 套餐。
* *Developer Pro*（不推荐）套餐只能更新为 *Professional Per Device* 或 *Professional 1 Application* 套餐。
* *Professional Per Capacity*（不推荐）套餐只能更新为 *Professional Per Device* 或 *Professional 1 Application* 套餐。
* *Professional Per Device* 套餐只能更新为 *Professional 1 Application* 套餐。
* *Professional 1 Application* 套餐只能更新为 *Professional Per Device* 套餐。
* 新的 *Developer* 套餐不支持套餐更新。
