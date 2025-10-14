---
title: Konfigurera kataloger
description: Lär dig hur du lägger till, redigerar och tar bort kataloger och konfigurerar användarhantering så att den virtuella listvyn används.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/setting_up_and_managing_domains
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 2c0dd472-9697-4b96-b3fa-3c50883bf425
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '3253'
ht-degree: 0%

---

# Konfigurera kataloger {#configuring-directories}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Ange de kataloger som autentiseringsprovidern efterfrågar användarinformation för varje företagsdomän som du konfigurerar. Du kan konfigurera flera kataloger för en domän.

## Lägga till kataloger eller anpassade SPI-filer {#adding-directories-or-custom-spis}

Ange de kataloger som autentiseringsprovidern efterfrågar användarinformation för varje företagsdomän som du konfigurerar. Du kan lägga till en katalog i en befintlig företagsdomän eller i en ny företagsdomän som du lägger till. Du kan konfigurera flera kataloger för en domän. Du kan också konfigurera en domän så att den använder ett anpassat SPI (Service Provider Interface) för synkronisering.

### Lägg till en katalog {#add-a-directory}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på Ny företagsdomän eller välj en befintlig företagsdomän.
1. Klicka på Lägg till katalog.
1. I rutan Profilnamn anger du ett namn som skiljer den här katalogen från varandra och klickar sedan på Nästa.
1. Konfigurera inställningarna för katalogservern. (Se [Kataloginställningar](configuring-directories.md#directory-settings).)
1. Kontrollera att det går att ansluta till LDAP-servern genom att klicka på Testa. Om testet misslyckas kontrollerar du undantaget i loggfilen för programservern för att fastställa felets rotorsak. Klicka på Stäng och sedan på Nästa.
1. Välj Användarinställningar och konfigurera inställningarna efter behov. (Se [Kataloginställningar](configuring-directories.md#directory-settings).)
1. Om du vill verifiera att det grundläggande unika namnet och andra konfigurerade attribut samlar in rätt grupp med användare klickar du på Testa. LDAP försöker hämta de första 200 posterna med de angivna inställningarna (till exempel bas-DN, sökfilter och alla attribut).

   Om användarna returneras visar resultaten de värden som tilldelas varje fält enligt attributuppsättningen. Om testet misslyckas på grund av ett servernamn som inte finns, felaktig auktoriseringsinformation eller felaktiga attribut visas följande felmeddelande:&quot;De sökvillkor som anges returnerade inget resultat&quot;. Granska undantaget i loggfilen för Application Server för att fastställa felets rotorsak. Klicka på Stäng och sedan på Nästa.

1. Välj Gruppinställningar och konfigurera inställningarna efter behov. (Se [Kataloginställningar](configuring-directories.md#directory-settings).)
1. Om du vill verifiera att det grundläggande unika namnet och andra konfigurerade attribut samlar in rätt gruppuppsättning klickar du på Testa. Om grupper returneras visar resultaten de värden som tilldelas varje fält enligt attributuppsättningen. Klicka på Stäng.

### Lägga till en anpassad SPI {#add-a-custom-spi}

Mer information om hur du skapar en anpassad SPI finns i&quot;Utveckla SPI för AEM-formulär&quot; i [Programmering med AEM-formulär](https://www.adobe.com/go/learn_aemforms_programming_63). Om du vill göra en nyligen distribuerad anpassad SPI tillgänglig för association med domänen startar du om servern.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på Ny företagsdomän eller välj en befintlig företagsdomän.
1. Klicka på Lägg till katalog.
1. Skriv ett namn i rutan Profilnamn, välj Anpassad SPI-leverantör och klicka sedan på Nästa.
1. Välj en anpassad användarleverantör i listan och klicka på Nästa.
1. Välj en anpassad gruppleverantör i listan och klicka på Slutför.

## Redigera en katalog {#edit-a-directory}

Du kan redigera informationen om en katalog som du tidigare konfigurerat.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på lämplig domän i listan och välj lämplig katalog i listan på sidan som visas.
1. Konfigurera inställningar för katalog, användare och grupp efter behov. (Se [Kataloginställningar](configuring-directories.md#directory-settings).)
1. Klicka på OK.

## Ta bort en katalog {#delete-a-directory}

När du synkroniserar dina domäner efter att ha tagit bort en katalog markeras alla användare och grupper i den katalogen som inaktuella i databasen. De returneras inte i någon sökning från administrationskonsolen.

>[!NOTE]
>
>Företagsdomäner kräver minst en autentiseringsprovider och katalogprovider.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på lämplig domän i listan.
1. Markera kryssrutan för lämplig katalog och klicka på Ta bort.
1. Klicka på OK på bekräftelsesidan som visas och klicka på OK igen.

## Kataloginställningar {#directory-settings}

När du lägger till en katalog i en domän anger du följande kataloginställningar.

**Server:** (obligatoriskt) Fullständigt kvalificerat domännamn (FQDN) för katalogservern. För en dator som till exempel heter x i adobe.com är FQDN x.adobe.com. En IP-adress kan användas i stället för FQDN-servernamnet.

**Port:** (obligatoriskt) Den port som katalogservern använder. Vanligtvis 389, eller 636, om SSL-protokollet (Secure Sockets Layer) används för att skicka autentiseringsinformation över nätverket.

**SSL:** (obligatoriskt) Anger om katalogservern använder SSL när data skickas över nätverket. Standardvärdet är Nej. Om du anger Ja måste motsvarande LDAP-servercertifikat betraktas som tillförlitligt av JRE (Java™ runtime environment) på programservern.

**Bindning** (obligatoriskt) Anger hur du ska få åtkomst till katalogen.

**Anonym:** Inget användarnamn eller lösenord krävs. En anonym användare kan hämta endast en begränsad mängd data. Det här alternativet kan vara användbart för inledande testning.

**Användare:** Autentisering krävs. Ange namnet på den användarpost som har åtkomst till katalogen i rutan Namn. Det är bäst att ange det fullständiga unika namnet (DN) för användarkontot, till exempel cn=Jane Doe, ou=användare, dc=can, dc=com. Ange det associerade lösenordet i rutan Lösenord. Dessa inställningar krävs när du väljer Användare som bindningsalternativ.

**Namn:** Namn som kan användas för att ansluta till LDAP-databasen när anonym åtkomst inte har aktiverats. Ange `[domain name]\[userid]` för Active Directory 2003. För Sun™ One, eDirectory eller IBM Tivoli Directory Server anger du det fullständiga, kvalificerade namnet på användaren, till exempel uid=lcuser,ou=it,o=company.com.

**Lösenord:** Lösenord som motsvarar det namn du angav för att ansluta till LDAP-databasen när anonym åtkomst inte är aktiverad.

**Fyll sidan med:** Om du väljer det här alternativet fylls attribut på användar- och gruppinställningssidorna med motsvarande LDAP-standardvärden.

**Hämta bas-DN:n:** Hämtar bas-DN:erna och visar dem i listrutan. Den här inställningen är användbar när du har flera bas-DN och behöver välja ett värde.

**Aktivera hänvisning:** Den här inställningen gäller när din organisation använder flera Active Directory-domäner som är organiserade i en hierarkisk struktur och du har angett kataloginställningar för endast den överordnade domänen. Om du väljer det här alternativet kan användarhantering komma åt användar- och gruppinformation från de underordnade domänerna.

>[!NOTE]
>
>Klicka på Testa för att verifiera att det går att ansluta till LDAP-servern. Om du vill ta reda på orsaken till eventuella fel granskar du undantaget i loggfilen för programservern.

### Användarinställningar {#user-settings}

**Unik identifierare:** (obligatoriskt) Ett unikt och konstantattribut som används för att identifiera användare. Använd ett icke-DN-attribut som unik identifierare eftersom användarens unika namn kan ändras om användaren flyttar till en annan del av organisationen. Den här inställningen beror på katalogservern. Värdet är objectGUID för Active Directory 2003, nsuniqueID för Sun™ One och guid för eDirectory.

>[!NOTE]
>
>Se till att du anger ett attribut som garanterat är unikt i din organisation. Om du anger ett felaktigt värde kan det orsaka allvarliga systemproblem.

**Bas-DN:** Ange som startpunkt för synkronisering av användare och grupper från LDAP-hierarkin. Det är bäst att ange ett bas-DN på den lägsta nivån i hierarkin som omfattar alla användare och grupper som behöver synkroniseras för tjänster.

Om du valde alternativet Aktivera hänvisning i kataloginställningarna anger du basens unika namn till *dc*-delen av DN. För att referensen ska fungera måste sökintervallet innehålla både överordnade och underordnade domäner.

>[!NOTE]
>
>Inkludera inte användarens unika namn i den här inställningen. Om du vill synkronisera en viss användare använder du inställningen Sökfilter.

Även om Base DN är en obligatorisk inställning i administrationskonsolen kan vissa katalogservrar som IBM Domino Enterprise Server kräva ett tomt BaseDN. Om du vill ange ett tomt Base-DN exporterar du filen config.xml, redigerar inställningen i filen config.xml och importerar den sedan på nytt. (Se [Importera och exportera konfigurationsfilen](/help/forms/using/admin-help/importing-exporting-configuration-file.md#importing-and-exporting-the-configuration-file).)

**Sökfilter:** (obligatoriskt) Sökfiltret som ska användas för att hitta posten som är associerad med användaren. Du kan göra en sökning på en nivå eller en sökning på undernivå. (Se Sökfiltersyntax eller RFC 2254.) Mer information om Microsoft AD-schemat finns i Active Directory-schema.

**Beskrivning:** Schemaattribut för användarbeskrivningen

**Fullständigt namn:** (obligatoriskt) Schemaattribut för användarens fullständiga namn

**Inloggnings-ID:** (obligatoriskt) Schemaattribut för användarens inloggnings-ID

**Efternamn:** (obligatoriskt) Schemaattribut för användarens efternamn

**Förnamn:** (obligatoriskt) Schemaattribut för användarens förnamn

**Initialer:** Schemaattribut för användarens initialer

**Affärskalender:** Du kan mappa en affärskalender till en användare baserat på värdet för den här inställningen (affärskalendernyckeln). Affärskalendrar definierar affärsdagar och icke-affärsdagar. AEM-formulär kan använda affärskalendrar vid beräkning av framtida datum och tidpunkter för händelser som påminnelser, deadlines och eskalering. Hur du tilldelar användare affärskalendernycklar beror på om du använder en företagsdomän, lokal domän eller hybriddomän. (Se Konfigurera affärskalendrar.)

Om du använder en företagsdomän kan du mappa inställningen för Business Calendar till ett fält i LDAP-katalogen. Om till exempel varje användarpost i din katalog innehåller fältet *country* och du vill tilldela affärskalendrar baserat på det land där användaren finns, anger du fältnamnet *country* som värde för inställningen för affärskalender. Du kan sedan mappa affärskalendernycklarna (de värden som definieras för fältet *country* i LDAP-katalogen) till affärskalendrar i formulärarbetsflödet.

Mängden utrymme som används för att visa namnet på affärskalendernyckeln på arbetsflödessidorna för formulär är begränsad. Begränsa namnet på affärskalendernyckeln till färre än 53 tecken så att det inte trunkeras på dessa sidor.

**Ändra tidsstämpel:** Om du vill aktivera delta-katalogsynkronisering anger du det här värdet till att ändra TimeStamp. (Se Aktivera katalogsynkronisering av ändringar.)

**Organisation:** Schemaattribut för namnet på den organisation som användaren tillhör.

**Primär e-postadress:** Schemaattribut för användarens primära e-postadress.

**Sekundär e-postadress:** Schemaattribut för användarens sekundära e-postadress.

**Telefon:** Schemaattribut för användarens telefonnummer.

**Postadress:** Schemaattribut för användarens postadress.

**Språk:** Schemaattribut som innehåller ISO-språkinformationen. Värdet är en språkkod med två bokstäver eller en språk- och landskod.

**Tidszon:** Schemaattribut som innehåller tidszonen där användaren finns. Värdet är en sträng som Ort/Land.

**Aktivera VLV-kontroll (Virtual List View):** En LDAP-kontroll som gör att AEM-formulär kan hämta data i grupper från katalogservern. Om du använder Sun One som LDAP-katalog och katalogen innehåller många användare, skapar aktivering av VLV ett index som kan användas av användarhantering vid sökning efter användare. Den här funktionen är användbar när du använder ett vanligt användarkonto som bara kan synkronisera en begränsad mängd data. Du kan också aktivera VLV för grupper. Om du väljer Aktivera VLV-kontroll (Virtual List View) anger du ett namn i rutan Sorteringsfält.

>[!NOTE]
>
>Konfigurera Sun One om du vill aktivera VLV. Se [Konfigurera användarhantering för att använda VLV (Virtual List View)](configuring-directories.md#configure-user-management-to-use-virtual-list-view-vlv).

**Sorteringsfält:** Om du valde Aktivera VLV-kontroll (Virtual List View) anger du det attributnamn som används för att sortera indexet. Det här attributnamnet (till exempel uid) är det som du angav när du skapade ett index för VLV på katalogservern.

### Gruppinställningar {#group-settings}

**Unik identifierare:** (obligatoriskt) Ett unikt och konstantattribut som används för att identifiera grupper. Använd ett icke-DN-attribut som unik identifierare. Den här inställningen beror på katalogservern. Värdet är objectGUID för Active Directory 2003, nsuniqueID för Sun One och guid för eDirectory.

>[!NOTE]
>
>Se till att du anger ett attribut som garanterat är unikt i din organisation. Om du anger ett felaktigt värde kan det orsaka allvarliga systemproblem.

**Bas-DN:** (obligatoriskt) Ange katalogens unika namn.

Även om Base DN är en obligatorisk inställning i administrationskonsolen, kräver vissa katalogservrar som IBM Domino Enterprise Server ett tomt BaseDN. Om du vill ange ett tomt Base-DN exporterar du filen config.xml, redigerar inställningen i filen config.xml och importerar den sedan på nytt. (Se [Importera och exportera konfigurationsfilen](/help/forms/using/admin-help/importing-exporting-configuration-file.md#importing-and-exporting-the-configuration-file).)

**Sökfilter:** (obligatoriskt) Sökfiltret som ska användas för att hitta posten som är associerad med gruppen. Du kan göra en sökning på en nivå eller en sökning på undernivå.

**Beskrivning:** Schemaattribut för beskrivningen av gruppen

**Fullständigt namn:** (obligatoriskt) Schemaattribut för hela gruppnamnet

**Medlems-DN:** (obligatoriskt) Schemaattribut för det särskiljande namnet på medlemmar i en grupp

**Unik medlemsidentifierare:** Unik identifierare för en användare eller grupp som är medlem i den valda gruppen. Värdet beror på katalogservern. Värdet är objectSID för AD2003, nsuniqueID för Sun One och guid för eDirectory.

Om medlems-DN anges med ett icke-DN-attribut använder Hantering av användare Unik identifierare för att fråga LDAP om användarens unika namn så som det motsvarar ett unikt identifierarvärde.

Om DN anges som en unik identifierare behöver du inte konfigurera Unik identifierare för medlem.

**Organisation:** Schemaattribut för namnet på organisationen som gruppen tillhör

**Primär e-postadress:** Schemaattribut för gruppens primära e-postadress

**Sekundär e-postadress:** Schemaattribut för gruppens sekundära e-postadress

**Ändra tidsstämpel:** Om du vill aktivera delta-katalogsynkronisering anger du det här värdet till att ändra TimeStamp. (Se Aktivera katalogsynkronisering av ändringar.)

**Aktivera VLV-kontroll (Virtual List View):** En LDAP-kontroll som gör att AEM-formulär kan hämta data i grupper från katalogservern. Om du använder Sun One som LDAP-katalog och katalogen innehåller många grupper, skapar aktivering av VLV ett index som användarhantering kan använda när grupper söks. Den här funktionen är användbar när du använder ett vanligt användarkonto som bara kan synkronisera en begränsad mängd data. Du kan även aktivera VLV för användare. Om du väljer Aktivera VLV-kontroll (Virtual List View) anger du ett sorteringsfältnamn.

>[!NOTE]
>
>Konfigurera Sun One om du vill aktivera VLV. Se [Konfigurera användarhantering för att använda VLV (Virtual List View)](configuring-directories.md#configure-user-management-to-use-virtual-list-view-vlv).

**Sorteringsfältnamn:** Om du valde Aktivera VLV-kontroll (Virtual List View) anger du attributnamnet som används för att sortera indexet. Det här attributnamnet är det som du angav när du skapade ett index för VLV på katalogservern.

>[!NOTE]
>
>Klicka på Testa för att verifiera att användar- och gruppinställningarna samlas in baserat på bas-DN och sökvillkor.

Om användare och grupper returneras visar resultaten de värden som tilldelas varje fält enligt attributuppsättningen.

>[!NOTE]
>
>Användarhantering stöder inte duplicerade användar-ID:n inom en domän. Endast en användare med användar-ID synkroniseras.

## Konfigurera användarhantering för att använda VLV (Virtual List View) {#configure-user-management-to-use-virtual-list-view-vlv}

Katalogsynkronisering är ett viktigt krav för användarhantering. Användare och grupper synkroniseras från en Enterprise-katalog till AEM formulärdatabas för att tilldela roller och behörigheter. Antalet användare varierar mellan 100 och 10000+ beroende på kraven, och det utgör en utmaning när det gäller att synkronisera data effektivt.

LDAP-protokollet innehåller en mekanism för att fråga stora datauppsättningar på ett sidnumrerat sätt med hjälp av begärandekontroller. När du använder Microsoft Active Directory används PagedResultsControl för att hämta data i grupper av en viss storlek vid synkroniseringen av LDAP-formulär till AEM-formulärdatabasen. Sun ONE Directory Server stöder inte den här kontrollen. Om du vill slutföra en sidnumrerad fråga mot Sun ONE Directory Server använder du VLV-kontrollen (Virtual List View). Den här kontrollen omfattar både konfiguration på katalogservern och implementering på klientsidan.

>[!NOTE]
>
>I det här avsnittet beskrivs hur du använder VLV-kontrollen för Sun ONE Directory Server. Du kan dock använda den här kontrollen för alla katalogservrar som har stöd för VLV-kontroll.

1. När du konfigurerar katalogen väljer du Aktivera VLV-kontroll (Virtual List View) på både sidan Användarinställningar och på sidan Gruppinställningar. När du markerar kryssrutan måste du även ange ett sorteringsnamn i rutan Sorteringsfält. Standardvärdet är uid. (Se [Lägga till kataloger eller anpassade SPI:er](configuring-directories.md#adding-directories-or-custom-spis) eller [Redigera en katalog](configuring-directories.md#edit-a-directory).)
1. Använd administrationskonsolen Sun ONE eller ett kommandoradsskript för att skapa LDAP VLV-poster för användare och grupper. Om du använder ett kommandoradsskript kan du använda exempelanvändarna och gruppera LDIF-filer. (Se [Konfigurera Sun ONE Directory Server for VLV](configuring-directories.md#configuring-the-sun-one-directory-server-for-vlv).)
1. Stoppa servern och skapa ett nödvändigt index. (Se [Skapa katalogserverindex för VLV](configuring-directories.md#create-the-directory-server-index-for-vlv).)

### Konfigurera Sun ONE Directory Server for VLV {#configuring-the-sun-one-directory-server-for-vlv}

För att skapa en VLV krävs ett par poster som innehåller objektklasserna `vlvSearch` och `vlvIndex`. vlvSearch-posten innehåller en sökbas och attributet `vlvFilter`, som anger objektklassen som innehåller de attribut du vill sortera. Objektklassen `vlvIndex` innehåller attributet `vlvSort` som anger ett eller flera attribut att sortera och den ordning i vilken de ska sorteras. (Ett minustecken (-) betecknar omvänd alfabetisk ordning). Användning av VLV med AEM-formulär kräver separata poster för användare och grupper.

>[!NOTE]
>
>Objektposterna kan skapas med hjälp av det grafiska användargränssnittet i Sun ONE (GUI) eller via ett kommandoradsskript. Instruktioner om hur du skapar objektposter med hjälp av det grafiska användargränssnittet finns i dokumentationen för Sun ONE.

Här följer ett exempel på LDIF för VLV-post för användare:

```text
 dn: cn=lcuser,cn=userRoot,cn=ldbm database,cn=plugins,cn=config
 objectclass: top
 objectclass: vlvSearch
 cn: lcuser
 vlvBase: dc=corp,dc=adobe,dc=com
 vlvScope: 2
 vlvFilter: (&(objectclass=inetOrgPerson))
 aci: (target="ldap:///cn=lcuser,cn=userRoot,cn=ldbm database,cn=plugins,cn=config")(targetattr="*")(version 3.0; acl "Config"
 ;allow(read,search,compare) userdn="ldap:///all"; )
 dn: cn=lcuser,cn=lcuser,cn=userRoot,cn=ldbm database,cn=plugins,cn=config
 cn: lcuser
 vlvSort: cn
 objectclass: top
 objectclass: vlvIndex
```

**Skapa objektposterna med ett skript**

1. Exempelskriptet har en LDAP-post med namnet `lcuser`. Den här posten är avsedd för VLV-relaterad konfiguration för användarsynkronisering i AEM-formulär. Ändra följande egenskaper i enlighet med detta:

   **Postnamn:** Posten i det här exemplet är `lcuser`. Om `lcuser` ändras måste den ändras i alla områden i exempelskriptet.

   **vlvBase:** Basens unika namn som har angetts på sidan Användarinställningar.

   **vlvFilter:** Sökfiltret som anges på sidan Användarinställningar.

   **vlvSort:** Sorteringsfältet som anges i avsnittet VLV-inställningar på sidan Användarinställningar. En VLV-kontroll kräver att du anger en sorteringskontroll. Det här fältet används som sorteringsparameter för det VLV-index som skapas.

   **aci:** Åtkomstkontrollen som anges i exempelskriptet ger alla autentiserade användare behörighet att komma åt VLV-index för read-, search- och compare-åtgärder. Administratören kan begränsa åtkomsten till en bindningsanvändare, som är konfigurerad på sidan Katalogserverinställningar som är angiven i användargränssnittet för användarhantering. Om behörigheter inte anges kan användarsökningen inte använda VLV och LDAP-servern genererar ett behörighetsundantag.

   >[!NOTE]
   >
   >Som en konvention är vlvIndex-postens namn också inställt på `lcuser`, men du kan ge det ett annat namn. Använd samma namn i vlvindex-verktyget. (Se [Skapa katalogserverindex för VLV &#x200B;](configuring-directories.md#create-the-directory-server-index-for-vlv)*.)*

1. Använd verktyget `ldapmodify` som ingår i Sun ONE Server för att skapa en liknande post för grupper med hjälp av gruppens basnamn, sökfilter respektive sorteringsfält:

   `server directory\shared\bin>ldapmodify -v -a -h host -p port -D "admin user" -w "password" -f "LDIF file location"`

   Skriv till exempel följande text:

   `D:\tools\ldap\sun\shared\bin> -v -a -h localhost -p 55850 -D "uid=admin,ou=administrators,ou=topologymanagement,o=netscaperoot" -w "admin" -f "D:\tools\ldap\data\vlv feature\users.ldif"`

### Skapa katalogserverindex för VLV {#create-the-directory-server-index-for-vlv}

När du har konfigurerat kataloginställningarna och skapat LDAP VLV-poster för användare och grupper stoppar du servern och skapar det index som krävs.

1. Stoppa Sun ONE Server när du har skapat objektposter.
1. Generera indexet med vlvindex-verktyget genom att skriva följande text:

   *katalogserverinstans* `\vlvindex.bat -n userRoot -T lcuser`

   Följande utdata genereras:

   ```shell
    D:\tools\ldap\sun\shared\bin>..\..\slapd-chetanmeh-xp3\vlvindex.bat -n userRoot -T livecycle
    [21/Nov/2007:16:47:26 +051800] - userRoot: Indexing VLV: livecycle
    [21/Nov/2007:16:47:27 +051800] - userRoot: Indexed 1000 entries (5%).
    [21/Nov/2007:16:47:27 +051800] - userRoot: Indexed 2000 entries (9%).
    ...
    [21/Nov/2007:16:47:29 +051800] - userRoot: Indexed 20000 entries (94%).
    [21/Nov/2007:16:47:29 +051800] - userRoot: Indexed 21000 entries (99%).
    [21/Nov/2007:16:47:29 +051800] - userRoot: Finished indexing.
   ```

   Vlvindex-verktyget finns i katalogserverns instanskatalog. Om Sun ONE Server har två instanser som kör server1 och server2 finns verktyget vlvindex i katalogen *Sun ONE server*\server1. Värdet för parametern `-T` är värdet för attributet `cn` för den Vlvindex-post som skapades tidigare i LDIF-exempelfilen. I det här fallet är det `lcuser`.

1. Om VLV även är aktiverat för grupper skapar du motsvarande index för grupperna. Kontrollera om indexen har skapats med följande kommando:

   *sol en serverkatalog* `\shared\bin>ldapsearch -h`*värdnamn* `-p`*port nr* `-s base -b "" objectclass=*`

   Utdata som följande exempeldata genereras:

   ```shell
    D:\tools\ldap\sun\shared\bin>ldapsearch.exe -h localhost -p 55850 -s base -b "" objectclass=*
    ldapsearch.exe: started Tue Nov 27 16:34:20 2007
    version: 1
    dn:
    objectClass: top
    namingContexts: dc=corp,dc=adobe,dc=com
    supportedExtension: 2.16.840.1.113730.3.5.7
    ...
    vlvsearch: cn=MCC ou=testdata dc=corp dc=adobe dc=com, cn=userRoot,cn=ldbm dat
        abase,cn=plugins,cn=config
    vlvsearch: cn=lcuser,cn=userRoot,cn=ldbm database,cn=plugins,cn=config
    vlvsearch: cn=Browsing ou=testdata,cn=userRoot,cn=ldbm database,cn=plugins,cn=
        config
    1 matches
   ```
