
1. Besuchen Sie das [Azure-Portal]. Klicken Sie auf **Alle durchsuchen** > **Mobile Apps** und dann auf das Back-End, das Sie gerade erstellt haben. Klicken Sie in den Einstellungen der mobilen App auf **Schnellstart** > **Android**. Klicken Sie unter **Konfigurieren Ihrer Clientanwendung** auf **Herunterladen**. Damit laden Sie ein vollständiges Android-Projekt für eine App herunter, das für eine Verbindung mit dem Back-End vorkonfiguriert ist. 
2. Öffnen Sie das Projekt in **Android Studio** mithilfe von **Import project (Eclipse ADT, Gradle, etc.)** (Projekt importieren (Eclipse ADT, Gradle usw.)). Achten Sie auf diese wichtige Importauswahl, um JDK-Fehler zu vermeiden.
3. Klicken Sie auf die Schaltfläche **Run 'app'** , um das Projekt zu erstellen und die App im Android-Simulator zu starten.
4. Geben Sie in der App einen sinnvollen Text wie z.B. *Tutorial fertigstellen* ein, und klicken Sie dann auf die Schaltfläche „Hinzufügen“. Damit wird eine POST-Anforderung an das Azure-Back-End gesendet, das Sie zuvor bereitgestellt haben. Die Back-End fügt Daten aus der Anforderung in die TodoItem-SQL-Tabelle ein und gibt Informationen über die neu gespeicherten Elemente an die mobile App zurück. Die mobile App zeigt diese Daten in der Liste an. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure-Portal]: https://portal.azure.com/


<!--HONumber=Nov16_HO2-->


