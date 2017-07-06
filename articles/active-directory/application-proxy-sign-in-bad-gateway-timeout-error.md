---
title: "Fehler „Zugriff auf diese Unternehmensanwendung nicht möglich“ bei Verwendung der Anwendungsproxyanwendung | Microsoft-Dokumentation"
description: "Informationen zum Beheben häufiger Probleme mit Azure AD-Anwendungsproxyanwendungen."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7c4e2f084571a2146e5013f24bd5da55319118af
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---

# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Fehler „Zugriff auf diese Unternehmensanwendung nicht möglich“ bei Verwendung der Anwendungsproxyanwendung

In diesem Artikel erhalten Sie Unterstützung bei der Problembehandlung von häufigen Fehlern, wenn der Fehler „Zugriff auf diese Unternehmensanwendung nicht möglich“ in einer Azure AD-Anwendungsproxyanwendung auftritt.

## <a name="overview"></a>Übersicht
Wenn dieser Fehler angezeigt wird, wird auch ein Statuscode auf der Seite freigegeben. Es handelt sich bei dem Code wahrscheinlich um einen der folgenden:

-   **Gatewaytimeout:** Der Anwendungsproxydienst kann den Connector nicht erreichen. Dies deutet in der Regel auf ein Problem mit der Connectorzuweisung, dem Connector selbst oder den Netzwerkregeln für den Connector hin.

-   **Ungültiges Gateway:** Der Connector kann die Back-End-Anwendung nicht erreichen. Dies kann auf eine Fehlkonfiguration der Anwendung hinweisen.

-   **Verboten:** Der Benutzer ist nicht autorisiert, auf die Anwendung zuzugreifen. Dies kann vorkommen, wenn der Benutzer der Anwendung in Azure Active Directory nicht zugewiesen ist oder wenn der Benutzer auf dem Back-End nicht über die Berechtigung zum Zugriff auf die Anwendung verfügt.

Den Code finden Sie im Text der Fehlermeldung im Feld „Statuscode“ unten links. Überprüfen Sie auch, ob auf der Seite ganz unten Hinweise mit zusätzlichen Tipps vorhanden sind.

   ![Fehler „Gatewaytimeout“](./media/application-proxy/connection-problem.png)

Informationen zur Behandlung der grundlegenden Ursache für diesen Fehler und weitere Informationen zu den vorgeschlagenen Fehlerbehebungen finden Sie im entsprechenden Abschnitt weiter unten.

## <a name="gateway-timeout-errors"></a>Fehler „Gatewaytimeout“

Ein Gatewaytimeout tritt auf, wenn der Dienst versucht, den Connector zu erreichen, und dies nicht innerhalb des Timeoutfensters möglich ist. Dies wird normalerweise durch eine Anwendung verursacht, die einer Connectorgruppe ohne funktionierende Connectors zugewiesen ist, oder wenn einige vom Connector benötigte Ports geschlossen sind.


## <a name="bad-gateway-errors"></a>Fehler „Ungültiges Gateway“

Der Fehler „Ungültiges Gateway“ gibt an, dass der Connector die Back-End-Anwendung nicht erreichen kann. Stellen Sie sicher, dass Sie die richtige Anwendung veröffentlicht haben. Häufige Ursachen dieses Fehlers:

-   Ein Tippfehler oder ein Fehler in der internen URL

-   Ein nicht veröffentlichtes Stammverzeichnis der Anwendung. Beispiel: <http://expenses/reimbursement> wird veröffentlicht, der Zugriff erfolgt jedoch auf <http://expenses>.

-   Probleme mit der Konfiguration der eingeschränkten Kerberos-Delegierung (KCD)

-   Probleme mit der Back-End-Anwendung

## <a name="forbidden-errors"></a>Fehler „Verboten“

Wenn der Fehler „Verboten“ angezeigt wird, wurde der Benutzer der Anwendung nicht zugewiesen. Dies kann entweder Azure Active Directory oder die Back-End-Anwendung betreffen.

Informationen über das Zuweisen von Benutzern zur Anwendung in Azure finden Sie in der [Konfigurationsdokumentation](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Wenn Sie bestätigen, dass der Benutzer der Anwendung in Azure zugewiesen ist, überprüfen Sie die Benutzerkonfiguration in der Back-End-Anwendung. Wenn Sie eingeschränkte Kerberos-Delegierung/integrierte Windows-Authentifizierung verwenden, finden Sie nützliche Informationen auf unserer Seite zur KCD-Problembehandlung.

## <a name="check-the-applications-internal-url"></a>Überprüfen der internen URL der Anwendung

Als ersten schnellen Schritt können Sie die interne URL überprüfen und korrigieren, indem Sie die Anwendung über **Unternehmensanwendungen** öffnen und dann das Menü **Anwendungsproxy** auswählen. Stellen Sie sicher, dass die richtige interne URL aus Ihrem lokalen Netzwerk für den Zugriff auf die Anwendung verwendet wird.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Überprüfen, ob die Anwendung einer funktionierenden Connectorgruppe zugewiesen wurde

So überprüfen Sie, ob die Anwendung einer funktionierenden Connectorgruppe zugewiesen wurde

1.  Öffnen Sie die Anwendung im Verwaltungsportal, indem Sie zu **Azure Active Directory** navigieren und dann auf **Unternehmensanwendungen** und **Alle Anwendungen** klicken. Öffnen Sie die Anwendung, und wählen Sie dann im linken Menü **Anwendungsproxy** aus.

2.  Sehen Sie sich das Feld „Connectorgruppe“ an. Wenn keine aktiven Connectors in der Gruppe vorhanden sind, wird eine Warnung angezeigt. Wenn keine Warnungen angezeigt werden, fahren Sie mit „Überprüfen, ob alle erforderlichen Ports in der Zulassungsliste enthalten sind“ fort.

3.  Ist dies die falsche Connectorgruppe, wählen Sie die richtige Gruppe über die Dropdownliste aus, und bestätigen Sie, dass keine Warnungen mehr angezeigt werden. Ist dies die gewünschte Connectorgruppe, klicken Sie auf die Warnung und öffnen die Seite zur Connectorverwaltung.

4.  Hier stehen Ihnen mehrere Möglichkeiten zur weiteren Problembehebung zur Verfügung:

  * Verschieben eines aktiven Connectors in die Gruppe: Wenn Sie über einen aktiven Connector verfügen, der dieser Gruppe angehören sollte und uneingeschränkten Zugriff auf die Back-End-Anwendung hat, können Sie den Connector in die zugewiesene Gruppe verschieben. Klicken Sie hierzu auf den Connector. Verwenden Sie im Feld „Connectorgruppe“ die Dropdownliste, um die richtige Gruppe auszuwählen, und klicken Sie dann auf „Speichern“.

  * Herunterladen eines neuen Connectors für diese Gruppe: Auf dieser Seite finden Sie den Link zum [Herunterladen eines neuen Connectors](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). Der Connector muss auf einem Computer mit uneingeschränktem Zugriff auf die Back-End-Anwendung installiert werden, und befindet sich in der Regel auf dem gleichen Server wie die Anwendung. Verwenden Sie den Downloadlink, um einen Connector auf den Zielcomputer herunterzuladen. Als Nächstes klicken Sie auf den Connector und verwenden die Dropdownliste „Connectorgruppe“, um sicherzustellen, dass er der richtigen Gruppe angehört.

  * Untersuchen eines inaktiven Connectors: Wenn ein Connector als inaktiv angezeigt wird, kann er den Dienst nicht erreichen. Dies liegt in der Regel darin begründet, dass einige erforderliche Ports blockiert werden. Um das Problem zu beheben, fahren Sie mit dem Schritt „Überprüfen, ob alle erforderlichen Ports in der Zulassungsliste enthalten sind“ fort.

Testen Sie nach dem Ausführen der Schritte die Anwendung erneut, um sicherzustellen, dass die Anwendung einer Gruppe funktionierender Connectors zugewiesen ist. Wenn der Fehler immer noch auftritt, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="check-all-required-ports-are-whitelisted"></a>Überprüfen, ob alle erforderlichen Ports in der Zulassungsliste enthalten sind

Informationen zur Überprüfung, ob alle erforderlichen Ports geöffnet sind, finden Sie in unserer Dokumentation zum Öffnen von Ports. Wenn alle erforderlichen Ports geöffnet sind, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="check-for-other-connector-errors"></a>Überprüfen auf andere Connectorfehler

Wenn keine der oben genannten Schritte das Problem beheben, sollten Sie im nächsten Schritt nach Problemen oder Fehlern mit dem Connector selbst suchen. Einige häufig auftretende Fehler sind im [Problembehandlungsdokument](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors) aufgeführt. 

Sie können auch direkt auf die Connectorprotokolle zugreifen, um Fehler zu ermitteln. Anhand vieler Fehlermeldungen können Sie genauere Empfehlungen für Fehlerbehebungen erhalten. Informationen zum Verständnis der Protokolle finden Sie in der [Connectordokumentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="additional-resolutions"></a>Weitere Lösungen

Wenn die oben genannten Schritte das Problem nicht beheben konnten, können einige andere Ursachen vorliegen. So identifizieren Sie das Problem

Wenn Ihre Anwendung für die integrierte Windows-Authentifizierung (IWA) konfiguriert ist, testen Sie die Anwendung ohne einmaliges Anmelden. Wenn dies nicht der Fall ist, lesen Sie im nächsten Absatz weiter. Öffnen Sie die Anwendung zum Überprüfen ohne einmaliges Anmelden über **Unternehmensanwendungen**, und wechseln Sie zum Menü **Einmaliges Anmelden**. Ändern Sie die Auswahl in der Dropdownliste von „Integrierte Windows-Authentifizierung“ in „Azure AD-SSO deaktiviert“. 

Öffnen Sie einen Browser, und versuchen Sie erneut, auf die Anwendung zuzugreifen. Es sollte eine Authentifizierungsaufforderung angezeigt und anschließend die Anwendung geöffnet werden. Wenn dies funktioniert, wird das Problem durch die Konfiguration der eingeschränkten Kerberos-Delegierung (KCD) verursacht, die das einmalige Anmelden ermöglicht. Weitere Informationen finden Sie auf der Seite zur KCD-Problembehandlung.

Wenn die Fehlermeldung weiterhin angezeigt wird, wechseln Sie zum Computer, auf dem der Connector installiert ist, öffnen einen Browser und versuchen, die interne URL der Anwendung aufzurufen. Der Connector fungiert wie ein weiterer Client vom gleichen Computer. Wenn Sie die Anwendung nicht erreichen können, überprüfen Sie, warum dieser Computer die Anwendung nicht erreichen kann, oder verwenden Sie einen Connector auf einem Server, der auf die Anwendung zugreifen kann.

Wenn die Anwendung von diesem Computer aus erreicht werden kann, sollten Sie nach Problemen und Fehlern des Connectors selbst suchen. Einige häufig auftretende Fehler sind im [Problembehandlungsdokument](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors) aufgeführt. Sie können auch direkt auf die Connectorprotokolle zugreifen, um Fehler zu ermitteln. Anhand vieler Fehlermeldungen können Sie genauere Empfehlungen für Fehlerbehebungen erhalten. Informationen zum Verständnis der Protokolle finden Sie in der [Connectordokumentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)

