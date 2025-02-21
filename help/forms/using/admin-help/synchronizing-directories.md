---
title: Synkroniserar kataloger
description: Lär dig synkronisera användarhanteringsdatabasen med ändringar i källkatalogservrarna med hjälp av manuell eller schemalagd synkronisering.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/setting_up_and_managing_domains
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 6352ade0206afc92e97c7ee0c4344a87b8b2cc83
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# Synkroniserar kataloger {#synchronizing-directories}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Om du vill synkronisera domäner kan du välja att göra en manuell eller schemalagd synkronisering. En *manuell synkronisering* synkroniserar alla markerade domäner. En *schemalagd synkronisering* synkroniserar alla domäner.

Katalogsynkronisering används för att hämta information från katalogservrarna som du har angett i dina kataloginställningar till databasen för användarhantering. Senare kan du även göra en manuell synkronisering om det sker ändringar eller uppdateringar på katalogservrarna. Du kan till exempel göra en manuell synkronisering om användare och grupper läggs till eller om ändringar görs i en användares konto.

Du kan också ställa in ett dagligt synkroniseringsschema så att användarhanteringsdatabasen automatiskt synkroniseras med ändringar eller uppdateringar av källkatalogservrarna. Den här processen använder dock nätverks- och serverresurser. Välj användningstider och undvik schemaläggning av onödiga synkroniseringar som knyter samman system- och nätverksresurser. Använd alternativet för omedelbar synkronisering i stället för att minimera onödiga synkroniseringar.

Du kan också ange om användar- och gruppinformation ska överföras till Adobe LiveCycle Content Services 9 (borttagen) när du synkroniserar domäner.

>[!NOTE]
>
>Skapa inte flera lokala användare och grupper medan en LDAP-katalogsynkronisering pågår. Om du försöker utföra den här processen kan det leda till fel.

>[!NOTE]
>
>Om domänsynkroniseringen avbryts (till exempel om programservern stängs av under processen) väntar du en stund innan du försöker synkronisera domänen. Om du vill utvärdera synkroniseringsstatus tittar du på statusen. Om användarhanteringen låstes innan den stängs av väntar du 10 minuter tills låset släpps när servern har startats om. Om synkroniseringsstatusen är Pågår men synkroniseringen avbryts eller har avbrutits, försöker användarhanteringen synkronisera igen efter 3 minuter. Efter tre misslyckade försök deklarerar användarhantering synkroniseringen som ett fel och frigör låset.

>[!NOTE]
>
>Adobe® LiveCycle® Content Services ES (utgått) är ett innehållshanteringssystem som installeras med LiveCycle. Det gör det möjligt för användarna att utforma, hantera, övervaka och optimera humancentrerade processer. Supporten för innehållstjänster (borttaget) upphör 2014-12-31. Se [Adobe livscykeldokument](https://www.adobe.com/support/products/enterprise/eol/eol_matrix.html).

## Aktivera deltakatalogsynkronisering {#enable-delta-directory-synchronization}

Synkronisering av Delta-katalog förbättrar effektiviteten vid katalogsynkronisering. När deltakatalogsynkronisering är aktiverat synkroniserar Hantering av användare endast användare och grupper som har lagts till eller uppdaterats sedan den senaste synkroniseringen.

Användarhantering utför följande steg när deltakatalogsynkronisering är aktiverat:

* Hämta alla användare från katalogservrarna men uppdatera databasen för användarhantering med endast de användare vars tidsstämpel har ändrats.
* Hämta alla grupper utom uppdatera databasen för användarhantering med endast de grupper vars tidsstämpel har ändrats.
* Hämta endast gruppmedlemmar för de grupper vars tidsstämplar har ändrats och uppdatera databasen för användarhantering med den informationen.

>[!NOTE]
>
>* Användare och grupper som tagits bort från katalogen tas inte bort från databasen för användarhantering förrän du utför en fullständig katalogsynkronisering.
>* Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Markera kryssrutan under Deltasynkronisering och klicka på Spara.
1. Redigera kataloginställningarna för var och en av de företagsdomäner som ska använda funktionen för deltakatalogsynkronisering. På sidorna Användarinställningar och Gruppinställningar letar du reda på inställningen Ändra tidsstämpel och anger `modify TimeStamp` som värde. Mer information om hur du redigerar företagsdomäner finns i [Redigera och konvertera befintliga domäner](/help/forms/using/admin-help/editing-converting-existing-domains.md#editing-and-converting-existing-domains).

## Aktivera eller inaktivera detaljerad loggning under synkronisering {#enable-or-disable-detailed-logging-during-synchronization}

Som standard loggar Hantering av användare detaljerad statistik under synkroniseringsprocessen.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Konfigurera avancerade systemattribut.
1. Avmarkera kryssrutan under Loggning av synkroniseringsstatistik för att inaktivera den detaljerade loggningen eller markera den för att aktivera loggning och klicka sedan på Spara.

## Konfigurera alternativet för nytt försök med katalogsynkronisering {#configure-the-directory-synchronization-retry-option}

Du kan konfigurera användarhantering så att det regelbundet görs en sökning efter misslyckade katalogsynkroniseringsförsök. Användarhantering försöker sedan slutföra de misslyckade synkroniseringarna.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Konfigurera avancerade systemattribut.
1. Under Synch Finisher Cron Expression anger du ett cron-uttryck som representerar intervallet där användarhanteringsförsök misslyckades med synkroniseringar. Användningen av cron-uttryck baseras på Quartz-systemet för jobbplanering med öppen källkod, version 1.4.0.

   Standardvärdet är 0/13 &amp;ast; ? &amp;ast; , vilket innebär att kontrollen utförs var 13:e minut.

## Synkronisera kataloger manuellt {#manually-synchronize-directories}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. (Valfritt) Om du vill överföra användar- och gruppinformation till innehållstjänster (borttagen) väljer du alternativet för att överföra användare och grupper till registrerade externa huvudlagringsleverantörer. Det här alternativet gäller också när nya användare och grupper läggs till via sidan Användare och grupper.
1. Markera kryssrutan för varje företagsdomän som ska synkroniseras och klicka på Synkronisera nu.

   Om du väljer flera domäner kan domänsynkroniseringen för alla domäner köras samtidigt. Om du väljer domänerna separat kan bara en domänsynkronisering köras åt gången.

## Schemalägg katalogsynkronisering {#schedule-directory-synchronization}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Schemalägg synkronisering:

   * Om du vill aktivera automatisk synkronisering dagligen väljer du Inträffar under Schemaläggaren. Välj Dagligen i listan och skriv in tiden i 24-timmarsformat i motsvarande ruta. När du sparar inställningarna konverteras det här värdet till ett cron-uttryck, som visas i rutan Kron-uttryck.
   * Om du vill schemalägga synkronisering på en viss dag i veckan eller månaden, eller under en viss månad, väljer du Cron Expression och skriver ett lämpligt uttryck i rutan. Synkronisera till exempel kl. 1.30 den sista fredagen i månaden.

Användningen av cron-uttryck baseras på Quartz-systemet för jobbplanering med öppen källkod, version 1.4.0.

* Om du vill inaktivera automatisk synkronisering väljer du Inträffar och väljer Aldrig i listan.
* (Valfritt) Om du vill överföra användar- och gruppinformation till innehållstjänster (borttagen) väljer du alternativet för att överföra användare och grupper till registrerade externa huvudlagringsleverantörer. Det här alternativet gäller också när nya användare och grupper läggs till via sidan Användare och grupper.
* Klicka på Spara.

## Stoppa alla pågående katalogsynkroniseringar {#stop-all-directory-synchronizations-currently-in-progress}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på Avbryt. Den här knappen visas bara när en katalogsynkronisering pågår.
