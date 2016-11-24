
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Häufig gestellte Fragen – Reverse-DNS für Ihre Azure zugewiesene IP-Adresse

### <a name="how-much-do-reverse-dns-records-cost"></a>Was kosten Reverse-DNS-Einträge?

Sie sind kostenlos!  Es fallen keine zusätzlichen Kosten für Reverse-DNS-Einträge oder -Abfragen an.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Werden meine Reverse-DNS-Einträge vom Internet aufgelöst?

Ja. Sobald Sie die Reverse-DNS-Eigenschaft für Ihren Clouddienst festlegen, verwaltet Azure alle DNS-Delegierungen und DNS-Zonen, die erforderlich sind, um sicherzustellen, dass Reverse-DNS-Einträge für alle Internetbenutzer auflöst.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Wird für meine Clouddienste ein Standard-Reverse-DNS-Eintrag erstellt?

Nein. Reverse-DNS ist eine optionale Funktion. Es werden keine Standard-DNS-Einträge erstellt, wenn Sie keine konfigurieren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wie sieht das Format für den vollqualifizierten Domänennamen (FQDN) aus?

FQDNs werden aufsteigend angegeben und müssen einen Punkt am Ende haben (z.B. „app1.contoso.com.“).

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Was geschieht bei Fehlern bei den Überprüfungen der Reverse-DNS, die ich angegeben habe?

Wo ein Fehler bei der Überprüfung der Reverse-DNS auftaucht, erfolgt ein Fehler beim Dienstverwaltungsvorgang. Korrigieren Sie bitte den Reverse-DNS-Wert entsprechend den Anforderungen, und versuchen Sie es erneut.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kann ich für meine Azure-Website Reverse-DNS verwalten?

Reverse-DNS wird für Azure Websites nicht unterstützt. Reverse-DNS wird für Azure-PaaS-Rollen und virtuelle IaaS-Computer unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Kann ich für meinen Clouddienst mehrere Reverse-DNS-Einträge konfigurieren?

Nein. Azure unterstützt für jeden Azure-Clouddienst nur einen Reverse-DNS-Eintrag. Jedoch kann jeder Azure-Clouddienst seinen eigenen Reverse-DNS-Eintrag haben.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich aus Azure Compute Services heraus E-Mails an externe Domänen senden?

Nein. [Azure Compute Services unterstützt das Senden von E-Mails an externe Domänen nicht.](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)


<!--HONumber=Nov16_HO3-->


