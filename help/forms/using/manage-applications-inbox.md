---
title: Hantera Forms-program och -uppgifter i AEM Inbox
description: Med AEM Inbox kan du starta Forms-centrerade arbetsflöden genom att skicka program och hantera uppgifter.
contentOwner: vishgupt
topic-tags: document_services, publish
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
source-git-commit: 168cb023768ff3139937ab7f437ab7d00185bca0
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Hantera Forms-program och -uppgifter i AEM Inbox{#manage-forms-applications-and-tasks-in-aem-inbox}

Ett av många sätt att starta eller utlösa ett Forms-centrerat arbetsflöde är genom program i AEM Inbox. Skapa ett arbetsflödesprogram om du vill göra ett Forms-arbetsflöde tillgängligt som program i Inbox. Mer information om arbetsflödesprogram och andra sätt att starta Forms-arbetsflöden finns i [Starta ett Forms-orienterat arbetsflöde i OSGi](../../forms/using/aem-forms-workflow.md#launch).

Dessutom konsoliderar AEM Inbox meddelanden och uppgifter från olika AEM-komponenter, inklusive Forms-arbetsflöden. När ett formulärarbetsflöde som innehåller ett tilldelningssteg aktiveras, visas det associerade programmet som en uppgift i den tilldelades inkorg. Om den som tilldelats är en grupp visas uppgiften i Inkorgen för alla gruppmedlemmar tills en enskild person gör anspråk på eller delegerar uppgiften.

Användargränssnittet i Inkorgen innehåller lista- och kalendervyer för att visa uppgifter. Du kan också konfigurera visningsinställningarna. Du kan filtrera uppgifter baserat på olika parametrar. Mer information om visning och filter finns i [Inkorgen](/help/sites-authoring/inbox.md).

Sammanfattningsvis kan du i Inkorgen skapa ett program och hantera tilldelade uppgifter.

>[!NOTE]
>
>Du måste vara medlem i gruppen för arbetsflödesanvändare för att kunna använda AEM Inbox.

## Skapa program {#create-application}

1. Gå till AEM Inbox på https://&#39;[server]:[port]&#39;/aem/inbox.
1. Välj **[!UICONTROL Create > Application]** i Inkorgsgränssnittet. Sidan Välj program visas.
1. Välj ett program och klicka på **[!UICONTROL Create]**. Det adaptiva formulär som är associerat med programmet öppnas. Fyll i informationen i det adaptiva formuläret och välj **[!UICONTROL Submit]**. Det startar det associerade arbetsflödet och skapar en uppgift i den tilldelades inkorg.

## Hantera uppgifter {#manage-tasks}

När ett Forms-arbetsflöde utlöses och du är tilldelad eller en del av den tilldelade gruppen, visas en uppgift i Inkorgen. Du kan visa uppgiftsinformation och utföra tillgängliga åtgärder för uppgiften inifrån Inkorgen.

### Anspråk eller delegera uppgifter {#claim-or-delegate-tasks}

Uppgifter som tilldelas en grupp visas i Inkorgen för alla gruppmedlemmar. Alla gruppmedlemmar kan göra anspråk på den uppgiften eller delegera den till en annan gruppmedlem. Så här gör du:

1. Välj det här alternativet om du vill välja en miniatyrbild av uppgiften. Alternativ för att öppna eller delegera uppgiften visas högst upp.

   ![select-task](assets/select-task.png)

1. Gör något av följande:

   * Välj **[!UICONTROL Delegate]** om du vill delegera uppgiften. Dialogrutan Delegera objekt öppnas. Välj en användare, lägg till en kommentar om du vill och välj **[!UICONTROL OK]**.

   ![delegat](assets/delegate.png)

   * Välj **[!UICONTROL Open]** om du vill göra anspråk på uppgiften. Dialogrutan Tilldela till mig själv öppnas. Välj **[!UICONTROL Proceed]** om du vill göra anspråk på uppgiften. Uppgiften visas med dig som tilldelad i din inkorg.

   ![anspråk](assets/claim.png)

### Visa information och utför åtgärder på uppgifter {#view-details-and-perform-actions-on-tasks}

När du öppnar en uppgift kan du visa uppgiftsinformation och utföra tillgängliga åtgärder. Vilka åtgärder som är tillgängliga för en uppgift definieras i steget Tilldela uppgift i det associerade Forms-arbetsflödet.

1. Välj det här alternativet om du vill välja en miniatyrbild av uppgiften. Alternativ för att öppna eller delegera den valda uppgiften visas högst upp.
1. Välj **Öppna** om du vill visa aktivitetsinformation. Den detaljerade uppgiftsvyn öppnas. I den här vyn kan du visa aktivitetsinformation och arbeta med uppgiften.

   >[!NOTE]
   >
   >Om en uppgift har tilldelats en grupp måste du göra anspråk på den för att den ska kunna öppnas i detaljerad vy.

![aktivitetsinformation](assets/task-details.png)

Den detaljerade uppgiftsvyn innehåller följande avsnitt:

* Uppgiftsinformation
* Formulär
* Information om arbetsflöde
* Verktygsfältet Åtgärder

#### Uppgiftsinformation {#task-details}

I avsnittet Uppgiftsinformation visas information om uppgiften. Vilken information som visas beror på konfigurationsinställningarna för [Tilldela aktivitetssteg](/help/sites-developing/workflows-step-ref.md) i arbetsflödet. I exemplet ovan visas beskrivning, status, startdatum och arbetsflöde som används för uppgiften. Det gör det även möjligt att bifoga en fil till uppgiften.

#### Formulär {#form}

Fliken Formulär i området för huvudinnehållet visar eventuella bifogade formulär och fältnivåbilagor.

#### Information om arbetsflöde {#workflow-details}

Fliken Arbetsflödesinformation överst visar förloppet för uppgiften genom olika steg i arbetsflödet. Här visas slutförda, aktuella och väntande faser för uppgiften. Stegen för ett arbetsflöde definieras i [Tilldela uppgiftssteg](/help/sites-developing/workflows-step-ref.md) i det associerade arbetsflödet.

Fliken visar också aktivitetshistorik för varje slutförd fas i arbetsflödet. Du kan välja **[!UICONTROL View Details]** för en slutförd fas om du vill veta mer om den scenen. Här visas kommentarer, formulär och uppgiftsbilagor, status, start- och slutdatum och så vidare, om uppgiften.

![arbetsflödesinformation](assets/workflow-details.png)

#### Verktygsfältet Åtgärder {#actions-toolbar}

Verktygsfältet Åtgärder visar alla tillgängliga alternativ för uppgiften. Medan Spara, Återställ och Delegera är standardåtgärder konfigureras andra tillgängliga åtgärder i [Tilldela aktivitetssteg](/help/sites-developing/workflows-step-ref.md). I exemplet ovan har Godkänn och Avvisa konfigurerats i arbetsflödet.

När du arbetar med uppgiften fortsätter den i arbetsflödet.

### Visa slutförda uppgifter {#view-completed-tasks}

I AEM Inbox visas endast aktiva uppgifter. Slutförda uppgifter visas inte i listan. Du kan emellertid använda inkorgsfilter för att filtrera uppgifter baserat på flera parametrar, som uppgiftstyp, status samt start- och slutdatum. Så här visar du slutförda uppgifter:

1. I AEM Inbox väljer du ![växlingspanel1](assets/toggle-side-panel1.png) för att öppna filterväljaren.
1. Välj dragspelet **[!UICONTROL Task Status]** och välj **[!UICONTROL Complete]**. Alla slutförda uppgifter visas.

   ![filter](assets/filter.png)

1. Välj om du vill välja en uppgift och klicka på **[!UICONTROL Open]**.

Uppgiften öppnas och visar dokumentet eller det adaptiva formulär som är kopplat till uppgiften. För anpassningsbara formulär visar aktiviteten det skrivskyddade adaptiva formuläret eller dess PDF-postdokument som konfigurerats på fliken Formulär/Dokument i [arbetsflödessteget Tilldela uppgift](/help/sites-developing/workflows-step-ref.md).

I avsnittet med aktivitetsinformation visas information om till exempel åtgärd, aktivitetsstatus, startdatum och slutdatum.

![slutförd-aktivitet](assets/completed-task.png)

Fliken **[!UICONTROL Workflow Details]** visar varje steg i arbetsflödet. Välj **[!UICONTROL View details]** om du vill få mer detaljerad information.

![completed-task-workflow](assets/completed-task-workflow.png)

## Felsökning {#troubleshooting-workflows}

### Det går inte att visa objekt som är relaterade till AEM Workflow i AEM inkorg {#unable-to-see-aem-worklow-items}

En arbetsflödesmodellägare kan inte visa objekt som är relaterade till AEM Workflow i AEM inkorg. Lös problemet genom att lägga till indexen nedan i din AEM-databas och återskapa indexet.

1. Använd någon av följande metoder för att lägga till index:

   * Skapa följande noder i CRX DE på `/oak:index/workflowDataLucene/indexRules/granite:InboxItem/properties` med respektive egenskaper enligt följande tabell:

     | Nod | Egenskap | Typ |
     |---|---|---|
     | sharedWith | sharedWith | STRÄNG |
     | låst | låst | BOOLEAN |
     | returnerade | returnerade | BOOLEAN |
     | allowInboxSharing | allowInboxSharing | BOOLEAN |
     | allowExplicitSharing | allowExplicitSharing | BOOLEAN |


   * Distribuera indexen via ett AEM-paket. Du kan använda ett [AEM Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=en) -projekt för att skapa ett distribuerbart AEM-paket. Använd följande exempelkod för att lägga till index i ett AEM Archetype-projekt:

   ```Java
      .property("sharedWith", "sharedWith").type(TYPENAME_STRING).propertyIndex()
      .property("locked", "locked").type(TYPENAME_BOOLEAN).propertyIndex()
      .property("returned", "returned").type(TYPENAME_BOOLEAN).propertyIndex()
      .property("allowInboxSharing", "allowInboxSharing").type(TYPENAME_BOOLEAN).propertyIndex()
      .property("allowExplicitSharing", "allowExplicitSharing").type(TYPENAME_BOOLEAN).propertyIndex()
   ```

1. [Skapa ett egenskapsindex och ange det till true](https://experienceleague.adobe.com/docs/experience-manager-65-lts/deploying/deploying/queries-and-indexing.html#the-property-index).

1. När du har konfigurerat index i CRX DE eller distribuerat via ett paket indexerar [om databasen](https://helpx.adobe.com/in/experience-manager/kb/HowToCheckLuceneIndex.html#Completelyrebuildtheindex).

https://experienceleague.adobe.com/docs/experience-manager-65-lts/deploying/deploying/queries-and-indexing.html
