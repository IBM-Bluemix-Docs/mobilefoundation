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

# 移轉 Mobile Foundation 服務方案

使用已淘汰的方案所建立的 Mobile Foundation 實例需要更新成新的方案。可能也需要根據實例使用情形來進行方案更新。
{: shortdesc}

## 範例情境：從 Professional Per Device 方案移轉至 Professional 1 Application 方案

1. 從 IBM Cloud 儀表板中，選取您要移轉的 IBM Mobile Foundation 實例。
2. 在導覽中，選取**方案**。
   ![現有 Mobile Foundation 方案](images/existing-plan.png)
3. 從列出的定價方案中，選取 Professional 1 Application。
   ![新的 Mobile Foundation 方案](images/new-plan.png)
4. 按一下**儲存**按鈕，確認方案移轉。
     現在已完成移轉至 Professional 1 Application，且保留所有現有資料。計費已變更，且沒有關閉時間。
5. 移轉方案後，需要在服務儀表板中重建 Mobile Foundation 實例，以使正確配置生效。此更新需要較短的關閉時間。您需要規劃關閉時間。在導覽中，選取**管理**，然後按一下**重建**。

如果您位於其中一個已淘汰的方案，則必須移轉至新的方案。
{: note}

## 支援的方案移轉

* *Developer*（已淘汰）方案只能更新至新的 *Developer* 方案。
* *Developer Pro*（已淘汰）方案只能更新至 *Professional Per Device* 或 *Professional 1 Application* 方案。
* *Professional Per Capacity*（已淘汰）方案只能更新至 *Professional Per Device* 或 *Professional 1 Application* 方案。
* *Professional Per Device* 方案只能更新至 *Professional 1 Application* 方案。
* *Professional 1 Application* 方案只能更新至 *Professional Per Device* 方案。
* 新的 *Developer* 方案不支援方案更新。
