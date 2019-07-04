---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:note: .note}

# 常見問題
{: #mfp-faq}

此常見問題提供 {{site.data.keyword.mobilefoundation_long}} 服務常見問題的回答。
{: shortdesc}

## 如何知道 {{site.data.keyword.mobilefoundation_short}} 服務的更新項目何時可用？
{: #maintupdates_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} 建立一個 {{site.data.keyword.mfserver_short_notm}}。{{site.data.keyword.mobilefoundation_short}} 伺服器的更新會通知使用者。通知顯示於服務實例儀表板中。使用者可以選擇在其所決定的維護時間範圍內，將更新套用至 {{site.data.keyword.mobilefoundation_short}}。您可以選擇在您方便的時候更新 {{site.data.keyword.mobilefoundation_short}} 伺服器。

{{site.data.keyword.mobilefoundation_short}} 服務更新會在下列其中一個元件更新時提供。

* {{site.data.keyword.mfserver_short_notm}}.
* 基礎 Liberty 版本。
* 基礎 Java 開發者套件版本。

## 如何套用 {{site.data.keyword.mobilefoundation_short}} 服務的更新項目？
{: #apply_update_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} 的更新可以藉由按一下**重建**來套用。套用更新時，伺服器版本（如 {{site.data.keyword.mfp_oc_short_notm}} 中所見）修改為指出伺服器更新版本。

* 使用者無法將自己的修正程式及更新套用至其 {{site.data.keyword.mobilefoundation_short}} 服務實例。
* 請參閱[在 Professional Per Device 方案中重建伺服器](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5)和[在 Professional 1 Application 方案中重建伺服器](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2)，以瞭解按一下**重建** 後不同方案之間的行為差異。
{: note}

## 如何配置 {{site.data.keyword.mobilefoundation_short}} 伺服器實例的自訂網域？
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} 佈建一個 {{site.data.keyword.mfserver_short_notm}}，可以使用 URL 進行存取，該 URL 具有根據 {{site.data.keyword.Bluemix_notm}} **Region** 的網域名稱。您也可以自行配置自訂網域。


URL 或路徑建立時是使用根據 {{site.data.keyword.Bluemix_notm}} `Region` 的預設網域名稱。

  |網域|地區|    
  |:----- | :----- |    
  |`mybluemix.net` |美國南部|    
  |`eu-gb.mybluemix.net` |英國|
  |`au-syd.mybluemix.net` |雪梨|   
  |`eu-de.mybluemix.net` |法蘭克福|   
  {: caption="表 1. 應用程式網域名稱，根據 {{site.data.keyword.Bluemix_notm}} 中的地區" caption-side="top"}

若要使用自己的網域，您需要執行下列步驟來配置自訂網域：

1.	藉由選擇其中一個支援的方案，建立 {{site.data.keyword.mobilefoundation_short}} 服務實例來建立 {{site.data.keyword.mfserver_short_notm}} 實例。

+ 將您想要使用的自訂網域，新增至您的 {{site.data.keyword.Bluemix_notm}} `Organization`。移至**管理組織 > 網域 > 新增網域**，以新增您自己的網域。

+ 設定伺服器的路徑，以使用您的自訂網域。

+ 移至網域的 DNS 提供者，然後新增 CNAME 項目，它會將您網域的資料流量遞送到預設的 {{site.data.keyword.Bluemix_notm}} 路徑，即伺服器執行所在位置。

+ 如果您想要為自訂網域配置 `https`，請在 {{site.data.keyword.Bluemix_notm}} 中上傳網域的 SSL 憑證。若要上傳 SSL 憑證，請移至**管理組織 > 網域**、選取您要配置 SSL 憑證的自訂網域、按一下**上傳憑證**來上傳網域的 SSL 憑證。如需相關資訊，請參閱[此貼文 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}。
