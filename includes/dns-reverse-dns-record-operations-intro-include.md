## <a name="what-is-reverse-dns"></a>Was ist Reverse-DNS?

Herkömmliche DNS-Einträge ermöglichen die Zuordnung eines DNS-Namens (Beispiel: www.contoso.com) zu einer IP-Adresse (Beispiel: 64.4.6.100).  Mit Reverse-DNS kann eine IP-Adresse (64.4.6.100) wieder in einen Namen (www.contoso.com) umgewandelt werden.

Reverse-DNS-Einträge werden in vielen Situationen verwendet. Ein gängiger Anwendungsbereich für Reverse-DNS-Einträge ist beispielsweise die Bekämpfung von Spam-E-Mails durch Überprüfung des E-Mail-Absenders.  Der E-Mail-Server, bei dem die Nachricht eingeht, ruft den Reverse-DNS-Eintrag der IP-Adresse des Servers ab, der die Nachricht gesendet hat, und überprüft, ob der Host zum Senden von E-Mails aus der Herkunftsdomäne autorisiert ist. (Beachten Sie jedoch, dass Azure Compute-Dienste das Senden von E-Mails an externe Domänen nicht unterstützen, wie [hier](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/) beschrieben.)

## <a name="how-reverse-dns-works"></a>Funktionsweise von Reverse-DNS

Reverse-DNS-Einträge werden in speziellen DNS-Zonen (so genannten ARPA-Zonen) gehostet.  Diese Zonen bilden parallel zur normalen Hierarchie, in der Domänen wie „contoso.com“ gehostet werden, eine separate DNS-Hierarchie.

Der DNS-Eintrag „www.contoso.com“ wird also beispielsweise unter Verwendung eines DNS-A-Eintrags namens „www“ in der Zone „contoso.com“ implementiert.  Dieser A-Eintrag verweist auf die entsprechende IP-Adresse (in diesem Fall: 64.4.6.100).  Das Reverse-Lookup wird separat implementiert. Hierbei wird ein PTR-Eintrag namens „100“ in der Zone „6.4.64.in-addr.arpa“ verwendet. (Beachten Sie, dass IP-Adressen in ARPA-Zonen umgekehrt werden.)  Bei korrekter Konfiguration verweist dieser PTR-Eintrag auf den Namen „www.contoso.com“.

Wenn einer Organisation ein IP-Adressblock zugewiesen wird, erhält sie auch die Berechtigung zum Verwalten der entsprechenden ARPA-Zone. Die ARPA-Zonen, die den von Azure verwendeten IP-Adressblöcken entsprechen, werden von Microsoft gehostet und verwaltet. Ihr Internetdienstanbieter hostet unter Umständen die ARPA-Zone für Ihre eigenen IP-Adressen für Sie oder ermöglicht es Ihnen, die ARPA-Zone in einem DNS-Dienst Ihrer Wahl zu hosten (beispielsweise in Azure DNS).

> [!NOTE]
> DNS-Forward-Lookups und DNS-Reverse-Lookups werden in separaten, parallelen DNS-Hierarchien implementiert. Das Reverse-Lookup für „www.contoso.com“ wird **nicht** in der Zone „contoso.com“ gehostet, sondern in der ARPA-Zone für den entsprechenden IP-Adressblock.

Weitere Informationen zu Reverse-DNS finden Sie [hier](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure-Unterstützung für Reverse-DNS

Azure unterstützt im Zusammenhang mit Reverse-DNS zwei separate Szenarien:

1. Hosten der entsprechenden ARPA-Zone für Ihren IP-Adressblock
2. Ermöglichen der Konfiguration des Reverse-DNS-Eintrags für die IP-Adresse, die Ihrem Azure-Dienst zugewiesen ist

Zur Unterstützung des ersten Szenarios können Sie mit Azure DNS Ihre ARPA-Zonen hosten und die PTR-Einträge für die einzelnen DNS-Reverse-Lookups verwalten.  Die Vorgehensweise zum Erstellen der ARPA-Zone, zum Einrichten der Delegierung und zum Konfigurieren der PTR-Einträge ist die gleiche wie bei regulären DNS-Zonen.  Einziger Unterschied: Die Delegierung muss über Ihren Internetdienstanbieter und nicht über Ihre DNS-Registrierungsstelle konfiguriert werden, und es darf nur der PTR-Eintragstyp verwendet werden.

Zur Unterstützung des zweiten Szenarios ermöglicht Azure die Konfiguration des Reverse-Lookups für die IP-Adressen, die Ihrem Dienst zugeordnet sind.  Dieses Reverse-Lookup wird von Azure als PTR-Eintrag in der entsprechenden ARPA-Zone konfiguriert.  Diese ARPA-Zonen entsprechen allen von Azure verwendeten IP-Adressbereichen und werden von Microsoft gehostet. **Dieses Szenario wird im weiteren Verlauf des Artikels ausführlich beschrieben.**


<!--HONumber=Nov16_HO3-->


