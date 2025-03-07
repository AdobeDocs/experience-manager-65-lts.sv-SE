---
title: Förbättra programserverns prestanda
description: I det här dokumentet beskrivs valfria inställningar som du kan konfigurera för att förbättra prestandan på AEM-formulärprogramservern.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_application_server
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 6bd234fb-28ad-405f-a018-bdf4fa412839
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# Förbättra programserverns prestanda{#enhancing-application-server-performance}

Det här innehållet beskriver valfria inställningar som du kan konfigurera för att förbättra prestandan på AEM-formulärprogramservern.

## Konfigurera datakällor för programservrar {#configuring-application-server-data-sources}

AEM-formulär använder AEM formulärdatabas som datakälla. AEM blankettarkiv lagrar programresurser och vid körning kan tjänster hämta resurser från databasen som en del av en automatiserad affärsprocess.

Åtkomsten till datakällan kan vara viktig, beroende på hur många AEM-formulärmoduler du kör och hur många användare som samtidigt använder programmet. Datakällans åtkomst kan optimeras med hjälp av anslutningspoolning. *Anslutningspoolning* är en teknik som används för att undvika att skapa nya databasanslutningar varje gång ett program eller serverobjekt kräver åtkomst till databasen. Anslutningspoolning används vanligtvis i webbaserade program och företagsprogram och hanteras vanligtvis av, men inte begränsat till, en programserver.

Det är viktigt att du konfigurerar anslutningspoolens parametrar på rätt sätt så att du aldrig får slut på anslutningar, vilket kan försämra programmets prestanda.

För att konfigurera inställningarna för anslutningspoolen på rätt sätt är det viktigt för programserveradministratören att övervaka anslutningspoolen under högsta antal timmar på dygnet. Övervakningen ser till att det alltid finns tillräckligt med anslutningar för program och användare. De flesta programservrar innehåller övervakningsverktyg.

Du kan övervaka olika typer av statistik för varje instans av JDBC-datakälla i din domän med hjälp av administrationskonsolen för WebLogic Server. Mer information finns i dokumentationen för WebLogic.

När programserveradministratören fastställer rätt inställningar för anslutningspoolen måste den personen meddela informationen till databasadministratören. Databasadministratören behöver den här informationen eftersom antalet databasanslutningar är lika med antalet anslutningar i anslutningspoolen för datakällan. Slutför sedan stegen för att konfigurera inställningarna för anslutningspoolen för programservern och datakälltypen enligt beskrivningen nedan.

### Konfigurera inställningar för anslutningspool för WebLogic för Oracle och MySQL {#configure-connection-pool-settings-for-weblogic-for-oracle-and-mysql}

1. Under Domänstruktur klickar du på Tjänster > JDBC > Datakällor och sedan på IDP_DS i den högra rutan.
1. På nästa skärm klickar du på fliken Konfiguration > Anslutningspool och anger ett värde i följande rutor:

   * Inledande kapacitet
   * Maximal kapacitet
   * Kapacitetsökning
   * Cachestorlek för uttryck

1. Klicka på Spara och sedan på Aktivera ändringar.
1. Starta om WebLogic-hanterad server.

### Konfigurera inställningar för anslutningspool för WebLogic för SQLServer {#configure-connection-pool-settings-for-weblogic-for-sqlserver}

1. Klicka på Lås och redigera under Ändringscenter.
1. Under Domänstruktur klickar du på Tjänster > JDBC > Datakällor och sedan på EDC_DS i den högra rutan.
1. På nästa skärm klickar du på fliken Konfiguration > Anslutningspool och anger ett värde i följande rutor:

   * Inledande kapacitet
   * Maximal kapacitet
   * Kapacitetsökning
   * Cachestorlek för uttryck

1. Klicka på Spara och sedan på Aktivera ändringar.
1. Starta om WebLogic-hanterad server.

### Konfigurera inställningar för anslutningspool för WebSphere för DB2 {#configure-connection-pool-settings-for-websphere-for-db2}

1. Klicka på Resurser > JDBC > JDBC Providers i navigeringsträdet. I den högra rutan klickar du på datakällan som du skapade, antingen DB2 Universal JDBC Driver Provider eller LiveCycle - db2 - IDP_DS.
1. Klicka på Datakällor under Ytterligare egenskaper och välj IDP_DS.
1. På nästa skärm, under Ytterligare egenskaper, klickar du på Egenskaper för anslutningspool och anger ett värde i rutan Maximalt antal anslutningar och rutan Minimalt antal anslutningar.
1. Klicka på OK eller Använd och sedan på Spara direkt till huvudkonfiguration.

### Konfigurera inställningar för anslutningspool för WebSphere för Oracle {#configure-connection-pool-settings-for-websphere-for-oracle}

1. Klicka på Resurser > JDBC > JDBC Providers i navigeringsträdet. Klicka på datakällan för Oracle JDBC Driver som du skapade i den högra rutan.
1. Klicka på Datakällor under Ytterligare egenskaper och välj IDP_DS.
1. På nästa skärm, under Ytterligare egenskaper, klickar du på Egenskaper för anslutningspool och anger ett värde i rutan Maximalt antal anslutningar och rutan Minimalt antal anslutningar.
1. Klicka på OK eller Använd och sedan på Spara direkt till huvudkonfiguration.

### Konfigurera inställningar för anslutningspool för WebSphere för SqlServer {#configure-connection-pool-settings-for-websphere-for-sqlserver}

1. Klicka på Resurser > JDBC > JDBC Providers i navigeringsträdet och klicka i den högra rutan på den användardefinierade datakällan för JDBC-drivrutin som du skapade.
1. Klicka på Datakällor under Ytterligare egenskaper och välj IDP_DS.
1. På nästa skärm, under Ytterligare egenskaper, klickar du på Egenskaper för anslutningspool och anger ett värde i rutan Maximalt antal anslutningar och rutan Minimalt antal anslutningar:
1. Klicka på OK eller Använd och sedan på Spara direkt till huvudkonfiguration.

## Optimera textbundna dokument och påverka JVM-minnet {#optimizing-inline-documents-and-impact-on-jvm-memory}

Om du vanligtvis bearbetar dokument med relativt liten storlek kan du förbättra prestandan som är kopplad till dokumentöverföringshastigheten och lagringsutrymmet. Implementera följande produktkonfigurationer för AEM-formulär:

* Öka standarddokumentets maximala textbundna storlek för AEM-formulär så att den är större än storleken för de flesta dokument.
* Om du vill bearbeta större filer anger du lagringskataloger på ett höghastighetsdisksystem eller en RAM-disk.

Den maximala textbundna storleken och lagringskatalogerna (AEM skapar en tillfällig filkatalog och GDS-katalogen) konfigureras i administrationskonsolen.

### Dokumentstorlek och maximal textbunden storlek {#document-size-and-maximum-inline-size}

När ett dokument som skickas för bearbetning av AEM-formulär är mindre än eller lika med standarddokumentets maximala textbundna storlek, lagras dokumentet på den infogade servern och dokumentet serialiseras som ett Adobe Document-objekt. Att lagra dokument textbundet kan ge avsevärda prestandafördelar. Om du däremot använder ett formulärarbetsflöde kan innehållet också lagras i databasen i spårningssyfte. Om du ökar den maximala textbundna storleken kan det därför påverka databasstorleken.

Ett dokument som är större än den maximala textbundna storleken lagras i det lokala filsystemet. Adobe Document-objektet som överförs till och från servern är bara en pekare till filen.

När dokumentinnehållet är infogat (d.v.s. mindre än den maximala infogade storleken) lagras innehållet i databasen som en del av dokumentets serialiseringsnyttolast. Om du ökar den maximala textbundna storleken kan det därför påverka databasstorleken.

**Ändra den maximala textbundna storleken**

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Inställningar > Systeminställningar > Konfigurationer.
1. Ange ett värde i rutan Maximal infogad storlek för standarddokument och klicka på OK.

   >[!NOTE]
   >
   >Värdet för egenskapen för maximal onlinestorlek för dokument måste vara identiskt för AEM Forms i JEE-miljö och AEM Forms i OSGi-paket som ingår i AEM Forms i JEE-miljö. Detta uppdaterade värde gäller endast för AEM Forms i JEE-miljö och inte för AEM Forms i OSGi-paket som innehåller AEM Forms i JEE-miljö.

1. Starta om programservern med följande systemegenskap:

   com.adobe.idp.defaultDocumentMaxInlineSize=`[value specified in Step 2]`

   >[!NOTE]
   >
   >Systemegenskapen ovan åsidosätter värdet för egenskapen för dokumentets maximala textbundna storlek som angetts för AEM Forms i JEE-miljön och för AEM Forms i OSGi-paketet som ingår i AEM Forms i JEE-miljön.

>[!NOTE]
>
>Den maximala standardstorleken för intern radbrytning är 65536 byte.

### Största stackstorlek för JVM {#jvm-maximum-heap-size}

En ökning av den maximala textbundna storleken kräver mer minne för att lagra serialiserade dokument. Därför krävs i allmänhet även en ökning av den maximala stackstorleken för JVM.

Ett system med stor belastning som bearbetar många dokument kan snabbt göra JVM-heap-minnet mer mättat. För att undvika ett OutOfMemoryError-undantag ökar du den maximala stackstorleken för JVM med en mängd som motsvarar storleken på de textbundna dokumenten multiplicerat med antalet dokument som vanligtvis körs vid en given tidpunkt.

Största ökning av stackstorleken för JVM = (intern dokumentstorlek) x (genomsnittligt antal bearbetade dokument).

**Beräknar den maximala stackstorleken för JVM**

I det här exemplet är den aktuella högsta stacken för JVM inställd på 512 MB och den maximala infogade storleken är 64 kB. Servern måste konfigureras för scenariot där 10 jobb körs samtidigt och varje jobb har nio indatafiler och en resultatfil (totalt 10 filer per jobb och 100 filer bearbetas samtidigt). Alla filer är mindre än 512 kB.

Om du vill lagra alla filer textbundna anger du en maximal textbunden storlek på minst 512 kB.

Den nödvändiga ökningen av JVM:s största stackstorlek beräknas med följande ekvation:

(512 kB) x (100) = 51 200 kB, eller 50 MB

Den största stackstorleken för JVM måste ökas med 50 MB för totalt 562 MB.

**Överväg stackfragmentering**

Om du ställer in storleken på textbundna dokument till stora värden ökar risken för ett OutOfMemoryError-fel på system som lätt kan heap-fragmentering. Om du vill lagra ett dokument textbundet måste JVM-heap-minnet ha tillräckligt sammanhängande utrymme. Vissa operativsystem, JVM:er och skräpinsamlingsalgoritmer är benägna att heap-fragmentering. Fragmentering minskar mängden sammanhängande stackutrymme och kan leda till ett OutOfMemoryError även om det finns tillräckligt med totalt ledigt utrymme.

Tidigare åtgärder på programservern lämnade till exempel JVM-heap i ett fragmenterat tillstånd och skräpinsamlaren kan inte komprimera heap tillräckligt för att återta stora block med ledigt utrymme. Ett OutOfMemoryError kan inträffa även om den maximala stackstorleken för JVM justerades för en ökning av den maximala infogade storleken.

Om du vill ta hänsyn till heap-fragmentering får den infogade dokumentstorleken inte vara större än 0,1 % av den totala stackstorleken. En JVM med en maximal stackstorlek på 512 MB kan till exempel ha stöd för en maximal intern storlek på 512 MB x 0,001 = 0,512 MB, eller 512 kB.

## WebSphere Application Server-förbättringar {#websphere-application-server-enhancements}

I det här avsnittet beskrivs inställningar som är specifika för en WebSphere Application Server-miljö.

### Öka det maximala minne som tilldelas JVM {#increasing-the-maximum-memory-allocated-to-the-jvm}

Om du kör Configuration Manager eller försöker generera Enterprise JavaBeans (EJB)-distributionskod med kommandoradsverktyget *ejbdeploy* och ett OutOfMemory-fel inträffar, ökar du mängden minne som allokerats till JVM.

1. Redigera ejbdeploy-skriptet i katalogen *[appserver root]*/deploytool/itp/:

   * (Windows) `ejbdeploy.bat`
   * (Linux och UNIX) `ejbdeploy.sh`

1. Hitta parametern `-Xmx256M` och ändra den till ett högre värde, till exempel `-Xmx1024M`.
1. Spara filen.
1. Kör kommandot `ejbdeploy` eller omdistribuera med Configuration Manager.

## Förbättra prestanda för Windows Server 2003 med LDAP {#improving-windows-server-2003-performance-with-ldap}

Det här innehållet beskriver inställningar som är specifika för en Microsoft Windows Server 2003-operativsystemmiljö.

Om du använder anslutningspoolning på sökanslutningen kan antalet portar som behövs minska med så mycket som 50 %. Detta beror på att anslutningen alltid använder samma autentiseringsuppgifter för en viss domän, och kontexten och relaterade objekt stängs uttryckligen.

### Konfigurera Windows Server för anslutningspoolning {#configure-your-windows-server-for-connection-pooling}

1. Klicka på Start > Kör för att starta Registereditorn och skriv `regedit` i rutan Öppna. Klicka sedan på OK.
1. Gå till registernyckeln `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters`
1. Leta reda på TcpTimedWaitDelay-värdenamnet i den högra rutan i Registereditorn. Om namnet inte visas väljer du Redigera > Nytt > DWORD-värde på menyraden för att lägga till namnet.
1. Skriv `TcpTimedWaitDelay` i rutan Namn

   >[!NOTE]
   >
   >Om du inte ser någon blinkande markör och `New Value #` inuti rutan högerklickar du i den högra panelen, väljer Byt namn och skriver `TcpTimedWaitDelay`*i rutan Namn.*

1. Upprepa steg 4 för värdenamnen MaxUserPort, MaxHashTableSize och MaxFreeTcbs.
1. Dubbelklicka i den högra rutan för att ange TcpTimedWaitDelay-värdet. Välj Decimal under Basvärde och skriv `30` i rutan Värde.
1. Dubbelklicka i den högra rutan för att ange värdet för MaxUserPort. Välj Decimal under Basvärde och skriv `65534` i rutan Värde.
1. Dubbelklicka inuti den högra rutan för att ange värdet för MaxHashTableSize. Välj Decimal under Basvärde och skriv `65536` i rutan Värde.
1. Dubbelklicka inuti den högra rutan för att ange värdet för MaxFreeTcbs. Välj Decimal under Basvärde och skriv `16000` i rutan Värde.

>[!NOTE]
>
>Allvarliga problem kan uppstå om du ändrar registret felaktigt med hjälp av Registereditorn eller med en annan metod. Dessa problem kan kräva att du installerar om operativsystemet. Ändra registret på egen risk.
