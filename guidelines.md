---
source-git-commit: 0deac9cb7fb13c6fec515739168eb2dbfd9a58c7
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 3%

---
# Riktlinjer för att bidra till Adobe Experience Manager-dokumentation

## Dokumentationsfilosofi

Adobe Experience Manager-användare arbetar i konkurrensutsatta miljöer och strävar efter att skapa digitala upplevelser som skiljer dem från konkurrenternas. Därför är det viktigt att Adobe, när de tillhandahåller avancerade nya verktyg i AEM, kompletterar dessa verktyg med korrekt och tydlig dokumentation som gör det möjligt för kunden att omedelbart använda sin investering i AEM och maximera avkastningen på investeringen.

Målet med AEM-dokumentationen är att skicka dokumentation till AEM-användare så snart som möjligt. Därför prioriterar AEM dokumentationsteam korrekt, användbar dokumentation och strävar efter att kontinuerligt uppdatera och förbättra den.

## Dokumentationsbidrag

I syfte att kontinuerligt förbättra AEM-dokumentationen är det välkommet att AEM-användare bidrar till dokumentationen. Vare sig det gäller förfrågningar eller frågor kan förbättringar av dokumentationen vara korrigeringar, förtydliganden, tillägg och ytterligare exempel.

## Dokumentationsstandarder

Medan Experience Manager dokumentationsgrupp välkomnar bidrag till Adobe dokumentation bör alla bidrag till AEM-dokumentationen - antingen i form av en pull-begäran eller ett problem - överensstämma med teamets bidrag- och dokumentationsstandarder.

Bidrag som inte uppfyller dessa standarder kan avvisas.

### Experience Manager dokumentationsteam dokumenterar standardanvändningsexempel.

AEM dokumentation täcker standardanvändningsfall. Användningsfall som inte omfattas av standardinstallation och -användning ingår inte i AEM-dokumentationen.

### Experience Manager dokumentationsteam dokumenterar vanligtvis inte fel eller tillfälliga lösningar.

AEM dokumentation täcker standardanvändningsfall. Av den anledningen dokumenteras inte buggar, effekter orsakade av buggar och tillfälliga lösningar för buggar.

Undantag från den här regeln gäller versionsinformationen där kända problem kan listas med möjliga lösningar som har godkänts av AEM Product Management.

### Dokumentationsbidragen är inte till för att besvara tekniska frågor.

Alla idéer du kan behöva för att förbättra AEM dokumentation är välkomna som bidrag. Kommentarer, utgåvor och pull-begäranden är dock endast avsedda för *bidrag*. De är inte avsedda att användas för att besvara frågor om hur du använder AEM, implementerar ditt AEM-projekt eller löser tekniska problem.

Om du har frågor om hur du använder AEM eller om tekniska fel som du har kan du få reda på det via den normala supportprocessen via [Experience Manager supportportal](https://experienceleague.adobe.com/?support-solution=Experience+Manager#home) eller i en diskussion i [Experience Manager-communityn](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community).

***AEM dokumentationsbidrag ersätter inte Adobe kundsupport*** och eventuella bidrag som söker svar på supportrelaterade frågor avvisas.

### Bidragen ska tydligt hänvisa till berörda dokumentationssidor.

Om du skapar ett problem som kan föreslå förbättringar av dokumentationen måste du inkludera länkar till de sidor som påverkas. Om du skapar ett ärende genom att använda länken **Redigera den här sidan** på en dokumentationssida skapas ärendet automatiskt med en länk till sidan.

Detta gäller inte för pull-begäranden eftersom pull-begäranden till sin natur refererar till de berörda sidorna.

## Riktlinjer för dokumentation

Alla bidrag till AEM-dokumentationen måste följa vissa riktlinjer för format.

Om du följer dessa riktlinjer blir det enklare att granska ditt bidrag och det går därför snabbare att integrera det i AEM-dokumentationen.

### Språk och format

#### Språk

* AEM dokumentation skrivs och underhålls på amerikansk engelska.
* Håll meningar så enkla som möjligt.
* Se till att språket är klart och koncist.

Kom ihåg att läsarna av AEM-dokumentation är från hela världen och att de inte kan förväntas vara inbyggda eller flytande engelska högtalare. Undvik kollokvialism och håll språket så tydligt och enkelt som möjligt.

#### Följ Microsoft® Manual of Style

[Handboken för Microsoft® Style](https://learn.microsoft.com/en-us/style-guide/welcome/) är en kostnadsfri handbok för dokumentationsformat som fokuserar på programvarudokumentation och AEM-dokumentation följer den här handboken när det är möjligt.

### Formatering

| Objekt | Stil |
|---|---|
| Element eller alternativ i användargränssnittet | **fet** |
| Filnamn, sökväg, användarindata, parametervärden | `monospaced` |
| Kod, kommandorad | ```Code Block``` |

### Skärmbilder

Skärmbilder ska användas med omdöme och endast när en textbeskrivning är otillräcklig.

Använd inte markörer eller andra anteckningar i skärmbilder (som röda ramar, pilar eller text). På så sätt är skärmbilderna enklare att återanvända eller replikera i lokaliserade versioner av dokumentationen.

### Versionsspecifika referenser

Undvik om möjligt direkta referenser till en viss version i dokumentationsinnehållet. Detta gör dokumentationen mer flexibel och utbyggbar för framtida versioner.

### Användning av Day, AEM, CQ, CRX

Produkten ska alltid anropas med sitt fullständiga namn **Adobe Experience Manager** för första gången i en artikel och kan därefter kallas **AEM**.

Använd inte Dag, Dag Software, CQ och CRX utom när det är oundvikligt, t.ex. i klassnamn eller med hänvisning till AEM historia.