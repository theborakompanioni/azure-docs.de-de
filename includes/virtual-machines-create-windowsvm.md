1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie oben links auf **Neu > Compute > Windows Server 2016 Datacenter**.

    ![Navigieren zu den Azure-VM-Images im Portal](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. Wählen Sie für Windows Server 2016 Datacenter das klassische Bereitstellungsmodell aus. Klicken Sie auf „Erstellen“.

    ![Screenshot, der die Azure-VM-Images zeigt, die im Portal verfügbar sind](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Blatt "Grundlagen"

Auf dem Blatt „Grundlagen“ werden administrative Informationen für den virtuellen Computer angefordert.

1. Geben Sie einen **Namen** für den virtuellen Computer an. Im Beispiel ist _HeroVM_ der Name des virtuellen Computers. Der Name kann bis zu 15 Zeichen lang sein und darf keine Sonderzeichen enthalten.

2. Geben Sie einen **Benutzernamen** und ein sicheres **Kennwort** für die Erstellung eines lokalen Kontos auf dem virtuellen Computer ein. Das lokale Konto wird für die Anmeldung bei dem virtuellen Computer sowie für dessen Verwaltung verwendet. Im Beispiel ist _azureuser_ der Benutzername.

 Das Kennwort muss 8 bis 123 Zeichen lang sein und drei der folgenden vier Komplexitätsanforderungen erfüllen: ein Kleinbuchstabe, ein Großbuchstabe, eine Zahl und ein Sonderzeichen. Weitere Informationen finden Sie unter den [Anforderungen für Benutzernamen und Kennwörter](../articles/virtual-machines/windows/faq.md).

3. Das **Abonnement** ist optional. Eine häufige Einstellung ist „Nutzungsbasierte Bezahlung“.

4. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. Im Beispiel ist _HeroVMRG_ der Name der Ressourcengruppe.

5. Wählen Sie einen **Standort** eines Azure-Datencenters aus, an dem Sie die VM ausführen möchten. Im Beispiel ist **USA, Osten** der gewählte Standort.

6. Klicken Sie anschließend auf **Weiter**, um zum nächsten Blatt zu gelangen.

    ![Screenshot mit den Einstellungen auf dem Blatt „Grundlagen“ für die Konfiguration eines virtuellen Azure-Computers](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Blatt „Größe“

Auf dem Blatt „Größe“ werden die Konfigurationsdetails der VM identifiziert und verschiedene Optionen angegeben, z.B. Betriebssystem, Anzahl von Prozessoren, Datenträger-Speichertyp und die geschätzten monatlichen Kosten.  

Wählen Sie eine VM-Größe aus, und klicken Sie anschließend auf **Auswählen**, um den Vorgang fortzusetzen. In diesem Beispiel ist _DS1_\__V2 Standard_ die VM-Größe.

  ![Screenshot des Blatts „Größe“ mit den Größenoptionen für den virtuellen Azure-Computer](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Blatt „Einstellungen“

Auf dem Blatt „Einstellungen“ werden Speicher- und Netzwerkoptionen angefordert. Sie können die Standardeinstellungen übernehmen. Azure erstellt bei Bedarf die entsprechenden Einträge.

Wenn Sie eine geeignete Größe für den virtuellen Computer ausgewählt haben, können Sie Azure Storage Premium durch Auswahl von Premium (SSD) unter „Datenträgertyp“ testen.

Klicken Sie auf **OK**, nachdem Sie alle gewünschten Änderungen vorgenommen haben.

## <a name="4-summary-blade"></a>4. Blatt „Zusammenfassung“

Auf dem Blatt „Zusammenfassung“ werden die auf den vorherigen Blättern angegebenen Einstellungen aufgeführt. Klicken Sie auf **OK**, wenn Sie zum Erstellen des Image bereit sind.

 ![Bericht auf dem Blatt „Zusammenfassung“ mit den für den virtuellen Computer angegebenen Einstellungen](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

Nach der Erstellung des virtuellen Computers wird der neue virtuelle Computer unter **Alle Ressourcen** aufgeführt, und im Dashboard wird eine Kachel für den virtuellen Computer angezeigt. Hier werden auch der entsprechende Clouddienst und das Speicherkonto erstellt und aufgelistet. Der virtuelle Computer und der Clouddienst werden automatisch gestartet. Das Verwaltungsportal zeigt ihren Status als **Wird ausgeführt** an.

 ![Konfigurieren Sie den VM-Agent und die Endpunkte des virtuellen Computers.](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
