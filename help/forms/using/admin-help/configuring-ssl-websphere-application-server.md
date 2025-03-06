---
title: Konfigurera SSL för WebSphere Application Server
description: Lär dig hur du konfigurerar SSL för WebSphere Application Server.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_ssl
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Document Security
role: User, Developer
exl-id: 0caac293-98b4-4e73-9440-f1db68c94054
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 0%

---

# Konfigurera SSL för WebSphere Application Server {#configuring-ssl-for-websphere-application-server}

I det här avsnittet beskrivs följande steg för att konfigurera SSL med IBM WebSphere Application Server.

## Skapa ett lokalt användarkonto i WebSphere {#creating-a-local-user-account-on-websphere}

För att aktivera SSL behöver WebSphere åtkomst till ett användarkonto i det lokala operativsystemets användarregister som har behörighet att administrera systemet:

* (Windows) Skapa en Windows-användare som ingår i gruppen Administratörer och har behörighet att fungera som en del av operativsystemet. (Se [Skapa en Windows-användare för WebSphere](configuring-ssl-websphere-application-server.md#create-a-windows-user-for-websphere).)
* (Linux, UNIX) Användaren kan vara en rotanvändare eller en annan användare som har rotbehörighet. När du aktiverar SSL på WebSphere använder du den här användarens server-ID och lösenord.

### Skapa en Linux- eller UNIX-användare för WebSphere {#create-a-linux-or-unix-user-for-websphere}

1. Logga in som root-användare.
1. Skapa en användare genom att ange följande kommando i en kommandotolk:

   * (Linux och Sun Solaris) `useradd`
   * (IBM AIX) `mkuser`

1. Ställ in lösenordet för den nya användaren genom att ange `passwd` i kommandotolken.
1. (Linux och Solaris) Skapa en skugglösenordsfil genom att ange `pwconv` (utan parametrar) i kommandotolken.

   >[!NOTE]
   >
   >(Linux och Solaris) För att säkerhetsregistret för det lokala operativsystemet WebSphere Application Server ska fungera måste det finnas en skugglösenordsfil. Skugglösenordsfilen heter **vanligtvis /etc/shadow** och baseras på filen /etc/passwd. Om skugglösenordsfilen inte finns uppstår ett fel när du har aktiverat global säkerhet och konfigurerat användarregistret som lokalt operativsystem.

1. Öppna gruppfilen från katalogen /etc i en textredigerare.
1. Lägg till användaren som du skapade i steg 2 i `root` gruppen.
1. Spara och stäng filen.
1. (UNIX med SSL aktiverat) Starta och stoppa WebSphere som rotanvändare.

### Skapa en Windows-användare för WebSphere {#create-a-windows-user-for-websphere}

1. Logga in i Windows med ett administratörsanvändarkonto.
1. Välj **Start > Kontrollpanelen > Administrationsverktyg > Datorhantering > Lokala användare och grupper**.
1. Högerklicka på Användare och välj **Ny användare**.
1. Skriv ett användarnamn och lösenord i rutorna och ange eventuell annan information som du vill ha i de övriga rutorna.
1. Avmarkera **Användaren måste ändra lösenordet vid nästa inloggning**, klicka på **Skapa** och klicka sedan på **Stäng**.
1. Klicka på **Användare**, högerklicka på den användare du skapade och välj **Egenskaper**.
1. Klicka på fliken **Medlem i** och sedan på **Lägg till**.
1. Skriv `Administrators` i rutan Ange de objektnamn som ska markeras och klicka på Kontrollera namn för att se till att gruppnamnet är korrekt.
1. Klicka på **OK** och sedan på **OK** igen.
1. Välj **Start > Kontrollpanelen > Administrationsverktyg > Lokal säkerhetsprincip > Lokala profiler**.
1. Klicka på Tilldelning av användarrättigheter, högerklicka sedan på Agera som en del av operativsystemet och välj Egenskaper.
1. Klicka på **Lägg till användare eller grupp**.
1. Skriv namnet på den användare som du skapade i steg 4 i rutan Ange de objektnamn som ska väljas, klicka på **Kontrollera namn** för att kontrollera att namnet är korrekt och klicka sedan på **OK**.
1. Klicka på **OK** för att stänga funktionsmakrot som en del av dialogrutan för operativsystemsegenskaper.

### Konfigurera WebSphere för att använda den nyskapade användaren som administratör {#configure-websphere-to-use-the-newly-created-user-as-administrator}

1. Kontrollera att WebSphere körs.
1. Välj **Säkerhet > Global säkerhet** i administrationskonsolen för WebSphere.
1. Välj **Administrativa användarroller** under Administrativ säkerhet.
1. Klicka på Lägg till och gör följande:

   1. Skriv **&amp;ast;** i sökrutan och klicka på Sök.
   1. Klicka på **Administratör** under roller.
   1. Lägg till den nyskapade användaren i Mappad till roll och mappa den till Administratör.

1. Klicka på **OK** och spara ändringarna.
1. Starta om WebSphere-profilen.

## Aktivera administrativ säkerhet {#enable-administrative-security}

1. Välj **Säkerhet > Global säkerhet** i administrationskonsolen för WebSphere.
1. Klicka på **Guiden Säkerhetskonfiguration**.
1. Kontrollera att kryssrutan **Aktivera programsäkerhet** är aktiverad. Klicka på **Nästa**.
1. Välj **Federated databaser** och klicka på **Nästa**.
1. Ange de autentiseringsuppgifter som du vill ange och klicka på **Nästa**.
1. Klicka på **Slutför**.
1. Starta om WebSphere-profilen.

   WebSphere börjar använda standardnyckelbehållaren och förtrostore.

## Aktivera SSL (anpassad nyckel och förvaltararkiv) {#enable-ssl-custom-key-and-truststore}

Du kan skapa förtroendelager och nyckelbehållare med hjälp av nyckelverktyget eller Admin Console. Se till att installationssökvägen för WebSphere inte innehåller parenteser så att nyckelmannen fungerar som den ska.

1. I administrationskonsolen för WebSphere väljer du **Säkerhet > SSL-certifikat och nyckelhantering**.
1. Klicka på **Nyckelarkiv och certifikat** under Relaterade objekt.
1. I listrutan **Nyckelarkiv använder** kontrollerar du att **SSL-nyckelbehållare** är markerat. Klicka på **Ny**.
1. Ange ett logiskt namn och en beskrivning.
1. Ange sökvägen där du vill att nyckelbehållaren ska skapas. Om du redan har skapat en nyckelbehållare via ikeyman anger du sökvägen till nyckellagringsfilen.
1. Ange och bekräfta lösenordet.
1. Välj typ av nyckelbehållare och klicka på **Använd**.
1. Spara huvudkonfigurationen.
1. Klicka på **Personligt certifikat**.
1. Om du redan har skapat en nyckelbehållare med nyckelhanteraren visas ditt certifikat. Annars måste du lägga till ett nytt självsignerat certifikat genom att utföra följande steg:

   1. Välj **Skapa > självsignerat certifikat**.
   1. Ange lämpliga värden i certifikatformuläret. Se till att du behåller Alias och nätverksnamn som fullständigt kvalificerade domännamn för datorn.
   1. Klicka på **Använd**.

1. Upprepa steg 2 till 10 för att skapa en truststore.

## Tillämpa anpassad keystore och truststore på servern {#apply-custom-keystore-and-truststore-to-the-server}

1. I WebSphere Administrationskonsol väljer du **Säkerhet > SSL-certifikat och nyckelhantering**.
1. Klicka på **Hantera konfiguration** av slutpunktssäkerhet. Den lokala topologikartan öppnas.
1. Under Inkommande väljer du direkt underordnad noderna.
1. Under Relaterade objekt väljer du **SSL-konfigurationer**.
1. Välj **NodeDeafultSSLSetting**.
1. I listrutorna för förvaltararkivnamn och nyckelbehållarnamn väljer du det anpassade förvaltararkivet och nyckelbehållaren som du skapade.
1. Klicka på **Använd**.
1. Spara huvudkonfigurationen.
1. Starta om WebSphere-profilen.

   Din profil kan nu köras med anpassade SSL-inställningar och ditt certifikat.

## Aktivera stöd för AEM-blanketter {#enabling-support-for-aem-forms-natives}

1. Välj **Säkerhet > Global säkerhet** i administrationskonsolen för WebSphere.
1. Expandera **RMI/IIOP-säkerhet** i avsnittet Autentisering och klicka på **Inkommande CSIv2-kommunikation**.
1. Kontrollera att **SSL-supported** är markerat i listrutan Transport.
1. Starta om WebSphere-profilen.

## Konfigurera WebSphere för att konvertera URL:er som börjar med https {#configuring-websphere-to-convert-urls-that-begins-with-https}

Om du vill konvertera en URL som börjar med https lägger du till ett signerarcertifikat för den URL:en på WebSphere-servern.

**Skapa ett signerarcertifikat för en https-aktiverad webbplats**

1. Kontrollera att WebSphere körs.
1. I administrationskonsolen för WebSphere navigerar du till signerarcertifikat och klickar sedan på Säkerhet > SSL-certifikat och nyckelhantering > Nyckelarkiv och certifikat > NodeDefaultTrustStore > Signerarcertifikat.
1. Klicka på Hämta från port och utför följande åtgärder:

   * Skriv URL-adressen i rutan Värd. Skriv till exempel `www.paypal.com`.
   * Skriv `443` i rutan Port. Den här porten är standardport för SSL.
   * Skriv ett alias i rutan Alias.

1. Klicka på Hämta signerarinformation och kontrollera sedan att informationen har hämtats.
1. Klicka på Använd och sedan på Spara.

Konvertering från HTML till PDF från den webbplats vars certifikat läggs till fungerar nu i tjänsten Generate PDF.

>[!NOTE]
>
>Ett signerarcertifikat krävs för att ett program ska kunna ansluta till SSL-webbplatser inifrån WebSphere. Den används av Java Secure Socket Extensions (JSSE) för att validera certifikat som fjärrsidan av anslutningen skickar under en SSL-handskakning.

## Konfigurera dynamiska portar {#configuring-dynamic-ports}

IBM WebSphere tillåter inte flera anrop till ORB.init() när Global Security är aktiverat. Du kan läsa om permanent begränsning på https://www-01.ibm.com/support/docview.wss?uid=swg1PK58704.

Utför följande steg för att ange att porten ska vara dynamisk och för att lösa problemet:

1. I administrationskonsolen för WebSphere väljer du **Servrar** > **Servertyper** > **Programserver för WebSphere**.
1. Välj servern i avsnittet Inställningar.
1. Expandera **Portar** under **Kommunikation** på fliken **Konfiguration** och klicka på **Information**.
1. Klicka på följande portnamn, ändra **portnummer** till 0 och klicka på **OK**.

   * `ORB_LISTENER_ADDRESS`
   * `SAS_SSL_SERVERAUTH_LISTENER_ADDRESS`
   * `CSIV2_SSL_SERVERAUTH_LISTENER_ADDRESS`
   * `CSIV2_SSL_MUTUALAUTH_LISTENER_ADDRESS`

## Konfigurera filen sling.properties {#configure-the-sling-properties-file}

1. Öppna `[aem-forms_root]`\crx-repository\launchpad\sling.properties för redigering.
1. Leta reda på egenskapen `sling.bootdelegation.ibm` och lägg till `com.ibm.websphere.ssl.*` i värdefältet. Det uppdaterade fältet ser ut så här:

   ```shell
   sling.bootdelegation.ibm=com.ibm.xml.*, com.ibm.websphere.ssl.*
   ```

1. Spara filen och starta om servern.
