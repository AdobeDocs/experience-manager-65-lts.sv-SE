---
title: Personalization
description: Lär dig mer om personalisering i Adobe Experience Manager för att ge användaren en skräddarsydd miljö med dynamiskt innehåll.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Administering,Personalization
role: Admin
source-git-commit: 2a406ca2870e241539819ae62c6a14904ee71211
workflow-type: tm+mt
source-wordcount: '1528'
ht-degree: 0%

---

# Personalization {#personalization}

## Vad är Personalization? {#what-is-personalization}

Det finns en allt större mängd innehåll idag, oavsett om det gäller webbplatser på internet, extranät eller intranät.

Personalization fokuserar på att förse användaren med en skräddarsydd miljö med dynamiskt innehåll som väljs ut utifrån deras specifika behov, oavsett om det baseras på fördefinierade profiler, användarval eller interaktivt användarbeteende.

Personaliseringen består av tre huvuddelar:

### Användare {#users}

* Har profiler, både enskilda och grupper. Dessa profiler innehåller egenskaper (t.ex. jobbbeskrivning, plats, intressen) som kan användas för att anpassa innehållet som de kan se.
* Agera. Dessa kan sedan analyseras och matchas mot beteenderegler för att skräddarsy det innehåll de ser.

### Innehåll {#content}

* Är det användaren vill se. Det är att föredra att innehåll är av intresse och används för att de ska kunna utföra sina uppgifter.
* Kan kategoriseras och därför göras tillgängliga för användare enligt fördefinierade regler.
* Måste vara dynamiskt.

Innehållet måste med andra ord på något sätt vara beroende av användaren. Om alla användare ser samma innehåll är personaliseringen överflödig.

### Regler {#rules}

* Definiera hur personalisering faktiskt sker - vilket innehåll användaren kan se och när.

Personalization kan vara antingen:

#### Explicit {#explicit}

* Anpassning: användaren gör val från ett urval av innehållskällor.

#### Implicit {#implicit}

* Regelbaserade: affärschefer definierar specifika regler för åtgärder baserade på specifika profiler och/eller beteenden.
* Enkel filtrering: val görs utifrån fördefinierade profiler på användar- och/eller gruppnivå.
* Kollaborativ-/rekommendationsfiltrering: användarbeteendet registreras enligt fördefinierade regler. Dessa regler bygger på beteenden som observeras med likasinnade individer. Den insamlade informationen används för att anpassa den information som visas för användaren, särskilt i form av rekommendationer.

## Hur och när kan Personalization användas? {#how-and-when-can-personalization-be-used}

Personalization kan användas i många fall, till exempel:

### Intranätsidor {#intranet-pages}

* Innehåll kan erbjudas baserat på en användares plats, avdelning och/eller roll, som redan definierats i ett internt nätverk.
* Beroende på vilket alternativ som är tillgängligt kan användaren göra fler val.

### Specifika, begränsade, målanvändargrupper - extranät {#extranets}

* Användare kräver en inloggning för auktorisering. Den länkas till en profil som innehåller information som krävs för personalisering, och kan innehålla information som plats, relation till produkten, användningshistorik, budgeteringsansvar och så vidare.
* Sådana instanser kan variera mellan webbplatser som:
* Företag som tillhandahåller webbplatser till en mycket specialiserad del av sin marknad, till exempel ett läkemedelsföretag som tillhandahåller en specialiserad webbplats för läkare.
* Företag som tillhandahåller webbplatser som gör det möjligt för sina kunder att visa aktuell konto- och faktureringsinformation, till exempel telefonleverantörer.

### Försäljnings- och distributionswebbplats {#sales-site}

* Försäljnings- och distributionswebbplatser, som Amazon, kan kombinera en användarprofil, användarens försäljningshistorik och användarens webbhistorik för att ge förslag på vad som kan intressera användaren härnäst.

### Sök på webbplatser {#search-site}

* Många av de stora sökmotorwebbplatserna har mycket kraftfulla analysverktyg som registrerar användarbeteende, söktermer de använder och de webbplatser de faktiskt besöker. Det används sedan för att anpassa innehållet - särskilt när det gäller att visa annonser.

### Styrkan hos Personalization och punkter att tänka på {#strengths-of-personalization-and-points-to-consider}

Följande är skäl till att personalisering bör användas:

* En användare kan uppleva en bekväm och fokuserad webbplats.
* Personalization kan användas för att automatiskt sprida åtkomst till den senaste versionen av innehåll.
* Funktioner för socialt samarbete är tillgängliga så att användarna kan kommunicera med varandra, eftersom de kan identifieras av sina profiler.
* En användare kan få det innehåll de behöver för att utföra en viss uppgift. I ett företags intranät kan detta vara ett värdefullt verktyg för att sprida information.
* Användarna kan få det innehåll de behöver/vill ha, vilket minskar tiden de behöver utföra sökåtgärder.
* Innehållsleverantören kan styra innehållet så att det kan ses av specifika användarkategorier.
* Regler kan definieras för att leverera innehåll baserat på kombinationer av både användaregenskaper och -beteende. Detta är en sofistikerad mekanism för att personalisera sin webbupplevelse.

Tänk på följande när du använder personalisering:

#### Prestanda {#performance}

* Den extra analysen och utvärderingen påverkar naturligtvis resultatet. De metoder som används är dock mycket avancerade och kan optimeras för att minimera påverkan.

#### Behörighet {#authorization}

* Personalization kräver en inloggningsfunktion eftersom webbplatsen måste kunna identifiera användaren.

#### Cachning {#caching}

* Cachelagring är en aspekt som användaren kommer att se när det gäller prestanda och precision - hur snabbt levererar webbplatsen personaliserat innehåll, och är det alltid aktuellt.
* Cachelagring är en viktig faktor när personalisering konfigureras och man måste se till att rätt implementering används.

>[!TIP]
>
>Personalization effekt på prestanda och relaterade cachelagringsämnen beskrivs närmare i dokumentet [Prestandaoptimering.](/help/sites-deploying/configuring-performance.md)

#### Regelernas exakthet {#accuracy}

* Personalization som realiseras genom att spåra användarens beteende, eller genom att ställa in regler baserat på användarens profil, måste vara korrekt och logiskt.
* Det finns inget mer frustrerande för användaren än att ha innehåll som tvingats på eller nekats till dem på grund av den felaktiga logiken i en regel.
* Därför måste regler vara väl genomtänkta - med användarens krav i förgrunden. Detta kan ta stora ansträngningar, och ska inte underskattas. Att definiera affärsreglerna uppväger ofta den tekniska ansträngningen vid personalisering.

#### När ska användas {#when-to-use}

* Precis som många andra funktioner på webben bör personalisering användas med försiktighet. Kommer användandet verkligen att gynna användaren? ska alltid vara det första övervägandet - eller om det önskade målet kan uppnås med mindre ansträngning med en annan metod. Personalization kan riskera att bli en funktion som användarna konfigurerar en gång (för att se hur det fungerar) och bara en gång - eftersom det inte ger dem några verkliga fördelar.
* Personalization är bara meningsfullt när innehållet är dynamiskt - beroende på användaren på något sätt. Om alla användare ser samma innehåll är personaliseringen överflödig.

#### Sekretess {#confidentiality}

* Många användare är oroade över dataskydd och -säkerhet. Särskilt när det gäller data som hämtats när de spåras när de surfar på webben.

## Personalization och Access {#personalization-and-access}

Personalization ska behandlas separat från åtkomstkontrollen, men de har en inbördes relation.

Personalization skapar inte någon form av åtkomstkontroll. Det är helt enkelt ett sätt att styra vad användaren ser. Det hindrar inte användaren från att få åtkomst till annat innehåll, och precis som med allt innehåll måste användaren ha rätt åtkomstkontroll tilldelad.

Åtkomstkontroll kan dock användas för att skapa en form av personalisering. Om du tillåter eller nekar en användare åtkomst till innehåll påverkar detta oundvikligen valet av innehåll som han/hon har tillgängligt, vilket innebär att hans/hennes webbupplevelse anpassas.

## Komponenter tillgängliga för Personalization {#components-available-for-personalization}

Olika komponenter ingår i AEM för personalisering. Vissa tillåter användare att logga in och redigera sina profiler, andra (som Mina gadgets) tillåter användarna att konfigurera en viss sida:

| Titel i Sidekick | Syfte |
|---|---|
| Kontrollerat lösenordsfält | Begär lösenord och bekräftelse av lösenord. |
| Kombinerad inloggningsregistrering | Låter användaren antingen logga in på ett befintligt konto eller registrera sig för ett nytt konto. |
| Forms-adressfält | Ett komplext fält som tillåter inmatning av en internationell adress. |
| Forms Begin | Startar en formulärdefinition |
| Forms Captcha | Ett fält som består av ett alfanumeriskt ord som uppdateras automatiskt. Captcha-komponenten skyddar webbplatser mot stötar. |
| Forms Checkbox Group | Flera objekt ordnade i en lista och föregås av kryssrutor. Användarna kan markera flera kryssrutor. |
| Forms-listruta | Flera objekt är ordnade i en nedrullningsbar lista. Växeln Flervalbar anger om flera element kan väljas i listan. |
| Forms End | Avslutar formulärdefinitionen. |
| Forms filöverföring | An upload element that allows the user to upload a file to the server. |
| Forms Dolt fält | Det här fältet visas inte för användaren. Den kan användas för att överföra ett värde till klienten och tillbaka till servern. Fältet ska inte ha några begränsningar. |
| Forms Image Button | En extra Skicka-knapp för formuläret som återges som en bild. |
| Forms-lösenordsfält | Samma som textfält, men bara en rad tillåts och textinmatningen från användaren visas inte i fältet. |
| Forms Radio Group | Flera objekt ordnade i en lista föregås av en alternativknapp. Användare får bara välja en alternativknapp. |
| Forms Submit Button | En extra Skicka-knapp för formuläret där titeln visas som text på knappen. |
| Forms textfält | Textfält där användarna kan ange information. |
| Mina gadgets | Gör att du kan ta med ett urval av tillgängliga gadgets. |
| Profil Avatar Photo | Tillåter indata från ett Avatar-foto. |
| Detaljerat profilnamn | Ange namndetaljer, inklusive element som titel, mellannamn och suffix om det behövs. |
| Profilvisningsnamn | Namn som ska visas. |
| Profil-e-post | Ange en e-postadress. |
| Profilkön | Tillåter indata för kön. |
| Profilens primära telefonnummer | Tillåter inmatning av ett telefonnummer. |
| Primär URL för profil | Tillåter indata för en URL. |
| Profil, allmän text, egenskap | Profilegenskaper. |
| Logga in | Gör att du kan ange ett användarnamn och lösenord när du loggar in. |
| Logga ut | Anger användaren som är inloggad och ger dig en länk för att logga ut. |
| Tag Cloud | Ett taggmoln som visar ett grafiskt presenterat urval av taggar på webbplatsen |
| Teaser | Ett innehåll (vanligtvis en bild) som visas på en huvudsida för att&quot;lura&quot; användarna att komma åt det underliggande innehållet. |

