---
title: Importera och hantera arkiv
description: Lär dig importera och hantera arkiv. Arkiverar import och hantering av LCA som skapats i Workbench. Du kan importera, konfigurera, använda och ta bort ett arkiv.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/importing_and_managing_applications_and_archives
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: a42d1094-293e-4f1a-a8f7-376dc8d34807
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1450'
ht-degree: 0%

---

# Importera och hantera arkiv {#import-and-manage-archives}

Använd fliken Arkiv för att importera och hantera LCA:er som har skapats i Workbench.

## Importera ett arkiv {#import-an-archive}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering och sedan på fliken Aktiviteter.
1. Klicka på Importera.
1. Klicka på Bläddra för att leta reda på arkivet som ska importeras och klicka sedan på Förhandsgranska.
1. Granska listan över resurser och objekt som ska installeras med arkivet. Kontrollera att det inte finns några konflikter med befintliga resurser, objekt och tjänstkonfigurationer eftersom det inte finns någon ångra-funktion tillgänglig.

   Om du väljer att importera tjänstkonfigurationerna importerar AEM-formulär alla processkonfigurationsfiler (slutpunkter, säkerhetsprofiler och tjänstkonfigurationsparametrar) som används av processerna i LCA.

1. Klicka på Importera.
1. Granska importresultaten och klicka antingen på Hoppa över konfiguration för att slutföra importprocessen eller klicka på Konfigurera för att konfigurera arkivet.

   >[!NOTE]
   >
   >Om du klickar på Hoppa över konfiguration kan du konfigurera arkivet senare.

1. Om du klickar på Konfigurera visas sidan Konfigurera slutpunkter där du kan göra de ändringar du behöver:

   * Om du vill byta namn på en slutpunkt eller redigera dess beskrivning klickar du på den.
   * Om du vill lägga till en Task Manager-slutpunkt klickar du på Lägg till TaskManager. Mer information om inställningar för Aktivitetshanteraren finns i [Konfigurera slutpunkter för Aktivitetshanteraren](/help/forms/using/admin-help/configuring-task-manager-endpoints.md#configuring-task-manager-endpoints).
   * Om du vill lägga till en bevakad mappslutpunkt klickar du på Lägg till bevakad mapp. Mer information om inställningarna för bevakad mapp finns i [Inställningar för bevakad mapp](/help/forms/using/admin-help/configuring-watched-folder-endpoints.md#watched-folder-endpoint-settings).
   * Om du vill lägga till en e-postslutpunkt klickar du på Lägg till e-post. Mer information om e-postinställningarna finns i [Inställningar för e-postslutpunkt](/help/forms/using/admin-help/configuring-email-endpoints.md#email-endpoint-settings).
   * Om du vill lägga till en EJB-slutpunkt klickar du på Lägg till EJB och anger ett namn och en beskrivning för slutpunkten.
   * Om du vill lägga till en SOAP-slutpunkt klickar du på Lägg till SOAP och anger ett namn och en beskrivning för slutpunkten.
   * Om du vill lägga till en fjärrslutpunkt klickar du på Lägg till fjärranslutning. Mer information om fjärrinställningar finns i [Fjärrinställningar för slutpunkter](/help/forms/using/admin-help/configuring-remoting-endpoints.md#remoting-endpoint-settings).
   * Om du vill lägga till en REST-slutpunkt klickar du på Lägg till REST och anger ett namn och en beskrivning för slutpunkten. Anteckna den URL för REST-anrop som visas på sidan Lägg till REST-slutpunkt.
   * Om du vill ta bort en slutpunkt markerar du kryssrutan bredvid den och klickar på Ta bort.

1. Klicka på Nästa.
1. Om en process eller tjänst i LCA har konfigurationsparametrar visas sidan Konfigurera parametrar där du konfigurerar tjänstparametrarna och klickar på Nästa.
1. Gör de ändringar du behöver på sidan Konfigurera säkerhetsprofil:

   * **Kräv att anropare autentiserar:** Den här inställningen anger om tjänsten kan anropas med eller utan autentiseringsuppgifter.

     Om *Anropare för närvarande krävs för att autentisera* visas, måste anroparen till tjänsten vara autentiserad och användarens huvudnamn för den anroparen måste vara auktoriserat för att kunna anropa tjänsten. I annat fall nekas anropsförsöket. Klicka på Tillåt oautentiserade anropare för att ta bort behovet av autentisering.

     Om *Anropare inte behöver autentisera* visas behöver inte tjänstens anropare autentiseras. Anropet av tjänsten lyckas alltid eftersom det inte finns någon auktoriseringskontroll. Klicka på Kräv att anropare autentiserar för att begära autentisering.

   * **Kör som:** Anger den körningsidentitet som används av en tjänst efter att den har anropats. Om du vill ändra det här alternativet klickar du på Ändra. Välj bland följande alternativ:

     **Ospecificerad:** Standardbeteendet används.

     **Anroparen:** Använder samma identitet som användaren som anropade tjänsten.

     **System:** Kör tjänsten med fullständig behörighet. Det här är standardinställningen för långvariga processer.

     **Namngiven användare:** Gör att du kan köra tjänsten som en specifik användare. Det här är standardinställningen för kortlivade processer. När du väljer det här alternativet klickar du på Välj användare för att visa sidan Välj huvudnamn, där du kan söka efter och välja användaren.

   * Om du vill lägga till ett huvudnamn i säkerhetsprofilen klickar du på Lägg till huvudnamn och väljer den användare eller grupp som ska läggas till som huvudnamn. Klicka på Nästa och välj sedan de behörigheter du vill tilldela detta huvudkonto:

     **INVOKE_PERM:** Så här anropar du alla åtgärder i tjänsten

     **ÄNDRA_CONFIG_PERM:** Så här ändrar du konfigurationen för en tjänst

     **SUPERVISOR_PERM:** Så här visar du processinstansdata för en tjänst som skapats från en process

     **START_STOP_PERM:** Starta och stoppa en tjänst

     **ADD_REMOVE_ENDPOINTS_PERM:** Lägga till, ta bort och ändra slutpunkter för en tjänst

     **CREATE_VERSION_PERM:** Skapa en version av tjänsten

     **DELETE_VERSION_PERM:** Ta bort en version av tjänsten

     **ÄNDRA_VERSION_PERM:** Så här ändrar du en version av tjänsten

     **READ_PERM:** Så här visar du tjänsten

     Klicka på Slutför för att lägga till säkerhetsobjektet i säkerhetsprofilen.

1. Klicka på Slutför för att slutföra konfigurationen.

## Konfigurera de AEM-formulär som ingår i en arkivfil {#configure-the-aem-forms-that-are-part-of-an-archive-file}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering och sedan på fliken Aktiviteter.
1. På sidan Arkivhantering väljer du den arkivfil som ska konfigureras.
1. På sidan Visa arkiv väljer du den markerade arkivresursen.
1. Konfigurera den importerade processarkivfilen.

## Använd konfigurationsguiden för att konfigurera de AEM-formulär som ingår i en arkivfil {#use-the-configuration-wizard-to-configure-the-aem-forms-that-are-part-of-an-archive-file}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering och sedan på fliken Aktiviteter.
1. Klicka på Konfigurera bredvid den arkivfil som ska konfigureras.
1. Sidan Konfigurera slutpunkter visas där du kan göra de ändringar du behöver:

   * Om du vill byta namn på en slutpunkt eller redigera dess beskrivning klickar du på den.
   * Om du vill lägga till en Task Manager-slutpunkt klickar du på Lägg till TaskManager. Mer information om inställningar för Aktivitetshanteraren finns i [Konfigurera slutpunkter för Aktivitetshanteraren](/help/forms/using/admin-help/configuring-task-manager-endpoints.md#configuring-task-manager-endpoints).
   * Om du vill lägga till en bevakad mappslutpunkt klickar du på Lägg till bevakad mapp. Mer information om inställningarna för bevakad mapp finns i [Inställningar för bevakad mapp](/help/forms/using/admin-help/configuring-watched-folder-endpoints.md#watched-folder-endpoint-settings).
   * Om du vill lägga till en e-postslutpunkt klickar du på Lägg till e-post. Mer information om e-postinställningarna finns i [Inställningar för e-postslutpunkt](/help/forms/using/admin-help/configuring-email-endpoints.md#email-endpoint-settings).
   * Om du vill lägga till en EJB-slutpunkt klickar du på Lägg till EJB och anger ett namn och en beskrivning för slutpunkten.
   * Om du vill lägga till en SOAP-slutpunkt klickar du på Lägg till SOAP och anger ett namn och en beskrivning för slutpunkten.
   * Om du vill lägga till en fjärrslutpunkt klickar du på Lägg till fjärranslutning. Mer information om fjärrinställningar finns i [Fjärrinställningar för slutpunkter](/help/forms/using/admin-help/configuring-remoting-endpoints.md#remoting-endpoint-settings).
   * Om du vill lägga till en REST-slutpunkt klickar du på Lägg till REST och anger ett namn och en beskrivning för slutpunkten. Anteckna den URL för REST-anrop som visas på sidan Lägg till REST-slutpunkt.
   * Om du vill ta bort en slutpunkt markerar du kryssrutan bredvid den och klickar på Ta bort.

1. Klicka på Nästa.
1. Om en process eller tjänst i LCA har konfigurationsparametrar visas sidan Konfigurera parametrar där du konfigurerar tjänstparametrarna och klickar på Nästa.
1. På sidan Konfigurera säkerhetsprofil kan du göra de ändringar du behöver:

   * **Kräv att anropare autentiserar:** Den här inställningen anger om tjänsten kan anropas med eller utan autentiseringsuppgifter.

     Om *Anropare för närvarande krävs för att autentisera* visas, måste anroparen till tjänsten vara autentiserad och användarens huvudnamn för den anroparen måste vara auktoriserat för att kunna anropa tjänsten. I annat fall nekas anropsförsöket. Klicka på Tillåt oautentiserade anropare för att ta bort behovet av autentisering.

     Om *Anroparna inte behöver autentisera* visas kanske tjänstens anropare inte autentiseras. Anropet av tjänsten lyckas alltid eftersom det inte finns någon auktoriseringskontroll. Klicka på Kräv att anropare autentiserar för att begära autentisering.

   * **Kör som:** Anger den körningsidentitet som används av en tjänst efter att den har anropats. Om du vill ändra det här alternativet klickar du på Ändra. Välj bland följande alternativ:

     **Ospecificerad:** Standardbeteendet används.

     **Anroparen:** Använder samma identitet som användaren som anropade tjänsten.

     **System:** Kör tjänsten med fullständig behörighet. Det här är standardinställningen för långvariga processer.

     **Namngiven användare:** Gör att du kan köra tjänsten som en specifik användare. Det här är standardinställningen för kortlivade processer. När du väljer det här alternativet klickar du på Välj användare för att visa sidan Välj huvudnamn, där du kan söka efter och välja användaren.

   * Om du vill lägga till ett huvudnamn i säkerhetsprofilen klickar du på Lägg till huvudnamn och väljer den användare eller grupp som ska läggas till som huvudnamn. Klicka på Nästa och välj sedan de behörigheter du vill tilldela detta huvudkonto:

     **INVOKE_PERM:** Så här anropar du alla åtgärder i tjänsten

     **ÄNDRA_CONFIG_PERM:** Så här ändrar du konfigurationen för en tjänst

     **SUPERVISOR_PERM:** Så här visar du processinstansdata för en tjänst som skapats från en process

     **START_STOP_PERM:** Starta och stoppa en tjänst

     **ADD_REMOVE_ENDPOINTS_PERM:** Lägga till, ta bort och ändra slutpunkter för en tjänst

     **CREATE_VERSION_PERM:** Skapa en version av tjänsten

     **DELETE_VERSION_PERM:** Ta bort en version av tjänsten

     **ÄNDRA_VERSION_PERM:** Så här ändrar du en version av tjänsten

     **READ_PERM:** Så här visar du tjänsten

     Klicka på Slutför för att lägga till säkerhetsobjektet i säkerhetsprofilen.

## Ta bort ett arkiv {#remove-an-archive}

>[!NOTE]
>
>Om du vill ta bort ett arkiv som innehåller resurser som lagras i en tredjepartsdatabas (EMC Documentum Content Server, IBM FileNet Content Manager eller IBM Content Manager) måste du även ta bort resursfilerna från databasen med Workbench.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Arkivhantering.
1. Markera kryssrutan för arkivet som ska tas bort på sidan Arkivhantering och klicka på Ta bort.
