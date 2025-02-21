---
title: Förhindra CSRF-attacker
description: Lär dig hur du förhindrar att CSRF-attacker (Cross-site Request ForVerification) angriper webbplatser och skyddar användardata från att äventyras.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# Förhindra CSRF-attacker {#preventing-csrf-attacks}

## Hur CSRF-attacker fungerar {#how-csrf-attacks-work}

CSRF (Cross-site request formunoration) är en sårbarhet på en webbplats där en giltig användares webbläsare används för att skicka en skadlig begäran, möjligen via en iFrame. Eftersom webbläsaren skickar cookies på domänbasis kan användarens data bli komprometterade om användaren är inloggad i ett program.

Tänk dig till exempel ett scenario där du är inloggad på administrationskonsolen i en webbläsare. Du får ett e-postmeddelande som innehåller en länk. Du klickar på länken, vilket öppnar en ny flik i webbläsaren. Sidan som du öppnade innehåller en dold iFrame som gör en skadlig begäran till Forms Server med hjälp av cookien från din autentiserade AEM-formulärsession. Eftersom användarhantering tar emot en giltig cookie, godkänns begäran.

## CSRF-relaterade termer {#csrf-related-terms}

**Referens:** Adressen till källsidan som en begäran kommer från. En webbsida på site1.com innehåller till exempel en länk till site2.com. När du klickar på länken skickas en begäran till site2.com. Referenten för denna begäran är site1.com eftersom begäran görs från en sida vars källa är site1.com.

**Tillåtslista URI:er:** URI:er identifierar resurser på Forms-servern som begärs, till exempel /adminui eller /contentspace. Vissa resurser kan tillåta att en begäran skickas till programmet från externa platser. Dessa resurser betraktas som tillåtslista URI:er. Forms Server utför aldrig någon referenskontroll från tillåtslista URI:er.

**Null-referens:** När du öppnar ett nytt webbläsarfönster eller en ny flik, skriver en adress och trycker på Retur är referenten null. Begäran är helt ny och kommer inte från en överordnad webbsida. Därför finns det ingen hänvisare för begäran. Forms Server kan ta emot en null-referens från:

* begäranden som gjorts på SOAP- eller REST-slutpunkter från Acrobat
* alla skrivbordsklienter som gör en HTTP-begäran på en AEM-blankett SOAP- eller REST-slutpunkt
* när ett nytt webbläsarfönster öppnas och webbadressen till inloggningssidan för AEM-formulär anges

Tillåt en null-referens på SOAP- och REST-slutpunkter. Tillåt även en null-referens på alla URI-inloggningssidor som /adminui och /contentspace och deras motsvarande mappade resurser. Den mappade servern för /contentspace är till exempel /contentspace/faces/jsp/login.jsp, som bör vara ett null-referensundantag. Det här undantaget är bara nödvändigt om du aktiverar GET-filtrering för webbprogrammet. Programmen kan ange om null-referenser ska tillåtas. Se Skydda från attacker med smidning av förfrågningar mellan webbplatser i [Förbättring och säkerhet för AEM-formulär](https://help.adobe.com/en_US/livecycle/11.0/HardeningSecurity/index.html).

**Undantag för tillåten referens:** Undantag för tillåten referens är en underlista till listan över tillåtna referenter, från vilka begäranden blockeras. Tillåtna referensundantag gäller endast för webbprogram. Om en delmängd av Tillåtna referenter inte ska kunna anropa ett visst webbprogram kan du blocklist referenterna med hjälp av Tillåtna refererundantag. Tillåtna referensundantag anges i filen web.xml för ditt program. (Se även&quot;Skydda mot attacker med förfalskade korswebbplatser&quot; på sidan Hjälp och självstudiekurser om skydd och säkerhet för AEM-formulär.)

## Hur tillåtna referenter fungerar {#how-allowed-referers-work}

AEM Forms tillhandahåller referentfiltrering, vilket kan bidra till att förhindra CSRF-attacker. Så här fungerar referensfiltrering:

1. Forms Server kontrollerar HTTP-metoden som används för anrop:

   * Om det är POST utför Forms Server en huvudkontroll av referenten.
   * Om det är GET åsidosätter Forms Server referenskontrollen, såvida inte CSRF_CHECK_GETS är inställt på true. I så fall utförs referentrubrikkontrollen. CSRF_CHECK_GETS anges i filen web.xml för ditt program. (Se &quot;Skydda från attacker med förfalskade korswebbplatser för förfrågningar&quot; i [Förstärknings- och säkerhetsguiden](https://help.adobe.com/en_US/livecycle/11.0/HardeningSecurity/index.html).)

1. Forms Server kontrollerar om den begärda URI:n är tillåtslista:

   * Om URI:n är tillåtslista, skickar servern begäran.
   * Om den begärda URI:n inte är tillåtslista hämtar servern referenten för begäran.

1. Om det finns en referent i begäran kontrollerar servern om det är en tillåten referent. Om det är tillåtet söker servern efter ett referensundantag:

   * Om det är ett undantag blockeras begäran.
   * Om det inte är ett undantag skickas begäran.

1. Om det inte finns någon referent i begäran kontrollerar servern om en null-referent är tillåten.

   * Om en null-referens tillåts, skickas begäran.
   * Om en null-referens inte tillåts, kontrollerar servern om den begärda URI:n är ett undantag för null-referenten och hanterar begäran därefter.

## Konfigurera tillåtna referenser {#configure-allowed-referers}

När du kör Configuration Manager läggs standardvärden och IP-adressen eller Forms-servern till i listan Tillåten referent. Du kan redigera den här listan i administrationskonsolen.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Konfigurera tillåtna referens-URL:er. Listan Tillåten referent visas längst ned på sidan.
1. Så här lägger du till en tillåten referent:

   * Skriv ett värdnamn eller en IP-adress i rutan Tillåtna referenter. Om du vill lägga till mer än en tillåten referent åt gången skriver du varje värdnamn eller IP-adress på en ny rad.
   * I rutorna HTTP-port och HTTPS-portar anger du vilka portar som ska tillåtas för HTTP, HTTPS eller båda. Om du lämnar rutorna tomma används standardportarna (port 80 för HTTP och port 443 för HTTPS). Om du anger `0` (noll) i rutorna aktiveras alla portar på den servern. Du kan också ange ett specifikt portnummer för att bara aktivera den porten.
   * Klicka på Lägg till.

1. Om du vill ta bort en post från listan Tillåtna referenter markerar du objektet i listan och klickar på Ta bort.

   Om listan över tillåtna referenser är tom slutar CSRF-funktionen att fungera och systemet blir osäkert.

1. När du har ändrat listan över tillåtna referenter startar du om AEM Forms Server.
