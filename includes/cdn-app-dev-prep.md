## <a name="prerequisites"></a>Voraussetzungen
Bevor wir CDN-Verwaltungscode schreiben können, müssen wir einige Vorbereitungsschritte ausführen, damit der Code mit dem Azure Resource Manager interagieren kann.  Gehen Sie hierzu wie folgt vor:

* Erstellen Sie eine Ressourcengruppe für das CDN-Profil, das wir in diesem Tutorial erstellen.
* Konfigurieren Sie Azure Active Directory für die Authentifizierung der Anwendung.
* Wenden Sie Berechtigungen auf die Ressourcengruppe an, sodass nur autorisierte Benutzer aus Ihrem Azure AD-Mandanten mit dem CDN-Profil interagieren können.

### <a name="creating-the-resource-group"></a>Erstellen der Ressourcengruppe
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie links oben auf die Schaltfläche **Neu** und anschließend auf **Verwaltung** > **Ressourcengruppe**.

    ![Erstellen einer neuen Ressourcengruppe](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Nennen Sie die Ressourcengruppe *CdnConsoleTutorial*.  Wählen Sie Ihr Abonnement aus, und wählen Sie einen Standort in Ihrer Nähe.  Wenn Sie möchten, können Sie das Kontrollkästchen **An Dashboard anheften** aktivieren, um die Ressourcengruppe an das Dashboard im Portal anzuheften.  Dadurch lässt sie sich später leichter wiederfinden.  Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **Erstellen**.

    ![Benennen der Ressourcengruppe](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Wenn Sie die Ressourcengruppe nicht an Ihr Dashboard angeheftet haben, können Sie nach der Erstellung danach suchen, indem Sie auf **Durchsuchen** > **Ressourcengruppen** klicken.  Klicken Sie auf die Ressourcengruppe, um sie zu öffnen.  Notieren Sie sich Ihre **Abonnement-ID**.  Sie benötigen sie später.

    ![Benennen der Ressourcengruppe](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Erstellen der Azure AD-Anwendung und Anwenden von Berechtigungen
Es gibt zwei Methoden für die App-Authentifizierung mit Azure Active Directory: per einzelnem Benutzer oder per Dienstprinzipal. Ein Dienstprinzipal ähnelt einem Dienstkonto in Windows.  Anstatt einen bestimmten Benutzer Berechtigungen für die Interaktion mit den CDN-Profilen zu gewähren, erteilen wir die Berechtigungen für den Dienstprinzipal.  Dienstprinzipale werden im Allgemeinen für automatisierte, nicht interaktive Prozesse verwendet.  Auch wenn in diesem Tutorial eine interaktive Konsolen-App erstellt wird, verwenden wir hier einen Dienstprinzipal.

Die Erstellung eines Dienstprinzipals umfasst mehrere Schritte, einschließlich der Erstellung einer Azure Active Directory-Anwendung.  [Absolvieren Sie dieses Tutorial](../articles/resource-group-create-service-principal-portal.md), um diese Schritte auszuführen.

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie alle Schritte in [diesem Tutorial](../articles/resource-group-create-service-principal-portal.md)ausführen.  Es ist *von größter Bedeutung* , dass Sie die Schritte exakt wie beschrieben ausführen.  Notieren Sie sich die folgenden Angaben, da Sie diese später benötigen werden: **Mandanten-ID**, **Domänenname des Mandanten** (üblicherweise eine *.onmicrosoft.com*-Domäne, sofern Sie keine benutzerdefinierte Domäne angegeben haben), **Client-ID** und **Clientauthentifizierungsschlüssel**.  Schützen Sie die **Client-ID** und den **Clientauthentifizierungsschlüssel** sorgfältig, da diese Anmeldeinformationen verwendet werden können, um Vorgänge als Dienstprinzipal auszuführen.
>
> Wenn Sie zum Schritt „Mehrinstanzenfähige Anwendung konfigurieren“ gelangen, wählen Sie **Nein** aus.
>
> Im Schritt [Zuweisen einer Anwendung zur Rolle](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role) verwenden Sie die zuvor erstellte Ressourcengruppe *CdnConsoleTutorial*, weisen Sie ihr aber nicht die Rolle **Leser**, sondern die Rolle **Mitwirkender von CDN-Profil** zu.  Nachdem Sie die Anwendung in Ihrer Ressourcengruppe der Rolle **Mitwirkender von CDN-Profil** zugewiesen haben, kehren Sie zu diesem Tutorial zurück. 
>
>

Nachdem Sie den Dienstprinzipal erstellt und die Rolle **Mitwirkender von CDN-Profil** zugewiesen haben, sollte das Blatt **Benutzer** für Ihre Ressourcengruppe in etwa wie folgt aussehen:

![Blatt „Benutzer“](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktive Benutzerauthentifizierung
Wenn Sie statt eines Dienstprinzipals lieber eine interaktive individuelle Benutzerauthentifizierung einrichten möchten, ähnelt der Prozess dem der Erstellung eines Dienstprinzipals.  Sie wenden im Grunde das gleiche Verfahren an, nehmen aber einige kleinere Änderungen vor.

> [!IMPORTANT]
> Führen Sie die nächsten Schritte nur aus, wenn Sie die individuelle Benutzerauthentifizierung anstelle eines Dienstprinzipals verwenden möchten.
>
>

1. Wählen Sie beim Erstellen der Anwendung anstelle von **Webanwendung** die Option **Native Anwendung** aus.

    ![Native Anwendung](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Auf der nächsten Seite werden Sie zur Eingabe eines **Umleitungs-URI**aufgefordert.  Der URI wird nicht überprüft, merken Sie sich jedoch, was Sie eingegeben haben.  Sie benötigen die Information später.
3. Sie müssen keinen **Clientauthentifizierungsschlüssel**erstellen.
4. Anstatt der Rolle **Mitwirkender von CDN-Profil** einen Dienstprinzipal zuzuweisen, weisen Sie einzelne Benutzer oder Gruppen zu.  In diesem Beispiel sehen Sie, dass ich den Benutzer *CDN-Demobenutzer* der Rolle **Mitwirkender von CDN-Profil** zugewiesen habe.  

    ![Individueller Benutzerzugriff](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
