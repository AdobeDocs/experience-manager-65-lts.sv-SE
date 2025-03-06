---
title: Aktivera loggning för HTML5-formulär
description: Med loggningsverktyget kan du logga ett formulär och felsöka formulärrelaterade problem.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 755966e5-6267-4633-bcad-05860a2eda6c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 3%

---

# Aktivera loggning för HTML5-formulär{#enable-logging-for-html-forms}

Du kan konfigurera loggningsverktyget så att loggar för HTML5-formulär skapas. Loggverktyget har olika nivåer, du kan ange en nivå efter dina krav. HTML5-formulär har server- och klientkomponenter. Du kan konfigurera loggar för båda komponenterna.

## Konfigurerar loggning på serversidan {#configuring-server-side-logging}

Utför följande steg för att konfigurera serversidesloggar:

1. Gå till `https://'[server]:[port]'/system/console/configMgr`. Leta reda på och öppna alternativet *Loggningskonfiguration för parenteser*. En dialogruta visas:

   ![ Dialogrutan Konfiguration av loggningslogg för parenteser ](assets/logconfig.png)

   Konfigurationsalternativet Loggningslogg för Apace Sling

1. Ändra **loggnivån** till **Felsök**.

1. Ange namn och sökväg för **loggfilen**.

   >[!NOTE]
   >
   >Om du vill generera loggar i loggkatalogen för HTML5-formulär lägger du till ../logs/ före filnamnet.

1. Ändra **Logger** till **HTMLFormsPerfLogger**. Klicka på **Spara**.

## Konfigurerar klientloggning {#configuring-client-logging}

Du kan använda följande metoder för att aktivera inloggning på HTML5-formulär på klientsidan:

* Använder parametern request med namnet `log`
* Använda CQ Configuration Manager

### Aktivera loggning med parametern request {#enabling-logging-using-request-parameter}

Med den här metoden kan du generera loggar för en viss begäran. Begärandeparameterns namn är `log`. Loggens URL är följande:

`https://<server>:<port>/content/xfaforms/profiles/test.html?contentRoot=<path of the folder containing form xdp>&template=<name of the xdp>&log=<log configuration>.`

Loggkonfigurationen består av loggnivån och loggkategorin.

#### Loggmål {#log-destination}

<table>
 <tbody>
  <tr>
   <th><strong>Loggmål</strong></th>
   <th><strong>Beskrivning</strong></th>
  </tr>
  <tr>
   <td>1</td>
   <td>Loggar dirigeras till webbläsaren <strong>Console</strong></td>
  </tr>
  <tr>
   <td>2</td>
   <td>Loggar samlas in i ett JavaScript-objekt på klientsidan och kan skickas till <strong>Server</strong> </td>
  </tr>
  <tr>
   <td>3</td>
   <td>Båda ovanstående alternativ: <br /> </td>
  </tr>
 </tbody>
</table>

#### Loggnivåer {#log-levels}

<table>
 <tbody>
  <tr>
   <th>Loggnivå</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>0</td>
   <td>AV <br type="_moz" /> </td>
  </tr>
  <tr>
   <td>1</td>
   <td>FATAL<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>2</td>
   <td>FEL: <br type="_moz" /> </td>
  </tr>
  <tr>
   <td>3</td>
   <td>VARNING<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>4</td>
   <td>INFO<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>5</td>
   <td>FELSÖK<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>6</td>
   <td>TRACE<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>7</td>
   <td>ALLA<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

#### Loggkategorier {#logger-categories}

<table>
 <tbody>
  <tr>
   <th>Loggkategori</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>a</td>
   <td>xfa (skriptmotorrelaterade loggar)</td>
  </tr>
  <tr>
   <td>b</td>
   <td>xfaView (Layoutmotorrelaterade loggar)<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>c</td>
   <td>xfaPerf (prestandarelaterade loggar)<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

#### Loggkonfiguration {#log-configuration}

I logg-URL:en definieras frågesträngsparametern för loggkonfigurationen så här:

`{destination}-{a level}-{b level}-{c level}`

Till exempel:

<table>
 <tbody>
  <tr>
   <th>Loggkonfiguration</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>2-a4-b5-c6<br type="_moz" /> </td>
   <td>Mål: Server<br /> xfa-nivå: INFO<br /> xfaView-nivå: DEBUG<br /> xfaPerf-nivå: TRACE</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Standardloggnivån för varje loggkategori a (xfa), b (xfaView) och c (xfaPerf) är 2 (ERROR). För loggkonfigurationen 2-b6 är loggnivåerna för olika kategorier därför följande:
>a (xfa): 2 (standardnivåfel)
>b (xfaView): 6 (användarspecificerad TRACE)
>a (xfaPerf): 2 (standardnivåfel)

### Aktivera loggning med Configuration Manager {#enabling-logging-using-configuration-manager}

Om du använder Configuration Manager för att aktivera loggning genereras loggar för varje återgivningsbegäran tills loggningen inaktiveras igen.

1. Logga in på CQ Configuration Manager på `https://'[server]:[port]'/system/console/configMgr` och logga in med administratörsuppgifter.
1. Sök efter och klicka på **Mobile Forms Configurations**.
1. I textrutan Felsökningsalternativ anger du de loggkonfigurationer som beskrivs i föregående avsnitt, till exempel **2-a4-b5-c6**

   ![Forms-konfiguration](assets/forms_configuration.png)

   Forms Configuration

## Överför loggar {#uploading-logs}

Om målet är 1 dirigeras alla klientskriptloggmeddelanden till konsolen. Om en administratör kräver dessa loggar tillsammans med serverloggar anger du målnivån till 2. På den här nivån samlas alla loggar in i ett JS-objekt på klientsidan. Om formuläret återges med standardprofil visas en **Skicka loggar**-knapp till vänster om knappen **Markera befintliga fält** i verktygsfältet. När användaren klickar på länken registreras alla insamlade loggar på servern och loggas i den konfigurerade felloggfilen på servern.

Som standard läggs all information till i filen error.log i katalogen /crx-database/logs/.

Så här ändrar du plats och namn på loggfilen:

1. Logga in på Configuration Manager som administratör. Konfigurationshanterarens standardwebbadress är `https://'[server]:[port]'/system/console/configMgr`.
1. Klicka på **Konfiguration för Apache Sling Logging Logger**. En dialogruta visas.

   ![logconfig-1](assets/logconfig-1.png)

1. Ändra **loggnivån** till Felsök.

1. Ange sökväg och namn för **loggfilen**.

   >[!NOTE]
   >
   >Om du vill skapa loggar i samma katalog som andra loggfiler finns i anger du ../logs/&lt;filnamn> i egenskapen Loggfiler.

1. Ändra **Logger** till **HTMLFormsPerfLogger** och klicka på **Save**.
