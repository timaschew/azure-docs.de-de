---
title: Erstellen einer Java Enterprise-Web-App unter Linux – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Java-Unternehmens-App für Wildfly in Azure App Service unter Linux funktionsfähig machen.
author: JasonFreeberg
manager: routlaw
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 11/13/2018
ms.author: jafreebe
ms.custom: seodec18
ms.openlocfilehash: 2a38b117cde6d6b51b101f6b0ef0eb3a19b42ba3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344226"
---
# <a name="tutorial-build-a-java-ee-and-postgres-web-app-in-azure"></a>Tutorial: Erstellen einer Java EE- und Postgres-Web-App in Azure

In diesem Tutorial erfahren Sie, wie Sie in Azure App Service eine Java Enterprise Edition (EE)-Web-App erstellen und mit einer Postgres-Datenbank verbinden. Anschließend besitzen Sie eine [WildFly](https://www.wildfly.org/about/)-Anwendung, die Daten in [Azure-Datenbank für Postgres](https://azure.microsoft.com/services/postgresql/) speichert und mit Azure [App Service für Linux](app-service-linux-intro.md) ausgeführt wird.

In diesem Lernprogramm lernen Sie Folgendes:
> [!div class="checklist"]
> * Bereitstellen einer Java EE-App in Azure mithilfe von Maven
> * Erstellen einer Postgres-Datenbank in Azure
> * Konfigurieren der WildFly-Server für die Verwendung von Postgres
> * Aktualisieren und erneutes Bereitstellen der App
> * Ausführen von Komponententests für WildFly

## <a name="prerequisites"></a>Voraussetzungen

1. [Herunterladen und Installieren von Git](https://git-scm.com/)
2. [Herunterladen und Installieren von Maven 3](https://maven.apache.org/install.html)
3. [Herunterladen und Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="clone-and-edit-the-sample-app"></a>Klonen und Bearbeiten der Beispiel-App

In diesem Schritt klonen Sie die Beispielanwendung und konfigurieren das Maven-Projektobjektmodell (POM oder „pom.xml“) für die Bereitstellung.

### <a name="clone-the-sample"></a>Klonen des Beispiels

Navigieren Sie im Terminalfenster zu einem Arbeitsverzeichnis, und klonen Sie [das Beispielrepository](https://github.com/Azure-Samples/wildfly-petstore-quickstart).

```bash
git clone https://github.com/Azure-Samples/wildfly-petstore-quickstart.git
```

### <a name="update-the-maven-pom"></a>Aktualisieren der Maven-POM-Datei

Aktualisieren Sie die Maven-POM-Datei mit dem gewünschten Namen und der Ressourcengruppe Ihrer App Service-Instanz. Diese Werte werden in das Azure-Plug-In eingefügt, das sich weiter unten in der Datei _pom.xml_ befindet. Sie müssen den App Service-Plan oder die Instanz nicht im Voraus erstellen. Das Maven-Plug-in erstellt die Ressourcengruppe und die App Service-Instanz, wenn sie nicht bereits vorhanden ist.

Sie können nach unten zum `<plugins>`-Abschnitt von _pom.xml_ scrollen, um das Azure-Plugin zu begutachten. Der Abschnitt der Konfiguration `<plugin>` in der Datei _pom.xml_ für „azure-webapp-maven-plugin“ muss die folgende Konfiguration enthalten:

```xml
      <!--*************************************************-->
      <!-- Deploy to WildFly in App Service Linux           -->
      <!--*************************************************-->
 
      <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.5.0</version>
        <configuration>
 
          <!-- Web App information -->
          <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
          <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
          <appName>${WEBAPP_NAME}</appName>
          <region>${REGION}</region>
 
          <!-- Java Runtime Stack for Web App on Linux-->
          <linuxRuntime>wildfly 14-jre8</linuxRuntime>
 
        </configuration>
      </plugin>
```

Ersetzen Sie die Platzhalter durch Ihre gewünschten Ressourcennamen:
```xml
<azure.plugin.appname>YOUR_APP_NAME</azure.plugin.appname>
<azure.plugin.resourcegroup>YOUR_RESOURCE_GROUP</azure.plugin.resourcegroup>
```


## <a name="build-and-deploy-the-application"></a>Erstellen und Bereitstellen der Anwendung

Wir verwenden jetzt Maven, um unsere Anwendung zu erstellen und in App Service bereitzustellen.

### <a name="build-the-war-file"></a>Erstellen der WAR-Datei

Das POM in diesem Projekt ist so konfiguriert, dass die Anwendung in eine Webarchivdatei (WAR) gepackt wird. Erstellen der Anwendung mithilfe von Maven:

```bash
mvn clean install -DskipTests
```

Die Testfälle in dieser Anwendung sollen ausgeführt werden, wenn die Anwendung auf WildFly bereitgestellt wird. Wir überspringen die Tests, um lokal zu erstellen, und Sie führen die Tests aus, sobald die Anwendung in App Service bereitgestellt ist.

### <a name="deploy-to-app-service"></a>Bereitstellen für App Service

Da die WAR-Datei jetzt bereit ist, können wir mithilfe des Azure-Plug-Ins in App Service bereitstellen:

```bash
mvn azure-webapp:deploy
```

Wenn die Bereitstellung abgeschlossen ist, fahren Sie mit dem nächsten Schritt fort.

### <a name="create-a-record"></a>Erstellen eines Datensatzes

Öffnen Sie einen Browser, und navigieren Sie zu `https://<your_app_name>.azurewebsites.net/`. Herzlichen Glückwunsch! Sie haben eine Java EE-Anwendung in Azure App Service bereitgestellt.

An diesem Punkt verwendet die Anwendung eine In-Memory Database (H2). Klicken Sie auf der Navigationsleiste auf „Administrator“, und erstellen Sie eine neue Kategorie. Der Datensatz in Ihrer In-Memory Database geht verloren, wenn Sie Ihre App Service-Instanz neu starten. In den folgenden Schritten beheben Sie dieses Problem, indem Sie eine Postgres-Datenbank in Azure bereitstellen und WildFly für deren Verwendung konfigurieren.

![Position der Administrator-Schaltfläche](media/tutorial-java-enterprise-postgresql-app/admin_button.JPG)

## <a name="provision-a-postgres-database"></a>Bereitstellen einer Postgres-Datenbank

Zum Bereitstellen eines Postgres-Datenbankservers öffnen Sie ein Terminal und führen den folgenden Befehl mit Ihren gewünschten Werten für Servername, Benutzername, Kennwort und Speicherort aus. Verwenden Sie die Ressourcengruppe, in der sich auch Ihre App Service-Instanz befindet. Notieren Sie Ihr Kennwort zur späteren Verwendung.

```bash
az postgres server create -n <desired-name> -g <same-resource-group> --sku-name GP_Gen4_2 -u <desired-username> -p <desired-password> -l <location>
```

Navigieren Sie zum Portal, und suchen Sie nach Ihrer Postgres-Datenbank. Wenn das Blatt geladen ist, kopieren Sie die Werte von „Servername“ und „Anmeldename des Serveradministrators“. Diese benötigen Sie später.

### <a name="allow-access-to-azure-services"></a>Zugriff auf Azure-Dienste zulassen

Im Bereich **Verbindungssicherheit** des Blatts „Azure-Datenbank“ setzen Sie die Schaltfläche „Zugriff auf Azure-Dienste zulassen“ auf die Position **EIN**.

![Zulassen des Zugriffs auf Azure-Dienste](media/tutorial-java-enterprise-postgresql-app/postgress_button.JPG)

## <a name="update-your-java-app-for-postgres"></a>Aktualisieren Ihrer Java-App für Postgres

Wir nehmen jetzt einige Änderungen an der Java-Anwendung vor, damit sie unsere Postgres-Datenbank verwenden kann.

### <a name="add-postgres-credentials-to-the-pom"></a>Hinzufügen von Postgres-Anmeldeinformationen in der POM-Datei

Ersetzen Sie in _pom.xml_ die Platzhalterwerte durch Ihren Postgres-Servernamen, den Anmeldenamen und das Kennwort des Administrators. Diese Werte werden als Umgebungsvariablen in Ihrer App Service-Instanz eingefügt, wenn Sie die Anwendung erneut bereitstellen.

```xml
<azure.plugin.postgres-server-name>SERVER_NAME</azure.plugin.postgres-server-name>
<azure.plugin.postgres-username>USERNAME@FIRST_PART_OF_SERVER_NAME</azure.plugin.postgres-username>
<azure.plugin.postgres-password>PASSWORD</azure.plugin.postgres-password>
```

### <a name="update-the-java-transaction-api"></a>Aktualisieren der Java-Transaktions-API

Als Nächstes müssen wir die Konfiguration unserer Java-Transaktions-API so bearbeiten, dass unsere Java-Anwendung mit Postgres anstelle der In-Memory H2-Database kommuniziert, die wir zuvor verwendet haben. Öffnen Sie _src/main/resources/META-INF/persistence.xml_ in einem Editor. Ersetzen Sie den Wert für `<jta-data-source>` durch `java:jboss/datasources/postgresDS`. Ihr JTA-XML sollte jetzt diese Einstellung aufweisen:

```xml
...
<jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
...
```

## <a name="configure-the-wildfly-application-server"></a>Konfigurieren des WildFly-Anwendungsservers

Vor der Bereitstellung der neu konfigurierten Anwendung müssen wir den WildFly-Anwendungsserver mit dem Postgres-Modul und den zugehörigen Abhängigkeiten aktualisieren. Zum Konfigurieren des Servers benötigen wir die vier Dateien im Verzeichnis `wildfly_config/`:

- **postgresql-42.2.5.jar:** Diese JAR-Datei ist der JDBC-Treiber für Postgres. Weitere Informationen finden Sie auf der [offiziellen Website](https://jdbc.postgresql.org/index.html).
- **postgres-module.xml:** Diese XML-Datei deklariert einen Namen für das Postgres-Modul (org.postgres). Sie gibt auch die erforderlichen Ressourcen und Abhängigkeiten für die Verwendung des Moduls an.
- **jboss_cli_commands.cl:** Diese Datei enthält Konfigurationsbefehle, die von der JBoss CLI ausgeführt werden. Die Befehle fügen das Postgres-Modul dem WildFly-Anwendungsserver hinzu, stellen die Anmeldeinformationen bereit, deklarieren einen JNDI-Namen, legen den Schwellenwert für das Zeitlimit fest usw. Wenn Sie nicht mit der JBoss CLI vertraut sind, informieren Sie sich in der [offiziellen Dokumentation](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- **startup_script.sh:** Dieses Shellskript wird bei jedem Starten Ihrer App Service-Instanz ausgeführt. Das Skript führt nur eine Funktion aus: Übergeben der Befehle in `jboss_cli_commands.cli` an die JBoss-CLI.

Es wird dringend empfohlen, die Inhalte dieser Dateien zu lesen, insbesondere _jboss_cli_commands.cli_.

### <a name="ftp-the-configuration-files"></a>Übertragen der Konfigurationsdateien per FTP

Wir müssen die Inhalte von `wildfly_config/` per FTP an unsere App Service-Instanz übertragen. Zum Abrufen Ihrer FTP-Anmeldeinformationen klicken Sie im Azure-Portal auf dem Blatt „App Service“ auf die Schaltfläche **Veröffentlichungsprofil abrufen**. Ihr FTP-Benutzername und das Kennwort sind im heruntergeladenen XML-Dokument enthalten. Weitere Informationen zum Veröffentlichungsprofil finden Sie in [diesem Dokument](https://docs.microsoft.com/azure/app-service/app-service-deployment-credentials).

Übertragen Sie die vier Dateien in `wildfly_config/` mithilfe eines FTP-Tools Ihrer Wahl an `/home/site/deployments/tools/`. (Hinweis: Übertragen Sie nicht das Verzeichnis, nur die Dateien selbst.)


### <a name="finalize-app-service"></a>Abschließen von App Service

Navigieren Sie auf dem Blatt „App Service“ zum Bereich „Anwendungseinstellungen“. Legen Sie unter „Runtime“ das Feld „Startdatei“ auf `/home/site/deployments/tools/startup_script.sh` fest. Dadurch wird sichergestellt, dass das Shellskript nach der Erstellung der App Service-Instanz, aber vor dem Starten des WildFly-Server ausgeführt wird.

Starten Sie dann Ihre App Service-Instanz neu. Die Schaltfläche befindet sich im Bereich „Übersicht“.

## <a name="redeploy-the-application"></a>Erneutes Bereitstellen der Anwendung

Öffnen Sie ein Terminalfenster, um Ihre Anwendung neu zu erstellen und erneut bereitzustellen.

```bash
mvn clean install -DskipTests azure-webapp:deploy
```

Glückwunsch! Ihre Anwendung verwendet nun eine Postgres-Datenbank, und alle in der Anwendung erstellten Datensätze werden in Postgres anstatt wie zuvor in der H3-In-Memory Database gespeichert. Um dies zu überprüfen, können Sie einen Datensatz erstellen und Ihre App Service-Instanz neu starten. Die Datensätze sind nach einem Neustart Ihrer Anwendung immer noch vorhanden.

## <a name="clean-up"></a>Bereinigen

Wenn Sie diese Ressourcen nicht für ein anderes Tutorial benötigen (siehe „Nächste Schritte“), können Sie sie löschen, indem Sie den folgenden Befehl ausführen:

```bash
az group delete --name <your_resource_group> 
```

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt eine Java EE-Anwendung in App Service bereitgestellt haben, lesen Sie das [Java Enterprise-Entwicklerhandbuch](https://aka.ms/wildfly-quickstart), um weitere Informationen zum Einrichten von Diensten, zur Problembehandlung und zur Skalierung Ihrer Anwendung zu erhalten.