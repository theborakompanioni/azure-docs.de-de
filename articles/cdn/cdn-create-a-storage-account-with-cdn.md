---
title: Integrieren eines Speicherkontos in CDN | Microsoft Docs
description: Erfahren Sie, wie Azure Content Delivery Network (CDN) für die Übermittlung breitbandiger Inhalte eingesetzt wird, indem Blobs aus Azure Storage zwischengespeichert werden.
services: cdn
documentationcenter: ''
author: camsoper
manager: erikre
editor: ''

ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper

---
# Integrieren eines Speicherkontos in CDN
Zum Zwischenspeichern von Inhalten aus Ihrem Azure-Speicher kann CDN aktiviert werden. Das Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung für die Übermittlung von Inhalten mit hohen Breitbandenanforderungen. Dazu werden Blobs und statische Inhalte von Compute-Instanzen an physischen Knotenpunkten in den USA, Europa, Asien, Australien und Südamerika zwischengespeichert.

## Schritt 1: Erstellen eines Speicherkontos
Gehen Sie folgendermaßen vor, um für ein Azure-Abonnement ein neues Speicherkonto zu erstellen. Ein Speicherkonto ermöglicht den Zugriff auf Speicherdienste von Azure. Das Speicherkonto stellt die höchste Ebene des Namespace für den Zugriff auf die verschiedenen Komponenten der Speicherdienste von Azure dar: Blob-, Warteschlangen- und Tabellenspeicherdienste. Weitere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md).

Um ein Speicherkonto zu erstellen, müssen Sie entweder der Dienstadministrator oder ein Co-Administrator für das zugehörige Abonnement sein.

> [!NOTE]
> Es gibt mehrere Methoden zum Erstellen eines Speicherkontos, einschließlich Azure-Portal und PowerShell. In diesem Tutorial wird das Azure-Portal verwendet.
> 
> 

**So erstellen Sie ein Speicherkonto für ein Azure-Abonnement**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie unten links auf **Neu**. Wählen Sie im Dialogfeld **Neu** die Option **Daten und Speicher** aus, und klicken Sie dann auf **Speicherkonto**.
   
   Das Blatt **Speicherkonto erstellen** wird angezeigt.
   
   ![Speicherkonto erstellen][create-new-storage-account]
3. Geben Sie in das Feld **Name** den Namen einer Unterdomäne ein. Dieser Eintrag kann 3 bis 24 Kleinbuchstaben und Zahlen enthalten.
   
    Dieser Wert wird der Hostname im URI, der zum Adressieren von Blob-, Warteschlangen- oder Tabellenspeicherressourcen für das Abonnement verwendet wird. Zum Adressieren einer Containerressource im Blobdienst verwenden Sie einen URI im folgenden Format; dabei bezieht sich *&lt;Speicherkontobezeichnung&gt;* auf den Wert, den Sie in **URL eingeben** eingegeben haben:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*
   
    **Wichtig:** Die URL-Bezeichnung bildet die Unterdomäne des Speicherkonto-URI und muss unter allen gehosteten Diensten in Azure eindeutig sein.
   
    Dieser Wert wird auch als Name für dieses Speicherkonto im Portal verwendet oder beim programmatischen Zugriff auf dieses Konto.
4. Behalten Sie die Standardwerte für **Bereitstellungsmodell**, **Kontoart**, **Leistung** und **Replikation** bei.
5. Wählen Sie das **Abonnement** aus, das für das Speicherkonto verwendet werden soll.
6. Wählen Sie eine **Ressourcengruppe** aus, oder erstellen Sie eine. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../resource-group-overview.md#resource-groups).
7. Wählen Sie einen Standort für Ihr Speicherkonto aus.
8. Klicken Sie auf **Erstellen**. Das Erstellen des Speicherkontos kann einige Minuten dauern.

## Schritt 2: Erstellen eines neuen CDN-Profils
Ein CDN-Profil ist eine Sammlung von CDN-Endpunkten. Jedes Profil enthält mindestens einen CDN-Endpunkt. Sie können mehrere Profile verwenden, um Ihre CDN-Endpunkte nach Internetdomäne, Webanwendung oder anderen Kriterien zu organisieren.

> [!TIP]
> Wenn Sie bereits über ein CDN-Profil verfügen, das Sie für dieses Tutorial verwenden möchten, fahren Sie mit [Schritt 3](#step-3-create-a-new-cdn-endpoint) fort.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## Schritt 3: Erstellen eines neuen CDN-Endpunkts
**So erstellen Sie einen neuen CDN-Endpunkt für das Speicherkonto**

1. Navigieren Sie im [Azure-Verwaltungsportal](https://portal.azure.com) zu Ihrem CDN-Profil. Eventuell haben Sie es im vorherigen Schritt an das Dashboard angeheftet. Andernfalls können Sie es ermitteln, indem Sie auf **Durchsuchen**, auf **CDN-Profile** und dann auf das Profil klicken, dem Sie den Endpunkt hinzufügen möchten.
   
    Das Blatt für das CDN-Profil wird angezeigt.
   
    ![CDN-Profil][cdn-profile-settings]
2. Klicken Sie auf die Schaltfläche **Endpunkt hinzufügen**.
   
    ![Schaltfläche „Endpunkt hinzufügen“][cdn-new-endpoint-button]
   
    Das Blatt **Endpunkt hinzufügen** wird angezeigt.
   
    ![Blatt „Endpunkt hinzufügen“][cdn-add-endpoint]
3. Geben Sie einen **Namen** für diesen CDN-Endpunkt ein. Dieser Name wird für den Zugriff auf die zwischengespeicherten Ressourcen in der Domäne `<endpointname>.azureedge.net` verwendet.
4. Wählen Sie in der Dropdownliste **Ursprungstyp** die Option *Speicher* aus.
5. Wählen Sie in der Dropdownliste **Origin hostname** Ihr Speicherkonto aus.
6. Behalten Sie die Standardwerte für **Ursprünglicher Pfad**, **Header des Ursprungshosts** und **Protocol/Origin port** bei. Sie müssen mindestens ein Protokoll (HTTP oder HTTPS) angeben.
   
   > [!NOTE]
   > Mit dieser Konfiguration werden alle Ihre öffentlich sichtbaren Container in Ihrem Speicherkonto für die Zwischenspeicherung im CDN aktiviert. Wenn Sie den Gültigkeitsbereich auf einen einzelnen Container beschränken möchten, verwenden Sie **Ursprünglicher Pfad**. Beachten Sie, dass dieser Container als öffentlich sichtbar festgelegt sein muss.
   > 
   > 
7. Klicken Sie auf die Schaltfläche **Hinzufügen**, um den neuen Endpunkt zu erstellen.
8. Sobald der Endpunkt erstellt wurde, wird er in einer Liste von Endpunkten für das Profil angezeigt. In der Listenansicht werden der URL für den Zugriff auf zwischengespeicherte Inhalte sowie die Ursprungsdomäne angezeigt.
   
    ![CDN-Endpunkt][cdn-endpoint-success]
   
   > [!NOTE]
   > Der Endpunkt kann nicht sofort verwendet werden. Es dauert bis zu 90 Minuten, bis die Registrierung über das CDN-Netzwerk weitergegeben wurde. Benutzer, die den CDN-Domänennamen sofort zu verwenden versuchen, sehen u. U. den Statuscode 404, bis die Inhalte über das CDN verfügbar sind.
   > 
   > 

## Schritt 4: Zugriff auf CDN-Inhalte
Verwenden Sie für den Zugriff auf die im CDN zwischengespeicherten Inhalte den im Portal bereitgestellten CDN-URL. Die Adresse eines zwischengespeicherten Blobs hat etwa folgendes Format:

http://<*EndpunktName*>.azureedge.net/<*meinÖffentlicherContainer*>/<*BlobName*>

> [!NOTE]
> Sobald Sie den CDN-Zugriff auf ein Speicherkonto oder einen gehosteten Dienst aktivieren, kommen alle öffentlich verfügbaren Objekte für den CDN-Edgezwischenspeicher in Frage. Wenn Sie ein Objekt ändern, das derzeit im CDN zwischengespeichert ist, stehen die neuen Inhalte erst dann über das CDN zur Verfügung, wenn das CDN seine Inhalte aktualisiert, wenn die Lebensdauer der zwischengespeicherten Inhalte abläuft.
> 
> 

## Schritt 5: Entfernen von Inhalten aus dem CDN
Wenn ein Objekt nicht mehr im Azure Content Delivery Network (CDN) zwischengespeichert werden soll, können Sie einen der folgenden Schritte ausführen:

* Sie können den öffentlichen Container zu einem privaten machen. Weitere Informationen finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](../storage/storage-manage-access-to-resources.md).
* Sie können den CDN-Endpunkt mit dem Verwaltungsportal deaktivieren oder löschen.
* Sie können den gehosteten Dienst so ändern, dass er nicht mehr auf Anforderungen des Objekts antwortet.

Ein bereits im CDN zwischengespeichertes Objekt bleibt zwischengespeichert, bis die Lebensdauer des Objekts abgelaufen ist oder der Endpunkt gelöscht wird. Wenn die Lebensdauer abläuft, prüft das CDN, ob der CDN-Endpunkt weiterhin gültig ist und ob weiterhin anonym auf das Objekt zugegriffen werden kann. Ist dies nicht der Fall, wird das Objekt nicht mehr zwischengespeichert.

## Zusätzliche Ressourcen
* [Zuordnen von CDN-Inhalt (Content Delivery Network) zu einer benutzerdefinierten Domäne](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png

<!---HONumber=AcomDC_0803_2016-->