---
title: Utveckla och utöka arbetsflöden
description: AEM har flera verktyg och resurser för att skapa arbetsflödesmodeller, utveckla arbetsflödessteg och för programmeringsinsatser i arbetsflöden
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: eff477824f2d1e5dfe5417835e8ced4b7eb85a0f
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 0%

---


# Utveckla och utöka arbetsflöden{#developing-and-extending-workflows}

AEM har flera verktyg och resurser för att skapa arbetsflödesmodeller, utveckla arbetsflödessteg och för programmässig interaktion med arbetsflöden.

Med arbetsflöden kan ni automatisera processer för hantering av resurser och publicering av innehåll i er AEM-miljö. Arbetsflödena består av en serie steg, där varje steg utgör en diskret uppgift. Du kan använda logik- och körtidsdata för att bestämma när en process kan fortsätta och välja nästa steg i ett av flera möjliga steg.

Affärsprocesserna för att skapa och publicera webbsidor innefattar till exempel att godkänna och godkänna av olika deltagare. Dessa processer kan utformas med AEM arbetsflöden och tillämpas på specifikt innehåll.

Nedan beskrivs de viktigaste aspekterna, medan följande sidor innehåller mer information:

* [Skapa arbetsflödesmodeller](/help/sites-developing/workflows-models.md)
* [Utöka arbetsflödesfunktioner](/help/sites-developing/workflows-customizing-extending.md)
* [Interagera med arbetsflöden programmatiskt](/help/sites-developing/workflows-program-interaction.md)
* [Referens för arbetsflödessteg](/help/sites-developing/workflows-step-ref.md)
* [Referens för arbetsflödesprocess](/help/sites-developing/workflows-process-ref.md)
* [Bästa praxis för arbetsflöden](/help/sites-developing/workflows-best-practices.md)

>[!NOTE]
>
>Mer information om:
>
>* Delta i arbetsflöden, se [Använda arbetsflöden](/help/sites-authoring/workflows.md).
>* Administrera arbetsflöden och arbetsflödesinstanser, se [Administrera arbetsflöden](/help/sites-administering/workflows.md).
>* En communityartikel från början till slut finns i [Ändra Digital Assets med Adobe Experience Manager-arbetsflöden.](https://experienceleague.adobe.com/docs/experience-manager-65-2025/assets/using/assets-workflow.html)
>* Se [Ask the AEM Experts Webinar on Workflows](https://communities.adobeconnect.com/p5s33iburd54/).
>* Ändringar av platserna för information finns i [Bästa praxis för arbetsflöde - Platser](/help/sites-developing/workflows-best-practices.md#locations).
>

## Modell {#model}

En `WorkflowModel` representerar en definition (modell) av ett arbetsflöde. Den är gjord av `WorkflowNodes` och `WorkflowTransitions`. Övergångarna ansluter noderna och definierar *flödet*. Modellen har alltid en startnod och en slutnod.

### Körningsmodell {#runtime-model}

Arbetsflödesmodeller är versionshanterade. När du kör en arbetsflödesinstans används och behålls arbetsflödets körningsmodell, så som den är tillgänglig när arbetsflödet startades.

En körningsmodell [ genereras när **Synkronisering** aktiveras i arbetsflödesmodellredigeraren](/help/sites-developing/workflows-models.md#sync-your-workflow-generate-a-runtime-model).

Redigeringar av arbetsflödesmodellen som inträffar, eller körningsmodeller som genereras, eller båda, *efter*, tillämpas inte på den instansen.

>[!CAUTION]
>
>Stegen som utförs definieras av [körningsmodellen](/help/sites-developing/workflows-models.md#sync-your-workflow-generate-a-runtime-model) som genereras när åtgärden **Synkronisera** aktiveras i arbetsflödesmodellredigeraren.
>
>Om arbetsflödesmodellen ändras efter den här tidpunkten (utan att **Synkronisera** aktiveras) återspeglas inte dessa ändringar i körningsinstansen. Endast körningsmodeller som genererats efter uppdateringen återspeglar ändringarna. Undantagen är de underliggande ECMA-skripten, som endast sparas en gång så att ändringarna genomförs.

### Steg {#step}

Varje steg ger en diskret uppgift. Det finns olika typer av arbetsflödessteg:

* Deltagare (Användare/grupp): Med de här stegen genereras en arbetsuppgift och tilldelas en användare eller grupp. En användare måste slutföra arbetsuppgiften för att kunna gå vidare i arbetsflödet.
* Process (Script, Java™-metodanrop): Dessa steg utförs automatiskt av systemet. Ett ECMA-skript eller en Java™-klass implementerar steget. Tjänsterna kan utvecklas för att avlyssna särskilda arbetsflödeshändelser och utföra uppgifter enligt affärslogiken.
* Behållare (underarbetsflöde): Den här typen av steg startar en annan arbetsflödesmodell.
* ELLER Dela/förena: Använd logik för att bestämma vilket steg som ska köras härnäst i arbetsflödet.
* AND Split/Join: Tillåter att flera steg körs samtidigt.

Alla steg delar följande gemensamma egenskaper: `Autoadvance` och `Timeout` varningar (skriptbara).

### Övergång {#transition}

En `WorkflowTransition` representerar en övergång mellan två `WorkflowNodes` av en `WorkflowModel`.

* Den definierar länken mellan två på varandra följande steg.
* Det är möjligt att tillämpa regler.

### WorkItem {#workitem}

En `WorkItem` är den enhet som skickas genom en `Workflow`-instans av en `WorkflowModel`. Den innehåller `WorkflowData` som instansen agerar på och en referens till `WorkflowNode` som beskriver det underliggande arbetsflödessteget.

* Den används för att identifiera uppgiften och placeras i respektive inkorg.
* En arbetsflödesinstans kan ha en eller flera `WorkItems` samtidigt (beroende på arbetsflödesmodellen).
* `WorkItem` refererar till arbetsflödesinstansen.
* I databasen lagras `WorkItem` under arbetsflödesinstansen.

### Nyttolast {#payload}

Refererar till resursen som måste avanceras via ett arbetsflöde.

Nyttolastimplementeringen refererar till en resurs i databasen (via sökväg, UUID eller URL) eller av ett serialiserat Java™-objekt. Att referera till en resurs i databasen är flexibelt och med sling-produktivitet. Den refererade noden kan till exempel återges som ett formulär.

### Livscykel {#lifecycle}

Skapas när ett nytt arbetsflöde startas (genom att man väljer respektive arbetsflödesmodell och definierar nyttolasten) och avslutas när slutnoden bearbetas.

Följande åtgärder är möjliga för en arbetsflödesinstans:

* Avsluta
* Gör uppehåll
* Återuppta
* Starta om

Slutförda och avslutade instanser arkiveras.

### Inkorg {#inbox}

Varje användarkonto har en egen arbetsflödesinkorg där den tilldelade `WorkItems` är tillgänglig.

`WorkItems` tilldelas antingen användarkontot direkt eller till gruppen som de tillhör.

### Arbetsflödestyper {#workflow-types}

Det finns olika typer av arbetsflöden som anges i konsolen Arbetsflödesmodeller:

![wf-upgrade-03](assets/wf-upgraded-03.png)

* **Standard**

  De här typerna är färdiga arbetsflöden som ingår i en standardinstans av AEM.

* Anpassade arbetsflöden (ingen indikator i konsolen)

  Dessa arbetsflöden har skapats som nya eller från färdiga arbetsflöden som har överlagrats med anpassningar.

* **Äldre**

  Arbetsflöden som skapats i en tidigare version av AEM. Dessa arbetsflöden kan behållas under en uppgradering eller exporteras som ett arbetsflödespaket från den tidigare versionen och sedan importeras till den nya versionen.

### Övergående arbetsflöden {#transient-workflows}

Standardarbetsflöden sparar körningsinformation (historik) under körningen. Du kan också definiera en arbetsflödesmodell som **Transient** för att undvika att den historiken sparas. Det här arbetsflödet används för prestandajustering eftersom det sparar tid och resurser som används för att lagra informationen.

Övergående arbetsflöden kan användas för alla arbetsflöden som:

* körs ofta.
* behöver inte arbetsflödeshistoriken.

Övergående arbetsflöden introducerades för inläsning av många resurser, där resursinformationen är viktig, men inte arbetsflödets körningshistorik.

>[!NOTE]
>
>Mer information finns i [Skapa ett tillfälligt arbetsflöde](/help/sites-developing/workflows-models.md#creating-a-transient-workflow).

>[!CAUTION]
>
>När en arbetsflödesmodell markeras som Transient finns det några scenarier när körningsinformationen fortfarande måste bevaras:
>
>* Nyttolasttypen (till exempel video) kräver externa steg för bearbetning. I sådana fall behövs körtidshistoriken för statusbekräftelse.
>* Arbetsflödet anger en **AND-delning**. I sådana fall krävs körtidshistoriken för statusbekräftelse.
>* När det tillfälliga arbetsflödet går in i ett deltagarsteg ändras läget, vid körning, till icke-tillfälligt. När aktiviteten skickas till en person måste historiken sparas.
>

>[!CAUTION]
>
>Inom ett tillfälligt arbetsflöde bör du inte använda ett **Gå till steg**.
>
>Orsaken är att **Gå till steg** skapar ett snedjobb som kan fortsätta arbetsflödet vid punkten `goto`. Det motverkar syftet att göra arbetsflödet övergående och genererar ett fel i loggfilen.
>
>Använd **ELLER Dela** för att göra val i ett tillfälligt arbetsflöde.

>[!NOTE]
>
>Se [Bästa praxis för Assets](/help/assets/performance-tuning-guidelines.md#transient-workflows) för mer information om hur tillfälliga arbetsflöden påverkar tillgångsprestanda.

### Stöd för flera resurser {#multi-resource-support}

Om du aktiverar **Multi Resource Support** för arbetsflödesmodellen startas en enda arbetsflödesinstans även när du väljer flera resurser. Varje paket bifogas som ett paket.

Om **Multi Resource Support** inte har aktiverats för arbetsflödesmodellen och flera resurser har valts, startas en enskild arbetsflödesinstans för varje resurs.

>[!NOTE]
>
>Mer information finns i [Konfigurera ett arbetsflöde för stöd för flera resurser](/help/sites-developing/workflows-models.md#configuring-a-workflow-for-multi-resource-support).

### Arbetsflödessteg {#workflow-stages}

Med arbetsflödesfaser kan du visualisera förloppet för ett arbetsflöde när du hanterar uppgifter. De kan användas för att ge en översikt över hur långt arbetsflödet är genom bearbetning. När arbetsflödet körs kan användaren visa förloppet som beskrivs av **Stage** (till skillnad från enskilda steg).

Eftersom namnen på de enskilda stegen kan vara specifika och tekniska, kan du definiera dem för att få en konceptuell vy över arbetsflödesförloppet.

För ett arbetsflöde med sex steg och fyra steg:

1. Du kan [konfigurera arbetsflödesfaser (som visar arbetsflödets förlopp) och sedan tilldela rätt fas till varje steg i arbetsflödet](/help/sites-developing/workflows-models.md#configuring-workflow-stages-that-show-workflow-progress):

   * Du kan skapa flera scennamn.
   * Sedan tilldelas varje steg ett enskilt scennamn (ett scennamn kan tilldelas ett eller flera steg).

   | **Stegnamn** | **Scen (tilldelad till steget)** |
   |---|---|
   | Steg 1 | Skapa |
   | Steg 2 | Skapa |
   | Steg 3 | Granska |
   | Steg 4 | Godkänn |
   | Steg 5 | Complete |
   | Steg 6 | Complete |

1. När arbetsflödet körs kan användaren visa förloppet enligt scennamnen (i stället för stegnamnen). Arbetsflödesförloppet visas på fliken [ARBETSFLÖDESINFO i fönstret med uppgiftsinformation i arbetsflödesobjektet](/help/sites-authoring/workflows-participating.md#opening-a-workflow-item-to-view-details-and-take-actions) som visas i [Inkorgen](/help/sites-authoring/inbox.md).

### Arbetsflöden och Forms {#workflows-and-forms}

Vanligtvis används arbetsflöden för att bearbeta formulärinskickade formulär i AEM. Det kan vara med [kärnkomponenterna från komponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/forms/form-container.html) som är tillgängliga i en AEM-standardinstans eller med [AEM Forms-lösningen](/help/forms/using/aem-forms-workflow.md).

När du skapar ett formulär kan du enkelt koppla formuläröverföringen till en arbetsflödesmodell. Du kan till exempel lagra innehållet på en viss plats i databasen eller meddela en användare om att formuläret skickas och dess innehåll.

### Arbetsflöden och översättning {#workflows-and-translation}

Arbetsflöden ingår också i [översättningsprocessen](/help/sites-administering/translation.md).
