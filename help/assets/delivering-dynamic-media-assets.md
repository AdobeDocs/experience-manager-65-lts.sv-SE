---
title: Leverera dynamiska medieresurser
description: Lär dig hur du levererar mediematerial för dynamiska medier, som video och bilder, till dina webbsidor.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
role: User, Admin
feature: Asset Management,Renditions
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Leverera dynamiska medieresurser{#delivering-dynamic-media-assets}

Hur ni kan leverera ert mediematerial i Dynamic Media - både video och bilder - beror på hur er webbplats implementeras.

Med Dynamic Media har du flera alternativ:

* Om din webbplats ligger hos Adobe Experience Manager vill du lägga till resurserna för dynamiska media direkt på din sida.
* Om din webbplats inte finns på Experience Manager kan du välja något av följande:

   * Bädda in videon eller bilden på webbplatsen.
   * Länka URL:er till webbprogrammet. Använd länkning när du vill leverera en videospelare som ett popup-fönster eller modalt fönster.
   * Om din webbplats är responsiv kan du [leverera optimerade bilder](/help/assets/responsive-site.md).

>[!NOTE]
>
>Smart bildbehandling fungerar med befintliga bildförinställningar och använder intelligens under de sista millisekunderna i leveransen för att ytterligare minska bildfilens storlek baserat på webbläsarens eller nätverkets anslutningshastighet. Mer information finns i [Smart bildbehandling](/help/assets/imaging-faq.md).

Mer information finns i följande avsnitt:

* [Lägga till dynamiska medieresurser på webbsidor](/help/assets/adding-dynamic-media-assets-to-pages.md)
* [Bädda in video- eller bildvisningsprogrammet på en webbsida](/help/assets/embed-code.md)
* [Aktivera hotlink-skydd i Dynamic Media](/help/assets/hotlink-protection.md)
* [Länka URL:er till ditt webbprogram](/help/assets/linking-urls-to-yourwebapplication.md)
* [Leverera optimerade bilder för en responsiv webbplats](/help/assets/responsive-site.md)
* [HTTP2-leverans av innehåll](/help/assets/http2.md)
* [Gör CDN-cachen ogiltig med Dynamic Media Classic](/help/assets/invalidate-cdn-cache-dm-classic.md)
* [Använd regeluppsättningar för att omforma URL:er](/help/assets/using-rulesets-to-transform-urls.md)


## HTTP/2-leverans av Dynamic Media-resurser {#http-delivery-of-dynamic-media-assets}

Experience Manager stöder nu leverans av allt dynamiskt medieinnehåll (bilder och video) via HTTP/2. Det betyder att en publicerad URL eller inbäddningskod för bilden eller videon är tillgänglig för integrering med alla program som accepterar en värdbaserad resurs. Den publicerade resursen levereras sedan via HTTP/2-protokollet. Den här leveransmetoden förbättrar sättet som webbläsare och servrar kommunicerar på, vilket ger bättre respons och laddningstider för alla dynamiska medieresurser.

Mer information finns i [HTTP/2-leverans av innehåll, vanliga frågor och svar](/help/sites-administering/scene7-http2faq.md).
