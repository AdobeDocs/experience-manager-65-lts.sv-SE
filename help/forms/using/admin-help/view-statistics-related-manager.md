---
title: Visa statistik för Work Manager
description: Fliken Arbetshanterare visar statistik som relaterar till Work Manager-objekt. Lär dig hur du kan visa och filtrera arbetsobjekten.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/health_monitor
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 7c1023ac-9d52-49f8-8e92-20e2d9d7079b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 0%

---

# Visa statistik för Work Manager {#view-statistics-related-to-work-manager}

Fliken Arbetshanterare visar statistik som relaterar till Work Manager-objekt. De här arbetsobjekten är i olika lägen beroende på var de befinner sig i processen. (Se [Status (endast för kategorierna Standard, Arbetsflöde och Händelser)](view-statistics-related-manager.md#status-for-default-workflow-or-events-categories-only).) Du kan filtrera informationen så att endast en delmängd av objekten visas med de olika alternativ som är tillgängliga (till exempel Status eller Kategori). Du kan sortera resulterande arbete eller jobbobjekt (i stigande eller fallande ordning) genom att klicka på en av kolumnrubrikerna. Du kan också hantera arbetsobjekten med de åtgärdsverktyg som visas ovanför listan med arbetsobjekt.

## Filtrera arbetsobjekten {#filter-the-work-items}

1. Klicka på fliken Arbetshanteraren.
1. Välj villkor för ett eller flera av de filter som beskrivs nedan och klicka sedan på Gå.

### Kategori {#category}

**Standard:** Alla arbetsobjekt som klienten inte tilldelade någon kategori till när de skickades. Work Manager hanterar dessa objekt och statusvärdena tillhör Work Manager.

**Jobbhanteraren:** Alla jobb som tillhör jobbhanteraren. Jobbhanteraren hanterar sina egna jobb och har egna jobbstatusvärden. Se de specifika jobbstatusarna som beskrivs nedan.

**Arbetsflöde:** Alla arbetsobjekt som tillhör arbetsflödeskörningen. Arbetsflödet hanterar inte sina egna arbetsobjekt utan är beroende av Work Manager. Statusen tillhör Work Manager.

**Händelser:** Alla arbetsobjekt som tillhör händelsehantering. Händelsehantering hanterar inte sina egna arbetsobjekt utan förlitar sig på Work Manager. Statusen tillhör Work Manager.

### Status (endast för kategorierna Standard, Arbetsflöde och Händelser) {#status-for-default-workflow-or-events-categories-only}

**Visa alla:** Visar alla aktuella arbetsuppgifter.

**Schemalagd:** Visar alla arbetsobjekt som kan köras av programservern men som inte har startats ännu.

**Pausad:** Visar alla schemalagda arbetsobjekt som klientprogrammet har pausat. Dessa objekt kan köras eller tas bort. (Se Hantera arbetsobjekt eller jobb.)

**Pågår:** Visar alla arbetsobjekt som programserverns Work Manager har hämtat och som antingen kommer att slutföras eller misslyckas. Du kan inte använda åtgärder för dessa arbetsobjekt.

**Slutförd:** Visar alla arbetsobjekt som har körts. Beständiga arbetsobjekt behålls i det här läget och icke-beständiga objekt tas bort när återanrop till återanropshanterarna slutförs. Du kan ta bort dessa objekt genom att använda åtgärden Ta bort objekt. (Se Hantera arbetsobjekt eller jobb.)

**Misslyckades:** Visar alla arbetsobjekt som inte slutfördes korrekt på grund av ett feltillstånd. Du kan göra om de här arbetsobjekten några gånger genom att använda åtgärden Försök igen. (Se Hantera arbetsobjekten eller jobben.) En fellänk i statuskolumnen ger dig åtkomst till information om felet.

**Okänd:** Visar alla arbetsobjekt vars status är okänd.

### Status (endast för jobbhanterarkategorin) {#status-for-job-manager-category-only}

**Slutförd:** Visar alla jobb som har slutförts. Beständiga arbetsobjekt behålls i det här läget och icke-beständiga objekt tas bort när återanrop till återanropshanterarna slutförs.

**Fullständigt begärd:** Visar jobb för vilka en fullständig begäran har gjorts.

**Misslyckades begärd:** Visar jobb för vilka en misslyckad begäran gjordes.

**Misslyckades:** Visar jobb som inte slutfördes korrekt på grund av ett feltillstånd. En fellänk i statuskolumnen ger dig åtkomst till information om felet.

**Avsluta begärd:** Visar jobb för vilka en avslutningsbegäran har gjorts.

**Avbruten:** Visar jobb som avslutades utan att slutföras.

**Begärt uppehåll:** Visar jobb för vilka en pausbegäran har gjorts.

**Avbrutet:** Visar jobb som har pausats.

**Återuppta begärd:** Visar jobb för vilka en CV-begäran gjordes.

**Köad:** Visar jobb som finns i kön.

**Körs:** Visar jobb som körs.

### Servernamn {#server-name}

Endast för klustrade servrar väljer du namnet på noden för att visa arbetsobjekten eller jobbobjekten som bara skapades på den servern. Om Visa alla är markerat visas alla arbetsobjekt för alla noder i ett kluster.

### Skapa tid {#create-time}

Välj ett alternativ i det här filtret om du bara vill visa de arbetsobjekt som skapades inom den tidsram du valde. Om du till exempel väljer 1 dag visas alla arbetsobjekt som skapades inom 24 timmar före den tidpunkt som angavs i filtret Föregående till.

### Före {#prior-to}

Anger det datum och den tid som filtret Skapa tid använder som slutdatum. Låt alternativet Använd aktuellt datum och tid vara markerat om du vill filtrera tillbaka från aktuellt datum och tid, eller avmarkera alternativet och ange lämpliga värden. Klicka på kalenderikonerna eller klockikonerna för att välja värden med dessa verktyg.

Om du till exempel väljer Skapa tid = 1 dag och Föregående = Använd aktuellt datum och tid returneras alla arbetsobjekt som skapades de senaste 24 timmarna.

>[!NOTE]
>
>I Oracle-databasdistributioner fungerar datumintervallfilter (d.v.s. Skapa tid och Före inställningar) inte korrekt. Använd ett annat filter för att hämta arbetsobjekt.

## Om flikgränssnittet Arbetshanteraren {#about-the-work-manager-tab-interface}

När du kör en arbetshanterarfråga eller utför en åtgärd på ett arbetsobjekt eller jobb visas ett meddelande ovanför listan. Det här meddelandet ger feedback om den åtgärd du har initierat och i vissa fall en länk till Mer information som ger mer information. Om den åtgärd du initierade till exempel misslyckades, anges så mycket som möjligt i meddelandet och en länk för att få information om felet.

När du klickar på Mer information visas en lista med de arbetsobjekt eller jobb som valdes under åtgärden i dialogrutan Åtgärdsinformation. Du kan klicka på varje listobjekt för att visa felinformationen längst ned i dialogrutan.

### Hantera arbetsuppgifter {#manage-the-work-items-or-jobs}

1. Använd åtgärdsverktygen som beskrivs nedan för att hantera arbetsobjekten eller jobben i listan.

   >[!NOTE]
   >
   >Åtgärderna är tillgängliga beroende på objektets status.

   **Ta bort objekt:** Tar bort det markerade arbetsobjektet eller jobbet.

   **Pausa objekt:** Pausar det markerade arbetsobjektet eller jobbet.

   **Återuppta objekt:** Återupptar det markerade arbetsobjektet eller jobbet från det pausade läget.

   **Försök igen:** Försöker köra det markerade arbetsobjektet eller jobbet igen från det aktuella läget.

   Du kan kontrollera om en åtgärd lyckades genom att klicka på Mer information ovanför listan. En dialogruta som innehåller de valda arbetsobjekten eller jobben och deras status visas.

## Ytterligare information om status för arbetsobjekt {#additional-information-about-work-item-statuses}

En vanlig övergång för ett arbetsobjekt är Nytt > Schemalagt > Pågår > Slutfört eller Fel.

Läget Pausad avbryter det här normala flödet. Antingen klientprogrammet eller systemadministratören kan initiera det här avbrottet (till exempel för underhåll eller uppgradering). Du kan ångra den här åtgärden genom att använda åtgärden Återuppta för att flytta arbetsobjektet tillbaka till ett schemalagt läge.

En arbetsuppgift i ett schemalagt läge är köad för körning som ännu inte har startats. Dessa objekt kan pausas eller tas bort, eller flyttas till läget Pågår när de tas från kön i Arbetshanteraren. Det går inte att ändra pågående arbetsuppgifter. De kommer antingen att slutföras eller misslyckas.

Tillståndet Misslyckades inträffar som ett resultat av ett feltillstånd som inträffar under körningen av arbetsobjektet. Om du misstänker att fel är indicier (på grund av kontexten vid tidpunkten för körningen) kan du försöka utföra körningen igen och placera arbetsposten i kön igen. Endast ett begränsat antal försök tillåts.
