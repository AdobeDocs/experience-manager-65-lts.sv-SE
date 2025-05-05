---
title: Ordna dina digitala resurser
description: Organisera dina digitala resurser, bilder, filer, mappar och så vidare med Experience Manager.
contentOwner: AG
role: User
feature: Asset Management,Search
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: cb7d28ce-c6bd-4760-b5fd-d0ecb3426844
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '795'
ht-degree: 0%

---

# Ordna dina digitala resurser {#organize-digital-assets}

| Version | Artikellänk |
| -------- | ---------------------------- |
| Adobe Experience Manager (AEM) as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/organize-assets.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

Alla digitala resurser, metadata och innehåll i Microsoft® Office- och PDF-dokument extraheras och blir sökbara. Sökning möjliggör avancerad filtrering av resurser och respekterar fullt ut rätt behörigheter. Metadata beskrivs i detalj i metadata i Digital Asset Management.

[!DNL Experience Manager Assets] har stöd för flera sätt att ordna innehåll. Du kan ordna dem hierarkiskt med hjälp av mappar eller så kan du ordna dem på ett oordnat sätt vid behov, t.ex. med hjälp av taggar. Användare kan redigera taggar i DAM-redigeraren där delresurser, återgivningar och metadata visas.

## Ordna resurser i mappar {#organize-using-folders}

Det mest grundläggande sättet att ordna resurser är att spara dessa i mappar. Det motsvarar att ordna filer i mappar i det lokala filsystemet. Mer information om hur du skapar och hanterar mappar finns i [Hantera resurser](manage-assets.md). Hur du namnger filer och mappar, hur du ordnar undermappar och hur du hanterar filerna i dessa mappar kan påverka hur dessa resurser bearbetas. Genom att använda enhetliga och lämpliga namngivningsstrategier för filer och mappar tillsammans med god metadatapraxis kan ni få ut det mesta av era digitala resurslager.

* Vanligtvis växer databasen med digitala resurser. Därför är det viktigt att formalisera metadataanvändning, mappstruktur och filnamngivning tidigt när du skapar innehåll.
* Använd endast mappar för att få en enhetlig lagringsstruktur för dina digitala resurser. Denna konsekvens hjälper er att arbeta och hantera ert material bättre. Resurser som placerats i följande typer av mappar kan till exempel hjälpa dig att använda rätt [profiler för resursbearbetning](processing-profiles.md):

   * **Utvecklingsmappar**: innehåller digitala resurser som du för närvarande arbetar med.
   * **Klientmappar**: innehåller digitala resurser som är baserade på klienter eller projektnamn.
   * **Primära mappar**: innehåller ursprungliga digitala källresurser.
   * **Återgivningsmappar**: innehåller återgivningar och kopior av det ursprungliga digitala källmaterialet.
   * **Filstorleksmappar**: innehåller digitala resurser baserat på liten, medelstor eller stor filstorlek.
   * **Mellanlagringsmappar**: innehåller digitala resurser som är klara att publiceras live på din webbplats.
   * **MIME-typmappar**: innehåller digitala resurser som är specifika för MIME-typer som bilder, dokument och multimedia.
   * **Arkivmappar**: innehåller kasserade digitala resurser.
   * **Datumbaserade mappar**: innehåller digitala resurser baserat på ett skapandedatum eller ett senast ändrat datum.

* Skapa en katalog med mappar som troligtvis inte ändras så att anpassningar och automatisering fortsätter att fungera. De tilldelade bearbetningsprofilerna fortsätter till exempel att fungera.
* Om en resurs redan har publicerats använder du [!DNL Experience Manager] för att flytta resursen till en annan mapp och publicera om från den nya platsen, är den ursprungliga publicerade resursplatsen fortfarande tillgänglig tillsammans med den nyligen publicerade resursen. Den ursprungliga publicerade resursen är *förlorad* till [!DNL Experience Manager] och kan inte avpubliceras. Därför bör du först avpublicera en resurs och sedan flytta den till en annan mapp.

## Ordna resurser med hjälp av taggar {#use-tags-to-organize-assets}

Med taggar, som metadata, kan du enkelt söka efter resurser, skapa samlingar med hjälp av sökresultaten, öka rankningen för vissa resurser och använda artificiella intelligensalgoritmer i Adobe Sensei för tillgångsidentifiering.

[!DNL Adobe Experience Manager Assets] använder en självlärande algoritm för att skapa väldigt beskrivande taggar som gör att du kan hitta rätt resurs med bara några klick. Smart taggning använder Adobe Sensei, Adobe ramverk för artificiell intelligens och maskininlärning, som kan utbildas för att känna igen och använda både standard- och företagsspecifika taggar på bilder. Smarta taggar kan även identifiera innehåll, enskilda ord eller fraser och automatiskt använda beskrivande taggar på resurser

Mer information finns i följande artiklar:

* [Om taggar i Experience Manager](/help/sites-authoring/tags.md)
* [Redigera metadata för resurser](metadata.md)
* [Förbättrade smarta taggar i Assets](enhanced-smart-tags.md)

## Ordna som samlingar {#organize-as-collections}

Med resurssamlingar i [!DNL Experience Manager Assets] kan du effektivisera möjligheten att skapa, redigera och dela resurser mellan användare. Skapa flera typer av samlingar baserat på hur du använder dem, inklusive samlingar som innehåller en statisk referenslista över resurser, mappar och samlingar samt samlingar som hämtar resurser baserat på sökvillkor. Du kan också skapa samlingar med resurser från olika platser och dela dem med flera användare med olika åtkomstnivåer, behörighet att visa och redigera.

Mer information finns i [Hantera samlingar](manage-collections.md).

<!-- TBD items: add screenshots where applicable
Any hints/recommendations of when to use what method of organizing? Some examples of how organizing helps towards a better taxonomy and improved content velocity.
Add back links to blog posts by marketing?
-->

## Ordna dina resurser så att de använder profiler {#organize-to-use-profiles}

En bearbetningsprofil innehåller [!DNL Assets] bearbetningskommandon som gäller för resurser som överförs till fördefinierade mappar. Profiler används för att automatisera bearbetningen av innehållet i en mapp eller nyligen överförda resurser. Du kan använda profiler för att ordna dina resurser bättre.

Genom att standardisera metadataanvändning, filnamngivning och mappstruktur säkerställer du att du kan tillämpa bearbetningsprofiler på mappar med större precision och enhetlighet när din pool med digitala resurser växer.

>[!MORELIKETHIS]
>
>* [Profiler för att bearbeta metadata, bilder och videor](processing-profiles.md).
>* [Metadataprofiler](/help/assets/metadata-config.md#metadata-profiles).
>* [Videoprofiler](video-profiles.md).
>* [Profiler för dynamiska mediabilder](image-profiles.md).
