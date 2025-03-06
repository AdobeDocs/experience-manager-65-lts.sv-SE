---
title: JSON-objektbeskrivning för AEM Forms arbetsyta
description: Konceptuell information om JSON JavaScript-objekt som används i LiveCycle AEM Forms-arbetsytan för anpassning, tillägg, ändring och återanvändning.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: Admin, User, Developer
exl-id: 1b6c09f7-6f89-4fe9-8217-bf1a301bf9cb
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2144'
ht-degree: 0%

---

# JSON-objektbeskrivning för AEM Forms arbetsyta {#aem-forms-workspace-json-object-description}

JSON-objekt som används i AEM Forms-arbetsytan beskrivs nedan.

1. Kategori

   Kategorier finns på arbetsytans startprocessflik. Dessa kategorier används för att klassificera startpunkterna.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>name</td>
   <td>F</td>
   <td>Kategorinamn</td>
  </tr>
  <tr>
   <td>id</td>
   <td>F</td>
   <td>Kategori-ID <br type="_moz" /> </td>
  </tr>
  <tr>
   <td>description<br type="_moz" /> </td>
   <td>F</td>
   <td>Kategoribeskrivning<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>parentOid<br type="_moz" /> </td>
   <td>F</td>
   <td>Innehåller ingen överordnad kategori <br type="_moz" /> </td>
  </tr>
  <tr>
   <td>startPointsList<br type="_moz" /> </td>
   <td>T</td>
   <td>Innehåller en lista med alla startpunkter i en kategori</td>
  </tr>
  <tr>
   <td>categoryList</td>
   <td>T</td>
   <td>Innehåller en lista med direkta underordnade kategorier i en kategori <br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Alla startpunkter och Favoriter är kategorier som definieras på klientsidan. Favoritkategorin innehåller alla startpunkter som användaren har markerat som favoriter. Kategorin Alla startpunkter innehåller alla startpunkter.

1. Startpunkt

   Startpunkten används för att starta en process från arbetsytan när den anropas.

   | **Egenskap** | **Endast klient** | **Kommentarer** |
   |---|---|---|
   | categoryId | F | Den innehåller ID för den kategori som startpunkten tillhör. |
   | description | F | Den innehåller en beskrivning för en startpunkt. |
   | name | F | Den innehåller namnet på startpunkten. |
   | serializedImageTicket | F | Den innehåller en bildbiljett som motsvarar startpunkten. Den här bildbiljetten används i fältet imageUrl i startpunkten för att hämta bilden för startpunkten från servern. |
   | serviceName | F | Det innehåller namnet på tjänsten för startpunkten. |
   | startpointId | F | Den innehåller ID för startpunkt. |
   | isFavorite | T | Anger om startpunkten är favorit eller inte. True if startpoint is favorite else false. |
   | isDefaultImage | T | Anger om det finns en bild angiven för bearbetning eller inte. True if there is no image associated with process else false. |
   | uppgift | T | Den innehåller uppgift som skapas när startpunkten anropas. |
   | imageUrl | T | Den innehåller URL för bilden som motsvarar startpunkten. |

1. Uppgift

   Uppgifter tilldelas användare/grupp och innehåller ett användargränssnitt - ett formulär eller en guide (utgått) - som kan fyllas i med data. När användare tilldelas en uppgift får de formuläret eller handboken för att slutföra och skicka.

<table>
 <tbody>
  <tr>
   <td>Egenskap<br /> </td>
   <td>Endast klient <br /> </td>
   <td>Kommentarer<br /> </td>
  </tr>
  <tr>
   <td>classOfTask</td>
   <td>F</td>
   <td>Klassen för aktiviteten är LC8 när aktiviteten är lc8-aktiviteten else Standard.<br /> </td>
  </tr>
  <tr>
   <td>completeTime<br /> </td>
   <td>F</td>
   <td>Den innehåller tidsstämpeln när aktiviteten har slutförts.<br /> </td>
  </tr>
  <tr>
   <td>SeeGroupId<br /> </td>
   <td>F</td>
   <td>Den innehåller ID för en grupp som uppgiften kan läsas av. Den ställs in under processdesignen.<br /> </td>
  </tr>
  <tr>
   <td>createTime<br /> </td>
   <td>F</td>
   <td>Den innehåller tidsstämpeln när aktiviteten skapas.<br /> </td>
  </tr>
  <tr>
   <td>creationId<br /> </td>
   <td>F</td>
   <td>Den innehåller ID:t för användaren som skapade aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>currentAssignment<br /> </td>
   <td>F</td>
   <td>Den innehåller information om aktuell tilldelning av aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>deadline<br /> </td>
   <td>F</td>
   <td>Den innehåller den tidsstämpel som används när en aktivitet når sin deadline.<br /> </td>
  </tr>
  <tr>
   <td>description<br /> </td>
   <td>F</td>
   <td>Den innehåller en beskrivning av aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>displayName<br /> </td>
   <td>F</td>
   <td>Den innehåller aktivitetens visningsnamn.<br /> </td>
  </tr>
  <tr>
   <td>forwardGroupId<br /> </td>
   <td>F</td>
   <td>Den innehåller ID för en grupp som uppgiften kan vidarebefordras till. Den ställs in under processdesignen.<br /> </td>
  </tr>
  <tr>
   <td>instruktioner<br /> </td>
   <td>F</td>
   <td>Den innehåller instruktioner för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>isLocked<br /> </td>
   <td>F</td>
   <td>True om aktiviteten är låst.<br /> </td>
  </tr>
  <tr>
   <td>isMustOpenToComplete<br /> </td>
   <td>F</td>
   <td>True if task form must open to complete the task.<br /> </td>
  </tr>
  <tr>
   <td>isOpenFullScreen<br /> </td>
   <td>F</td>
   <td>Om värdet är true aktiveras helskärmsskärmen första gången formuläret öppnas.<br /> </td>
  </tr>
  <tr>
   <td>isRouteSelectionRequired<br /> </td>
   <td>F</td>
   <td>Om true måste du välja flöde för att slutföra uppgiften.<br /> </td>
  </tr>
  <tr>
   <td>isShowAttachments<br /> </td>
   <td>F</td>
   <td>Bifogade filer visas om det är sant.<br /> </td>
  </tr>
  <tr>
   <td>isStartTask<br /> </td>
   <td>F</td>
   <td>Om true skapas aktiviteten från startpunkten.<br /> </td>
  </tr>
  <tr>
   <td>isVisible<br /> </td>
   <td>F</td>
   <td>True if task is visible in workspace.<br /> </td>
  </tr>
  <tr>
   <td>nextReminder<br /> </td>
   <td>F</td>
   <td>Tidsstämpel för nästa påminnelse.<br /> </td>
  </tr>
  <tr>
   <td>prioritet<br /> </td>
   <td>F</td>
   <td>Den innehåller uppgiftens prioritet.<br /> 1 = Högsta prioritet<br /> 2 = Hög prioritet <br /> 3 = Normal prioritet <br /> 4 = Låg prioritet <br /> 5 = Lägsta prioritet<br /> </td>
  </tr>
  <tr>
   <td>processInstanceId</td>
   <td>F</td>
   <td>ID för processinstansen som aktiviteten är en del av.<br /> </td>
  </tr>
  <tr>
   <td>processInstanceStatus<br /> </td>
   <td>F</td>
   <td>Status för aktivitetens processinstans.<br /> </td>
  </tr>
  <tr>
   <td>reminderCount<br /> </td>
   <td>F</td>
   <td>Den innehåller antal påminnelser för aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>routeList<br /> </td>
   <td>F</td>
   <td>Den innehåller en lista med vägar som är associerade med aktiviteten. Användaren kan slutföra uppgiften genom att välja någon av rutterna i ruttlistan.<br /> </td>
  </tr>
  <tr>
   <td>selectedRoute<br /> </td>
   <td>F</td>
   <td>Den innehåller namnet på den väg som valdes när aktiviteten slutfördes.<br /> </td>
  </tr>
  <tr>
   <td>serializedImageTicket<br /> </td>
   <td>F</td>
   <td>Den innehåller bildbiljett som motsvarar uppgiften. Den här bildbiljetten används i aktivitetsfältet imageUrl för att hämta bilden för aktiviteten från servern.<br /> <br /> </td>
  </tr>
  <tr>
   <td>serviceName<br /> </td>
   <td>F</td>
   <td>Det innehåller namnet på tjänsten för aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>serviceTitle<br /> </td>
   <td>F</td>
   <td>Den innehåller tjänstens titel för aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>status<br /> </td>
   <td>F</td>
   <td>1 = Skapad (aktiviteten skapas från startpunkten)<br /> 2 = Skapad och sparad (aktiviteten skapas från startpunkten och sparas.)<br /> 3 = Tilldelad (uppgiften tilldelas användaren när processen har startats.)<br /> 4 = Tilldelad och sparad (uppgiften har tilldelats och sparats.)<br /> 100 = Slutförd (uppgiften har slutförts.)<br /> 101 = Deadline (aktiviteten har nått tidsgränsen.)<br /> 102 = Avbruten<br /> </td>
  </tr>
  <tr>
   <td>stepName<br /> </td>
   <td>F</td>
   <td>Den innehåller namnet på uppgiftsuppsättningen under processdesignen.<br /> </td>
  </tr>
  <tr>
   <td>summaryUrl<br /> </td>
   <td>F</td>
   <td>Den innehåller en URL för uppgiftssammanfattning.<br /> </td>
  </tr>
  <tr>
   <td>taskACL<br /> </td>
   <td>F</td>
   <td>Det är en åtkomstkontrollista för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>taskId<br /> </td>
   <td>F</td>
   <td>ID för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>updateTime<br /> </td>
   <td>F</td>
   <td>Tidsstämpel när aktiviteten senast uppdaterades.<br /> </td>
  </tr>
  <tr>
   <td>formUrl<br /> </td>
   <td>T</td>
   <td>Den innehåller en URL för ett formulär för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>taskFormType<br /> </td>
   <td>T</td>
   <td>Den innehåller formulärtyp för uppgift. Med det här fältet återges aktiviteten på klienten som PDF för, swf-formulär och så vidare.<br /> </td>
  </tr>
  <tr>
   <td>showDirectActions<br /> </td>
   <td>T</td>
   <td>Om true visas flödesåtgärder på arbetsytan.<br /> </td>
  </tr>
  <tr>
   <td>showACLAactions<br /> </td>
   <td>T</td>
   <td>Om true visas åtgärder som framåtblickande, konsultera och dela på arbetsytan.<br /> </td>
  </tr>
  <tr>
   <td>supportsOffline<br /> </td>
   <td>T</td>
   <td>Om true kan formuläret tas offline. Detta gäller endast PDF-formulär.<br /> </td>
  </tr>
  <tr>
   <td>supportsSave<br /> </td>
   <td>T</td>
   <td>Om true kan användaren spara aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>readerSubmitOptions<br /> </td>
   <td>T</td>
   <td>Det här objektet innehåller alternativ som används för att skicka PDF-formulär via läsare om PDF-formuläret inte innehåller någon skicka-knapp.<br /> </td>
  </tr>
  <tr>
   <td>isDefaultImage<br /> </td>
   <td>T</td>
   <td>Anger om det finns en bild angiven för bearbetning eller inte. True if there is no image associated with process else false.<br /> </td>
  </tr>
  <tr>
   <td>historyTaskList<br /> </td>
   <td>T</td>
   <td>Den innehåller en lista över aktiviteter som används på fliken Historik för aktivitetsinformation.<br /> </td>
  </tr>
  <tr>
   <td>isOwner<br /> </td>
   <td>T</td>
   <td>True if logged in user is owner of task.<br /> </td>
  </tr>
  <tr>
   <td>availableCommands<br /> </td>
   <td>T</td>
   <td>Den innehåller alla åtgärder som kan utföras för aktiviteten.<br /> </td>
  </tr>
  <tr>
   <td>availableCommands.directCommands<br /> </td>
   <td>T</td>
   <td>Den innehåller alla vägåtgärder som är tillgängliga för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>availableCommands.taskACLCommands<br /> </td>
   <td>T</td>
   <td>Den innehåller kommandon som vidarebefordra, dela och fråga om en uppgift är tillgänglig.<br /> </td>
  </tr>
  <tr>
   <td>availableCommands.otherCommands<br /> </td>
   <td>T</td>
   <td>Den innehåller kommandon som lock, unlock,quit, return, claim, osv., som är tillgängliga.<br /> </td>
  </tr>
  <tr>
   <td>processInstanceInfo<br /> </td>
   <td>T</td>
   <td>Den innehåller information om aktivitetens processinstans.<br /> </td>
  </tr>
  <tr>
   <td>processVariables<br /> </td>
   <td>T<br /> </td>
   <td>Den innehåller en array med objekt av processvariabler om sådana finns.<br /> </td>
  </tr>
  <tr>
   <td>pendingTasks<br /> </td>
   <td>T</td>
   <td>Den innehåller en lista över väntande aktiviteter för aktivitetens processinstans.<br /> </td>
  </tr>
  <tr>
   <td>userActions<br /> </td>
   <td>T</td>
   <td>Det är en array med objekt. Varje objekt innehåller information om flödet och motsvarande bekräftelsemeddelande om det finns.<br /> </td>
  </tr>
  <tr>
   <td>dataUrl<br /> </td>
   <td>T</td>
   <td>Det är URL för data i formuläret för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>externalAppConfig<br /> </td>
   <td>T</td>
   <td>Detta är en konfiguration för programformulär från tredje part.<br /> </td>
  </tr>
  <tr>
   <td>skickat<br /> </td>
   <td>T</td>
   <td>True om aktiviteten har skickats.<br /> </td>
  </tr>
  <tr>
   <td>bilagor<br /> </td>
   <td>T</td>
   <td>Lista över bifogade filer för en aktivitet.<br /> </td>
  </tr>
  <tr>
   <td>tilldelningar<br /> </td>
   <td>T</td>
   <td>Lista över tilldelningar av en aktivitet.<br /> </td>
  </tr>
 </tbody>
</table>

1. Filter

   Filtret är i princip en kö med användare eller grupper. När en uppgift tilldelas till en användare/grupp läggs uppgiften till i motsvarande kö.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>isDefault <br type="_moz" /> </td>
   <td>F</td>
   <td>True if queue is default queue of the loggin user, else false.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>namn<br type="_moz" /> </td>
   <td>F</td>
   <td>Namnet på köägaren.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>qid</td>
   <td>F</td>
   <td>ID för kön.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>type</td>
   <td>F</td>
   <td>Den innehåller typen av kö.<br /> 0 - Användarkö.<br /> 1. Delad kö.<br /> 2. Gruppkö.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>fråga</td>
   <td>T</td>
   <td>Detta innehåller en fråga som är associerad med ett filter. Den här frågan används för att söka efter aktiviteter från den fullständiga aktivitetslistan.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>uppgifter</td>
   <td>T</td>
   <td>Den innehåller en lista med alla aktiviteter som tillhör ett filter.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. Frånvarande

   Du kan hantera ditt frånvaroschema och styra flödet av uppgifter som tilldelats dig om du inte är anställd.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong><br type="_moz" /> </td>
   <td><strong>Endast klient</strong><br type="_moz" /> </td>
   <td><strong>Kommentarer</strong><br type="_moz" /> </td>
  </tr>
  <tr>
   <td>dateRanges<br type="_moz" /> </td>
   <td>F</td>
   <td>Den innehåller arrayobjekt för användarens inbyggda scheman. I varje schemaobjekt innehåller fältet startDate schemats startdatum och fältet endDate innehåller schemats slutdatum. Om endDate har värdet null i schemat innebär det att användaren inte har schemalagt slutdatumet för schemat utanför kontoret.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>isNoPrimaryDesignate<br type="_moz" /> </td>
   <td>F</td>
   <td>True if there is no primär designate in case if user is out-of-office.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>isOutOfOffice<br type="_moz" /> </td>
   <td>F</td>
   <td>True if user is out-of-office.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>outOfOfficeDesignate<br type="_moz" /> </td>
   <td>F</td>
   <td>Den innehåller information om användare som har tilldelats som primär beteckning av användaren.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processSpecificDesignates<br type="_moz" /> </td>
   <td>F</td>
   <td>Den innehåller en array med objekt för processspecifika, ej kontorsrelaterade designer. I varje processspecifikt designobjekt innehåller processName processens namn, isNotDesignated är true om ingen användare har tilldelats för motsvarande process och userDesignated är null om ingen användare har tilldelats någon annan information om användaren som tilldelats för motsvarande process.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processer<br type="_moz" /> </td>
   <td>T</td>
   <td>Den innehåller en lista över alla processer som är tillgängliga för användaren.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>initialOutOfOfficeSettings<br type="_moz" /> </td>
   <td>T</td>
   <td>Den innehåller ursprungliga användarens inställningar utanför kontoret som hämtas från början.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>outOfOfficeSettings<br type="_moz" /> </td>
   <td>T</td>
   <td>Den innehåller ändrade inställningar utanför kontoret.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>userSearchHistory<br type="_moz" /> </td>
   <td>T</td>
   <td>Den innehåller en lista över användare som genomsöks av en inloggad användare fram till datumet.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. Processinstans

   En processinstans skapas när en process anropas via arbetsytan eller workbench.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>description<br type="_moz" /> </td>
   <td>F</td>
   <td>Beskrivning av processinstansen <br type="_moz" /> </td>
  </tr>
  <tr>
   <td>initiator</td>
   <td>F</td>
   <td>Namnet på initieraren för en processinstans.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>initiatorId</td>
   <td>F</td>
   <td>ID för initieraren för processinstansen.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processCompleteTime<br type="_moz" /> </td>
   <td>F</td>
   <td>Tidsstämpel när processen har slutförts.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processInstanceId<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för processinstans.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processInstanceStatus<br type="_moz" /> </td>
   <td>F</td>
   <td>0 = Startades<br /> 1 = Körs<br /> 2 = Fullständigt<br /> 3 = Slutför<br /> 4 = Avbrutet<br /> 5 = Avbryter<br /> 6 = Upphävt<br /> 7 = Pausat <br /> 8 = Upphävande<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processName<br type="_moz" /> </td>
   <td>F</td>
   <td>Processens namn.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processStartTime<br type="_moz" /> </td>
   <td>F</td>
   <td>Tidsstämpel när processen har startats.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processVariables<br type="_moz" /> </td>
   <td>F</td>
   <td>Array med objekt av processvariabler. Varje processvariabelobjekt innehåller ett namn som är namn på processvariabeln, ett värde som är värdet på processvariabeln och en typ som är typ av processvariabel.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>aktivitetslista<br type="_moz" /> </td>
   <td>T</td>
   <td>Aktiviteter som genereras av den här processinstansen.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. Processnamn

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>processMajorVersion<br type="_moz" /> </td>
   <td>F</td>
   <td>Huvudversion av en process.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processMinorVersion<br type="_moz" /> </td>
   <td>F</td>
   <td>Mindre version av en process.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processName<br type="_moz" /> </td>
   <td>F</td>
   <td>Processens namn.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processTitle<br type="_moz" /> </td>
   <td>F</td>
   <td>Processens titel.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>processInstanceList<br type="_moz" /> </td>
   <td>T</td>
   <td>Lista över processinstanser för den här processen.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. Uppgiftstilldelningsobjekt

   Uppgiftstilldelningsobjektet innehåller information om tilldelning av uppgift. Här följer egenskaperna för uppgiftens tilldelning.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>assignCreateTime<br type="_moz" /> </td>
   <td>F</td>
   <td>Tidsstämpel när den här tilldelningen av en uppgift skapas.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>assignType<br type="_moz" /> </td>
   <td>F</td>
   <td>0 = Inledande tilldelning<br /> 1 = Framåt (aktiviteten har vidarebefordrats till den aktuella ägaren av aktiviteten.)<br /> 2 = Returnerad (aktiviteten har returnerats till den aktuella ägaren av uppgiften av den tidigare ägaren av uppgiften.)<br /> 3 = Anspråk (Aktiviteten har tagits i anspråk av den aktuella ägaren av aktiviteten.)<br /> 4 = Eskalering (Aktiviteten har tilldelats den aktuella ägaren av aktiviteten efter eskalering.)<br /> 5 = Administratör tilldelad (aktiviteten har tilldelats av administratören till den aktuella ägaren av uppgiften.)<br /> 6 = Konsulterad (aktiviteten har konsulterats till den aktuella ägaren av uppgiften.)<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>assignUpdateTime<br type="_moz" /> </td>
   <td>F</td>
   <td>Tidsstämpel när den här tilldelningen av en aktivitet uppdateras.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>queueId<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för kö för den aktuella ägaren av uppgiften.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>queueOwner<br type="_moz" /> </td>
   <td>F</td>
   <td>Namn på den aktuella ägaren av uppgiften.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>queueOwnerId<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för den aktuella aktivitetens ägare.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. ACL-objekt för aktivitet

   ACL-objektet för aktiviteten innehåller information om behörigheter som vidarebefordra, dela, läsa och så vidare för en uppgift. Här följer egenskaperna för aktivitetens ACL.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>canAddAttachments<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true kan bilagor läggas till i aktiviteten.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>canAddNotes<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true kan anteckningar läggas till i aktiviteten.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>canClaim<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true kan aktiviteten tas i anspråk.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>canConsult<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true kan aktiviteten genomsökas.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>canForward<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true kan aktiviteten vidarebefordras.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>canShare<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true kan aktiviteten delas.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. Bifogad uppgift

   Bifogade filer kan läggas till i en uppgift. Bifogade filer kan vara av typen Bifogade filer och anteckningar. Här följer egenskaperna för ett objekt i en bifogad fil.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>creationDate<br type="_moz" /> </td>
   <td>F</td>
   <td>Tidsstämpel när bifogad fil skapas.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>creatorId<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för den användare som lade till den bifogade filen.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>creatorName<br type="_moz" /> </td>
   <td>F</td>
   <td>Namnet på den användare som lade till den bifogade filen.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>description<br type="_moz" /> </td>
   <td>F</td>
   <td>Beskrivning av den bifogade filen.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>fileName<br type="_moz" /> </td>
   <td>F</td>
   <td>Namnet på den bifogade filen.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>id<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för bifogad fil.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>lastModifiedDate<br type="_moz" /> </td>
   <td>F</td>
   <td>Tidsstämpel när den bifogade filen senast ändrades.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>noteExtended<br type="_moz" /> </td>
   <td>F</td>
   <td>Om true är anteckningen en utökad (lång) anteckning.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>behörigheter<br type="_moz" /> </td>
   <td>F</td>
   <td>Behörigheter som är kopplade till en bifogad fil. Fältet allowRead är för läsbehörighet, allowWrite är för skrivbehörighet, allowDelete är för borttagningsbehörighet.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>storlek <br type="_moz" /> </td>
   <td>F</td>
   <td>Storlek på bifogad fil i byte.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>taskId<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för aktiviteten som den bifogade filen läggs till i.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>typ <br type="_moz" /> </td>
   <td>F</td>
   <td>Typen är bifogad för filer och Type är anteckning för anteckningar.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>formattedCreationDate<br type="_moz" /> </td>
   <td>T</td>
   <td>Det innehåller datum då bilagan skapades enligt användarens gränssnittsinställningar.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>formattedDescription<br type="_moz" /> </td>
   <td>T</td>
   <td>Formaterad beskrivning av bifogad fil. Används för att visa specialtecken i bilagebeskrivningen på arbetsytan i AEM Forms.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>formattedFileName<br type="_moz" /> </td>
   <td>T</td>
   <td>Formaterat namn på bifogad fil. Används för att visa specialtecken i bilagenamn på arbetsytan i AEM Forms. Detta är endast för anteckningar.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

1. Användare

   Följande är egenskaperna för användarobjektet.

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Endast klient</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>adress<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens adress.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>commonName<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens allmänna namn.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>description<br type="_moz" /> </td>
   <td>F</td>
   <td>Beskrivning av användaren.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>directGroupMemberships<br type="_moz" /> </td>
   <td>F</td>
   <td>Lista över användarens grupp.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>displayName<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens visningsnamn.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>e-post<br type="_moz" /> </td>
   <td>F</td>
   <td>E-post-ID för användaren.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>isOutOfOffice<br type="_moz" /> </td>
   <td>F</td>
   <td>True if user is out-of-office.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>lastName<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens efternamn.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>firstName<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens förnamn.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>oid<br type="_moz" /> </td>
   <td>F</td>
   <td>ID för användaren.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>organisation<br type="_moz" /> </td>
   <td>F</td>
   <td>Namnet på användarens organisation.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>mailAddress<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens postadress.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>telefon<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens kontaktnummer.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>phoneNumber<br type="_moz" /> </td>
   <td>F</td>
   <td>Användarens kontaktnummer.<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>userid<br type="_moz" /> </td>
   <td>F</td>
   <td>Inloggnings-ID för användaren.<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>
