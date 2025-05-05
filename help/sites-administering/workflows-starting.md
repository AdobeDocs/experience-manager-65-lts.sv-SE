---
title: Starta arbetsflöden
description: Lär dig hur du administrerar arbetsflöden i Adobe Experience Manager så att du kan starta dem med olika metoder, antingen manuellt eller automatiskt.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Operations
role: Admin
exl-id: a8b1fab9-1a63-4f99-87e1-48f6167e9953
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# Starta arbetsflöden{#starting-workflows}

När du administrerar arbetsflöden kan du starta dem på olika sätt:

* Manuellt:

   * Från en [arbetsflödesmodell](#workflow-models).
   * Använder ett arbetsflödespaket för [gruppbearbetning](#workflow-packages-for-batch-processing).

* Automatiskt:

   * Som svar på nodändringar: [använder en startprogram](#workflows-launchers).

>[!NOTE]
>
>Andra metoder är också tillgängliga för författare. Mer information finns i:
>
>* [Använda arbetsflöden på sidor](/help/sites-authoring/workflows-applying.md)
>* [Använda arbetsflöden för DAM-resurser](/help/assets/assets-workflow.md)
>* [AEM Forms](https://helpx.adobe.com/se/aem-forms/6-2/aem-workflows-submit-process-form.html)
>* [Översättningsprojekt](/help/sites-administering/tc-manage.md)
>

## Arbetsflödesmodeller {#workflow-models}

Du kan starta ett arbetsflöde [baserat på någon av modellerna](/help/sites-administering/workflows.md#workflow-models-and-instances) som visas på konsolen Arbetsflödesmodeller. Den enda obligatoriska informationen är nyttolasten, men du kan även lägga till en titel och/eller kommentar.

## Arbetsflöden - startare {#workflows-launchers}

Workflow Launcher övervakar ändringar i innehållsdatabasen för att starta arbetsflöden beroende på platsen och resurstypen för den ändrade noden.

Med **startprogrammet** kan du:

* Se de arbetsflöden som redan har startats för specifika noder.
* Välj ett arbetsflöde som ska startas när en viss nod/nodtyp har skapats/ändrats/tagits bort.
* Ta bort en befintlig relation mellan arbetsflöde och nod.

En startfil kan skapas för alla noder. Ändringar av vissa noder startar dock inte arbetsflöden. Ändringar av noder under följande sökvägar leder inte till att arbetsflöden startar:

* `/var/workflow/instances`
* Alla arbetsflödesinkorgsnoder som finns någonstans i grenen `/home/users`
* `/tmp`
* `/var/audit`
* `/var/classes`
* `/var/eventing`
* `/var/linkchecker`
* `/var/mobile`
* `/var/statistics`

   * Undantag: Ändringar av noder under `/var/statistics/tracking` *do* startar arbetsflöden.

Olika definitioner ingår i standardinstallationen. Dessa används för hantering av digitala resurser och samarbete i sociala medier:

![wf-100](assets/wf-100.png)

## Arbetsflödespaket för gruppbearbetning {#workflow-packages-for-batch-processing}

Arbetsflödespaket är paket som kan skickas till ett arbetsflöde som nyttolast för bearbetning, vilket gör att flera resurser kan bearbetas.

Ett arbetsflödespaket:

* innehåller länkar till en uppsättning resurser (till exempel sidor, resurser).
* innehåller paketinformation, t.ex. datum då paketet skapades, den användare som skapade paketet och en kort beskrivning.
* definieras med en särskild sidmall. På sådana sidor kan användaren ange resurserna i paketet.
* kan användas flera gånger.
* kan ändras av användaren (lägg till eller ta bort resurser) medan arbetsflödesinstansen körs.

## Starta ett arbetsflöde från Models Console {#starting-a-workflow-from-the-models-console}

1. Navigera till konsolen **Models** med **Tools**, **Workflow** och sedan **Models**.
1. Markera arbetsflödet (enligt konsolvyn). Du kan även använda Sök (längst upp till vänster) om det behövs:

   ![wf-103](assets/wf-103.png)

   >[!NOTE]
   >
   >Indikatorn **[Övergående](/help/sites-developing/workflows.md#transient-workflows)** visar arbetsflöden där arbetsflödeshistoriken inte bevaras.

1. Välj **Starta arbetsflöde** i verktygsfältet.
1. Dialogrutan Kör arbetsflöde öppnas och du kan ange:

   * **Nyttolast**

     Detta kan vara en sida, nod, resurs, paket med flera resurser.

   * **Titel**

     En valfri titel som hjälper dig att identifiera den här instansen.

   * **Kommentar**

     En valfri kommentar som hjälper till att ange detaljer för den här instansen.

   ![wf-104](assets/wf-104.png)

## Skapa en startkonfiguration {#creating-a-launcher-configuration}

1. Navigera till konsolen **Arbetsflödeskörare** med **Verktyg**, **Arbetsflöde** och sedan **Startare**.
1. Välj **Skapa** och sedan **Lägg till startprogram** för att öppna dialogrutan:

   ![wf-105](assets/wf-105.png)

   * **Händelsetyp**

     Händelsetypen som startar arbetsflödet:

      * Skapad
      * Ändrad
      * Borttagen

   * **Nodetype**

     Den typ av nod som arbetsflödets startprogram gäller för.

   * **Sökväg**

     Sökvägen som arbetsflödets startprogram gäller för.

   * **Körningsläge(n)**

     Den typ av server som arbetsflödets startprogram gäller för. Välj **Författare**, **Publicera** eller **Författare och publicera**.

   * **Villkor**

     En lista med villkor för nodvärden som, när de utvärderas, avgör om arbetsflödet startas. Följande villkor gör att arbetsflödet startar när noden har ett egenskapsnamn med värdet User:

     name==User

   * **Funktioner**

     En lista över funktioner som ska aktiveras. Välj önskade funktioner med hjälp av den nedrullningsbara väljaren.

   * **Inaktiverade funktioner**

   En lista över funktioner som ska inaktiveras. Välj önskade funktioner med hjälp av den nedrullningsbara väljaren.

   * **Arbetsflödesmodell**

     Det arbetsflöde som ska startas när händelsetypen inträffar på noden och/eller sökvägen under det definierade villkoret.

   * **Beskrivning**

     Din egen text som beskriver och identifierar startkonfigurationen.

   * **Aktivera**

     Anger om arbetsflödets startprogram är aktiverat:

      * Välj **Aktivera** om du vill starta arbetsflöden när konfigurationsegenskaperna är uppfyllda.
      * Välj **Inaktivera** när arbetsflödet inte ska köras (inte ens när konfigurationsegenskaperna är uppfyllda).

   * **Uteslut lista**

     Detta anger alla JCR-händelser som ska uteslutas (d.v.s. ignoreras) när du avgör om ett arbetsflöde ska utlösas.

     Den här startegenskapen är en kommaavgränsad lista med objekt: &quot;

      * `property-name` ignorerar alla `jcr`-händelser som utlöstes för det angivna egenskapsnamnet. &quot;
      * `event-user-data:<*someValue*>` ignorerar alla händelser som innehåller `*<someValue*`> `user-data` som angetts via [`ObservationManager` API ] (https://developer.adobe.com/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/observation/ObservationManager.html#setUserData(java.lang.String).

     Till exempel:

     `jcr:lastModified,dc:modified,dc:format,jcr:lastModifiedBy,imageMap,event-user-data:changedByWorkflowProcess`

     Den här funktionen kan användas för att ignorera ändringar som utlöses av en annan arbetsflödesprocess genom att lägga till exkluderingsobjektet:

     `event-user-data:changedByWorkflowProcess`

1. Välj **Skapa** för att skapa startprogrammet och återgå till konsolen.

   När den lämpliga händelsen inträffar aktiveras startprogrammet och arbetsflödet startas.

## Hantera en startkonfiguration {#managing-a-launcher-configuration}

När du har skapat startarkonfigurationen kan du använda samma konsol för att markera instansen och sedan **Visa egenskaper** (och redigera dem) eller **Ta bort**.
