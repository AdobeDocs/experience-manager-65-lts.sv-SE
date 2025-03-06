---
title: Konfigurera Connector for EMC Documentum
description: Lär dig hur du konfigurerar anslutningsprogrammet för EMC Documentum så att det går att kommunicera mellan AEM-formulär och EMC Documentum.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/connecting_to_a_content_management_system
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms
exl-id: a9eae40f-531f-4354-ade0-853e84dd88d9
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1032'
ht-degree: 0%

---

# Konfigurera Connector for EMC Documentum {#configuring-connector-for-emc-documentum}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Koppling för EMC Documentum möjliggör kommunikation mellan AEM-formulär och EMC Documentum. Mer bakgrundsinformation finns i &quot;Connectors for ECM&quot; i [Services Reference](https://www.adobe.com/go/learn_aemforms_services_63).

När du konfigurerar Connector för EMC Documentum måste du konfigurera serveranslutningen och databasautentiseringsuppgifterna.

>[!NOTE]
>
>I tidigare versioner kunde resurserna lagras i en ECM-databas. I den aktuella versionen lagras resurserna i AEM-formulärens inbyggda databas och databasleverantörens tjänster har tagits bort. När du uppgraderar till AEM-formulär migreras resurser från en ECM-databas till AEM-formulärdatabasen. Mer information finns i uppgraderingsguiden för AEM-formulär för programservern.

## Konfigurera serveranslutningen {#configuring-the-server-connection}

I det här avsnittet beskrivs de uppgifter för Connector for EMC Documentum som du kan utföra på sidan Konfigurationsinställningar.

>[!NOTE]
>
>Om du konfigurerar alla inställningar samtidigt behöver du bara klicka på Spara en gång.

### Konfigurera servern {#configure-the-server}

Du måste konfigurera serverinformation för anslutningsutjämning. Den här informationen är nödvändig för att ansluta till Documentum-innehållsdatabaser och starta Connector for EMC Documentum.

1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Konfigurationsinställningar.
1. Ange värdnamnet eller IP-adressen och portnumret för anslutningsförmedlaren i området Documentum Configuration Information. Portnumret måste vara ett positivt heltal (till exempel 1489).
1. Klicka på Spara.

### Konfigurera huvudautentiseringsuppgifter {#configure-principal-credentials}

När du konfigurerar de huvudsakliga inloggningsuppgifterna beror det databasnamn du anger på om ett explicit databasnamn anges under inloggningen.

Om du anger fel användarnamn eller lösenord får du följande resultat, beroende på om tjänsten körs för närvarande:

* Om både EMC Documentum-databasprovidertjänsten och EMC Documentum Content Repository Connector-tjänsten stoppas visas inget fel när du sparar tjänstens konfigurationsinformation. Nästa gång du startar tjänsten genereras dock ett undantag och tjänsten kommer inte att starta.
* Om antingen EMC Documentum Repository Provider-tjänsten eller EMC Documentum Content Repository Connector-tjänsten startas försöker tjänsten omedelbart validera inloggningsinformationen när du sparar tjänstens konfigurationsinformation. I det här fallet inträffar ett fel och konfigurationsinformationen sparas inte.

1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Konfigurationsinställningar.
1. Ange användarnamnet och lösenordet för en användare med superadministratörsbehörighet i området Information om Documentum Principal Credentials.
1. Om inget explicit databasnamn anges vid inloggning anger du det databasnamn som autentiseringsuppgifterna är kopplade till.
1. Klicka på Spara.

### Ändra databasens tjänstleverantör {#change-the-repository-service-provider}

Du kan konfigurera vilken databastjänstleverantör som ska användas med Documentum. Databastjänstanrop delegeras till providern som du konfigurerar. Följande alternativ är tillgängliga:

**Aktuellt namn på databastjänstleverantör:** Namnet på den aktuella databastjänstprovidern

**ECM Documentum-databasprovider:** gör Documentum-databasprovidern till databasprovidern. Det här alternativet har tagits bort

**databasprovider:** Gör den inbyggda databasprovidern till databasprovidern

>[!NOTE]
>
>Om du vill välja en annan databastjänstleverantör än de som anges konfigurerar du RepositoryService i Program och tjänster > Tjänsthantering. <!-- Fix broken link (See Managing Services) -->.

1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Konfigurationsinställningar.
1. Välj den alternativa databastjänstleverantören i informationsfältet för databastjänstleverantör.
1. Klicka på Spara.

## Konfigurerar databasinloggningsuppgifter {#configuring-repository-credentials}

Documentum-inloggningsinformationen används i systemkontexten för AEM-formulär. Databasens autentiseringsuppgifter är specifika för vissa databaser i Documentum. Du kan ange autentiseringsuppgifter för ett valfritt antal databaser, men du kan bara ange en uppsättning uppgifter per databas.

### Lägg till en databasautentiseringsuppgift {#add-a-repository-credential}

1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Inställningar för databasautentiseringsuppgifter.
1. Klicka på Lägg till. Sidan Documentum System Credentials Information visas.
1. Ange ett namn för databasen.
1. Ange ett användarnamn och lösenord.
1. Klicka på Spara.

Om Content Repository Connector för EMC Documentum-tjänsten och/eller Repository Service för EMC Documentum körs kontrolleras inloggningsinformationen mot den angivna databasen innan den lagras i databasen. Om autentiseringsuppgifterna är ogiltiga eller finns visas ett felmeddelande.

### Ta bort databasautentiseringsuppgifter {#remove-a-repository-credential}

1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Konfigurationsinställningar.
1. Markera kryssrutan bredvid databasen som du vill ta bort uppgifter från och klicka på Ta bort. Autentiseringsuppgifterna för den valda databasen tas bort från databasen.

### Ändra användarnamn och lösenord för en databasreferens {#change-the-user-name-and-password-for-a-repository-credential}

1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Konfigurationsinställningar.
1. Klicka på namnet på databasen som du vill redigera inloggningsuppgifter för.
1. Ändra databasens användarnamn eller lösenord, eller både och. (Databasnamnet är skrivskyddat.)
1. Klicka på Spara.

Om Content Repository Connector för EMC Documentum-tjänsten och/eller Repository Service för EMC Documentum körs kontrolleras inloggningsinformationen mot den angivna databasen innan den lagras i databasen. Om autentiseringsuppgifterna är ogiltiga eller finns visas ett felmeddelande.

## Aktivera begäran om delning av Workspace-uppgiftsköer {#enable-the-request-for-sharing-of-workspace-task-queues}

Vissa manuella steg krävs för att se till att funktionen för begäran om delning av uppgiftskön i Workspace fungerar korrekt med Connector for EMC Documentum.

1. När AEM-formulär har distribuerats och Workbench har installerats loggar du in på Workbench och öppnar resursvyn. I den här vyn avgör du var filen QueueSharing.swf finns.
1. Dra filen QueueSharing.swf från resursvyn till skrivbordet i Windows eller en motsvarande plats, beroende på operativsystem.
1. I administrationskonsolen klickar du på Tjänster > Koppling för EMC Documentum > Konfigurationsinställningar.
1. Under Information om databasleverantör ändrar du den konfigurerade databasprovidern till EMC Documentum-databasprovidern.
1. Starta Workbench och kopiera filen QueueSharing.swf från den plats där du ursprungligen kopierade den (till exempel skrivbordet i Windows eller någon annan plats) till en befintlig katalog i EMC Documentum-databasen.
1. Öppna processen Ködelning i vyn Workbench Processes.
1. I variabelvyn öppnar du egenskaperna för variabeln&quot;theForm&quot; och ändrar URI:n så att den matchar sökvägen där du placerade filen QueueSharing.swf i steg 5.
1. Spara processen.
1. Migrera processen till produktionsmiljön enligt organisationens policy.
