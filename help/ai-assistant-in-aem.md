---
title: AI Assistant i AEM 6.5 LTS
description: Använd AI-assistenten för att hitta svar och felsöka de lösningar som finns tillgängliga i Adobe Experience Manager.
solution: Experience Manager, Experience Manager 6.5 LTS
feature: Authoring, AI Assistant, AI Tools
role: Admin, Architect, Developer, User
exl-id: 391d46e3-05c9-4af1-8882-ffd39b04a701
source-git-commit: 86ca5b498d0a51e21e247d07ce186d8a01c95baa
workflow-type: tm+mt
source-wordcount: '1312'
ht-degree: 2%

---

# AI Assistant i AEM 6.5 LTS {#about-ai-assistant-in-aem}

>[!IMPORTANT]
>
>AEM 6.5- och AEM 6.5 LTS-kunder som inte använder Cloud Manager/Experience Hub måste kontakta sin Adobe Customer Success Engineer för att få tillgång till AI Assistant.

AEM (Adobe Experience Manager) AI Assistant har ett konversationsgränssnitt som underlättar för dig att hitta svar på dina Adobe Experience Manager-relaterade frågor. Det hjälper dig att få snabba svar på dina produktrelaterade AEM-frågor (*som är tillgängliga för alla användare*) och automatisera skapandet av supportärenden (*som är tillgängliga för supportadministratörer*).

AI Assistant stöder AEM as a Cloud Service, inklusive följande lösningar:

* Experience Hub - översikt
* Edge Delivery Services
* Sites
* Assets
* Forms
* Dynamiska medier
* Cloud Manager


Det är direkt inbäddat i AEM och tillgängligt från AEM Experience Hub, Cloud Manager och redigeringsgränssnittet.

Följande 25-sekundersvideo innehåller stegvisa guider för AI Assistant i AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3475362/?captions=swe&learn=on&enablevpops)

## Få tillgång till AI Assistant i AEM{#get-access}

För att få tillgång till AI Assistant i AEM måste man ha följande:

* Tillstånd att använda AI Assistant i AEM för produktkunskap. Med den här behörigheten kan du ställa produktrelaterade frågor i AI Assistant-chatten. Den här behörigheten måste aktiveras.
* Behörighet att öppna supportärenden, vilket kräver rollen **supportadministratör**.

>[!NOTE]
>
>AI Assistant-begäranden i AEM autentiseras via Adobe Identity Management Services (IMS). Mer information finns i [Adobe Identity Management Services - översikt](https://www.adobe.com/content/dam/cc/en/trust-center/ungated/whitepapers/corporate/adobe-identity-management-services-security-overview.pdf).

**Så här får du åtkomst till AI Assistant i AEM:**

1. Kunderna måste ha ett extra avtal för att få tillgång till de flesta AI-baserade och autentiska funktionerna i Adobe Experience Manager. Kontakta Adobe för mer information.

1. Om du vill använda AI Assistant i AEM måste du ha tillstånd att få tillgång till produktkunskap via AI Assistant. Den här behörigheten är som standard PÅ.

   Om du vill kontrollera vem som har åtkomst till produktinformation skickar du ett e-postmeddelande till [aemaiassistant@adobe.com](mailto:aemaiassistant@adobe.com) från den e-postadress som är kopplad till din Adobe ID. Adobe kan aktivera åtkomstkontroll på användarnivå. När det är aktiverat kan din administratör bevilja åtkomst på användarnivå genom att följa stegen i [Konfigurera AI-assistenten i AEM](/help/ai-assistant-in-aem-admin.md).


## Omfång {#scope}

AI Assistant i AEM har för närvarande fokus på produktkunskapsfrågor för AEMr. as a Cloud Service. Detta omfattar omfattande stöd för nyckelområden. <!--, such as Sites, Assets, Forms, Edge Delivery Services, Dynamic Media, and Cloud Manager. -->

* **Ytor**: Finns i AEM Experience Hub, Author UI, Cloud Manager.
* **Funktioner**: Produktkunskap och första stopp för felsökning och vägledning, automatiskt skapande av supportärenden och sökning.
* **Värde**: sparar tid, snabbar upp inlärningen och värdesätter tid, minskar behovet av att skapa supportärenden manuellt och förbättrar effektiviteten när det gäller att skapa supportärenden.

## Integritet, säkerhet och styrning{#privacy-security-governance}

AI Assistant i AEM är utformat med stor betoning på sekretess, säkerhet och styrning.

I den här artikeln beskrivs de förtroendecentrerade funktioner som du kan förvänta dig av AI Assistant i AEM:

* AI Assistant i AEM använder inga personuppgifter, inte ens i utbildningssyfte.
* AI Assistant i AEM har inte tillgång till konsumentdata.
* Det krävs explicit tillstånd för att interagera med AI Assistant i AEM.
* Användarspecificerade uppmaningar (frågor, frågor och så vidare) delas inte med andra kunder.

<!-- See also [Security at Adobe whitepaper](). NEED ACTIVE LINK FROM ADRIAN NICOLAE TANASE. CURRENTLY 404. -->

## Lär känna AI Assistant i AEM för produktkännedom och automatisk hantering av supportärenden {#ai-prod-insights}

Produktkunskap omfattar begrepp och ämnen som härletts ur Adobe Experience League-dokumentationen. Dessa frågor kan kategoriseras i följande undergrupper:


| Produktkunskap | Tillgängligt för alla användare<br>Exempel |
| :--- | :--- |
| Undervisning | <ul><li>Vad är den universella redigeraren?</li><li>Hur skapar jag ett program i Cloud Manager?</li></ul> |
| Öppna identifiering | <ul><li>Hur använder jag Universal Editor?</li><li>Finns det något sätt att kopiera innehåll från en miljö till en annan?</li></ul> |
| Felsökning | <ul><li>Varför har jag inte åtkomst till Universal Editor?</li><li>Varför misslyckas min pipeline?</li></ul> |
| **Skapa supportbiljett** | **Endast tillgängligt för supportadministratörer &#x200B;**<br>**Exempel** |
| Automatiserad framtagning av supportbiljetter som fångar AI Assistant-chatthistorik och kontext | <ul><li>Skapa en supportbiljett åt mig.</li></ul> |
| Hämta status för supportanmälan | <ul><li>Visa alla supportärenden som jag har öppnat.</li><li>Visa status för biljetten E—</li></ul> |

{style="table-layout:auto"}


## Skapa effektiva frågor {#ai-craft-questions}

För att få de mest korrekta svaren från AI Assistant i AEM är det viktigt att formulera dina frågor tydligt och i rätt sammanhang. Använd följande tips för att kontrollera att dina frågor är tydliga och välstrukturerade:

* Ange tydligt din uppgift eller fråga på ett kortfattat sätt.
* Undvik tvetydiga formuleringar eller alltför komplex syntax för att öka förståelsen.
* Lägg in relevant information om arbetsuppgifterna eller frågorna eftersom AI Assistant i AEM ger mer exakta och relevanta svar.
I uppmaningen får du hjälp att namnge den AEM-lösning du arbetar i - Sites, Assets, Dynamic Media, Edge Delivery Services, Cloud Manager eller Forms.

### Exempel på frågor som inte stöds {#ai-unsupported-questions}

| Område | Exempel |
| --- | --- |
| Driftsinsikter | <ul><li>Hur många utvecklingsmiljöer finns det i min hyresgäst?</li><li>Vem startade den senaste produktionslinjen?</li></ul> |
| Felsökning | <ul><li>Varför misslyckas min produktionsprocess?</li></ul> |
| Uppgift och automatisering | <ul><li>Konfigurera en pipeline för kodkvalitet från en utvecklingsgren åt mig.</li></ul> |


## Använd AI-assistenten i AEM {#ai-use}

<!-- UNHIDE AFTER BETA or at GA
### Enable AI Assistant in AEM access through Admin Console 

To use AI Assistant in AEM, your organization must opt in at the Admin Console level. A product administrator creates (or chooses) a user group and grants it the new "AI Assistant" permission. Anyone added to that group instantly gains access to the Assistant across AEM. If the goal is company-wide availability, the admin simply assigns all users to that group.

![AI Assistant in AEM in the Admin Console](/help/implementing/cloud-manager/assets/ai-assistant-admin-console.png)

From an employee's perspective, the process is straightforward: identify the product administrator for Adobe Experience Manager in your organization and request to be added to the AI-enabled user group. Once you appear in that group, the Assistant icon shows up automatically the next time you sign in.

Administrators should keep normal Cloud Manager governance in mind. Hold product administrator rights in the Admin Console to create profiles, manage user groups, or edit permissions. If users also need the Assistant's built-in **Create Support Ticket** feature, add the standard **Support Admin** role (standard Admin Console role) to the same individuals or group.

![Technical support ticket creation in AI Assistant in AEM of the Admin Console](/help/implementing/cloud-manager/assets/ai-assistant-admin-console-support-ticket.png)

For a guided walkthrough of setting up users and groups in AEM as a Cloud Service, see [Configuring access to AEM as a Cloud Service ](https://experienceleague.adobe.com/sv/docs/experience-manager-learn/cloud-service/accessing/overview). 

See also [Custom Permissions](/help/implementing/cloud-manager/custom-permissions.md). -->


### Starta en AI-assistent i AEM-konversationer

Du kan återställa AI-assistenten i AEM och starta en ny konversation när du vill ändra ämnen. Den här funktionen är särskilt användbar vid felsökning av frågor som är felaktiga eller ger felaktig information.

**Så här startar du en AI-assistent i AEM-konversationen:**

1. Klicka på ikonen **AI-assistenten** i det övre högra hörnet av AEM användargränssnitt (antingen från Cloud Manager-sidor eller från författarinstansen av AEM-miljöerna).

   ![AI Assistant-ikon i verktygsfältet](/help/assets/assets-ai/ai-assistant-icon.png)

1. Skriv din fråga eller fråga i textrutan på panelen **AI Assistant** längst ned och tryck sedan på `Enter` eller klicka på ![ikonen Skicka](https://spectrum.adobe.com/static/icons/workflow_18/Smock_Send_18_N.svg).

   >[!NOTE]
   >
   >Personuppgifter bör inte inkluderas i dina indata, eftersom det är onödigt att använda det här verktyget.

   ![Textruta längst ned på AI-assistentpanelen](/help/assets/assets-ai/ai-assistant-prompt-text-box.png)

1. Om du vill starta en ny konversation (nytt ämne eller en ändring i ämnet) klickar du på ![Mer ikon](https://spectrum.adobe.com/static/icons/workflow_18/Smock_More_18_N.svg) > **Starta ny konversation**.

   ![Starta en ny konversation i AI Assistant från ellipsikonen](/help/assets/assets-ai/ai-assistant-start-new-conversation.png)

### Upptäck uppmaningar per kategori

AI Assistant i AEM innehåller en funktion som hjälper dig att utforska ämnen och kategorier som stöds.

**Så här identifierar du uppmaningar per kategori:**

1. Klicka på ikonen ![Lär dig](https://spectrum.adobe.com/static/icons/workflow_18/Smock_Learn_18_N.svg) på AI Assistant-panelen för att aktivera panelen för promptidentifiering.

   ![Panel där du kan utforska uppmaningar efter kategori i AI Assistant](/help/assets/assets-ai/ai-assistant-discover-prompts.png)
   *Panel med promptkategorier i AI Assistant.*

1. Välj en kategori om du vill visa en lista med relaterade uppmaningar.
1. Välj en uppmaning om att se exempel på de typer av frågor som AI-assistenten kan besvara.

1. Om du vill dölja frågeidentifieringspanelen klickar du på ![ikonen Lär dig](https://spectrum.adobe.com/static/icons/workflow_18/Smock_Learn_18_N.svg) igen.

### Dela dina synpunkter på AI Assistant i AEM

Dina indata hjälper Adobe att förbättra AI Assistant för bättre prestanda och precision.

Dela med dig av dina synpunkter på din upplevelse med AI Assistant i AEM genom följande alternativ:

![Tummen uppåt, tummen nedåt och flaggikoner](/help/assets/assets-ai/ai-assistant-feedback-icons.png)

| Klicka på | Beskrivning |
| --- | --- |
| ![Miniatyrbilderna är en ikon](https://spectrum.adobe.com/static/icons/workflow_18/Smock_ThumbUpOutline_18_N.svg) | Ange vad som gick bra och dela positiv feedback. |
| ![Miniatyrbilder nedåt, ikon](https://spectrum.adobe.com/static/icons/workflow_18/Smock_ThumbDownOutline_18_N.svg) | Ge förslag på förbättringar. Lägg till specifika kommentarer om din upplevelse, som granskas dagligen. |
| ![Flaggikon](https://spectrum.adobe.com/static/icons/workflow_18/Smock_Flag_18_N.svg) | Rapportera problem eller lämna detaljerad feedback om din interaktion med AI Assistant i AEM. |

## Frågor och svar om AI Assistant i AEM {#ai-faq}

Här är svar på några vanliga frågor om AI Assistant:

* **Tillhandahåller AI Assistant information i AEM i realtid?**\
  Nej. AI Assistant hämtar sitt innehåll från Adobe Experience League-dokumentation. Det kan ta lite tid att reflektera över uppdateringarna i svaren.
* **Vilka Adobe-program stöder AI Assistant i AEM?**\
  För närvarande stöder AI Assistant produktkunskapsfrågor i AEM as a Cloud Service, inklusive Sites, Assets, Dynamic Media, Cloud Manager och Forms.
* **Vilka funktioner har AI Assistant i AEM?**\
  AI Assistant i AEM är utformat för att besvara frågor som rör Adobe produktkunskaper.
* **Använder AI-assistenten i AEM personlig information för utbildningsdata?**\
  Nej. AI Assistant i AEM använder inte personuppgifter i utbildningssyfte. Undvik att dela personuppgifter om dig själv eller andra, inklusive namn eller kontaktuppgifter, med AI-assistenten i AEM.

<!-- IS THE DOCUMENTATION BELOW STILL NEEDED? IF SO, GO AHEAD AND DELETE THE COMMENT TAGS!!

## AEM Forms AI Assistant (Forms Experience Builder) {#ai-forms-builder}

In addition to the general AI Assistant in AEM for product knowledge, AEM offers a specialized **[AEM Forms AI Assistant (Forms Experience Builder)](/help/edge/docs/forms/forms-ai-assistant.md)**. This enhanced assistant can actively help you create and configure forms through natural language prompts and answer questions specific to forms.

### Key capabilities

The AEM Forms AI Assistant provides:

* **Form Creation**: Create new forms from scratch using natural language descriptions.
* **Design Import**: Convert existing designs (PDF, Figma, images) into functional AEM Forms. 
* **Form Configuration**: Add fields, panels, validation rules, and conditional logic.
* **Layout Management**: Organize form structure and optimize for different devices.
* **Integration Setup**: Configure form submissions and data handling.
* **Product Knowledge**: Answer questions about AEM Forms features and best practices.

### Where to access

The AEM Forms AI Assistant is available in the following:

* **Universal Editor**: For Edge Delivery Services forms with visual editing capabilities.
* **Adaptive Forms Editor**: For detailed form configuration and advanced features.
* **Forms Management UI**: For high-level form creation and management tasks.

### Getting started

>[!NOTE]
>
> The AEM Forms AI Assistant (Forms Experience Builder) is available under the private beta program. Send an email from your work address to [aem-forms-ea@adobe.com](mailto:aem-forms-ea@adobe.com) to request access.

To learn more about using the AEM Forms AI Assistant , see the [AEM Forms AI Assistant](/help/edge/docs/forms/forms-ai-assistant.md) documentation.

### Example Use Cases

* **"Create a customer feedback form with name, email, rating, and comments fields"**
* **"Convert this uploaded PDF application form into a digital adaptive form"**  
* **"Add conditional logic to show spouse information only when marital status is 'Married'"**
* **"Configure this form to submit data to the Customer Relationship Management system"**

This specialized AEM Forms AI Assistant represents the next evolution in form building, combining the power of AI with AEM's robust forms capabilities to streamline your form creation workflow.
-->
