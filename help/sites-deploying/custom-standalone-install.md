---
title: Anpassad fristående installation
description: Läs mer om de alternativ som är tillgängliga när du installerar en fristående AEM-instans.
content-type: reference
topic-tags: deploying
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
exl-id: 93dc74b3-dfe3-442f-9dec-1b7af41cd4a1
source-git-commit: 5c7ead37f55175bb565f8661c0c156f76f135131
workflow-type: tm+mt
source-wordcount: '1563'
ht-degree: 0%

---

# Anpassad fristående installation{#custom-standalone-install}

I det här avsnittet beskrivs de alternativ som är tillgängliga när du installerar en fristående AEM-instans. Du kan även läsa [Lagringselement](/help/sites-deploying/storage-elements-in-aem-6.md) om du vill ha mer information om hur du väljer lagringstyp för serverdelen när du har installerat AEM 6.

## Ändra portnumret genom att byta namn på filen {#changing-the-port-number-by-renaming-the-file}

Standardporten för AEM är 4502. Om porten inte är tillgänglig eller redan används konfigureras Quickstart automatiskt till att använda det första tillgängliga portnumret enligt följande: 4502, 8080, 8081, 8082, 8083, 8084, 8085, 888, 9362, `<*random*>`.

Du kan också ange portnumret genom att byta namn på filen quickstart jar så att filnamnet innehåller portnumret, till exempel `cq5-publish-p4503.jar` eller `cq5-author-p6754.jar`.

Det finns olika regler som ska följas när man byter namn på filen quickstart jar:

* När du byter namn på filen måste den börja med `cq;` som i `cq5-publish-p4503.jar`.

* Vi rekommenderar att du *alltid* anger portnumret med -p, som i cq5-publish-p4503.jar eller cq5-author-p6754.jar.

>[!NOTE]
>
>Detta för att säkerställa att du inte behöver bekymra dig om att du uppfyller reglerna som används för att extrahera portnumret:
>
>* portnumret måste bestå av 4 eller 5 siffror
>* dessa siffror måste komma efter ett streck
>* om det finns andra siffror i filnamnet måste portnumret föregås av `-p`
>* &quot;cq5&quot; i början av filnamnet ignoreras
>

>[!NOTE]
>
>Du kan också ändra portnumret med alternativet `-port` i startkommandot.

### Java 17/Java 21 - överväganden {#java-considerations}

Om du kör Oracle Java 17 eller Java 21 måste ytterligare växlar läggas till på kommandoraden när du startar AEM.

* Nedan visas ett exempel på hur de ytterligare JVM-parametrarna ska se ut när AEM startas på Java 17/Java 21:

```shell
-XX:+UseG1GC --add-opens=java.desktop/com.sun.imageio.plugins.jpeg=ALL-UNNAMED --add-opens=java.base/sun.net.www.protocol.jrt=ALL-UNNAMED --add-opens=java.naming/javax.naming.spi=ALL-UNNAMED --add-opens=java.xml/com.sun.org.apache.xerces.internal.dom=ALL-UNNAMED --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/jdk.internal.loader=ALL-UNNAMED --add-opens=java.base/java.net=ALL-UNNAMED --add-opens=java.base/java.lang=org.apache.sling.commons.threads -Djdk.util.zip.disableZip64ExtraFieldValidation=true
```

* [Endast Forms] nedan är ett exempel som kontrollerar att AEM Forms fungerar med Java 17/Java21. Inkludera följande JVM-parametrar:

```shell
--add-opens=java.base/java.util=ALL-UNNAMED --add-exports=java.xml/com.sun.org.apache.xml.internal.serialize=ALL-UNNAMED
```

## Körningslägen {#run-modes}

Med **körningslägen** kan du trimma din AEM-instans för ett specifikt ändamål, till exempel författare eller publicering, testning, utveckling, intranät och så vidare. Med de här lägena kan du styra användningen av exempelinnehåll. Det här exempelinnehållet definieras innan snabbstarten byggs och kan innehålla paket, konfigurationer och så vidare. Detta kan vara särskilt användbart för produktionsklara installationer när du vill hålla installationen smidig och utan exempelinnehåll. Mer information finns i:

* [Körningslägen](/help/sites-deploying/configure-runmodes.md)

## Lägga till en filinstallationsprovider {#adding-a-file-install-provider}

Som standard bevakas mappen `crx-quickstart/install` för filer.
Den här mappen finns inte, men kan skapas under körning.

Om ett paket, en konfiguration eller ett innehållspaket placeras i den här katalogen hämtas det automatiskt och installeras. Om den tas bort avinstalleras den.
Det är ett annat sätt att skicka paket, innehållspaket eller konfigurationer till databasen.

Detta är särskilt intressant för flera användningsområden:

* Under utvecklingen kan det vara enklare att lägga in något i filsystemet.
* Om något går fel går det inte att nå webbkonsolen och databasen. Med detta kan du lägga in ytterligare paket i den här katalogen och de bör installeras.
* Mappen `crx-quickstart/install` kan skapas innan snabbstart startas och ytterligare paket kan placeras där.

## Installera och starta Adobe Experience Manager som en Windows-tjänst {#installing-and-starting-adobe-experience-manager-as-a-windows-service}

>[!NOTE]
>
>Var noga med att utföra följande procedur när du är inloggad som administratör eller starta/kör dessa steg med snabbmenyvalet **Kör som administratör**.
>
>Inloggningen som användare med administratörsbehörighet är **otillräcklig**. Om du inte är inloggad som administratör när du slutför de här stegen får du felmeddelanden om **åtkomst nekad**.

Så här installerar och startar du AEM som en Windows-tjänst:

1. Öppna crx-quickstart\opt\helpers\instsrv.bat i en textredigerare.
1. Om du konfigurerar en 64-bitars Windows-server ersätter du alla instanser av prunsrv med något av följande kommandon, enligt operativsystemet:

   * prunsrv_amd64
   * prunsrv_ia64

   Det här kommandot anropar rätt skript som startar Windows-tjänstdaemon i 64-bitars Java i stället för 32-bitars Java.

1. Öppna kommandotolken, ändra den aktuella katalogen till mappen crx-quickstart/opt/help i AEM-installationen och ange följande kommando för att skapa tjänsten:

   `instsrv.bat cq5`

   Om du vill verifiera att tjänsten har skapats öppnar du Tjänster på kontrollpanelen Administrationsverktyg eller skriver `start services.msc` i kommandotolken. Cq5-tjänsten visas i listan.

1. Starta tjänsten genom att göra något av följande:

   * Klicka på cq5 på kontrollpanelen Tjänster och klicka på Start.

   ![chlimage_1-11](assets/chlimage_1-11.png)

   * Skriv net start cq5 på kommandoraden.

   ![chlimage_1-12](assets/chlimage_1-12.png)

1. Windows anger att tjänsten körs. AEM startas och den körbara prunsrv-filen visas i Task Manager. Navigera till AEM i webbläsaren, till exempel `https://localhost:4502`, om du vill börja använda AEM.

   ![chlimage_1-13](assets/chlimage_1-13.png)

>[!NOTE]
>
>Egenskapsvärdena i filen instsrv.bat används när Windows-tjänsten skapas. Om du redigerar egenskapsvärdena i instsrv.bat måste du avinstallera och sedan installera om tjänsten.

>[!NOTE]
>
>När du installerar AEM som tjänst måste du ange den absoluta sökvägen för loggkatalogen i `com.adobe.xmp.worker.files.ncomm.XMPFilesNComm` från Configuration Manager.

Om du vill avinstallera tjänsten klickar du antingen på **Stoppa** på kontrollpanelen **Tjänster** eller på kommandoraden, navigerar till mappen och skriver `instsrv.bat -uninstall cq5`. Tjänsten tas bort från listan på kontrollpanelen **Tjänster** eller från listan på kommandoraden när du skriver `net start`.

## Definiera om platsen för den tillfälliga arbetskatalogen {#redefining-the-location-of-the-temporary-work-directory}

Standardplatsen för den temporära mappen för Java-datorn är `/tmp`. AEM använder den här mappen också när du skapar paket.

Om du vill ändra platsen för den tillfälliga mappen (till exempel om du behöver en katalog med mer ledigt utrymme) definierar du en * `<new-tmp-path>`* genom att lägga till JVM-parametern:

`-Djava.io.tmpdir="/<*new-tmp-path*>"`

till antingen

* serverstartkommandoraden
* CQ_JVM_OPTS-miljöparametern i serverctl eller startskriptet

## Fler alternativ är tillgängliga från QuickStart-filen {#further-options-available-from-the-quickstart-file}

Ytterligare alternativ och namnbyteskonventioner beskrivs i hjälpfilen för QuickStart, som är tillgänglig via alternativet -help. Om du vill få hjälp skriver du:

* `java -jar <jar-name>.jar -help`

>[!CAUTION]
>
>Dessa alternativ gäller från och med den ursprungliga versionen av AEM 6.5 LTS. Ändringar i senare Service Pack-versioner är möjliga.

```shell
Loading quickstart properties: default
Loading quickstart properties: instance
Setting properties from filename '/Users/Desktop/AEM/<jar-name>.jar'
--------------------------------------------------------------------------------
Adobe Experience Manager Quickstart (build 20250226)                            
--------------------------------------------------------------------------------
Usage:                                                                          
 Use these options on the Quickstart command line.                              
--------------------------------------------------------------------------------

-help (--help,-h)
         Show this help message                                                 
-quickstart.server.port (-p,-port) <port>
         Set server port number                                                 
-contextpath (-c,-org.apache.felix.http.context_path) <contextpath>
         Set context path                                                       
-debug <port>
         Enable Java Debugging on port number; forces forking                   
-gui 
         Show GUI if running on a terminal                                      
-nobrowser (-quickstart.nobrowser)
         Do not open browser at startup                                         
-unpack
         Unpack installation files only, do not start the server (implies       
         -verbose)                                                              
-v (-verbose)
         Do not redirect stdout/stderr to files and do not close stdin          
-nofork
         Do not fork the JVM, even if not running on a console                  
-fork
         Force forking the JVM if running on a console, using recommended       
         default memory settings for the forked JVM.                            
-forkargs <args> [<args> ...]
         Additional arguments for the forked JVM, defaults to '-Xmx1024m '.     
         Use -- to specify values starting with -, example: '-forkargs --       
         -server'                                                               
-a (--interface) <interface>
         Optional IP address (interface) to bind to                             
-pt <string>
         Process type (main/fork) - do not use directly, used when forking a    
         process                                                                
-r <string> [<string> [<string> [<string> [<string> [<string> [<string> [<string> [<string> [<string>]]]]]]]]]
         Runmode(s) - Use this to define the run mode(s)                        
-b <string>
         Base folder - defines the path under which the quickstart work folder  
         is created                                                             
-low-mem-action <string>
         Low memory action - what to do if memory is insufficient at startup    
-use-control-port
         Start a control port                                                   
-nointeractive
         Start with no interactivity                                            
-ll <level>
         Define launchpad log level (1 = error...4 = debug)                     
-n   
         Do not install shutdown hook                                           
-D<property>=<value>
         Additional framework properties.                                       
-listener-port <listener-port>
         Set listener port number                                               
-x <string>
         Run a Quickstart extension.                                            
  Options for executing Quickstart extensions:
                                                                                
    -xargs <arg> [<arg> ...]
         Construct an arguments list for a Quickstart extension (e.g. -xargs -- 
         -arg1 val1 -arg2 val2).                                                
--------------------------------------------------------------------------------
Quickstart filename options                                                     
--------------------------------------------------------------------------------
Usage:                                                                          
 Rename the jar file, including one of the patterns shown below, to set the     
corresponding option. Command-line options have priority on these filename      
patterns.                                                                       
--------------------------------------------------------------------------------

-NNNN
         Include -NNNN.jar or -pNNNN in the renamed jar filename to run on port 
         NNNN, for example: quickstart-8085.jar                                 
-nobrowser
         Include -nobrowser in the renamed jar filename to avoid opening the    
         browser at startup, example: quickstart-nobrowser-8085.jar             
-publish
         Include -publish in the renamed jar filename to run in "publish" mode, 
         example: cq-publish-7502.jar                                           
-dynamicmedia
         Include -dynamicmedia in the renamed jar filename to run in            
         "dynamicmedia" mode, example: quickstart-dynamicmedia-4502.jar         
-dynamicmedia_scene7
         Include -dynamicmedia_scene7 in the renamed jar filename to run in     
         "dynamicmedia_scene7" mode, example:                                   
         quickstart-dynamicmedia_scene7-p4502.jar                               
--------------------------------------------------------------------------------
The license.properties file
--------------------------------------------------------------------------------
  The license.properties file stores licensing information, created from the    
  licensing form displayed on first startup and stored in the folder from where 
  Quickstart is run.                                                            
--------------------------------------------------------------------------------
Log files
--------------------------------------------------------------------------------
  Once Quickstart has been unpacked and started, log files can be found under   
  /Users/Desktop/AEM/crx-quickstart/logs.               
--------------------------------------------------------------------------------
```

## Installera AEM i Amazon EC2-miljön {#installing-aem-in-the-amazon-ec-environment}

När du installerar AEM på en Amazon Elastic Compute Cloud-instans (EC2) installeras författaren och publiceras på EC2-instansen på rätt sätt enligt proceduren [Installera instanser av AEM Manager](#installinginstancesofaemmanager), men publiceringsinstansen blir författare.

Innan du installerar Publish-instansen i EC2-miljön gör du följande:

1. Packa upp jar-filen för Publish-instansen innan du startar instansen för första gången. Använd följande kommando för att packa upp filen:

   ```xml
   java -jar quickstart.jar -unpack
   ```

   >[!NOTE]
   >
   >Om du ändrar läget **efter att** startat instansen första gången kan du inte ändra körningsläget.

1. Starta instansen genom att köra:

   ```xml
   java -jar quickstart.jar -r publish
   ```

   >[!CAUTION]
   >
   >Kontrollera att du först kör instansen när du har packat upp den genom att köra kommandot ovan. Annars genereras inte fyllningen quickstart.properties. Utan den här filen kommer eventuella framtida uppgraderingar av AEM att misslyckas.

1. Öppna skriptet **start** i mappen **bin** och kontrollera följande avsnitt:

   ```xml
   # runmode(s)
   if [ -z "$CQ_RUNMODE" ]; then
    CQ_RUNMODE='author'
   fi
   ```

1. Ändra körningsläget till **publicera** och spara filen.

   ```xml
   # runmode(s)
   if [ -z "$CQ_RUNMODE" ]; then
    CQ_RUNMODE='publish'
   fi
   ```

1. Stoppa instansen och starta om den genom att köra skriptet **start**.

## Verifierar installationen {#verifying-the-installation}

Följande länkar kan användas för att verifiera att installationen fungerar (alla exempel baseras på att instansen körs på port 8080 på den lokala värden, att CRX är installerat under /crx och Launchpad under /):

* `https://localhost:8080/crx/de`
CRXDE Lite-konsolen.

* `https://localhost:8080/system/console`
Webbkonsolen.

## Åtgärder efter installation {#actions-after-installation}

Även om det finns många möjligheter att konfigurera AEM WCM bör vissa åtgärder vidtas eller åtminstone granskas omedelbart efter installationen:

* Gå till [checklistan för säkerhet](/help/sites-administering/security-checklist.md) för uppgifter som krävs för att säkerställa att systemet förblir säkert.
* Granska listan över standardanvändare och -grupper som installeras med AEM WCM. Kontrollera om du vill vidta åtgärder för andra konton. Mer information finns i [Säkerhet och användaradministration](/help/sites-administering/security.md).

>[!NOTE]
>
> För nya AEM 6.5 LTS-installationer måste indexdefinitioner installeras separat. Mer information finns i [this](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#index-definitions).

## Åtkomst till CRXDE Lite och webbkonsolen {#accessing-crxde-lite-and-the-web-console}

När AEM WCM har startats har du även tillgång till:

* [CRXDE Lite](#accessing-crxde-lite) - används för att komma åt och hantera databasen
* [Webbkonsol](#accessing-the-web-console) - används för att hantera eller konfigurera OSGi-paket (kallas även OSGi-konsolen)

### Åtkomst till CRXDE Lite {#accessing-crxde-lite}

Om du vill öppna CRXDE Lite kan du välja **CRXDE Lite** från välkomstskärmen eller använda webbläsaren för att navigera till

```
 https://<host>:<port>/crx/de/index.jsp
```

Till exempel:
`https://localhost:4502/crx/de/index.jsp`

![installcq_crxdelite](assets/installcq_crxdelite.png)

#### Åtkomst till webbkonsolen {#accessing-the-web-console}

Om du vill komma åt Adobe CQ-webbkonsolen kan du välja **OSGi Console** från välkomstskärmen eller använda webbläsaren för att navigera till

```
 https://<host>:<port>/system/console
```

Till exempel:
`https://localhost:4502/system/console`
eller för sidan Bundles
`https://localhost:4502/system/console/bundles`

![chlimage_1-14](assets/chlimage_1-14.png)

Mer information finns i [OSGi-konfiguration med webbkonsolen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-with-the-web-console).

## Avinstallerar Adobe Experience Manager {#uninstalling-adobe-experience-manager}

Eftersom AEM installeras i en enda katalog behövs inget avinstallationsverktyg. Det kan vara så enkelt att avinstallera hela installationskatalogen, men hur du avinstallerar AEM beror på vad du vill göra och vilken beständig lagring du använder.

Om beständig lagring är inbäddad i installationskatalogen, till exempel i standardinstallationen för TPM, tas även data bort om du tar bort mappar.

>[!NOTE]
>
>Adobe rekommenderar att du säkerhetskopierar databasen innan du tar bort AEM. Om du tar bort hela &lt;cq-installation-directory> tar du bort databasen. Om du vill behålla databasdata innan du tar bort, flyttar eller kopierar du mappen &lt;cq-installation-directory>/crx-quickstart/database någon annanstans innan du tar bort de andra mapparna.

Om din installation av AEM använder extern lagring, till exempel en databasserver, tas inte data bort automatiskt när du tar bort mappen, men lagringskonfigurationen tas bort, vilket gör det svårt att återställa JCR-innehållet.
