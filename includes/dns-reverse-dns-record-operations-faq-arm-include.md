
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Häufig gestellte Fragen – Reverse-DNS für Ihre Azure zugewiesene IP-Adresse

### <a name="how-much-do-reverse-dns-records-cost"></a>Was kosten Reverse-DNS-Einträge?

Sie sind kostenlos!  Es fallen keine zusätzlichen Kosten für Reverse-DNS-Einträge oder -Abfragen an.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Werden die Reverse-DNS-Einträge für meine Azure zugewiesene öffentliche IP-Adresse aus dem Internet aufgelöst?

Ja. Sobald Sie die Reverse-DNS-Eigenschaft für Ihre öffentliche IP-Adresse festlegen, verwaltet Azure alle DNS-Delegierungen und DNS-Zonen, die erforderlich sind, um sicherzustellen, dass Reverse-DNS-Einträge für alle Internetbenutzer auflöst.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Wird für meine öffentlichen IP-Adressen ein Standard-Reverse-DNS-Eintrag erstellt?

Nein. Reverse-DNS ist eine optionale Funktion. Es werden keine Standard-DNS-Einträge erstellt, wenn Sie keine konfigurieren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wie sieht das Format für den vollqualifizierten Domänennamen (FQDN) aus?

FQDNs werden aufsteigend angegeben und müssen einen Punkt am Ende haben (z.B. „app1.contoso.com.“).

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Was geschieht bei Fehlern bei den Überprüfungen der Reverse-DNS, die ich angegeben habe?

Wo ein Fehler bei der Überprüfung der Reverse-DNS auftaucht, erfolgt ein Fehler beim Dienstverwaltungsvorgang. Korrigieren Sie bitte den Reverse-DNS-Wert entsprechend den Anforderungen, und versuchen Sie es erneut.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kann ich für meine Azure-Website Reverse-DNS verwalten?

Reverse-DNS wird für Azure Websites nicht unterstützt. Reverse-DNS wird für Azure Virtual Machines unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Kann ich für meine öffentliche IP-Adresse mehrere Reverse-DNS-Einträge konfigurieren?

Nein. Azure unterstützt für jede öffentliche IP-Adresse nur einen Reverse-DNS-Eintrag. Jedoch kann jede öffentliche IP-Adresse ihren eigenen Reverse-DNS-Eintrag besitzen.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Kann ich Reverse-DNS-Einträge für eine öffentliche IPv6-Adresse konfigurieren?

Nein.  Zu diesem Zeitpunkt werden Reverse-DNS-Einträge nur für öffentliche IPv4-Adressen unterstützt.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Kann ich einen umgekehrten DNS-Eintrag für meine öffentliche IP-Adresse konfigurieren ohne ein DomainNameLabel anzugeben?

Nein. Um umgekehrte DNS-Einträge für Ihre öffentlichen IP-Adressen zu verwenden, müssen Sie die Eigenschaft „DomainNameLabel“ angeben.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich aus Azure Compute Services heraus E-Mails an externe Domänen senden?

Nein. [Azure Compute Services unterstützt das Senden von E-Mails an externe Domänen nicht.](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)
