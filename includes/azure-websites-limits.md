| Ressource | Kostenlos | Shared (Vorschau) | Basic | Standard | Premium (Vorschau)</th> |
| --- | --- | --- | --- | --- | --- |
| [Web-Apps, mobile Apps oder API-Apps](https://azure.microsoft.com/services/app-service/) pro [App Service-Plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Unbegrenzt<sup>2</sup> |Unbegrenzt<sup>2</sup> |Unbegrenzt<sup>2</sup> |
| [Logik-Apps](https://azure.microsoft.com/services/app-service/logic/) pro [App Service-Plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |20 pro Kern |20 pro Kern |
| [App Service-Plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |1 pro Region |10 pro Ressourcengruppe |100 pro Ressourcengruppe |100 pro Ressourcengruppe |100 pro Ressourcengruppe |
| Serverinstanztyp |Shared
 |Shared
 |Dediziert<sup>3</sup> |Dediziert<sup>3</sup> |Dediziert<sup>3</sup></p> |
| [Horizontal skalieren](../articles/app-service-web/web-sites-scale.md) (max. Instanzen) |1 freigegebene |1 freigegebene |3 dedizierte<sup>3</sup> |10 dedizierte<sup>3</sup> |20 dedizierte (50 in ASE)<sup>3,4</sup> |
| Speicher<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| CPU-Zeit (5 min)<sup>6</sup> |3 Minuten |3 Minuten |Unbegrenzt, normal zu [bezahlen](https://azure.microsoft.com/pricing/details/app-service/)</a> |Unbegrenzt, normal zu bezahlen |Unbegrenzt, normal zu bezahlen |
| CPU-Zeit (Tag)<sup>6</sup> |60 Minuten |240 Minuten |Unbegrenzt, normal zu [bezahlen](https://azure.microsoft.com/pricing/details/app-service/)</a> |Unbegrenzt, normal zu bezahlen |Unbegrenzt, normal zu bezahlen |
| Arbeitsspeicher (1 Stunde) |1024 MB pro App Service-Plan |1024 MB pro App |N/V |N/V |N/V |
| Bandbreite |165 MB |Unbegrenzt, Preise für [Dateiübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/) gelten |Unbegrenzt, Preise für Dateiübertragungen gelten |Unbegrenzt, Preise für Dateiübertragungen gelten |Unbegrenzt, Preise für Dateiübertragungen gelten |
| Anwendungsarchitektur |32 Bit |32 Bit |32 Bit/64 Bit |32 Bit/64 Bit |32 Bit/64 Bit |
| Websockets pro Instanz<sup>7</sup> |5 |35 |350 |Unbegrenzt |Unbegrenzt |
| Gleichzeitige [Debuggerverbindungen](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) pro Anwendung |1 |1 |1 |5 |5 |
| [azurewebsites.net-Unterdomäne mit FTP/S und SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md) |X |X |X |X |X |
| [benutzerdefinierte Domäne](../articles/app-service-web/web-sites-custom-domain-name.md)  | |X |X |X |X |
| benutzerdefinierte Domäne [SSL-Unterstützung](../articles/app-service-web/web-sites-configure-ssl-certificate.md) | | |Unbegrenzt |Unbegrenzt, 5 SNI SSL-Verbindungen und 1 IP-SSL-Verbindung inbegriffen |Unbegrenzt, 5 SNI SSL-Verbindungen und 1 IP-SSL-Verbindung inbegriffen |
| Integrierter Lastenausgleich | |X |X |X |X |
| [Immer aktiviert](../articles/app-service-web/web-sites-configure.md) | | |X |X |X |
| [Geplante Sicherungen](../articles/app-service-web/web-sites-backup.md) | | | |Einmal pro Tag |Alle 5 Minuten<sup>8</sup> |
| [Automatische Skalierung](../articles/app-service-web/web-sites-scale.md) | | |X |X |X |
| [WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md)<sup>9</sup> |X |X |X |X |X |
| [Azure Scheduler](https://azure.microsoft.com/services/scheduler/) -Unterstützung: | |X |X |X |X |
| [Endpunktüberwachung](../articles/app-service-web/web-sites-monitor.md) | | |X |X |X |
| [Stagingslots](../articles/app-service-web/web-sites-staged-publishing.md) | | | |5 |20 |
| Benutzerdefinierte Domänen pro App</a> | |500 |500 |500 |500 |
| SLA | |<p> |99,9 % |99,95 %<sup>10</sup> |99,95 %<sup>10</sup> |

<sup>1</sup>Sofern nicht anderweitig vermerkt, gelten Apps- und Speicherkontingente gemäß dem App Service-Plan.  
<sup>2</sup>Die tatsächliche Anzahl der Apps, die Sie auf diesen Computern hosten können, hängt von der Aktivität der Apps, der Größe der Computerinstanzen und der entsprechenden Ressourcenauslastung ab.  
<sup>3</sup>Dedizierte Instanzen können unterschiedliche Größen aufweisen. Weitere Informationen finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/).  
<sup>4</sup>Im Premium-Tarif sind bei Verwendung von App Service-Umgebungen bis zu 50 Serverinstanzen (sofern verfügbar) und 500 GB Speicherplatz zulässig, andernfalls 20 Serverinstanzen und 250 GB Speicherplatz.  
<sup>5</sup>Die Speicherbegrenzung ist die gesamte Inhaltsgröße aller Apps im gleichen App Service-Plan. Weitere Speicheroptionen sind unter [App Service-Umgebung](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage) verfügbar.  
<sup>6</sup>Diese Ressourcen werden durch physische Ressourcen auf den dedizierten Instanzen (die Instanzengröße und die Anzahl der Instanzen) beschränkt.  
<sup>7</sup>Wenn Sie eine App im Basic-Tarif auf zwei Instanzen skalieren, stehen Ihnen 350 gleichzeitige Verbindungen für jede der beiden Instanzen zur Verfügung.  
<sup>8</sup>Im Premium-Tarif sind bei Verwendung von App Service-Umgebungen Sicherungsintervalle von bis zu alle 5 Minuten möglich, andernfalls 50 Sicherungen pro Tag.  
<sup>9</sup>Sie können benutzerdefinierte ausführbare Dateien und/oder Skripts bei Bedarf, nach Zeitplan oder fortwährend als Hintergrundaufgabe in Ihrer App Service-Instanz ausführen. Für die fortlaufende WebJobs-Ausführung ist „Immer bereit“ erforderlich. Für geplante WebJobs ist Azure Scheduler Free oder Standard erforderlich. Es gibt keinen vordefinierten Grenzwert für die Anzahl der WebJobs, die in einer App Service-Instanz ausgeführt werden können. Aber es gibt praktische Grenzwerte, die sich danach richten, was mit dem Anwendungscode erreicht werden soll.   
<sup>10</sup>Die SLA von 99,95 % gilt für Bereitstellungen mit mehreren Instanzen, bei denen Azure Traffic Manager für Failover konfiguriert ist.  



<!--HONumber=Feb17_HO1-->


