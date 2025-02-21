---
title: Sårbarheter för Minska strängar 2 för Experience Manager Forms i JEE
description: Sårbarheter för Minska strängar 2 för Experience Manager Forms i JEE
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Security
geptopics: SG_AEMFORMS/categories/jee
role: Admin
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 887dc1d6d7e11672b62ef5ca5463ea6181ff0320
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Säkerhetsluckor i Struts 2 för Experience Manager Forms {#mitigatin-struts2-rce-vulnerabilities-for-aem-forms}

## Problem

Allvarliga säkerhetsluckor har rapporterats för Struts 2, ett populärt ramverk för webbapplikationer med öppen källkod för utveckling av Java EE-webbapplikationer. Följande säkerhetsluckor har analyserats:

| Sårbarhet | Vad har påverkat? | Vad påverkas inte? |
|---|---|---|
| [CVE-2023-50164](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2023-50164) | Experience Manager 6.5 Forms på JEE (alla versioner från 6.5 GA till 6.5.19.0) | <ul><li> Experience Manager Forms Workbench (alla versioner)</li> <li> Experience Manager Forms on OSGi (alla versioner) </li> <li> Experience Manager Forms as a Cloud Service </li> <ul> |

## Upplösning

I följande tabell visas upplösning för alla påverkade versioner:

| Frigör | Aktuell version | Användaråtgärd |
|---|---|---|
| Experience Manager 6.5 Forms on JEE | 6.5.19.0 | [Installera den senaste Service Pack-versionen](https://experienceleague.adobe.com/docs/experience-manager-65-2025/release-notes/aem-forms-current-service-pack-installation-instructions.html?lang=en) |
| Experience Manager 6.5 Forms on JEE | 6.5.13.0 - 6.5.18.0 | Använd någon av följande metoder: <ul><li>  <a href="https://experienceleague.adobe.com/docs/experience-manager-65-2025/release-notes/aem-forms-current-service-pack-installation-instructions.html?lang=en"> Installera den senaste Service Pack-versionen </a> </li> <li> <a href ="#use-manual-mitigation-steps"> Använd manuell begränsning </a> |
| Experience Manager 6.5 Forms on JEE | 6.5 - 6.5.12.0 | [Installera den senaste Service Pack-versionen](https://experienceleague.adobe.com/docs/experience-manager-65-2025/release-notes/aem-forms-current-service-pack-installation-instructions.html?lang=en) </br> </br> **OBS!** AEM Forms stöder för närvarande versionerna 6.5.13.0 till 6.5.19.0. Om du använder en äldre version rekommenderar vi att du uppgraderar till 6.5.13.0 eller en senare version. Anvisningar om hur du installerar AEM 6.5.13.0 eller senare finns i versionsinformationen. |

### Använd manuella begränsningssteg {#use-manual-mitigation-steps}

Du kan använda de manuella begränsningsstegen för att lösa problemet på AEM 6.5 Form Server som kör Service Pack 13 till AEM 6.5 Form Server som kör Service Pack 18 (6.5.13.0 - 6.5.18.0):

1. Hämta [strängkärnan 2.5.33 jar](https://repo1.maven.org/maven2/org/apache/struts/struts2-core/2.5.33/struts2-core-2.5.33.jar) till en lokal mapp. Exempel: C:\Users\labuser\Desktop\struts2-core-2.5.33.jar.
1. Ladda ned AEM Forms on JEE Manual Patching Tool från [Software Distribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack/fd/patch_utility/archive-patcher-1.0.0.zip).
1. Zippa upp det manuella arkivet med korrigeringsverktyg. Extrahera till exempel till `/Users/labuser/Desktop/archive-patcher-1.0.0 folder`. Följande filer extraheras:
   * archive-patcher-1.0.0.jar
   * patch-archive.bat
   * patch-archive.sh

>[!BEGINTABS]

>[!TAB Windows]

1. Stäng alla serverinstanser och sökare.

1. Öppna terminalfönstret och navigera till den mapp som innehåller AEM Forms on JEE Manual Patching Tool (extraherade filer).

1. Kör följande kommando för att söka i alla filer med äldre struts2-bibliotek. Innan du kör kommandot måste du ersätta sökvägen i kommandot med sökvägen till din AEM Forms-server:


   ```
   patch-archive.bat -root=C:\Adobe\Adobe_Experience_Manager_Forms\configurationManager\export -pattern=.*struts2-core.*jar$
   ```

   >[!NOTE]
   >
   >
   >Verktyget kräver Internetanslutning eftersom beroenden hämtas vid körning. Kontrollera därför att du är ansluten till Internet innan du kör verktyget.

1. Kör följande kommandon i den angivna ordningen för rekursiv ersättning på plats. Innan du kör kommandot måste du ersätta sökvägen i kommandot med sökvägen till AEM Forms-servern och filen `struts2-core-2.5.33.jar`.



   ```
   patch-archive.bat -root=C:\Adobe\Adobe_Experience_Manager_Forms\configurationManager\export -pattern=.*struts2-core.*jar$ -action=replace C:\Users\labuser\Desktop\struts2-core-2.5.33.jar
   ```

   Ovanstående steg korrigerar alla öronfiler med äldre struts2-bibliotek.

1. Avdistribuera den äldre EAR-filen och distribuera den patchade EAR-filen, som finns i exportmappen, till programservern.

1. Starta AEM Forms Server.

>[!TAB Linux]

1. Stäng alla serverinstanser och sökare.

1. Öppna terminalfönstret och navigera till den mapp som innehåller AEM Forms on JEE Manual Patching Tool (extraherade filer).

1. Kör följande kommando för att söka i alla filer med äldre struts2-bibliotek. Innan du kör kommandot måste du ersätta sökvägen i kommandot med sökvägen till din AEM Forms-server:


   ```
   ./patch-archive.sh -root=/opt/Adobe/Adobe_Experience_Manager_Forms/configurationManager/export/ -pattern=.*struts2-core.*jar$
   ```

   >[!NOTE]
   >
   >
   >Verktyget kräver Internetanslutning eftersom beroenden hämtas vid körning. Kontrollera därför att du är ansluten till Internet innan du kör verktyget.

1. Kör följande kommandon i den angivna ordningen för rekursiv ersättning på plats. Innan du kör kommandot måste du ersätta sökvägen i kommandot med sökvägen till AEM Forms-servern och filen `struts2-core-2.5.33.jar`.



   ```
   ./patch-archive.sh -root=/opt/Adobe/Adobe_Experience_Manager_Forms/configurationManager/export/ -pattern=.*struts2-core.*jar$ -action=replace /opt/struts2-core-2.5.33.jar
   ```

   Ovanstående steg korrigerar alla öronfiler med äldre struts2-bibliotek.

1. Avdistribuera den äldre EAR-filen och distribuera den patchade EAR-filen, som finns i exportmappen, till programservern.

1. Starta AEM Forms Server.

>[!ENDTABS]




<!-- 
### Manual patching tool 


>[!BEGINTABS]

>[!TAB Windows]

    ```
    
    patch-archive.bat [-root=dir-or-file] [-pattern=regex] [-action=list(default)|delete|replace <replacement-file>]

    ```

* **dir-or-file**: Specifies path of directory containing multiple archives to patch. The default path for AEM Forms on JEE is <>. 
* **regex**: Specifies regular expression identifying a file or an archive entry to patch. It is tested against each file's or archive entry's absolute path. For example, the pattern `.*struts2-core-2.5.30.jar$` search for all the lines that end with the exact string `struts2-core-2.5.30.jar`.
* **list**: Lists the matched files or archive entries. It recursively searches for and reports all instances of the supplied pattern matched in any entry present in any archive file (zip/jar/war/ear) inside the supplied root directory. No changes are made to any file. It is the default action of the tool, when no action is specified.
* **delete**: Deletes the matched files or archive entries. If the matched entity is an archive, deletion happens before traversing it. This prevents any potentially matching entries inside it from being reported.  
* **replace**: Substitutes the matched files or archive entries with the supplied replacement. If the matched entity is an archive, replacement happens before traversing it. This prevents any potentially matching entries inside it from being reported.

>[!TAB macOS]

    ```
    
    patch-archive.sh [-root=dir-or-file] [-pattern=regex] [-action=list(default)|delete|replace <replacement-file>]

    ```

* **dir-or-file**: Specifies path of directory containing multiple archives to patch. The default path for AEM Forms on JEE is <>. 
* **regex**: Specifies regular expression identifying a file or an archive entry to patch. It is tested against each file's or archive entry's absolute path. For example, the pattern `.*struts2-core-2.5.30.jar$` search for all the lines that end with the exact string `struts2-core-2.5.30.jar`.
* **list**: Lists the matched files or archive entries. It recursively searches for and reports all instances of the supplied pattern matched in any entry present in any archive file (zip/jar/war/ear) inside the supplied root directory. No changes are made to any file. It is the default action of the tool, when no action is specified.
* **delete**: Deletes the matched files or archive entries. If the matched entity is an archive, deletion happens before traversing it. This prevents any potentially matching entries inside it from being reported.  
* **replace**: Substitutes the matched files or archive entries with the supplied replacement. If the matched entity is an archive, replacement happens before traversing it. This prevents any potentially matching entries inside it from being reported.  

>[!TAB Linux]

    ```
    
    patch-archive.sh [-root=dir-or-file] [-pattern=regex] [-action=list(default)|delete|replace <replacement-file>]

    ```

* **dir-or-file**: Specifies path of directory containing multiple archives to patch. The default path for AEM Forms on JEE is <>. 
* **regex**: Specifies regular expression identifying a file or an archive entry to patch. It is tested against each file's or archive entry's absolute path. For example, the pattern `.*struts2-core-2.5.30.jar$` search for all the lines that end with the exact string `struts2-core-2.5.30.jar`.
* **list**: Lists the matched files or archive entries. It recursively searches for and reports all instances of the supplied pattern matched in any entry present in any archive file (zip/jar/war/ear) inside the supplied root directory. No changes are made to any file. It is the default action of the tool, when no action is specified.
* **delete**: Deletes the matched files or archive entries. If the matched entity is an archive, deletion happens before traversing it. This prevents any potentially matching entries inside it from being reported.  
* **replace**: Substitutes the matched files or archive entries with the supplied replacement. If the matched entity is an archive, replacement happens before traversing it. This prevents any potentially matching entries inside it from being reported.  



>[!ENDTABS]









-->
