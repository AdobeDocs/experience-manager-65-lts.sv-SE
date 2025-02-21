---
title: Mallar för referensbrev
description: AEM Forms innehåller layoutmallar för Correspondence Management-brev som du kan använda för att snabbt skapa brev.
products: SG_EXPERIENCEMANAGER/6.3/FORMS
content-type: reference
topic-tags: correspondence-management
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Foundation Components
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Mallar för referensbrev {#reference-letter-templates}

I Correspondence Management innehåller en brevmall typiska formulärfält, layoutfunktioner som sidhuvud och sidfot samt tomma målområden för innehållsplacering.

Correspondence Management tillhandahåller brevmallar i [AEM Forms-tilläggspaketet](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en). Du kan anpassa mallarna i Designer efter företagets grafiska profil och behov. Paketet innehåller följande mallar:

* Klassisk
* Klassisk enkel
* Balanserad vänster
* Balanserad höger
* Visual Left
* Visual Top
* Visual Top - Classic

När du har installerat paketet visas layoutmallarna (XDP) i mappen templates på följande plats:

`https://'[server]:[port]'/[context-root]/aem/forms.html/content/dam/formsanddocuments/templates-folder`

Följande är de gemensamma fälten i alla mallar i det här paketet:

* Datum
* Hälsning
* Stänger text
* Signaturtext

![Alla CM-brevmallar listade](assets/templatescorrespondence.png)

När du har installerat paketet AEM-FORMS-6.3-REFERENCE-LAYOUT-TEMPLATES visas mallarna i mappen templates

## Klassisk {#classic}

Klassisk mall med en logotyp överst passar för ett vanligt professionellt brev.

![klassisk](assets/classic.png)

PDF-förhandsgranskning av ett brev som skapats med mallen Classic

## Klassisk enkel {#classic-simple}

Inkluderar fält för att hämta telefonnummer och e-postadress. En klassisk enkel mall liknar den klassiska mallen förutom att den inte har fält där du kan ange mottagarens adress.

![Kontaktinformationsfragment](assets/classicsimple.png)

PDF-förhandsgranskning av ett brev som skapats med den klassiska enkla mallen

## Balanserad vänster {#balanced-left}

Mallen Balanced Left innehåller logotyp till vänster om brevet.

![balanserat vänster](assets/balancedleft.png)

PDF-förhandsgranskning av ett brev som skapats med mallen Balanserat vänster

## Balanserad höger {#balanced-right}

Mallen Balanced Right har företagslogotypen till vänster och ger utrymme för att ange mottagarnas adress på själva brevet. Mallen för balanserat höger innehåller även en sidfot som flödar om när brevet har flera sidor.

![utjämnad höger](assets/balancedright.png)

PDF-förhandsgranskning av ett brev som skapats med mallen för balanserat höger

## Visual Left {#visual-left}

Mallen Visual Left har ett sidohuvud till vänster på sidan med företagets logotyp placerad över sidhuvudet. Mallen Visual Left har ett ämnesfält men ingen sidfot.

![visuell vänster](assets/visualleft.png)

PDF-förhandsgranskning av ett brev som skapats med mallen Visual Left

## Visual Top {#visual-top}

Visuell toppmall har en visuell marginal högst upp. Mallen Visual Top innehåller ett fält där du kan ange mottagarens adress på själva sidan. Mallen Visual Top innehåller ämnesfältet och en sidfot som flödar om för bokstäver som sträcker sig över flera sidor.

![visualtop](assets/visualtop.png)

PDF-förhandsgranskning av ett brev som skapats med mallen Visual Top

## Visual Top - Classic {#visual-top-classic}

Mallen Visual Top - Classic har en rubrik ovanför sidan med företagslogotypen. Mallen Visual Top - Classic har ett fält där du kan ange ett ämne men ingen sidfot.

![visualtopclassic](assets/visualtopclassic.png)

PDF-förhandsgranskning av ett brev som skapats med mallen Visual Top - Classic
