---
title: Finjustera prestanda för hälsoövervakning
description: Lär dig hur du finjusterar prestandan för hälsoövervakaren. Styr systemstatistiken som påverkar formulärmiljöns prestanda med JAVA-inställningsalternativet.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/health_monitor
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Finjustera prestanda för hälsoövervakning{#fine-tuning-health-monitor-performance}

Insamling av systemstatistik som fyller i Health Monitor har viss inverkan på prestandan i AEM-formulärmiljön. Den här effekten kan styras genom att du anger de Java-alternativ som anges nedan på programservern.

<table>
 <thead>
  <tr>
   <th><p>Egenskap</p></th>
   <th><p>Syfte</p></th>
   <th><p>Standardvärde</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>adobe.healthmonitor.enabled</p></td>
   <td><p>Aktivera eller inaktivera hälsoövervakartråden</p></td>
   <td><p>true</p></td>
  </tr>
  <tr>
   <td><p>adobe.cache.statistics-enabled</p></td>
   <td><p>Aktivera eller inaktivera Gemfire-cachelagring</p></td>
   <td><p>true</p></td>
  </tr>
  <tr>
   <td><p>adobe.healthmonitor.refresh-interval</p></td>
   <td><p>Intervallet i millisekunder efter vilket hälsoövervakartråden samlar in statistiken</p></td>
   <td><p>10 minuter (600 000 millisekunder)</p></td>
  </tr>
  <tr>
   <td><p>adobe.cache.multicast-port</p></td>
   <td><p>Multicast-porten som används för att kommunicera med andra medlemmar i det distribuerade systemet. Om värdet är noll inaktiveras multicast för både medlemsidentifiering och -distribution. </p><p>Obs! Välj olika multicast-adresser och portar för olika distribuerade system. Använd inte olika adresser.</p></td>
   <td><p>Inget standardvärde. Giltiga värden är från 0 till 65535.</p></td>
  </tr>
  <tr>
   <td><p>statistisk-samplingsfrekvens</p></td>
   <td><p>Frekvensen i millisekunder med vilken statistik samplas. Operativsystemsstatistik uppdateras endast när ett exempel tas.</p></td>
   <td><p>600000</p></td>
  </tr>
  <tr>
   <td><p>adobe.workmanager.healthmonitor.enabled</p></td>
   <td><p>Den här egenskapen aktiverar eller inaktiverar insamling av statistik för Work Manager, till exempel antalet jobb eller arbetsobjekt.</p></td>
   <td><p>true</p></td>
  </tr>
 </tbody>
</table>

## Lägg till Java-alternativ i JBoss {#add-java-options-to-jboss}

1. Stoppa JBoss-programservern.
1. Öppna *[appserver root]*/bin/run.bat (Windows) eller run.sh (Linux eller UNIX) i en redigerare och lägg till eventuella Java-alternativ.
1. Starta om servern.

## Lägg till Java-alternativ i WebLogic {#add-java-options-to-weblogic}

1. Starta administrationskonsolen för WebLogic genom att skriva https://[värdnamn]:&#39;port&#39;/console i URL-raden i en webbläsare.
1. Skriv användarnamnet och lösenordet som du skapade för WebLogic Server-domänen och klicka på Logga under Change Center och klicka på Lock &amp; Edit.
1. Klicka på Miljö > Servrar under Domänstruktur och klicka på namnet på den hanterade servern i den högra panelen.
1. På nästa skärm klickar du på fliken Konfiguration > fliken Serverstart.
1. I rutan Argument lägger du till de argument du vill ha i slutet av det aktuella innehållet. Om du till exempel lägger till - `Dadobe.healthmonitor.enabled=false` inaktiveras hälsoövervakaren.
1. Klicka på Spara och sedan på Aktivera ändringar.
1. Starta om WebLogic-hanterad server.

## Lägg till Java-alternativ i WebSphere {#add-java-options-to-websphere}

1. Gör följande för programservern i navigeringsträdet i WebSphere Administrative Console:

   (WebSphere 6.x) Klicka på Servrar > Programservrar

   (WebSphere 7.x) Klicka på Servrar > Servertyper > Webbplatsprogramservrar

1. Klicka på servernamnet i den högra rutan.
1. Under Serverinfrastruktur klickar du på arbetsflödet Java och formulär > Processdefinition.
1. Klicka på Java Virtual Machine under Additional Properties (Ytterligare egenskaper).
1. Skriv de argument du vill ha i rutan Allmänt om JVM-argument.
1. Klicka på OK eller Använd och sedan på Spara direkt i huvudkonfigurationen.
