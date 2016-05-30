##TCP-Einstellungen für virtuelle Azure-Computer

Die Kommunikation virtueller Azure-Computer mit dem öffentlichen Internet erfolgt über [NAT][nat] (Netzwerkadressübersetzung). NAT-Geräte weisen einem virtuellen Azure-Computer eine öffentliche IP-Adresse und einen Port zu, sodass der virtuelle Computer einen Socket für die Kommunikation mit anderen Geräten einrichten kann. Wenn Pakete nach einer bestimmten Zeit nicht mehr durch diesen Socket geleitet werden, beendet das NAT-Gerät die Zuordnung, und der Socket kann von anderen virtuellen Computern verwendet werden.

Dies ist ein allgemeines NAT-Verhalten, das zu Kommunikationsproblemen in TCP-basierten Anwendungen führen kann, in denen erwartet wird, dass ein Socket über einen Timeoutzeitraum hinaus beibehalten wird. Bei Sitzungen mit *bestehender Verbindung* sind zwei Einstellungen für das Leerlauftimeout zu berücksichtigen:

- **Eingehend** und über den [Azure Load Balancer][azure-lb-timeout]. Dieses Timeout ist standardmäßig auf 4 Minuten festgelegt und kann bis hin zu 30 Minuten geändert werden.
- **Ausgehend** unter Verwendung von [SNAT][snat] (Quell-NAT). Dieses Timeout ist auf 4 Minuten festgelegt und kann nicht angepasst werden.

Um sicherzustellen, dass Verbindungen nach dem Timeout nicht getrennt werden, sollten Sie entweder dafür sorgen, dass die Sitzung in der Anwendung aktiv bleibt, oder das zugrunde liegende System so konfigurieren, dass dies gewährleistet ist. Die zu verwendenden Einstellungen unterscheiden sich bei Linux- und Windows-Systemen (siehe unten).

Für [Linux][linux], müssen Sie die folgenden Kernelvariablen ändern: net.ipv4.tcp\_keepalive\_time = 120 net.ipv4.tcp\_keepalive\_intvl = 30 net.ipv4.tcp\_keepalive\_probes = 8
 
Für [Windows][windows] müssen Sie die folgenden Registrierungswerte ändern: KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Mit den oben aufgeführten Einstellungen wird sichergestellt, dass ein Keep Alive-Paket zunächst nach 2 Minuten (120 Sekunden) Leerlaufzeit und anschließend alle 30 Sekunden gesendet wird. Wenn 8 dieser Pakete fehlschlagen, wird die Sitzung abgebrochen.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md