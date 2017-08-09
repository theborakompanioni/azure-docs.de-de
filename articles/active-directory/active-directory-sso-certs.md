---
title: Verwalten von Verbundzertifikaten in Azure AD | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie das Ablaufdatum für Verbundzertifikate anpassen und Zertifikate erneuern, die in Kürze ablaufen."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 1283b570200f05003658824760ecbb6722f241d9
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory
In diesem Artikel werden häufige gestellte Fragen im Zusammenhang mit den Zertifikaten behandelt, die Azure Active Directory (Azure AD) erstellt, um die einmalige Verbundanmeldung (Single Sign-On, SSO) für Ihre SaaS-Anwendungen einzurichten. Fügen Sie Anwendungen aus dem Azure AD-App-Katalog oder über Anwendungsvorlagen hinzu, die nicht aus dem Katalog stammen. Konfigurieren der Anwendung mit der Option für Verbund-SSO.

Dieser Artikel ist für Apps relevant, die über einen SAML-Verbund für Azure AD-SSO konfiguriert sind, wie im folgenden Beispiel gezeigt:

![Azure AD – einmaliges Anmelden](./media/active-directory-sso-certs/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatisch generiertes Zertifikat für Katalog- und Nicht-Kataloganwendungen
Wenn Sie eine neue Anwendung aus dem Katalog hinzufügen und die SAML-basierte Anmeldung konfigurieren, generiert Azure AD ein drei Jahre lang gültiges Zertifikat für die Anwendung. Sie können dieses Zertifikat aus dem Abschnitt **SAML-Signaturzertifikat** herunterladen. Bei Kataloganwendungen wird in diesem Abschnitt möglicherweise eine Option angezeigt, über die Sie je nach Anforderung der Anwendung das Zertifikat oder Metadaten herunterladen können.

![Azure AD-SSO](./media/active-directory-sso-certs/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Anpassen des Ablaufdatums für das Verbundzertifikat und Rollover zu einem neuen Zertifikat
Standardmäßig wird für Zertifikate eine Gültigkeitsdauer von drei Jahren festgelegt. Führen Sie die folgenden Schritte aus, um ein anderes Ablaufdatum für das Zertifikat auszuwählen.
In den Screenshots wurde Salesforce als Beispiel verwendet, diese Schritte können jedoch für jede beliebige SaaS-Verbund-App verwendet werden.

1. Klicken Sie im [Azure-Portal](https://aad.portal.azure.com) im linken Bereich auf **Unternehmensanwendung** und dann auf der Seite **Übersicht** auf **Neue Anwendung**:

   ![Öffnen des SSO-Konfigurations-Assistenten](./media/active-directory-sso-certs/enterprise_application_new_application.png)

2. Suchen Sie nach der Kataloganwendung, und wählen Sie die Anwendung aus, die Sie hinzufügen möchten. Wenn Sie die gewünschte Anwendung nicht finden können, fügen Sie die Anwendung über die Option **Nicht-Kataloganwendung** hinzu. Dieses Feature ist nur in der SKU Azure AD Premium (P1 und P2) verfügbar.

    ![Azure AD-SSO](./media/active-directory-sso-certs/add_gallery_application.png)

3. Klicken Sie im linken Bereich auf den Link **Einmaliges Anmelden**, und ändern Sie den **Modus** in **SAML-basierte Anmeldung**. In diesem Schritt wird das drei Jahre lang gültige Zertifikat für Ihre Anwendung generiert.

4. Um ein neues Zertifikat zu erstellen, klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf den Link **Neues Zertifikat erstellen**.

    ![Neues Zertifikat generieren](./media/active-directory-sso-certs/create_new_certficate.png)

5. Durch Klicken auf den Link **Neues Zertifikat erstellen** wird das Kalendersteuerelement geöffnet. Sie können ein beliebiges Datum inklusive Uhrzeit in einem Zeitraum von bis zu drei Jahren festlegen. Der ausgewählte Wert für Datum und Uhrzeit ist das neue Ablaufdatum mit Uhrzeit für das neue Zertifikat. Klicken Sie auf **Speichern**.

    ![Herunterladen und Hochladen des Zertifikats](./media/active-directory-sso-certs/certifcate_date_selection.PNG)

6. Das neue Zertifikat steht jetzt zum Download bereit. Klicken Sie auf den Link **Zertifikat**, um das Zertifikat herunterzuladen. An diesem Punkt ist Ihr Zertifikat noch nicht aktiv. Wenn Sie jetzt einen Rollover zu diesem Zertifikat ausführen möchten, aktivieren Sie das Kontrollkästchen **Als neues Zertifikat festlegen**, und klicken Sie auf **Speichern**. Von diesem Zeitpunkt an verwendet Azure AD das neue Zertifikat zum Signieren der Antwort.

7.  Informationen zum Hochladen des Zertifikats in Ihre spezifische SaaS-Anwendung erhalten Sie unter dem Link **Tutorial zur Anzeige der Anwendungskonfiguration**.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Erneuern eines in Kürze ablaufenden Zertifikats
Die folgenden Schritte zur Erneuerung sollten zu keinen wesentlichen Ausfallzeiten für Ihre Benutzer führen. In den Screenshots in diesem Abschnitt wurde Salesforce als Beispiel verwendet, diese Schritte können jedoch für jede beliebige SaaS-Verbund-App angewendet werden.

1. Generieren Sie auf der Seite **Einmaliges Anmelden** für die **Azure Active Directory**-Anwendung das neue Zertifikat für Ihre Anwendung. Klicken Sie hierzu im Abschnitt **SAML-Signaturzertifikat** auf den Link **Neues Zertifikat erstellen**.

    ![Neues Zertifikat generieren](./media/active-directory-sso-certs/create_new_certficate.png)

2. Wählen Sie das gewünschte Ablaufdatum inklusive Uhrzeit für das neue Zertifikat aus, und klicken Sie dann auf die Schaltfläche **Speichern**.

3. Laden Sie das Zertifikat über die Option **SAML-Signaturzertifikat** herunter. Laden Sie das neue Zertifikat im Bestätigungsbildschirm der einmaligen Anmeldung der SaaS-Anwendung hoch. Informationen zur Vorgehensweise für Ihre spezifische SaaS-Anwendung erhalten Sie unter dem Link **Tutorial zur Anzeige der Anwendungskonfiguration**.
   
4. Um das neue Zertifikat in Azure AD zu aktivieren, aktivieren Sie das Kontrollkästchen **Als neues Zertifikat festlegen**, und klicken Sie dann oben auf der Seite auf die Schaltfläche **Speichern**. Hierdurch wird ein Rollover zum neuen Zertifikat in Azure AD durchgeführt. Der Status des Zertifikats ändert sich von **Neu** in **Aktiv**. Von diesem Zeitpunkt an verwendet Azure AD das neue Zertifikat zum Signieren der Antwort. 
   
    ![Neues Zertifikat generieren](./media/active-directory-sso-certs/new_certificate_download.png)

## <a name="related-articles"></a>Verwandte Artikel
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Problembehandlung bei SAML-basiertem einmaligem Anmelden](active-directory-saml-debugging.md)

