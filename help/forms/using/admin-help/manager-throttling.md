---
title: Arbetshanteraren och begränsning
description: Det här dokumentet innehåller bakgrundsinformation om Work Manager och anvisningar om hur du konfigurerar begränsningsalternativ för Work Manager.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_aem_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: e0aa9304-3da0-4ae6-a465-089dc96c427e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1042'
ht-degree: 0%

---

# Arbetshanteraren och begränsning{#work-manager-and-throttling}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

AEM-formulär (och tidigare versioner) använde JMS-köer för att köra åtgärder asynkront. I AEM-formulär har JMS-köer ersatts av Work Manager. Det här dokumentet innehåller bakgrundsinformation om Work Manager och anvisningar om hur du konfigurerar begränsningsalternativ för Work Manager.

## Om långvariga (asynkrona) åtgärder {#about-long-lived-asynchronous-operations}

I AEM-formulär kan åtgärder som utförs av tjänster vara antingen kortlivade (synkrona) eller långlivade (asynkrona). Kortlivade åtgärder slutförs synkront på samma tråd som de anropades från. Dessa åtgärder väntar på ett svar innan de fortsätter.

Långvariga operationer kan spänna över flera system eller till och med sträcka sig utanför organisationen, t.ex. när en kund måste fylla i och skicka in en låneblankett som en del av en större lösning som integrerar flera automatiserade och mänskliga uppgifter. Sådana åtgärder måste fortsätta i väntan på svar. Långvariga åtgärder utför sitt underliggande arbete asynkront, vilket gör att resurser kan användas på annat sätt i väntan på slutförande. Till skillnad från en kortvarig åtgärd hanterar Work Manager inte en långvarig åtgärd som slutförs när den anropas. En extern utlösare, till exempel ett system som begär en annan åtgärd på samma tjänst eller en användare som skickar ett formulär, måste inträffa för att åtgärden ska kunna slutföras.

## Om Work Manager {#about-work-manager}

AEM-formulär (och tidigare versioner) använde JMS-köer för att köra åtgärder asynkront. AEM-formulär använder Work Manager för att schemalägga och köra asynkrona åtgärder via hanterade trådar.

Asynkrona åtgärder hanteras på följande sätt:

1. Work Manager tar emot en arbetsuppgift för körning.
1. Arbetshanteraren lagrar arbetsuppgiften i en databastabell och tilldelar en unik identifierare till arbetsuppgiften. Databasposten innehåller all information som krävs för att köra arbetsposten.
1. Arbetshanterartrådar hämtar in arbetsobjekt när trådarna blir kostnadsfria. Innan du lägger in arbetsuppgifter kan trådarna kontrollera om de tjänster som krävs har startats, om det finns tillräckligt med stackstorlek för att dra in nästa arbetsuppgift och om det finns tillräckligt med CPU-cykler för att bearbeta arbetsuppgiften. Arbetshanteraren utvärderar också attribut för arbetsuppgiften (till exempel prioritet) när körningen schemaläggs.

AEM formuläradministratörer kan använda Health Monitor för att kontrollera statistik för Work Manager, t.ex. antalet arbetsobjekt i kön och deras status. Du kan också använda Hälsoövervakning för att pausa, återuppta, försöka igen eller ta bort arbetsobjekt. (Se [Visa statistik för Work Manager](/help/forms/using/admin-help/view-statistics-related-manager.md#view-statistics-related-to-work-manager).)

## Konfigurera begränsningsalternativ för Work Manager {#configuring-work-manager-throttling-options}

Du kan konfigurera begränsning för Work Manager så att arbetsobjekt schemaläggs endast när det finns tillräckligt med minnesresurser. Du konfigurerar begränsning genom att ange följande JVM-alternativ på programservern.

<table>
 <thead>
  <tr>
   <th><p>Egenskap</p></th>
   <th><p>Beskrivning</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><code> adobe.work-manager.queue-refill-interval</code></td>
   <td><p>Anger det tidsintervall i millisekunder som används i Arbetshanteraren vid sökning efter nya objekt i kön.</p><p>Värdet för det här alternativet är ett heltal. Standardvärdet är <code>1000</code> millisekunder (1 sekund). </p><p>Om volymen för asynkrona anrop är låg kan du öka värdet. Du kan till exempel öka den till något mellan 2 000 och 5 000 (2-5 sekunder). </p><p>Om volymen för asynkrona anrop är hög bör standardvärdet vara tillräckligt, men du kan använda ett lägre värde om det behövs. Om du minskar det här värdet för mycket (till exempel under 50, vilket ger en avfrågningsfrekvens på 20 gånger per sekund) genereras en avsevärd belastning på systemet.</p></td>
  </tr>
  <tr>
   <td><code> adobe.workmanager.debug-mode-enabled</code></td>
   <td><p>Ange det här alternativet till <code>true</code> om du vill aktivera felsökningsläget eller till false om du vill inaktivera det. </p><p>I felsökningsläget loggas meddelanden om brott mot policyn i Work Manager och om att pausa/återuppta åtgärder i Work Manager. Ange att det här alternativet endast ska vara true vid felsökning.</p></td>
  </tr>
  <tr>
   <td><code> adobe.workmanager.memory-control.enabled</code></td>
   <td><p>Ange det här alternativet till <code>true</code> om du vill aktivera begränsning baserat på inställningarna för minneskontroll som beskrivs nedan, eller till <code>false</code> om du vill inaktivera begränsning.</p></td>
  </tr>
  <tr>
   <td><code> adobe.workmanager.memory-control.high-limit</code></td>
   <td><p>Anger den maximala procentandel av minnet som kan användas innan Work Manager stryper inkommande jobb.</p><p>Standardvärdet för alternativet är <code>95</code>. Det här värdet bör vara bra för de flesta system. Öka den bara om ditt system behöver utnyttja sin maximala kapacitet. Men tänk på att när du ökar det här värdet ökar även risken för minnesbrist.</p><p>Om du kör AEM-formulär i en klustermiljö kanske du vill ange inställningar för minneskontrollgräns på olika noder i klustret. Du kan till exempel ha en lägre hög gräns för noderna A och B, som är programmerade i belastningsutjämnaren för interaktivt arbete. Och du kan ha högre höga gränser för noderna C och D, som inte används av belastningsutjämnaren, utan reserveras för asynkront arbete.</p></td>
  </tr>
  <tr>
   <td><code> adobe.workmanager.memory-control.low-limit</code></td>
   <td><p>Anger den maximala procentandel av minnet som kan användas innan Work Manager slutar begränsa inkommande jobb.</p><p>Standardvärdet för alternativet är <code>20</code>. Det här värdet bör vara bra för de flesta system.</p></td>
  </tr>
  <tr>
   <td><code>Dadobe.workmanager.allocate.max-batch-size</code></td>
   <td><p>Anger den maximala batchstorleken för arbetshanteraren. Standardbatchstorleken är 10.</p><p>Om statusen för en process i arbetshanteraren inte uppdateras även efter att uppgiften är slutförd, ställer du in batchstorleken på 1.</p></td>
  </tr>
 </tbody>
</table>

**Lägg till Java-alternativ i JBoss**

1. Stoppa JBoss-programservern.
1. Öppna *[appserver root]*/bin/run.bat (Windows) eller run.sh (Linux eller UNIX) i en redigerare och lägg till eventuella Java-alternativ i formatet `-Dproperty=value`.
1. Starta om servern.

**Lägg till Java-alternativ i WebLogic**

1. Starta WebLogic Administration Console genom att skriva `https://[host name]:[port]/console` i en webbläsare.
1. Skriv användarnamnet och lösenordet som du skapade för WebLogic Server-domänen och klicka på Logga under Change Center och klicka på Lock &amp; Edit.
1. Klicka på Miljö > Servrar under Domänstruktur och klicka på namnet på den hanterade servern i den högra panelen.
1. På nästa skärm klickar du på fliken Konfiguration > fliken Serverstart.
1. I rutan Argument lägger du till de argument du vill ha i slutet av det aktuella innehållet. Om du till exempel vill inaktivera hälsoövervakaren lägger du till:

   `-Dadobe.healthmonitor.enabled=false` inaktiverar hälsoövervakning.

1. Klicka på Spara och sedan på Aktivera ändringar.
1. Starta om WebLogic-hanterad server.

**Lägg till Java-alternativ i WebSphere**

1. Klicka på Servrar > Servertyper > WebSphere-programservrar i navigeringsträdet för administrationskonsolen.
1. Klicka på servernamnet i den högra rutan.
1. Under Serverinfrastruktur klickar du på arbetsflödet Java och formulär > Processdefinition.
1. Klicka på Java Virtual Machine under Additional Properties (Ytterligare egenskaper).
1. Skriv de argument du vill ha i rutan Allmänt om JVM-argument.
1. Klicka på OK eller Använd och sedan på Spara direkt i huvudkonfigurationen.
