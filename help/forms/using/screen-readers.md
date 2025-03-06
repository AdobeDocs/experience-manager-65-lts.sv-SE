---
title: Skärmläsare för HTML5-formulär
description: Visar en lista över skärmläsare som stöds i HTML5-formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
discoiquuid: 53c57180-7004-4534-9146-603f7770a6fe
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: cf652b91-ee92-4d54-8a29-2653d882d5f2
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# Skärmläsare för HTML5-formulär {#screen-readers-for-html-forms}

HTML5-formulärkomponenter återger XFA-formulärmallen till ett HTML5-format. Alla standardwebbläsare som stöder HTML5 kan återge dessa formulär. För att ge stöd för liknande datainhämtning i PDF- och HTML5-formulär behålls layouten för PDF forms i HTML5-formulär.

HTML5-blanketter använder HTML standardkonstruktioner som gör det möjligt att använda vanliga hjälpmedelsverktyg för HTML för dessa blanketter. Om ett formulär är utformat enligt bästa praxis för hjälpmedelsförberedda formulär fungerar det med alla skärmläsare som stöds. Dessutom är sådana formulär aktiverade för tangentbordsnavigering.

## Tillgänglighetsstandarder {#accessibility-standards}

HTML5-blanketter uppfyller kraven i avsnitt 508 för tillgänglighet med kända undantag. Mer information finns i [VPAT för HTML5-formulär](https://www.adobe.com/content/dam/cc1/en/accessibility/compliance/pdfs/adobe-livecycle-es4-section-508-vpat-portfolio.pdf).

## Certifierade skärmläsare för HTML5-formulär {#certified-screen-readers-for-html-forms}

* JAWS 14.0 i Microsoft® Windows
* VoiceOver på macOS X och iPad

### JAWS {#jaws}

Alla standardtangenttryckningar och kortkommandon fungerar för HTML5-formulär. Mer information om JAWS finns på [https://www.freedomscientific.com/jaws-hq.asp](https://www.freedomscientific.com/jaws-hq.asp).

### VoiceOver {#voiceover}

HTML5-formulär har stöd för alla standardtangenttryckningar och -gester i Voice over. Mer information om hur du konfigurerar och använder VoiceOver finns i [https://www.apple.com/accessibility/vision/](https://www.apple.com/accessibility/vision/).

## Kända fel {#known-issues}

* **(Endast Internal Explorer 9)** I HTML5-formulär läses sidorna in vid behov (dynamiskt). Sidinläsning on demand orsakar problem med skärmläsarnas funktion. När skärmläsarens fokus är på det sista fältet på sidan och användaren trycker på fliken, återgår skärmläsaren fokus till det första fältet på formulärets första sida.
* **(Endast för Internal Explorer 9)** Kontrollen för datumväljaren i HTML5-formulär är inte helt tillgänglig med tangentbordet. Om du trycker på upp-/nedtangenterna flera gånger i datumväljaren stängs datumväljaren och fokus flyttas till nästa/sista fält.

* VoiceOver kan inte identifiera piltangenter på datumwidgeten i iPad Safari.
