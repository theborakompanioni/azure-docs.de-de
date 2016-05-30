##Lösungen für hohe Verfügbarkeit mit Azure Traffic Manager

Sie müssen festlegen, ob die Anforderungen an hohe Verfügbarkeit der Workload durch die alleinige Verwendung von Azure Traffic Manager erfüllt werden können oder ob Traffic Manager mit anderen DNS-Lösungen oder Prozessen kombiniert werden muss. Je nach Ihren Anforderungen haben Sie folgende Möglichkeiten:

- **Alleinige Verwendung von Traffic Manager**: Wenn eine 99,99%ige Betriebszeit für Ihre Workload ausreicht, können Sie nur Traffic Manager verwenden. Bei einem Ausfall im Traffic Manager-Dienst können Benutzer erst dann wieder auf Ihre Workload zugreifen, wenn der Traffic Manager-Dienst wiederhergestellt ist.

- **Verwendung einer anderen Lösung zusammen mit Azure Traffic Manager:** Für den Fall eines Ausfalls im Traffic Manager-Dienst können Sie den CNAME-Eintrag so ändern, dass er auf einen anderen Dienst zur Verwaltung des Datenverkehrs verweist. Der Zugriff auf Ihre Workload ist weiterhin verfügbar und wird an alle Standorte verteilt, an denen die Workload gehostet wird. Dies ist die kostenintensivste Lösung, die für Workloads mit höherer SLA jedoch erforderlich sein kann.
