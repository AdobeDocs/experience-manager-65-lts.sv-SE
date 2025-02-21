---
title: Konfigurera inställningar för säker administration för AEM Forms på JEE
description: Lär dig hur du administrerar användarkonton och tjänster som, trots att de krävs i en privat utvecklingsmiljö, inte krävs i en produktionsmiljö i AEM Forms på JEE.
content-type: reference
topic-tags: Security
products: SG_EXPERIENCEMANAGER/6.4
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Document Security,Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '847'
ht-degree: 0%

---

# Konfigurera inställningar för säker administration för AEM Forms på JEE {#configuring-secure-administration-settings-for-aem-forms-on-jee}

Lär dig hur du administrerar användarkonton och tjänster som, trots att de krävs i en privat utvecklingsmiljö, inte krävs i en produktionsmiljö i AEM Forms på JEE.

I allmänhet använder inte utvecklare produktionsmiljön för att bygga och testa applikationer. Därför måste du administrera användarkonton och tjänster som, trots att de krävs i en privat utvecklingsmiljö, inte krävs i en produktionsmiljö.

I den här artikeln beskrivs metoder för att minska den totala attackytan med hjälp av administrationsalternativ som finns i AEM Forms på JEE.

## Inaktiverar icke nödvändig fjärråtkomst till tjänster {#disabling-non-essential-remote-access-to-services}

När AEM Forms har installerats och konfigurerats på JEE är många tjänster tillgängliga för fjärranrop via SOAP och Enterprise JavaBeans™ (EJB). Termen fjärranrop avser i det här fallet alla anropare som har nätverksåtkomst till SOAP-, EJB- eller AMF-portarna (Action Message Format) för programservern.

Även om AEM Forms på JEE-tjänster kräver att giltiga autentiseringsuppgifter skickas för en auktoriserad anropare, bör du endast tillåta fjärråtkomst till de tjänster som du behöver för att kunna fjärråtkomst. För att uppnå begränsad tillgänglighet bör du minska uppsättningen fjärranslutna tjänster till minsta möjliga för ett fungerande system och sedan aktivera fjärranrop för de ytterligare tjänster du behöver.

AEM Forms på JEE-tjänster behöver alltid minst SOAP-åtkomst. Dessa tjänster krävs vanligtvis för Workbench, men omfattar även tjänster som anropas av Workspace webbprogram.

Gör så här med webbsidan Program och tjänster i administrationskonsolen:

1. Logga in på administrationskonsolen genom att skriva följande URL i en webbläsare:

   ```java
            https://[host name]:'port'/adminui
   ```

1. Klicka på **Tjänster > Program och tjänster > Inställningar**.
1. Ställ in inställningarna så att upp till 200 tjänster och slutpunkter visas på samma sida.
1. Klicka på **Tjänster** > **Program och tjänster** > **Endpoint Management**.
1. Välj **EJB** i listan **Provider** och klicka sedan på **Filter**.
1. Om du vill inaktivera alla EJB-slutpunkter markerar du kryssrutan bredvid var och en av dem i listan och klickar på **Inaktivera**.
1. Klicka på **Nästa** och upprepa föregående steg för alla EJB-slutpunkter. Kontrollera att EJB finns med i leverantörskolumnen innan du inaktiverar slutpunkterna.
1. Välj **SOAP** i listan **Provider** och klicka sedan på **Filter**.
1. Om du vill ta bort SOAP-slutpunkter markerar du kryssrutan bredvid var och en av dem i listan och klickar på **Ta bort**. Ta inte bort följande slutpunkter:

   * AuthenticationManagerService
   * DirectoryManagerService
   * JobManager
   * event_management_service
   * event_configuration_service
   * ProcessManager
   * TemplateManager
   * RepositoryService
   * TaskManagerService
   * TaskQueueManager
   * TaskManagerQueryService
   * WorkspaceSingleSignOn
   * ApplicationManager

1. Klicka på **Nästa** och upprepa föregående steg för SOAP-slutpunkter som inte finns i ovanstående lista. Kontrollera att SOAP finns med i leverantörskolumnen innan du tar bort slutpunkterna.

## Inaktiverar onödvändig anonym åtkomst till tjänster {#disabling-non-essential-anonymous-access-to-services}

Vissa Forms Server-tjänster tillåter oautentiserade (anonyma) anrop för vissa åtgärder. Det innebär att en eller flera åtgärder som exponeras av tjänsten kan anropas som en autentiserad användare eller som ingen autentiserad användare alls.

1. Logga in på administrationskonsolen genom att skriva följande URL i en webbläsare:

   ```java
            https://[host name]:'port'/adminui
   ```

1. Klicka på **Tjänster > Program och tjänster > Tjänsthantering**.
1. Klicka på namnet på den tjänst som du vill inaktivera (till exempel AuthenticationManagerService).
1. Klicka på fliken **Säkerhet**, avmarkera **Anonym åtkomst tillåten** och klicka på **Spara**.
1. Slutför steg 3 och 4 för följande tjänster:

   * AuthenticationManagerService
   * EJB
   * E-post
   * JobManager
   * WatchedFolder
   * UsermanagerUtilService
   * Remoting
   * RepositoryProviderService
   * EMCDocumentumRepositoryProvider
   * IBMFilenetRepositoryProvider
   * FormAugmenter
   * TaskManagerService
   * TaskManagerKoppling
   * TaskManagerQueryService
   * TaskQueueManager
   * AktivitetSlutpunktshanterare
   * UserService
   * WorkspaceSearchTemplateService
   * WorkspacePropertyService
   * OutputService
   * FormsService

   Om du tänker visa någon av dessa tjänster för fjärranrop bör du även inaktivera anonym åtkomst för dessa tjänster. Annars kan anropare med nätverksåtkomst till den här tjänsten anropa tjänsten utan att skicka giltiga autentiseringsuppgifter.

   Anonym åtkomst bör inaktiveras för alla tjänster som inte behövs. Många interna tjänster kräver att anonym autentisering är aktiverat eftersom de måste anropas av en potentiell användare i systemet utan att vara förauktoriserade.

## Ändra standardtimeout för global {#changing-the-default-global-time-out}

Slutanvändare kan autentisera till AEM Forms via Workbench, AEM Forms webbprogram eller anpassade program som anropar AEM Forms servertjänster. En global timeout-inställning används för att ange hur lång tid dessa användare kan interagera med AEM Forms (med en SAML-baserad försäkran) innan de tvingas autentisera igen. Standardinställningen är två timmar. I en produktionsmiljö måste tiden minskas till det minsta antal minuter som tillåts.

### Minimera tidsgränsen för omautentisering {#minimize-reauthentication-time-limit}

1. Logga in på administrationskonsolen genom att skriva följande URL i en webbläsare:

   ```java
            https://[host name]:'port'/adminui
   ```

1. Klicka på **Inställningar > Användarhantering > Konfiguration > Importera och exportera konfigurationsfiler**.
1. Klicka på **Exportera** om du vill skapa en config.xml-fil med de befintliga AEM Forms-inställningarna.
1. Öppna XML-filen i en redigerare och leta reda på följande post:

   `<entry key="assertionValidityInMinutes" value="120"/>`

1. Ändra värdet till ett tal som är större än 5 (i minuter) och spara filen.
1. Gå till sidan Importera och exportera konfigurationsfiler i administrationskonsolen.
1. Ange sökvägen till den ändrade filen config.xml eller klicka på Bläddra för att navigera till den.
1. Klicka på **Importera** för att överföra den ändrade filen config.xml och klicka sedan på **OK**.
