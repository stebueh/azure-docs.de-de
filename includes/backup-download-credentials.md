## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Verwenden von Tresoranmeldeinformationen zur Authentifizierung beim Azure Backup-Dienst
Der lokale Server (Windows-Client oder Windows Server oder Data Protection Manager-Server) muss bei einem Sicherungstresor authentifiziert werden, bevor Daten in Azure gesichert werden können. Die Authentifizierung erfolgt mithilfe von "Tresoranmeldeinformationen". Das Konzept der Tresoranmeldeinformationen ähnelt dem Konzept einer Datei mit "Veröffentlichungseinstellungen", die in Azure PowerShell verwendet wird.

### <a name="what-is-the-vault-credential-file"></a>Was ist die Datei mit Tresoranmeldeinformationen?
Die Datei mit Tresoranmeldeinformationen ist ein Zertifikat, das vom Portal für jeden Sicherungstresor generiert wird. Das Portal lädt anschließend den öffentlichen Schlüssel in den Access Control Service (ACS) hoch. Der private Schlüssel des Zertifikats wird dem Benutzer während des Workflows zur Verfügung gestellt und dient als Eingabe für den Workflow zur Computerregistrierung. Dadurch wird der Computer zum Senden von Sicherungsdaten an einen identifizierten Tresor im Azure Backup-Dienst authentifiziert.

Die Tresoranmeldeinformationen werden nur während des Registrierungsworkflows verwendet. Es ist Aufgabe des Benutzers sicherzustellen, dass die Datei mit den Tresoranmeldeinformationen sicher aufbewahrt wird. Fällt sie in die Hände eines böswilligen Benutzers, kann dieser die Datei mit den Tresoranmeldeinformationen zur Registrierung weiterer Computer beim selben Tresor verwenden. Da die Sicherungsdaten jedoch durch eine Passphrase verschlüsselt sind, die dem Kunden gehört, sind vorhandene Sicherungsdaten nicht gefährdet. Um dieses Risiko auf ein Mindestmaß zu verringern, laufen die Tresoranmeldeinformationen nach 48 Stunden ab. Sie können die Tresoranmeldeinformationen beliebig oft von einem Sicherungstresor herunterladen – jedoch nur die neueste Datei mit Tresoranmeldeinformationen ist für den Registrierungsworkflow gültig.

### <a name="download-the-vault-credential-file"></a>Herunterladen der Datei mit Tresoranmeldeinformationen
Die Datei mit Tresoranmeldeinformationen wird über einen sicheren Kanal aus dem Azure-Portal heruntergeladen. Der Azure Backup-Dienst kennt nicht den privaten Schlüssel des Zertifikats, und der private Schlüssel wird weder im Portal noch im Dienst aufbewahrt. Gehen Sie folgendermaßen vor, um die Datei mit Tresoranmeldeinformationen auf einen lokalen Computer herunterzuladen.

1. Melden Sie sich beim [Verwaltungsportal](https://manage.windowsazure.com/)
2. Klicken Sie im linken Navigationsbereich auf **Recovery Services** , und wählen Sie den Sicherungstresor aus, den Sie erstellt haben. Klicken Sie auf das Cloudsymbol, um die Ansicht "Schnellstart" des Sicherungstresors aufzurufen.
   
   ![Schnellansicht](./media/backup-download-credentials/quickview.png)
3. Klicken Sie auf der Seite "Schnellstart" auf **Tresoranmeldeinformationen herunterladen**. Das Portal generiert die Datei mit Tresoranmeldeinformationen, die zum Download zur Verfügung gestellt wird.
   
   ![Download](./media/backup-download-credentials/downloadvc.png)
4. Das Portal generiert Tresoranmeldeinformationen mit einer Kombination aus dem Tresornamen und dem aktuellen Datum. Klicken Sie auf **Speichern**, um die Tresoranmeldeinformationen in den Downloadordner des lokalen Kontos herunterzuladen. Sie können auch im Menü „Speichern“ die Option „Speichern unter“ auswählen, um einen anderen Speicherort anzugeben.

### <a name="note"></a>Hinweis
* Stellen Sie sicher, dass die Tresoranmeldeinformationen an einem Ort gespeichert werden, der von Ihrem Computer aus zugänglich ist. Wenn sie in einer Dateifreigabe/einem SMB gespeichert sind, überprüfen Sie die Zugriffsberechtigungen.
* Die Datei mit den Tresoranmeldeinformationen wird nur während des Registrierungsworkflows verwendet.
* Die Datei mit den Tresoranmeldeinformationen läuft nach 48 Stunden ab und kann über das Portal heruntergeladen werden.
* Bei Fragen zum Workflow finden Sie Informationen in den [häufig gestellten Fragen](../articles/backup/backup-azure-backup-faq.md) zu Azure Backup.



<!--HONumber=Nov16_HO3-->


