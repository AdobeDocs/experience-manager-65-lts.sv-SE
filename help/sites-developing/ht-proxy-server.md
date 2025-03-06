---
title: Så här använder du proxyserververktyget
description: Proxyservern fungerar som en mellanliggande server som vidarebefordrar begäranden mellan en klient och en server
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
exl-id: e33640ba-6039-4057-8942-b4faa9b2e250
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 0%

---

# Så här använder du proxyserververktyget{#how-to-use-the-proxy-server-tool}

Proxyservern fungerar som en mellanliggande server som vidarebefordrar begäranden mellan en klient och en server. Proxyservern spårar alla klient-server-interaktioner och genererar en logg över hela TCP-kommunikationen. På så sätt kan du övervaka exakt vad som händer, utan att behöva komma åt huvudservern.

Proxyservern finns i din AEM-installation här:

`crx-quickstart/opt/helpers/proxy-2.1.jar`

Du kan använda proxyservern för att övervaka all klientserverinteraktion, oavsett vilket kommunikationsprotokoll som ligger till grund för det. Du kan till exempel övervaka följande protokoll:

* HTTP för webbsidor
* HTTPS för säkra webbsidor
* SMTP för e-postmeddelanden
* LDAP för användarhantering

Du kan till exempel placera proxyservern mellan två program som kommunicerar via ett TCP/IP-nätverk, till exempel en webbläsare och AEM. På så sätt kan du övervaka exakt vad som händer när du begär en CQ-sida.

## Starta proxyserververktyget {#starting-the-proxy-server-tool}

Starta servern på kommandoraden:

`java -jar proxy-2.1.jar <host> <remoteport> <localport> [options]`

**Parametrar**

`<host>`

Det här är värdadressen för den CRX-instans som du vill ansluta till. Om instansen finns på den lokala datorn är det `localhost`.

`<remoteport>`

Detta är värdporten för CRX-målinstansen. Standardvärdet för en nyinstallerad AEM-installation är till exempel **`4502`** och standardvärdet för en nyinstallerad AEM-författarinstans är `4502`.

`<localport>`

Det här är den port på den lokala datorn som du vill ansluta för att komma åt CRX-instansen via proxyn.

**Alternativ**

`-q` (tyst läge)

Skriver inte utdata till konsolfönstret. Använd det här alternativet om du inte vill göra anslutningen långsammare eller om du loggar utdata till en fil (se alternativet -logfile).

`-b` (binärt läge)

Om du letar efter specifika bytekombinationer i trafiken ska du aktivera binärt läge. Utdata kommer då att innehålla hexadecimala utdata och teckenutdata.

`-t` (tidsstämpelloggposter)

Lägger till en tidsstämpel i varje loggutdata. Tidsstämpeln är i sekunder, så den kanske inte är lämplig för att kontrollera enstaka begäranden. Använd den för att hitta händelser som inträffar vid en viss tidpunkt om du använder proxyservern under en längre tidsperiod.

`-logfile <filename>`(skriv till loggfil)

Skriver klient-server-konversationen till en loggfil. Den här parametern fungerar även i tyst läge.

**`-i <numIndentions>`**(lägg till indrag)

Varje aktiv anslutning är indragen för bättre läsbarhet. Standardvärdet är 16 nivåer. Den här funktionen infördes med `proxy.jar version 1.16`.

### Loggformat {#log-format}

Loggposterna som skapas med proxy-2.1.jar har följande format:

`[timestamp (optional)] [Client|Server]-[ConnectionNumber]-[BytePosition] ->[Character Stream]`

En begäran om en webbsida kan till exempel se ut så här:

`C-0-#000000 -> [GET /author/prox.html?CFC_cK=1102938422341 HTTP/1.1 ]`

* C anger att posten kommer från klienten (det är en begäran om en webbsida)
* 0 är anslutningens nummer (anslutningsräknaren startar vid 0)
* #00000 förskjutningen i byteflödet. Detta är den första posten, så förskjutningen är 0.
* `[GET <?>]` är innehållet i begäran, i exemplet en av HTTP-rubrikerna (url).

När en anslutning stängs loggas följande information:

```
C-6-Finished: 758 bytes (1.0 kb/s)
S-6-Finished: 665 bytes (1.0 kb/s)
```

Detta visar antalet byte som passerat mellan klienten ( `C`) och servern ( `S`) på den sjätte anslutningen och med medelhastigheten.

**Ett exempel på loggutdata**

Som ett exempel kan du titta på en sida som skapar följande kod när den efterfrågas:

### Exempel {#example}

Ta som exempel ett enkelt HTML-dokument i databasen på

`/content/test.html`

Vid sidan av en bildfil på

`/content/test.jpg`

Innehållet i `test.html` är:

```xml
<html>
<head>
    <title>Test</title>
</head>
<body>
    Test<br>
    <img src="test.jpg">
</body>
</html>
```

Om AEM-instansen körs på `localhost:4502` startas proxyn så här:

`java -jar proxy.jar localhost 4502 4444 -logfile test.log`

CQ/CRX-instansen kan nu nås via proxyn på `localhost:4444` och all kommunikation via den här porten loggas på `test.log`.

Om du nu tittar på utdata från proxyn ser du interaktionen mellan webbläsaren och AEM-instansen.

Vid start skickar proxyn följande:

```xml
starting proxy for localhost:4502 on port 4444
using logfile: <some-dir>/crx-quickstart/opt/helpers/test.log
```

Öppna nu en webbläsare och gå till testsidan:

`http://localhost:4444/content/test.html`

Du ser att webbläsaren gör en `GET`-begäran för sidan:

```shell
C-0-#000000 -> [GET /content/test.html HTTP/1.1 ]
C-0-#000033 -> [Host: localhost:4444 ]
C-0-#000055 -> [Connection: keep-alive ]
C-0-#000079 -> [User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.57 Safari/536.11 ]
C-0-#000212 -> [Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 ]
C-0-#000285 -> [Accept-Encoding: gzip,deflate,sdch ]
C-0-#000321 -> [Accept-Language: en-US,en;q=0.8 ]
C-0-#000354 -> [Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3 ]
C-0-#000402 -> [Cookie: login-token=179ba6bd-e0a7-4909-a965-e11c7f2bc2fc%3a618bd8a8-fbaf-43c5-827d-c84c62248c5e_22ee860cc9036fee%3acrx.default%3b21148fb0-eb6c]
C-0-#000543 -> [-43c9-a2b9-c8d40618d8ae%3ad87a3d1a-5e9a-4d5a-bab1-0ee60ad6d8df_d0e4ddce0fcd84b6%3acrx.default%3b5cb95227-ea51-47bf-850b-68ad1dfd7297%3af3bbb6]
C-0-#000684 -> [59-7913-4285-8857-832c087bafd5_c484727d3b3665ad%3acrx.default; ys-cq-siteadmin-tree=o%3Awidth%3Dn%253A240%5EselectedPath%3Ds%253A/content ]
C-0-#000824 -> [ ]
```

AEM-instansen svarar med innehållet i filen `test.html`:

```shell
S-0-#000000 -> [HTTP/1.1 200 OK ]
S-0-#000017 -> [Connection: Keep-Alive ]
S-0-#000041 -> [Server: Day-Servlet-Engine/4.1.24  ]
S-0-#000077 -> [Content-Type: text/html;charset=utf-8 ]
S-0-#000116 -> [Content-Length: 104 ]
S-0-#000137 -> [Date: Mon, 16 Jul 2012 11:23:38 GMT ]
S-0-#000174 -> [Last-Modified: Mon, 16 Jul 2012 11:19:27 GMT ]
S-0-#000220 -> [ ]
S-0-#000222 -> [<html>]
S-0-#000229 -> [<head>]
S-0-#000236 -> [    <title>Test</title>]
S-0-#000260 -> [</head> ]
S-0-#000269 -> [<body>]
S-0-#000276 -> [ Test<br>]
S-0-#000286 -> [    <img src="test.jpg">]
S-0-#000311 -> [</body>]
S-0-#000319 -> [</html>]
```

### Proxyserverns användning {#uses-of-the-proxy-server}

Följande scenarier visar några av de syften som proxyservern kan användas för:

**Sök efter cookies och deras värden**

I följande exempel på loggpost visas alla cookies och deras värden som klienten skickade på den sjätte anslutningen som öppnats sedan proxyn startades:

`C-6-#000635 -> [Cookie: cq3session=7e39bc51-ac72-3f48-88a9-ed80dbac0693; Show=ShowMode; JSESSIONID=68d78874-cabf-9444-84a4-538d43f5064d ]`

**Söker efter rubriker och deras värden**

I följande exempel på loggpost visas att servern kan skapa en keep-alive-anslutning och att innehållets längdhuvud är korrekt inställt:

```
S-7-#000017 -> [Connection: Keep-Alive ]
 ...
 S-7-#000107 -> [Content-Length: 124 ]
```

**Kontrollerar om Keep-Alive fungerar**

Keep-alive är en funktion i HTTP som gör att en klient kan återanvända TCP-anslutningen till servern för att göra flera begäranden (för sidkod, bilder, formatmallar och så vidare). Utan att hålla kontakten vid liv måste klienten upprätta en ny anslutning för varje begäran.

Så här kontrollerar du om keep-alive-funktionen fungerar:

* Starta proxyservern.
* Begär en sida.
* Om keep-alive fungerar bör anslutningsräknaren aldrig gå över 5 till 10 anslutningar.
* Om keep-alive inte fungerar ökar anslutningsräknaren snabbt.

**Söker efter förlorade begäranden**

Om du förlorar begäranden i en komplex serverinställning, till exempel med en brandvägg och en Dispatcher, kan du använda proxyservern för att ta reda på var begäran förlorades. Om det finns en brandvägg:

* Starta en proxy före en brandvägg
* Starta en annan proxy efter en brandvägg
* Använd dessa för att se hur långt förfrågningarna kommer.

**Förfrågningar som hänger**

Om du ibland får väntande förfrågningar:

* Starta proxyn.
* Vänta eller skriv åtkomstloggen i en fil där varje post har en tidsstämpel.
* När begäran börjar gälla kan du se hur många anslutningar som var öppna och vilken begäran som orsakar problem.
