---

copyright:
  years: 2016, 2018
lastupdated:  "2018-11-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock:  .codeblock}

# 配置 Mobile Foundation 伺服器的自訂網域
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} 建立一個 {{site.data.keyword.mfserver_short_notm}}，可以使用 URL 進行存取，該 URL 具有根據 {{site.data.keyword.Bluemix_notm}} **Region** 的網域名稱。您也可以自行配置自訂網域。
{: shortdesc}

URL 或路徑建立時是使用根據 {{site.data.keyword.Bluemix_notm}} `Region` 的預設網域名稱。

  |網域|地區|    
  |:----- | :----- |    
  |`mybluemix.net` |美國南部|    
  |`eu-gb.mybluemix.net` |英國|
  |`au-syd.mybluemix.net` |雪梨|   
  |`eu-de.mybluemix.net` |法蘭克福|   
  {: caption="表 1. 應用程式網域名稱，根據 {{site.data.keyword.Bluemix_notm}} 中的地區" caption-side="top"}

您需要執行下列步驟來配置自訂網域，才能使用自己的網域：

1.	藉由選擇其中一個支援的方案，建立 {{site.data.keyword.mobilefoundation_short}} 服務實例來建立 {{site.data.keyword.mfserver_short_notm}} 實例。

+ 將您想要使用的自訂網域，新增至您的 {{site.data.keyword.Bluemix_notm}} `Organization`。移至**管理組織 > 網域 > 新增網域**，以新增您自己的網域。

+ 設定伺服器的路徑，以使用您的自訂網域。

+ 移至網域的 DNS 提供者，然後新增 CNAME 項目，它會將您網域的資料流量遞送到預設的 {{site.data.keyword.Bluemix_notm}} 路徑，即伺服器執行所在位置。

+ 如果您想要為自訂網域配置 `https`，請在 {{site.data.keyword.Bluemix_notm}} 中上傳網域的 SSL 憑證。若要這麼做，請移至**管理組織 > 網域**、選取您要配置 SSL 憑證的自訂網域、按一下**上傳憑證**來上傳網域的 SSL 憑證。如需相關資訊，請參閱[此貼文 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}。
