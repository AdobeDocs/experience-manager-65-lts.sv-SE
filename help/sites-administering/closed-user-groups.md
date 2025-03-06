---
title: Stängda användargrupper i AEM
description: Läs mer om slutna användargrupper och de fördelar de medför för skalbarhet och säkerhet i AEM.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
docset: aem65
feature: Security
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 0c20efb1-9b01-41ef-b38d-261fb4b0ff91
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '6662'
ht-degree: 0%

---

# Stängda användargrupper i AEM{#closed-user-groups-in-aem}

## Introduktion {#introduction}

Sedan AEM 6.3 finns en ny implementering av en sluten användargrupp som är avsedd att åtgärda de problem med prestanda, skalbarhet och säkerhet som den befintliga implementeringen innebär.

>[!NOTE]
>
>För enkelhetens skull används förkortningen CUG genomgående i denna dokumentation.

Målet med den nya implementeringen är att vid behov täcka befintliga funktioner och samtidigt åtgärda problem och designbegränsningar från äldre versioner. Resultatet blir en ny CUG-design med följande egenskaper:

* Tydlig separation av autentiserings- och auktoriseringselement, som kan användas var för sig eller i kombination.
* Dedikerad tillståndsmodell som återspeglar den begränsade läsåtkomsten vid de konfigurerade CUG-träden utan att påverka andra åtkomstkontrollinställningar och behörighetskrav.
* Åtkomstkontrollinställningarna för den begränsade läsåtkomsten, som behövs för redigeringsförekomster, skiljer sig åt och behörighetsutvärderingen som bara är önskvärd vid publicering.
* Redigering av begränsad läsbehörighet utan eskalering av behörigheter.
* Dedikerat nodtypstillägg för att markera autentiseringskravet.
* Valfri inloggningssökväg som är associerad med autentiseringskravet.

### Implementering av ny anpassad användargrupp {#the-new-custom-user-group-implementation}

En CUG som den är känd i AEM består av följande steg:

* Begränsa läsåtkomst för trädet som måste skyddas och endast tillåta läsning för objekt som antingen är listade med en viss CUG-instans eller exkluderade från CUG-utvärderingen. Detta kallas **auktoriseringselementet**.
* Tvinga autentisering för ett visst träd och ange eventuellt en dedikerad inloggningssida för det trädet som sedan exkluderas. Detta kallas elementet **authentication**.

Den nya implementeringen har utformats för att skapa en gräns mellan autentiserings- och auktoriseringselementen. Från och med AEM 6.3 är det möjligt att begränsa läsåtkomst utan att uttryckligen lägga till ett autentiseringskrav. Om till exempel en viss instans kräver autentisering helt eller ett visst träd redan finns i ett underträd som redan kräver autentisering.

På samma sätt kan ett visst träd markeras med ett autentiseringskrav utan att ändra den gällande behörighetsinställningen. Kombinationerna och resultaten visas i avsnittet [Kombinera CUG-principer och autentiseringskrav](/help/sites-administering/closed-user-groups.md#combining-cug-policies-and-the-authentication-requirement).

## Ökning {#overview}

### Behörighet: Begränsa läsåtkomst {#authorization-restricting-read-access}

Nyckelfunktionen i en CUG-fil är att begränsa läsåtkomst för ett visst träd i innehållsdatabasen för alla utom valda huvuden. I stället för att manipulera standardinnehållet för åtkomstkontroll på direkten har den nya implementeringen en annan metod genom att definiera en dedikerad typ av åtkomstkontrollprincip som representerar en CUG.

#### Åtkomstkontrollprincip för CUG {#access-control-policy-for-cug}

Den nya typen av policy har följande egenskaper:

* Åtkomstkontrollprincip av typen org.apache.jackrabbit.api.security.permission.PrincipalSetPolicy (definieras av API:t för Apache Jackrabbit).
* PrincipalSetPolicy ger behörighet till en uppsättning av huvudkonton som kan ändras.
* De privilegier som ges och åtgärdens omfattning är en genomförandedetalj.

Implementeringen av PrincipalSetPolicy som används för att representera CUG:er definierar dessutom följande:

* CUG-principer ger endast läsåtkomst till vanliga JCR-objekt (till exempel utelämnas åtkomstkontrollinnehåll).
* Omfånget definieras av den åtkomststyrda nod som innehåller CUG-principen.
* CUG-principer kan kapslas, en kapslad CUG startar en ny CUG utan att ärva huvuduppsättningen i CUG-filen för överordnad.
* Om utvärdering är aktiverat ärvs effekten av principen till hela underträdet ned till nästa kapslade CUG.

Dessa CUG-principer distribueras till en AEM-instans via en separat autentiseringsmodul som kallas ekaauktoriseringskug. Den här modulen har en egen åtkomststyrningshantering och behörighetsutvärdering. Med andra ord, standardkonfigurationen för AEM levereras en konfiguration för Oak innehållsdatabas som kombinerar flera auktoriseringsmekanismer. Mer information finns på [den här sidan i dokumentationen för Apache Oak ](https://jackrabbit.apache.org/oak/docs/security/authorization/composite.html).

I den här sammansatta konfigurationen ersätter inte en ny CUG det befintliga åtkomstkontrollsinnehållet som är kopplat till målnoden. Det är i stället ett tillägg som också kan tas bort senare utan att den ursprungliga åtkomstkontrollen påverkas. Som standard är det en åtkomstkontrollista i AEM.

Till skillnad från den tidigare implementeringen identifieras och behandlas de nya CUG-reglerna alltid som innehåll för åtkomstkontroll. Det innebär att de skapas och redigeras med JCR-API:t för åtkomstkontroll. Mer information finns i avsnittet [Hantera CUG-principer](#managing-cug-policies).

#### Behörighetsutvärdering av CUG-principer {#permission-evaluation-of-cug-policies}

Förutom en dedikerad åtkomstkontrollshantering för CUG:er kan du med den nya auktoriseringsmodellen villkorligt aktivera behörighetsutvärdering för dess principer. Detta gör att du kan konfigurera CUG-principer i en staging-miljö, och bara aktiverar utvärdering av de effektiva behörigheterna när de har replikerats till produktionsmiljön.

Behörighetsutvärderingen för CUG-profiler och interaktionen med standardauktoriseringsmodellen eller någon annan auktoriseringsmodell följer mönstret som utformats för flera auktoriseringsmekanismer i Apache Jackrabbit Oak. Det innebär att en viss uppsättning behörigheter beviljas endast om alla modeller beviljar åtkomst. Mer information finns i [Jackrabbit Oak-dokumentationen](https://jackrabbit.apache.org/oak/docs/security/authorization/composite.html).

Följande egenskaper gäller för behörighetsutvärderingen som är kopplad till behörighetsmodellen som är utformad för att hantera och utvärdera CUG-principer:

* Det hanterar bara läsbehörigheter för vanliga noder och egenskaper, men inte läsåtkomstkontrollinnehåll
* Det hanterar inte skrivbehörigheter eller andra typer av behörigheter som krävs för att ändra skyddat JCR-innehåll (åtkomstkontroll, information om nodtyp, versionshantering, låsning eller användarhantering bland annat). Dessa behörigheter påverkas inte av en CUG-princip och utvärderas inte av den associerade auktoriseringsmodellen. Om dessa behörigheter beviljas beror på de andra modellerna som har konfigurerats i säkerhetsinställningarna.

Effekten av en enda CUG-princip vid utvärdering av tillstånd kan sammanfattas enligt följande:

* Läsåtkomst nekas för alla utom för ämnen som innehåller uteslutna principer eller principer som anges i policyn.
* Principen får verkan på den åtkomststyrda nod som innehåller policyn och dess egenskaper.
* Effekten ärvs också nedåt i hierarkin, dvs. det objektträd som definieras av den åtkomststyrda noden.
* Den påverkar dock varken syskon eller överordnade noder för den åtkomststyrda noden.
* Arvet av en viss CUG stoppas vid en kapslad CUG.

#### Bästa praxis {#best-practices}

Följande metodtips bör omfatta definition av begränsad läsåtkomst via CUG:er:

* Fatta ett medvetet beslut om huruvida ditt behov av en CUG handlar om att begränsa läsåtkomst eller autentiseringskrav. Om det senare, eller om det finns ett behov av båda, finns mer information om autentiseringskrav i avsnittet Bästa metoder
* Skapa en hotmodell för data eller innehåll som måste skyddas för att identifiera hotgränser och få en tydlig bild av känsligheten hos data och de roller som är kopplade till auktoriserad åtkomst
* Modellera databasinnehållet och kundupplevelsegrupperna med hänsyn till allmänna aspekter av auktorisering och bästa praxis:

   * Kom ihåg att läsbehörighet endast beviljas om en viss användargränssnittskontroll och utvärderingen av andra moduler som distribueras i konfigurationsbidraget tillåter att ett visst ämne läser ett visst databasobjekt
   * Undvik att skapa redundanta användargrupper där läsåtkomst redan är begränsad av andra auktoriseringsmoduler
   * För stort behov av kapslade CUG:er kan potentiellt markera problem i innehållsdesignen
   * För stort behov av användargränssnitten (t.ex. på varje sida) kan tyda på att det finns ett behov av en anpassad auktoriseringsmodell som eventuellt är bättre anpassad för de specifika säkerhetsbehoven i programmet och det aktuella innehållet.

* Begränsa sökvägarna som stöds för CUG-principer till några få träd i databasen för optimerade prestanda. Tillåt t.ex. bara CUG:er under noden /content som levererats som standardvärde sedan AEM 6.3.
* CUG-profiler är utformade för att ge läsåtkomst till en liten uppsättning huvudobjekt. Behovet av ett stort antal huvudansvariga kan lyfta fram frågor i innehållet eller programdesignen och bör omprövas.

### Autentisering: Definiera autentiseringskrav {#authentication-defining-the-auth-requirement}

De autentiseringsrelaterade delarna av CUG-funktionen gör att du kan markera träd som kräver autentisering och eventuellt ange en dedikerad inloggningssida. I enlighet med den tidigare versionen kan du med den nya implementeringen markera träd som kräver verifiering i innehållsdatabasen. Den aktiverar även villkorligt synkronisering med `Sling org.apache.sling.api.auth.Authenticator`som är ansvarig för att i slutändan verkställa kravet och omdirigera till en inloggningsresurs.

Dessa krav har registrerats med autentiseraren av en OSGi-tjänst som tillhandahåller registreringsegenskapen `sling.auth.requirements`. Dessa egenskaper används sedan för att dynamiskt utöka autentiseringskraven. Mer information finns i [säljdokumentationen](https://sling.apache.org/apidocs/sling7/org/apache/sling/auth/core/AuthConstants.html#AUTH_REQUIREMENTS).

#### Definiera autentiseringskravet med en dedikerad blandningstyp {#defining-the-authentication-requirement-with-a-dedicated-mixin-type}

Av säkerhetsskäl ersätter den nya implementeringen användningen av en kvarvarande JCR-egenskap med en dedikerad mixin-typ, `granite:AuthenticationRequired`, som definierar en valfri egenskap av typen STRING för inloggningssökvägen `granite:loginPath`. Det är bara innehållsändringar som är relaterade till den här mixin-typen som leder till en uppdatering av de krav som registrerats med Apache Sling Authenticator. Ändringarna spåras vid beständiga tillfälliga ändringar och kräver därför ett `javax.jcr.Session.save()`-anrop för att börja gälla.

Samma sak gäller för egenskapen `granite:loginPath`. Den respekteras endast om den definieras av autenticeringskravet för relaterad blandningstyp. Om du lägger till en restegenskap med det här namnet i en ostrukturerad JCR-nod visas inte den önskade effekten och egenskapen ignoreras av hanteraren som ansvarar för uppdateringen av OSGi-registreringen.

>[!NOTE]
>
>Inställningen av inloggningssökvägsegenskapen är valfri och behövs bara om trädet som kräver autentisering inte kan återgå till standardsidan eller en på annat sätt ärvd inloggningssida. Se [Utvärdering av inloggningssökväg](/help/sites-administering/closed-user-groups.md#evaluation-of-login-path) nedan.

#### Registrerar autentiseringskravet och inloggningssökvägen med SSLING-autentiseraren {#registering-the-authentication-requirement-and-login-path-with-the-sling-authenticator}

Eftersom den här typen av autentiseringskrav förväntas begränsas till vissa körningslägen och till en liten delmängd av träd i innehållsdatabasen, är spårning av kravet på blandningstyp och egenskaper för inloggningssökväg villkorlig. Den är dessutom bunden till en motsvarande konfiguration som definierar de sökvägar som stöds (se Konfigurationsalternativ nedan). Därför utlöser endast ändringar inom omfånget för de här sökvägarna en uppdatering av OSGi-registreringen, någon annanstans ignoreras både mixin-typen och egenskapen.

AEM standardinställning använder nu den här konfigurationen genom att tillåta att mixinen ställs in i redigeringsläget, men att den endast får effekt vid replikering till publiceringsinstansen. I dokumentationen för [Sling Authentication - Framework](https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-framework.html) finns mer information om hur Sling verkställer autentiseringskravet.

Om du lägger till blandningstypen `granite:AuthenticationRequired` i de sökvägar som stöds, uppdateras OSGi-registreringen av den ansvariga hanteraren med en ny, extra post med egenskapen `sling.auth.requirements`. Om ett givet autentiseringskrav anger den valfria egenskapen `granite:loginPath`, registreras värdet även med autentiseraren med ett &#39;-&#39;-prefix som ska uteslutas från autentiseringskravet.

#### Utvärdering och arv av autentiseringskrav {#evaluation-and-inheritance-of-the-authentication-requirement}

Autentiseringskraven för Apache Sling ärvs via sid- eller nodhierarkin. Själva detaljerna om arvet och utvärderingen av autentiseringskrav som ordning och prioritet betraktas som en implementeringsdetalj och kommer inte att dokumenteras i den här artikeln.

#### Utvärdering av inloggningssökväg {#evaluation-of-login-path}

Utvärderingen av inloggningssökvägen och omdirigeringen till motsvarande resurs vid autentisering är en implementeringsinformation i autentiseringshanteraren för Adobe Granite-inloggningsväljaren ( `com.day.cq.auth.impl.LoginSelectorHandler`), som är en Apache Sling AuthenticationHandler som konfigurerats med AEM som standard.

När hanteraren anropar `AuthenticationHandler.requestCredentials` försöker den avgöra vilken inloggningssida för mappningen som användaren omdirigeras till. Detta inkluderar följande steg:

* Skilja mellan utgångna lösenord och behovet av regelbunden inloggning som orsak till omdirigeringen.
* Om en vanlig inloggning används testas om en inloggningssökväg kan hämtas i följande ordning:

   * från LoginPathProvider som implementerats av nya `com.adobe.granite.auth.requirement.impl.RequirementService`,
   * från den gamla inaktuella CUG-implementeringen,
   * från inloggningssidmappningar, enligt definition i `LoginSelectorHandler`,
   * och slutligen, återgå till standardinloggningssidan, enligt definitionen med `LoginSelectorHandler`.

* När en giltig inloggningssökväg hämtas via de anrop som anges ovan, dirigeras användarens begäran om till den sidan.

Målet för den här dokumentationen är utvärderingen av inloggningssökvägen som visas av det interna `LoginPathProvider`-gränssnittet. Implementeringen som gjordes sedan AEM 6.3 fungerar på följande sätt:

* Registrering av inloggningssökvägar beror på skillnaden mellan lösenord som har upphört att gälla och behovet av regelbunden inloggning som orsak till omdirigeringen
* Om du loggar in regelbundet kontrolleras om en inloggningssökväg kan hämtas i följande ordning:

   * från `LoginPathProvider` som implementerats av nya `com.adobe.granite.auth.requirement.impl.RequirementService`,
   * från den gamla inaktuella CUG-implementeringen,
   * från inloggningssidmappningar som definierats med `LoginSelectorHandler`,
   * och till slut återgå till standardinloggningssidan som definierats med `LoginSelectorHandler`.

* När en giltig inloggningssökväg hämtas via de anrop som anges ovan, dirigeras användarens begäran om till den sidan.

`LoginPathProvider`, som implementeras av det nya stödet för autentiseringskrav i Granite, visar inloggningssökvägar som definieras av `granite:loginPath` -egenskaperna, som i sin tur definieras av blandningstypen enligt beskrivningen ovan. Mappningen av resurssökvägen som innehåller inloggningssökvägen och egenskapsvärdet behålls i minnet och utvärderas för att hitta en lämplig inloggningssökväg för andra noder i hierarkin.

>[!NOTE]
>
>Utvärderingen utförs endast för begäranden som är kopplade till resurser som finns i de konfigurerade sökvägarna som stöds. För andra förfrågningar utvärderas de alternativa sätten att bestämma inloggningssökvägen.

#### Bästa praxis {#best-practices-1}

Följande bästa metoder bör beaktas när autentiseringskrav definieras:

* Undvik inkapslade autentiseringskrav: det bör räcka att placera en enda auth-required-markör i början av ett träd och den ska ärvas till hela det underträd som definieras av målnoden. Ytterligare autentiseringskrav inom det trädet ska betraktas som redundanta och kan leda till prestandaproblem när autentiseringskraven utvärderas i Apache Sling. I och med separationen av auktoriserings- och autentiseringsrelaterade CUG-områden är det möjligt att begränsa läsåtkomst via CUG eller andra typer av principer samtidigt som autentisering för hela trädet upprätthålls.
* Modelldatabasinnehåll så att autentiseringskraven gäller för hela trädet utan att kapslade underträd behöver uteslutas från kravet igen.
* För att undvika att ange och sedan registrera redundanta inloggningssökvägar:

   * förlita sig på arv och undvika att definiera kapslade inloggningssökvägar,
   * anger inte den valfria inloggningssökvägen till ett värde som motsvarar standardvärdet eller ett ärvt värde,
   * programutvecklare bör identifiera vilka inloggningssökvägar som ska konfigureras i de globala konfigurationerna för inloggningssökväg (både standard och mappningar) som är kopplade till `LoginSelectorHandler`.

## Representation i databasen {#representation-in-the-repository}

### CUG-principrepresentation i databasen {#cug-policy-representation-in-the-repository}

I Oak-dokumentationen beskrivs hur de nya CUG-profilerna återspeglas i databasinnehållet. Mer information finns i [Jackrabbit Oak-dokumentationen om hur du hanterar åtkomst med CUG](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html#Representation_in_the_Repository).

### Autentiseringskrav i databasen {#authentication-requirement-in-the-repository}

Behovet av en separat autentiseringskrav återspeglas i databasinnehållet med en dedikerad mixin-nodtyp som placeras på målnoden. MixIn-typen definierar en valfri egenskap som anger en dedikerad inloggningssida för trädet som definieras av målnoden.

Sidan som är kopplad till inloggningssökvägen kan finnas inuti eller utanför det trädet. Det ingår inte i autentiseringskravet.

```java
[granite:AuthenticationRequired]
      mixin
      - granite:loginPath (STRING)
```

## Hantera CUG-principer och autentiseringskrav {#managing-cug-policies-and-authentication-requirement}

### Hantera CUG-principer {#managing-cug-policies}

Den nya typen av åtkomstkontrollprinciper som begränsar läsåtkomst för en CUG hanteras med JCR-åtkomstkontrollsgränssnittet och följer de mekanismer som beskrivs i [JCR 2.0-specifikationen](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/2.0/16_Access_Control_Management.html).

#### Ange en ny CUG-princip {#set-a-new-cug-policy}

Kod för att tillämpa en ny CUG-princip på en nod som inte hade en CUG-inställning tidigare. Observera att `getApplicablePolicies` bara returnerar nya profiler som inte har angetts tidigare. I slutet måste principen skrivas tillbaka och ändringarna måste sparas.

```java
String path = [...] // needs to be a supported, absolute path

Principal toAdd1 = [...]
Principal toAdd2 = [...]
Principal toRemove = [...]

AccessControlManager acMgr = session.getAccessControlManager();
PrincipalSetPolicy cugPolicy = null;

AccessControlPolicyIterator it = acMgr.getApplicablePolicies(path);
while (it.hasNext()) {
        AccessControlPolicy policy = it.nextAccessControlPolicy();
        if (policy instanceof PrincipalSetPolicy) {
           cugPolicy = (PrincipalSetPolicy) policy;
           break;
        }
}

if (cugPolicy == null) {
   log.debug("no applicable policy"); // path not supported or no applicable policy (for example,
                                                   // the policy was set before)
   return;
}

cugPolicy.addPrincipals(toAdd1, toAdd2);
cugPolicy.removePrincipals(toRemove));

acMgr.setPolicy(path, cugPolicy); // as of this step the policy can be edited/removed
session.save();
```

#### Redigera en befintlig CUG-princip {#edit-an-existing-cug-policy}

Följande steg krävs för att redigera en befintlig CUG-princip. Den ändrade principen måste skrivas tillbaka och ändringarna måste sparas med `javax.jcr.Session.save()`.

```java
String path = [...] // needs to be a supported, absolute path

Principal toAdd1 = [...]
Principal toAdd2 = [...]
Principal toRemove = [...]

AccessControlManager acMgr = session.getAccessControlManager();
PrincipalSetPolicy cugPolicy = null;

for (AccessControlPolicy policy : acMgr.getPolicies(path)) {
     if (policy instanceof PrincipalSetPolicy) {
        cugPolicy = (PrincipalSetPolicy) policy;
        break;
     }
}

if (cugPolicy == null) {
   log.debug("no policy to edit"); // path not supported or policy not set before
   return;
}

if (cugPolicy.addPrincipals(toAdd1, toAdd2) || cugPolicy.removePrincipals(toRemove)) {
   acMgr.setPolicy(path, cugPolicy);
   session.save();
} else {
     log.debug("cug policy not modified");
}
```

### Hämta effektiva CUG-principer {#retrieve-effective-cug-policies}

Hanteringen av JCR-åtkomstkontroll definierar en bästa metod för att hämta principer som börjar gälla vid en viss sökväg. Eftersom utvärderingen av CUG-principer är villkorsstyrd och beroende på vilken konfiguration som ska aktiveras, är det bekvämt att anropa `getEffectivePolicies` för att verifiera om en viss CUG-princip börjar gälla i en viss installation.

>[!NOTE]
>
>Skillnaden mellan `getEffectivePolicies` och det efterföljande kodexemplet som går upp i hierarkin för att hitta om en angiven sökväg redan ingår i en befintlig CUG.

```java
String path = [...] // needs to be a supported, absolute path

AccessControlManager acMgr = session.getAccessControlManager();
PrincipalSetPolicy cugPolicy = null;

// log an debug message of all CUG policies that take effect at the given path
// there could be zero, one or many (creating nested CUGs is possible)
for (AccessControlPolicy policy : acMgr.getEffectivePolicies(path) {
     if (policy instanceof PrincipalSetPolicy) {
        String policyPath = "-";
        if (policy instanceof JackrabbitAccessControlPolicy) {
           policyPath = ((JackrabbitAccessControlPolicy) policy).getPath();
        }
        log.debug("Found effective CUG for path '{}' at '{}', path, policyPath);
     }
}
```

#### Hämta ärvda CUG-principer {#retrieve-inherited-cug-policies}

Söker efter alla kapslade CUG:er som har definierats på en viss sökväg oavsett om de börjar gälla eller inte. Mer information finns i avsnittet [Konfigurationsalternativ](/help/sites-administering/closed-user-groups.md#configuration-options).

```java
String path = [...]

List<AccessControlPolicy> cugPolicies = new ArrayList<AccessControlPolicy>();
while (isSupportedPath(path)) {
     for (AccessControlPolicy policy : acMgr.getPolicies(path)) {
         if (policy instanceof PrincipalSetPolicy) {
            cugPolicies.add(policy);
         }
      }
      path = (PathUtils.denotesRoot(path)) ? null : PathUtils.getAncestorPath(path, 1);
}
```

#### Hantera CUG-principer efter huvudnamn {#managing-cug-policies-by-pincipal}

De tillägg som definieras av `JackrabbitAccessControlManager` och som gör att du kan redigera åtkomstkontrollprinciper efter huvudobjekt implementeras inte med CUG-åtkomstkontrollshantering, eftersom en CUG-princip per definition alltid påverkar alla principer: de som anges med `PrincipalSetPolicy` beviljas läsåtkomst medan alla andra huvudobjekt inte kan läsa innehåll i trädet som definieras av målnoden.

Motsvarande metoder returnerar alltid en tom principarray, men genererar inga undantag.

### Hantera autentiseringskrav {#managing-the-authentication-requirement}

Ett nytt autentiseringskrav skapas, ändras eller tas bort genom att målnodens effektiva nodtyp ändras. Egenskapen för den valfria inloggningssökvägen kan sedan skrivas med det vanliga JCR-API:t.

>[!NOTE]
>
>Ändringarna av en angiven målnod som nämns ovan återspeglas endast i Apache Sling Authenticator om `RequirementHandler` har konfigurerats och målet finns i de träd som definieras av de sökvägar som stöds (se avsnittet Konfigurationsalternativ).
>
>Mer information finns i [Tilldela mixnodtyper] (https://docs.adobe.com/docs/en/spec/jcr/2.0/10_Writing.html#10.10.3 Tilldela mixnodtyper) och [Lägga till noder och ange egenskaper] (https://docs.adobe.com/docs/en/spec/jcr/2.0/10_Writing.html#10.4 Lägga till noder och ange egenskaper)

#### Lägga till ett nytt verifieringskrav {#adding-a-new-auth-requirement}

Steg för att skapa ett autentiseringskrav beskrivs nedan. Kravet registreras endast med Apache Sling-autentiseraren om `RequirementHandler` har konfigurerats för trädet som innehåller målnoden.

```java
Node targetNode = [...]

targetNode.addMixin("granite:AuthenticationRequired");
session.save();
```

#### Lägg till ett nytt autentiseringskrav med inloggningssökväg {#add-a-new-auth-requirement-with-login-path}

Steg för att skapa ett autentiseringskrav, inklusive en inloggningssökväg. Kravet och undantaget för inloggningssökvägen registreras bara med Apache Sling Authenticator om `RequirementHandler` har konfigurerats för trädet som innehåller målnoden.

```java
Node targetNode = [...]
String loginPath = [...] // STRING property

Node targetNode = session.getNode(path);
targetNode.addMixin("granite:AuthenticationRequired");

targetNode.setProperty("granite:loginPath", loginPath);
session.save();
```

#### Ändra en befintlig inloggningssökväg {#modify-an-existing-login-path}

Stegen för att ändra en befintlig inloggningssökväg beskrivs nedan. Ändringen registreras bara med Apache Sling Authenticator om `RequirementHandler` har konfigurerats för trädet som innehåller målnoden. Tidigare värde för inloggningssökväg tas bort från registreringen. Autentiseringskravet som är associerat med målnoden påverkas inte av den här ändringen.

```java
Node targetNode = [...]
String newLoginPath = [...] // STRING property

if (targetNode.isNodeType("granite:AuthenticationRequired")) {
   targetNode.setProperty("granite:loginPath", newLoginPath);
   session.save();
} else {
     log.debug("cannot modify login path property; mixin type missing");
}
```

#### Ta bort en befintlig inloggningssökväg {#remove-an-existing-login-path}

Steg för att ta bort en befintlig inloggningssökväg. Inloggningssökvägsposten avregistreras endast från Apache Sling Authenticator om `RequirementHandler` har konfigurerats för trädet som innehåller målnoden. Autentiseringskravet som är associerat med målnoden påverkas inte.

```java
Node targetNode = [...]

if (targetNode.hasProperty("granite:loginPath") &&
   targetNode.isNodeType("granite:AuthenticationRequired")) {
   targetNode.setProperty("granite:loginPath", null);
   session.save();
} else {
     log.debug("cannot remove login path property; mixin type missing");
}
```

Du kan också använda metoden nedan för att uppnå samma syfte:

```java
String path = [...] // absolute path to target node

String propertyPath = PathUtils.concat(path, "granite:loginPath");
if (session.propertyExists(propertyPath)) {
    session.getProperty(propertyPath).remove();
    // or: session.removeItem(propertyPath);
    session.save();
}
```

#### Ta bort ett autenticeringskrav {#remove-an-auth-requirement}

Steg för att ta bort ett befintligt autentiseringskrav. Kravet avregistreras endast från Apache Sling Authenticator om `RequirementHandler` har konfigurerats för trädet som innehåller målnoden.

```java
Node targetNode = [...]
targetNode.removeMixin("granite:AuthenticationRequired");

session.save();
```

#### Hämta effektiva autentiseringskrav {#retrieve-effective-auth-requirements}

Det finns inget dedikerat offentligt API för att läsa alla effektiva autentiseringskrav som registrerats med Apache Sling Authenticator. Listan visas dock i systemkonsolen på `https://<serveraddress>:<serverport>/system/console/slingauth` under avsnittet **Konfiguration av autentiseringskrav**.

Följande bild visar autentiseringskraven för en AEM-publiceringsinstans med demoinnehåll. Den markerade sökvägen på communitysidan visar hur ett krav som lagts till av implementeringen som beskrivs i det här dokumentet återspeglas i Apache Sling Authenticator.

>[!NOTE]
>
>I det här exemplet har den valfria egenskapen för inloggningssökväg inte angetts. Därför har ingen andra post registrerats hos autentiseraren.

![chlimage_1-24](assets/chlimage_1-24.jpeg)

#### Hämta den effektiva inloggningssökvägen {#retrieve-the-effective-login-path}

Det finns för närvarande inget offentligt API för att hämta inloggningssökvägen som börjar gälla anonymt när en resurs som kräver autentisering används. Se avsnittet Utvärdering av inloggningssökväg för implementeringsinformation om hur inloggningssökvägen hämtas.

Observera dock att utöver inloggningssökvägarna som definieras med den här funktionen finns det andra sätt att ange omdirigering till inloggningen, vilket bör beaktas när innehållsmodellen och autentiseringskraven för en viss AEM-installation utformas.

#### Hämta det ärvda autentiseringsbehovet {#retrieve-the-inherited-auth-requirement}

Precis som med inloggningssökvägen finns det inget offentligt API för att hämta de ärvda autentiseringskrav som definierats i innehållet. Följande exempel visar hur du listar alla autentiseringskrav som har definierats med en viss hierarki oavsett om de börjar gälla eller inte. Mer information finns i [Konfigurationsalternativ](/help/sites-administering/closed-user-groups.md#configuration-options).

>[!NOTE]
>
>Vi rekommenderar att du förlitar dig på arvsmekanismen både för autentiseringskrav och inloggningssökväg och undviker att skapa kapslade autentiseringskrav.
>
>Mer information finns i [Utvärdering och arv av autentiseringskrav](#evaluation-and-inheritance-of-the-authentication-requirement), [Utvärdering av inloggningssökväg](#evaluation-of-login-path) och [Bästa praxis](#best-practices).

```java
String path = [...]
Node node = session.getNode(path);

Map<String, String> authRequirements = new ArrayList<String, String>();
while (isSupported(node)) {
     if (node.isNodeType("granite:AuthenticationRequired")) {
         String loginPath = (node.hasProperty("granite:loginPath") ?
                                     node.getProperty("granite:loginPath").getString() :
                                     "";
        authRequirements.put(node.getPath(), loginPath);
        node = node.getParent();
     }
}
```

### Kombinera CUG-principer och autentiseringskrav {#combining-cug-policies-and-the-authentication-requirement}

I följande tabell visas giltiga kombinationer av CUG-principer och autentiseringskrav i en AEM-instans som har båda modulerna aktiverade via konfigurationen.

| **Autentisering krävs** | **Inloggningssökväg** | **Begränsad läsåtkomst** | **Förväntad effekt** |
|---|---|---|---|
| Ja | Ja | Ja | En viss användare kan bara visa det underträd som är markerat med CUG-principen om en effektiv behörighetsutvärdering ger åtkomst. En oautentiserad användare omdirigeras till den angivna inloggningssidan. |
| Ja | Nej | Ja | En viss användare kan bara visa det underträd som är markerat med CUG-principen om en effektiv behörighetsutvärdering ger åtkomst. En oautentiserad användare omdirigeras till en ärvd standardinloggningssida. |
| Ja | Ja | Nej | En oautentiserad användare omdirigeras till den angivna inloggningssidan. Huruvida det är tillåtet att visa trädet som är markerat med auth-required beror på de effektiva behörigheterna för de enskilda objekten i det underträdet. Det finns ingen dedikerad CUG som begränsar läsåtkomst på plats. |
| Ja | Nej | Nej | En oautentiserad användare omdirigeras till en ärvd standardinloggningssida. Om det är tillåtet att visa trädet som är markerat med behörighetskraven beror på de faktiska behörigheterna för de enskilda objekten i det underträdet. Det finns ingen dedikerad CUG som begränsar läsåtkomst på plats. |
| Nej | Nej | Ja | En given autentiserad eller oautentiserad användare kan bara visa det underträd som är markerat med CUG-principen om en effektiv behörighetsutvärdering beviljar åtkomst. En oautentiserad användare behandlas likadant och omdirigeras inte till inloggning. |

>[!NOTE]
>
>Kombinationen av Verifieringskrav = Nej och Inloggningssökväg = Ja anges inte ovan eftersom Inloggningssökväg är ett valfritt attribut som är associerat med ett Autentiseringskrav. Om du anger en JCR-egenskap med det namnet utan att lägga till den definierande mixin-typen har det ingen effekt och ignoreras av motsvarande hanterare.

## OSGi-komponenter och konfiguration {#osgi-components-and-configuration}

I det här avsnittet finns en översikt över OSGi-komponenterna och de enskilda konfigurationsalternativen som introducerades i den nya CUG-implementeringen.

Se även dokumentationen för CUG-mappning för en omfattande mappning av konfigurationsalternativen mellan den gamla och den nya implementeringen.

### Auktorisering: Installation och konfiguration {#authorization-setup-and-configuration}

De nya, auktoriseringsrelaterade delarna finns i **Oak CUG Authorization**-paketet ( `org.apache.jackrabbit.oak-authorization-cug`), som är en del av AEM standardinstallation. Paketet definierar en separat auktoriseringsmodell som ska användas som ett ytterligare sätt att hantera läsåtkomst.

#### Konfigurera CUG-auktorisering {#setting-up-cug-authorization}

Konfigureringen av CUG-auktorisering beskrivs i detalj i [relevant Apache-dokumentation](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html#pluggability). Som standard har AEM CUG-auktorisering distribuerat i alla körningslägen. Stegvisa instruktioner kan också användas för att inaktivera CUG-auktorisering i de installationer som kräver en annan auktoriseringsinställning.

#### Konfigurera referensfiltret {#configuring-the-referrer-filter}

Du måste också konfigurera [Sling Referrer-filtret](/help/sites-administering/security-checklist.md#the-sling-referrer-filter) med alla värdnamn som kan användas för åtkomst till AEM, till exempel via CDN, belastningsutjämnare och andra.

Om referensfiltret inte är konfigurerat visas fel, som följande, när en användare försöker logga in på en CUG-plats:

```shell
31.01.2017 13:49:42.321 *INFO* [qtp1263731568-346] org.apache.sling.security.impl.ReferrerFilter Rejected referrer header for POST request to /libs/granite/core/content/login.html/j_security_check : https://hostname/libs/granite/core/content/login.html?resource=%2Fcontent%2Fgeometrixx%2Fen%2Ftest-site%2Ftest-page.html&$$login$$=%24%24login%24%24&j_reason=unknown&j_reason_code=unknown
```

#### Egenskaper hos OSGi-komponenter {#characteristics-of-osgi-components}

Följande två OSGi-komponenter har introducerats för att definiera autentiseringskrav och ange dedikerade inloggningssökvägar:

* `org.apache.jackrabbit.oak.spi.security.authorization.cug.impl.CugConfiguration`
* `org.apache.jackrabbit.oak.spi.security.authorization.cug.impl.CugExcludeImpl`

**org.apache.jackrabbit.oak.spi.security.permission.cug.impl.CugConfiguration**

<table>
 <tbody>
  <tr>
   <td>Etikett</td>
   <td>Konfiguration av Apache Jackrabbit Oak CUG</td>
  </tr>
  <tr>
   <td>Beskrivning</td>
   <td>Auktoriseringskonfiguration som är dedikerad för att konfigurera och utvärdera CUG-behörigheter.</td>
  </tr>
  <tr>
   <td>Konfigurationsegenskaper</td>
   <td>
    <ul>
     <li><code>cugSupportedPaths</code></li>
     <li><code>cugEnabled</code></li>
     <li><code>configurationRanking</code></li>
    </ul> <p>Se även <a href="#configuration-options">Konfigurationsalternativ</a> nedan.</p> </td>
  </tr>
  <tr>
   <td>Konfigurationsprincip</td>
   <td><code>ConfigurationPolicy.REQUIRE</code></td>
  </tr>
  <tr>
   <td>Referenser</td>
   <td><code>CugExclude (ReferenceCardinality.OPTIONAL_UNARY)</code></td>
  </tr>
 </tbody>
</table>

**org.apache.jackrabbit.oak.spi.security.permission.cug.impl.CugExcludeImpl**

<table>
 <tbody>
  <tr>
   <td>Etikett</td>
   <td>Apache Jackrabbit Oak CUG Exclude List</td>
  </tr>
  <tr>
   <td>Beskrivning</td>
   <td>Gör att du kan exkludera huvudobjekt med konfigurerade namn från CUG-utvärderingen.</td>
  </tr>
  <tr>
   <td>Konfigurationsegenskaper</td>
   <td>
    <ul>
     <li><code>principalNames</code></li>
    </ul> <p>Se även avsnittet Konfigurationsalternativ nedan.</p> </td>
  </tr>
  <tr>
   <td>Konfigurationsprincip</td>
   <td><code>ConfigurationPolicy.REQUIRE</code></td>
  </tr>
  <tr>
   <td>Referenser</td>
   <td>NA</td>
  </tr>
 </tbody>
</table>

#### Konfigurationsalternativ {#configuration-options}

Konfigurationsalternativen är:

* `cugSupportedPaths`: Ange de underträd som kan innehålla CUG:er. Inget standardvärde har angetts
* `cugEnabled`: konfigurationsalternativ för att aktivera behörighetsutvärdering för aktuella CUG-principer.

De tillgängliga konfigurationsalternativen som är kopplade till modulen CUG-auktorisering listas och beskrivs mer ingående i [dokumentationen för Apache Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html#configuration).

#### Utesluta huvudkonton från CUG-utvärderingen {#excluding-principals-from-cug-evaluation}

Undantaget av enskilda huvudkonton från CUG-utvärderingen har antagits från den tidigare tillämpningen. Den nya CUG-auktoriseringen täcker detta med ett dedikerat gränssnitt som heter CugExclude. Apache Jackrabbit Oak 1.4 levereras med en standardimplementering som utesluter en fast uppsättning huvudnamn och en utökad implementering som låter dig konfigurera enskilda huvudnamn. Den senare konfigureras i AEM publiceringsinstanser.

Standardinställningen eftersom AEM 6.3 förhindrar att följande objekt påverkas av CUG-principer:

* administratörsobjekt (admin-användare, administratörsgrupp)
* användarkonton för tjänst
* intern systemsäkerhetsobjekt

Mer information finns i tabellen i avsnittet [Standardkonfiguration sedan AEM 6.3](#default-configuration-since-aem) nedan.

Exkluderingen av gruppen &quot;administratörer&quot; kan ändras eller utökas i systemkonsolen i konfigurationsavsnittet i **Exkluderingslistan för Apache Jackrabbit Oak CUG**.

Det går också att tillhandahålla och distribuera en anpassad implementering av gränssnittet CugExclude för att justera uppsättningen med undantagna principer om det finns särskilda behov. Mer information och ett exempel på implementering finns i dokumentationen för [CUG-plug-inen](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html#pluggability).

### Autentisering: Installation och konfiguration {#authentication-setup-and-configuration}

De nya autentiseringsrelaterade delarna finns i paketet **Adobe Granite Authentication Handler** ( `com.adobe.granite.auth.authhandler` version 5.6.48). Det här paketet ingår i AEM standardinstallation.

För att ställa in autentiseringskravsersättning för det inaktuella CUG-stödet måste vissa OSGi-komponenter finnas och vara aktiva i en viss AEM-installation. Mer information finns i **Egenskaper för OSGi-komponenter** nedan.

>[!NOTE]
>
>På grund av det obligatoriska konfigurationsalternativet med RequirementHandler är de autentiseringsrelaterade delarna bara aktiva om funktionen har aktiverats genom att ange en uppsättning sökvägar som stöds. Med en standardinstallation av AEM är funktionen inaktiverad i redigeringsläge och aktiverad för /content i publiceringskörningsläge.

**Egenskaper för OSGi-komponenter**

Följande två OSGi-komponenter har introducerats för att definiera autentiseringskrav och ange dedikerade inloggningssökvägar:

* `com.adobe.granite.auth.requirement.impl.RequirementService`
* `com.adobe.granite.auth.requirement.impl.DefaultRequirementHandler`

**com.adobe.granite.auth.requirements.impl.RequirementService**

<table>
 <tbody>
  <tr>
   <td>Etikett</td>
   <td>-</td>
  </tr>
  <tr>
   <td>Beskrivning</td>
   <td>Dedikerad OSGi-tjänst för autentiseringskrav som registrerar en observatör för innehållsändringar som påverkar auth-krav (via blandningstypen <code>granite:AuthenticationRequirement</code>) och inloggningssökvägar med visas för <code>LoginSelectorHandler</code>. </td>
  </tr>
  <tr>
   <td>Konfigurationsegenskaper</td>
   <td>-</td>
  </tr>
  <tr>
   <td>Konfigurationsprincip</td>
   <td><code>ConfigurationPolicy.OPTIONAL</code></td>
  </tr>
  <tr>
   <td>Referenser</td>
   <td>
    <ul>
     <li><code>RequirementHandler (ReferenceCardinality.MANDATORY_UNARY)</code></li>
     <li><code>Executor (ReferenceCardinality.MANDATORY_UNARY)</code></li>
    </ul> </td>
  </tr>
 </tbody>
</table>

**com.adobe.granite.auth.requirements.impl.DefaultRequirementHandler**

| Etikett | Autentiseringskrav och hanterare för inloggningssökväg för Adobe Granite |
|---|---|
| Beskrivning | Implementeringen av `RequirementHandler` som uppdaterar autentiseringskraven för Apache Sling och motsvarande undantag för de associerade inloggningssökvägarna. |
| Konfigurationsegenskaper | `supportedPaths` |
| Konfigurationsprincip | `ConfigurationPolicy.REQUIRE` |
| Referenser | NA |

#### Konfigurationsalternativ {#configuration-options-1}

De autentiseringsrelaterade delarna av CUG-omskrivningen har endast ett konfigurationsalternativ som är kopplat till Adobe Granite-autentiseringskrav och inloggningssökvägshanteraren:

**&quot;Autentiseringskrav och hanterare för inloggningssökväg&quot;**

<table>
 <tbody>
  <tr>
   <td>Egenskap</td>
   <td>Typ</td>
   <td>Standardvärde</td>
   <td>Beskrivning</td>
  </tr>
  <tr>
   <td><p>Etikett = Sökvägar som stöds</p> <p>Namn = 'supportedPaths'</p> </td>
   <td>Ange&lt;String&gt;</td>
   <td>-</td>
   <td>Sökvägar som autentiseringskraven gäller för den här hanteraren. Lämna konfigurationsuppsättningen inaktiv om du vill lägga till <code>granite:AuthenticationRequirement</code>-blandningstypen i noder utan att använda dem (till exempel i författarinstanser). Funktionen är inaktiverad om den saknas. </td>
  </tr>
 </tbody>
</table>

## Standardkonfiguration sedan AEM 6.3 {#default-configuration-since-aem}

Nya installationer av AEM använder som standard de nya implementeringarna både för auktoriserings- och autentiseringsrelaterade delar av CUG-funktionen. Den gamla implementeringen &quot;Adobe Granite Closed User Group (CUG) Support&quot; har tagits bort och kommer som standard att inaktiveras i alla AEM-installationer. De nya implementeringarna aktiveras i stället enligt följande:

### Författarinstanser {#author-instances}

| **&quot;Konfiguration av Apache Jackrabbit Oak CUG&quot;** | **Förklaring** |
|---|---|
| Sökvägar som stöds `/content` | Åtkomststyrningshantering för CUG-principer är aktiverat. |
| CUG-utvärdering aktiverad FALSE | Utvärdering av behörighet är inaktiverad. CUG-profiler träder inte i kraft. |
| Rankning | 200 | Se Oak dokumentation. |

>[!NOTE]
>
>Ingen konfiguration finns för **Apache Jackrabbit Oak CUG Exclude List** och **Adobe Granite Authentication Requirement och Login Path Handler** i standardförfattarinstanserna.

### Publicera instanser {#publish-instances}

| **&quot;Konfiguration av Apache Jackrabbit Oak CUG&quot;** | **Förklaring** |
|---|---|
| Sökvägar som stöds `/content` | Åtkomststyrningshantering för CUG-principer aktiveras under de konfigurerade sökvägarna. |
| CUG Evaluation Enabled TRUE | Behörighetsutvärderingen aktiveras under de konfigurerade sökvägarna. CUG-profiler börjar gälla `Session.save()`. |
| Rankning | 200 | Se Oak dokumentation. |

| **&quot;Exkluderingslista för Apache Jackrabbit Oak CUG&quot;** | **Förklaring** |
|---|---|
| Administratörer för huvudnamn | Exkluderar administratörsobjekt från CUG-utvärderingen. |

| **&quot;Autentiseringskrav för Adobe Granite och hanterare för inloggningssökväg&quot;** | **Förklaring** |
|---|---|
| Sökvägar som stöds `/content` | Autentiseringskrav som definieras i databasen av blandningstypen `granite:AuthenticationRequired` börjar gälla under `/content` `Session.save()`. Sling Authenticator uppdateras. Att lägga till blandningstypen utanför de banor som stöds ignoreras. |

## Inaktiverar CUG-auktoriserings- och autentiseringskrav {#disabling-cug-authorization-and-authentication-requirement}

Den nya implementeringen kan inaktiveras helt om en viss installation inte använder användargränssnitten eller använder olika metoder för autentisering och auktorisering.

### Inaktivera CUG-auktorisering {#disable-cug-authorization}

Mer information om hur du tar bort CUG-auktoriseringsmodellen från den sammansatta auktoriseringsinställningen finns i dokumentationen för [CUG-plug-ability](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html#pluggability) .

### Inaktivera autentiseringskravet {#disable-the-authentication-requirement}

Om du vill inaktivera stöd för autentiseringskravet som tillhandahålls av modulen `granite.auth.authhandler` räcker det att ta bort konfigurationen som är kopplad till **Adobe Granite Authentication Requirement och Login Path Handler**.

>[!NOTE]
>
>Observera dock att om du tar bort konfigurationen avregistreras inte blandningstypen, som fortfarande gällde för noder utan att börja gälla.

## Interaktion med andra moduler {#interaction-with-other-modules}

### Apache Jackrabbit API {#apache-jackrabbit-api}

För att återspegla den nya typen av åtkomstkontrollprincip som används av CUG-auktoriseringsmodellen har API:t som definieras av Apache Jackrabbit utökats. Sedan version 2.11.0 av modulen `jackrabbit-api` definieras ett nytt gränssnitt som heter `org.apache.jackrabbit.api.security.authorization.PrincipalSetPolicy` och som sträcker sig från `javax.jcr.security.AccessControlPolicy`.

### Apache Jackrabbit FileVault {#apache-jackrabbit-filevault}

Importmekanismen för Apache Jackrabbit FileVault har justerats för att hantera åtkomstkontrollprinciper av typen `PrincipalSetPolicy`.

### Distribution av Apache Sling-innehåll {#apache-sling-content-distribution}

Se avsnittet [Apache Jackrabbit FileVault](/help/sites-administering/closed-user-groups.md#apache-jackrabbit-filevault) ovan.

### Adobe Granite Replication {#adobe-granite-replication}

Replikeringsmodulen har justerats något för att kunna replikera CUG-principer mellan olika AEM-instanser:

* `DurboImportConfiguration.isImportAcl()` tolkas ordagrant och påverkar bara åtkomstkontrollprinciper som implementerar `javax.jcr.security.AccessControlList`

* `DurboImportTransformer` respekterar endast den här konfigurationen för äkta åtkomstkontrollistor
* Andra principer, till exempel `org.apache.jackrabbit.api.security.authorization.PrincipalSetPolicy` instanser som skapas av CUG-auktoriseringsmodellen, replikeras alltid och konfigurationsalternativet `DurboImportConfiguration.isImportAcl`() ignoreras.

Det finns en begränsning för replikering av CUG-principer. Om en angiven CUG-princip tas bort utan att motsvarande mixin-nodtyp `rep:CugMixin,` tas bort återspeglas inte borttagningen vid replikeringen. Detta har åtgärdats genom att man alltid har tagit bort blandningen när man har tagit bort policyn. Begränsningen kan dock visa sig om blandningstypen läggs till manuellt.

### Autentiseringshanterare för Adobe Granite {#adobe-granite-authentication-handler}

Autentiseringshanteraren **Adobe Granite HTTP Header Authentication Handler** som levereras med paketet `com.adobe.granite.auth.authhandler` innehåller en referens till gränssnittet `CugSupport` som definieras av samma modul. Den används för att beräkna &quot;sfären&quot; under vissa omständigheter och återgår till sfären som konfigurerats med hanteraren.

Den här inställningen har justerats så att referensen till `CugSupport` är valfri för att säkerställa maximal bakåtkompatibilitet om en given konfiguration beslutar att återaktivera den borttagna implementeringen. Installationer som använder implementeringen kommer inte längre att få den sfär som extraheras från CUG-implementeringen, men de kommer alltid att visa sfären som definierats med **Adobe Granite HTTP Header Authentication Handler**.

>[!NOTE]
>
>Som standard är **Adobe Granite HTTP Header Authentication Handler** bara konfigurerad i publiceringskörningsläge med alternativet Inaktivera inloggningssida ( `auth.http.nologin`) aktiverat.

### AEM LiveCopy {#aem-livecopy}

När du konfigurerar CUG-grupper med LiveCopy visas en extra nod och en extra egenskap i databasen enligt följande:

* `/content/we-retail/us/en/blueprint/rep:cugPolicy`
* `/content/we-retail/us/en/LiveCopy@granite:loginPath`

Båda dessa element skapas under `cq:Page`. Med den aktuella designen hanterar MSM bara noder och egenskaper som finns under noden `cq:PageContent` (`jcr:content`).

Därför kan CUG-grupper inte rullas ut till Live-kopior från utkast. Planera runt detta när du konfigurerar Live Copy.

## Ändringar i den nya CUG-implementeringen {#changes-with-the-new-cug-implementation}

Syftet med detta avsnitt är att ge en översikt över de ändringar som gjorts i CUG-funktionen och en jämförelse mellan den gamla och den nya implementeringen. Här listas de ändringar som påverkar hur CUG-stöd konfigureras och hur och av vilka CUG-grupper hanteras i databasinnehållet.

### Skillnader i CUG-inställningar och konfiguration {#differences-in-cug-setup-and-configuration}

Den inaktuella OSGi-komponenten **Adobe Granite Closed User Group (CUG) Support** ( `com.day.cq.auth.impl.cug.CugSupportImpl`) har ersatts av nya komponenter för att separat kunna hantera autentiserings- och autentiseringsrelaterade delar av den tidigare CUG-funktionen.

## Skillnader i hantering av kundupplevelser i databasinnehållet {#differences-in-managing-cugs-in-the-repository-content}

I följande avsnitt beskrivs skillnaderna mellan den gamla och den nya implementeringen i implementerings- och säkerhetsperspektiven. Den nya implementeringen har samma funktionalitet, men det finns små förändringar som är viktiga att känna till när den nya CUG-filen används.

### Skillnader i fråga om tillstånd {#differences-with-regards-to-authorization}

De viktigaste skillnaderna från ett auktoriseringsperspektiv sammanfattas i listan nedan:

**Dedikerat åtkomstkontrollinnehåll för användargränssnitten**

I den gamla implementeringen användes standardauktoriseringsmodellen för att ändra åtkomstkontrollistans principer vid publicering och ersätta befintliga ACE:n med de inställningar som krävs av CUG:n. Detta utlöstes av att vanliga, kvarvarande JCR-egenskaper som tolkades vid publicering skrevs.

I och med den nya implementeringen påverkas inte åtkomstkontrollinställningen för standardauktoriseringsmodellen av någon CUG som skapas, ändras eller tas bort. I stället tillämpas en ny typ av princip med namnet `PrincipalSetPolicy` som extra åtkomstkontrollinnehåll på målnoden. Den här extra principen finns som underordnad till målnoden och skulle vara en jämställd principnod om sådan finns.

**Redigerar CUG-principer i åtkomstkontrollhantering**

Den här förändringen från kvarvarande JCR-egenskaper till en dedikerad åtkomstkontrollprincip påverkar behörigheten som behövs för att skapa eller ändra auktoriseringsdelen av CUG-funktionen. Eftersom detta betraktas som en ändring av åtkomstkontrollinnehåll, krävs det att `jcr:readAccessControl`- och `jcr:modifyAccessControl`-behörighet skrivs till databasen. Därför kan bara innehållsförfattare som har behörighet att ändra innehållet i åtkomstkontrollen på en sida konfigurera eller ändra det här innehållet. Detta står i kontrast till den gamla implementeringen där möjligheten att skriva vanliga JCR-egenskaper var tillräcklig, vilket resulterar i eskalering av behörigheter.

**Målnod definierad av princip**

Skapa CUG-principer på JCR-noden som definierar det underträd som ska ha begränsad läsåtkomst. Detta är sannolikt en AEM-sida om CUG förväntas påverka hela trädet.

Om du placerar CUG-principen endast vid jcr:content-noden under en viss sida, begränsas endast åtkomsten till innehållet s.str för en viss sida, men det kommer inte att gälla för några jämställda eller underordnade sidor. Detta kan vara ett giltigt användningssätt och det är möjligt att göra detta med en databasredigerare där du kan använda detaljerat åtkomstinnehåll. Den kontrasterar emellertid den tidigare implementeringen där placeringen av en cq:cugEnabled-egenskap på jcr:content-noden mappades om internt till sidnoden. Mappningen utförs inte längre.

**Behörighetsutvärdering med CUG-principer**

Genom att gå från det gamla CUG-stödet till en ytterligare behörighetsmodell ändras det sätt på vilket effektiva läsbehörigheter utvärderas. Enligt beskrivningen i [Jackrabbit-dokumentationen](https://jackrabbit.apache.org/oak/docs/security/authorization/composite.html) beviljas läsåtkomst endast för ett givet huvudobjekt som har behörighet att visa `CUGcontent` om behörighetsutvärderingen för alla modeller som har konfigurerats i Oak-databasen ger läsåtkomst.

För utvärderingen av de gällande behörigheterna beaktas alltså både `CUGPolicy`- och standardåtkomstkontrollposterna, och läsåtkomst för CUG-innehållet beviljas bara om det har beviljats av båda typerna av principer. I en publiceringsinstallation av AEM där läsåtkomst till det fullständiga `/content`-trädet beviljas för alla, är effekten av CUG-principerna densamma som för den gamla implementeringen.

**On-Demand Evaluation**

Med CUG-auktoriseringsmodellen kan du aktivera åtkomstkontroll och behörighetsutvärdering separat:

* åtkomststyrningshantering är aktiverad om modulen har en eller flera sökvägar som stöds där CUG kan skapas
* behörighetsutvärdering är bara aktiverat om alternativet **CUG Evaluation Enabled** också är markerat.

I den nya AEM standardinställningsutvärderingen av CUG-principer aktiveras den bara med körläget&quot;publish&quot;. Mer information finns i informationen om [standardkonfigurationen sedan AEM 6.3](#default-configuration-since-aem). Detta kan verifieras genom att man jämför effektiva profiler för en viss sökväg med de profiler som lagras i innehållet. Effektiva profiler visas bara om behörighetsutvärdering för användargränssnitten är aktiverat.

Som förklaras ovan lagras CUG-åtkomstkontrollprinciperna nu alltid i innehållet, men utvärdering av de gällande behörigheterna som följer av dessa principer kommer endast att tillämpas om **CUG Evaluation Enabled** aktiveras i systemkonsolen på Apache Jackrabbit Oak **CUG Configuration.** Som standard är det bara aktiverat med körläget &quot;publish&quot;.

### Skillnader i fråga om autentisering {#differences-with-regards-to-authentication}

Skillnaderna gällande autentisering beskrivs nedan.

#### Dedikerad blandningstyp för autentiseringskrav {#dedicated-mixin-type-for-authentication-requirement}

I den tidigare implementeringen utlöstes både autentiserings- och autentiseringsaspekterna för en CUG av en enda JCR-egenskap ( `cq:cugEnabled`). När det gäller autentisering resulterade detta i en uppdaterad lista över autentiseringskrav som lagrats med implementeringen av Apache Sling Authenticator. Med den nya implementeringen uppnås samma resultat genom att målnoden markeras med en dedikerad mixin-typ ( `granite:AuthenticationRequired`).

#### Egenskap för att exkludera inloggningssökväg {#property-for-excluding-login-path}

Med blandningstypen definieras en enda valfri egenskap med namnet `granite:loginPath`, som i princip motsvarar egenskapen `cq:cugLoginPage`. Till skillnad från den tidigare implementeringen respekteras bara egenskapen för inloggningssökväg om dess deklarerande nodtyp är den ovannämnda mixin. Om du lägger till en egenskap med det namnet utan att ange blandningstypen har det ingen effekt och varken ett nytt krav eller ett undantag för inloggningssökvägen rapporteras till autentiseraren.

#### Privilegium för autentiseringskrav {#privilege-for-authentication-requirement}

Om du lägger till eller tar bort en blandningstyp måste du ha `jcr:nodeTypeManagement`-behörighet. I den tidigare implementeringen används privilegiet `jcr:modifyProperties` för att redigera den kvarvarande egenskapen.

För `granite:loginPath` krävs samma behörighet för att lägga till, ändra eller ta bort egenskapen.

#### Målnod definierad av blandningstyp {#target-node-defined-by-mixin-type}

Skapa autentiseringskrav på JCR-noden som definierar det underträd som ska underställas obligatorisk inloggning. Detta är sannolikt en AEM Page om CUG förväntas påverka hela trädet och användargränssnittet för den nya implementeringen lägger därför till den obligatoriska mixin-typen på sidnoden.

Om du bara placerar CUG-principen vid jcr:content-noden som finns under en viss sida begränsas endast åtkomsten till innehållet. Däremot påverkas inte sidnoden och inte heller underordnade sidor.

Detta kan vara ett giltigt scenario och är möjligt med en databasredigerare där du kan placera mixinen på valfri nod. Beteendet står dock i kontrast till den tidigare implementeringen, där en cq:cugEnabled- eller cq:cugLoginPage-egenskap placerades internt på jcr:content-noden. Mappningen utförs inte längre.

#### Konfigurerade sökvägar som stöds {#configured-supported-paths}

Både blandningstypen `granite:AuthenticationRequired` och egenskapen granite:loginPath respekteras endast inom det omfång som definieras av konfigurationsalternativet **Supported Paths** som finns med **Adobe Granite Authentication Requirement och Login Path Handler**. Om inga sökvägar anges är funktionen för autentiseringskrav inaktiverad helt. I det här fallet börjar blandningstyp eller -egenskap gälla när den läggs till eller ställs in på en viss JCR-nod.

### Mappning av JCR-innehåll, OSGi-tjänster och konfigurationer {#mapping-of-jcr-content-osgi-services-and-configurations}

Dokumentet nedan innehåller en omfattande mappning av OSGi-tjänster, konfigurationer och databasinnehåll mellan den gamla och den nya implementeringen.

CUG-mappning sedan AEM 6.3

[Hämta fil](assets/cug-mapping.pdf)

## Uppgradera CUG {#upgrade-cug}

### Befintliga installationer som använder den inaktuella CUG-filen {#existing-installations-using-the-deprecated-cug}

Den gamla CUG-supportimplementeringen har tagits bort och kommer att tas bort i framtida versioner. Vi rekommenderar att du går över till de nya implementeringarna när du uppgraderar från versioner äldre än AEM 6.3.

För uppgraderad installation av AEM är det viktigt att se till att endast en CUG-implementering är aktiverad. Kombinationen av det nya och det gamla, föråldrade CUG-stödet testas inte och kommer troligen att orsaka oönskat beteende:

* kollisioner i Sling Authenticator gällande autentiseringskrav
* nekad läsåtkomst när ACL-inställningen som är kopplad till en gammal CUG kolliderar med en ny CUG-princip.

### Migrerar befintligt CUG-innehåll {#migrating-existing-cug-content}

Adobe tillhandahåller ett verktyg för migrering till den nya CUG-implementeringen. Så här använder du den:

1. Gå till `https://<serveraddress>:<serverport>/system/console/cug-migration` för att komma åt verktyget.
1. Ange den rotsökväg som du vill kontrollera användargränssnitten för och tryck på knappen **Utför torr**. Detta söker efter CUG som är berättigade till konvertering på den valda platsen.
1. När du har granskat resultaten trycker du på knappen **Utför migrering** för att migrera till den nya implementeringen.

>[!NOTE]
>
>Om du stöter på problem är det möjligt att konfigurera en specifik loggare på **DEBUG**-nivå på `com.day.cq.auth.impl.cug` för att få fram utdata från migreringsverktyget. Mer information om hur du gör detta finns i [Loggning](/help/sites-deploying/configure-logging.md).
