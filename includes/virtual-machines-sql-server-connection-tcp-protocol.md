1. Suchen Sie nach **Konfigurations-Manager**, während eine Verbindung mit dem virtuellen Computer über Remotedesktop besteht:

    ![SSCM öffnen](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. Erweitern Sie im SQL Server-Konfigurationsmanager im Konsolenbereich **SQL Server-Netzwerkkonfiguration**.

1. Klicken Sie im Konsolenbereich auf **Protokolle für MSSQLSERVER** (der Standardinstanzenname). Klicken Sie im Detailbereich mit der rechten Maustaste auf **TCP**, und klicken Sie auf **Aktivieren**, wenn die Option noch nicht aktiviert ist.

    ![TCP aktivieren](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. Klicken Sie im linken Bereich auf **SQL Server-Dienste**. Klicken Sie im Detailbereich mit der rechten Maustaste auf **SQL Server (*Instanzenname*)** (die Standardinstanz ist **SQL Server (MSSQLSERVER)**), und klicken Sie dann auf **Neu starten**, um die Instanz von SQL Server zu stoppen und neu zu starten.

    ![Datenbankmodul neu starten](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Schließen Sie den SQL Server-Konfigurations-Manager.

Weitere Informationen zur Aktivierung von Protokollen für SQL Server-Datenbankmodule finden Sie unter [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](http://msdn.microsoft.com/library/ms191294.aspx).
