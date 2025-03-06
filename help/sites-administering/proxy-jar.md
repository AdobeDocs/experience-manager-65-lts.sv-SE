---
title: Proxy Server Tool (proxy.jar)
description: Läs mer om Proxy Server Tool (proxy.jar) i Adobe Experience Manager.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: d918ddf2-aa70-4742-97d5-24a2c51f578a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 0%

---

# Proxy Server Tool (proxy.jar){#proxy-server-tool-proxy-jar}

Proxyservern fungerar som en mellanliggande server som vidarebefordrar begäranden mellan en klient och en server. Proxyservern spårar alla klient-server-interaktioner och genererar en logg över hela TCP-kommunikationen. På så sätt kan du övervaka exakt vad som händer, utan att behöva komma åt huvudservern.

Proxyservern finns i lämplig installationsmapp:

* &lt;cq_install_path>/opt/helpers/proxy.jar
* &lt;crx_install_path>/opt/helpers/proxy.jar

Du kan använda proxyservern för att övervaka all klientserverinteraktion, oavsett vilket kommunikationsprotokoll som ligger till grund för det. Du kan till exempel övervaka följande protokoll:

* HTTP för webbsidor
* HTTPS för säkra webbsidor
* SMTP för e-postmeddelanden
* LDAP för användarhantering

Du kan till exempel placera proxyservern mellan två program som kommunicerar via ett TCP/IP-nätverk, till exempel en webbläsare och AEM. På så sätt kan du övervaka exakt vad som händer när du begär en AEM-sida.

## Starta proxyserververktyget {#starting-the-proxy-server-tool}

Verktyget finns i mappen /opt/help i AEM-installationen. Börja med att skriva:

```xml
java -jar proxy.jar <host> <remoteport> <localport> [options]
```

### Alternativ {#options}

* **q (tyst läge)** Skriver inte begäranden till konsolfönstret. Använd det här alternativet om du inte vill göra anslutningen långsammare eller om du loggar utdata till en fil (se alternativet -logfile).
* **b (binärt läge)** Aktivera binärt läge om du söker efter specifika bytekombinationer i trafiken. Utdata innehåller hexadecimala utdata och teckenutdata.
* **t (tidsstämpelloggposter)** Lägger till en tidsstämpel i varje loggutdata. Tidsstämpeln är i sekunder, så den kanske inte är lämplig för att kontrollera enstaka begäranden. Använd den för att hitta händelser som inträffar vid en viss tidpunkt om du använder proxyservern under en längre tidsperiod.
* **loggfil &lt;filnamn> (skriv till loggfil)** Skriver klient-server-konversationen till en loggfil. Den här parametern fungerar även i tyst läge.
* **i &lt;numIndentions> (add indention)** Varje aktiv anslutning är indragen för bättre läsbarhet. Standardvärdet är 16 nivåer. (Nytt i proxy.jar version 1.16).

## Användning av proxyserververktyget {#uses-of-the-proxy-server-tool}

Följande scenarier visar några av de syften som proxyserververktyget kan användas för:

**Sök efter cookies och deras värden**

I följande exempel på loggpost visas alla cookies och deras värden som klienten skickade på den sjätte anslutningen som öppnats sedan proxystart:

```xml
C-6-#000635 -> [Cookie: cq3session=7e39bc51-ac72-3f48-88a9-ed80dbac0693; Show=ShowMode; JSESSIONID=68d78874-cabf-9444-84a4-538d43f5064d ]
```

**Söker efter rubriker och deras värden** I följande loggpostexempel visas att servern kan skapa en keep-alive-anslutning och att innehållets längdhuvud har angetts korrekt:

```xml
S-7-#000017 -> [Connection: Keep-Alive ]
...
S-7-#000107 -> [Content-Length: 124 ]
```

**Kontrollerar om Keep-Alive fungerar**

**Keep-Alive** innebär att en klient återanvänder anslutningen till servern för att överföra flera filer (sidkod, bilder, formatmallar osv.). Utan att hålla kontakten vid liv måste klienten upprätta en ny anslutning för varje begäran.

Så här kontrollerar du om keep-alive-funktionen fungerar:

1. Starta proxyservern.
1. Begär en sida.

* Om keep-alive fungerar bör anslutningsräknaren aldrig gå över 5 till 10 anslutningar.
* Om keep-alive inte fungerar ökar anslutningsräknaren snabbt.

**Söker efter förlorade begäranden**

Om du förlorar begäranden i en komplex serverinställning, till exempel med en brandvägg och en Dispatcher, kan du använda proxyservern för att ta reda på var begäran förlorades. Om det finns en brandvägg:

1. Starta en proxy före en brandvägg
1. Starta en annan proxy efter en brandvägg
1. Använd dessa för att se hur långt förfrågningarna kommer.

**Förfrågningar som hänger**

Om du ibland får väntande förfrågningar:

1. Starta proxy.jar.
1. Vänta eller skriv åtkomstloggen i en fil - där varje post har en tidsstämpel.
1. När begäran börjar gälla kan du se hur många anslutningar som var öppna och vilken begäran som orsakar problem.

## Formatet på loggmeddelanden {#the-format-of-log-messages}

Loggposterna som skapas av proxy.jar har följande format:

```xml
[timestamp (optional)] [<b>C</b>lient|<b>S</b>erver]-[ConnectionNumber]-[BytePosition] ->[Character Stream]
```

En begäran om en webbsida kan till exempel se ut så här:

```xml
C-0-#000000 -> [GET /author/prox.html?CFC_cK=1102938422341 HTTP/1.1 ]
```

* C anger att posten kommer från klienten (det är en begäran om en webbsida)
* 0 är anslutningens nummer (anslutningsräknaren startar vid 0)
* #00000 förskjutningen i byteflödet. Detta är den första posten, så förskjutningen är 0.
* [GET &lt;?>] är innehållet i begäran, i exemplet en av HTTP-rubrikerna (url).

När en anslutning stängs loggas följande information:

```xml
C-6-Finished: 758 bytes (1.0 kb/s)
S-6-Finished: 665 bytes (1.0 kb/s)
```

Detta visar antalet byte som passerat mellan klient och server på den sjätte anslutningen och med medelhastigheten.

## Ett exempel på loggutdata {#an-example-of-log-output}

Granska en enkel mall som skapar följande kod när den efterfrågas:

```xml
<html>
  <head>
    <title>Welcome</title>
  </head>
  <body>
    Welcome to Playground<br>
    <img src="/logo.gif">
  </body>
</html>
```

Om AEM körs på localhost:4303 startar du proxyservern enligt följande:

```xml
java -jar proxy.jar localhost 4303 4444 -logfile test.log
```

Du kan komma åt servern (`localhost:4303`) utan proxyservern, men om du kommer åt den via `localhost:4444` loggar proxyservern kommunikationen. Öppna en webbläsare och öppna en sida som skapats med mallen ovan. Titta sedan på loggfilen.

>[!NOTE]
>
>Till och med proxy.jar version 1.14 synkroniseras inte loggposterna för en anslutning, vilket innebär att loggposterna för en klient-/serveranslutning inte behövs i rätt ordningsföljd. De nyare versionerna (>=1.14) av proxyservern har inte det här problemet.

Vid start skrivs följande information till loggen:

```xml
starting proxy for localhost:4303 on port 4444
using logfile: C:\CQUnify355default\opt\helpers\test.log
```

Följande rubrikfält visas i början av den första anslutningen (0) som begär HTML-huvudsidan:

```xml
C-0-#000000 -> [GET /author/prox.html?CFC_cK=1102936796533 HTTP/1.1 ]
C-0-#000053 -> [Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/vnd.ms-powerpoint, application/vnd.ms-excel, application/msword, appl]
C-0-#000194 -> [ication/x-shockwave-flash, */* ]
C-0-#000227 -> [Accept-Language: de-ch ]
C-0-#000251 -> [Accept-Encoding: gzip, deflate ]
C-0-#000283 -> [User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0) ]
C-0-#000347 -> [Host: localhost:4444 ]
```

Klienten begär en anslutning som håller sig vid liv så att servern kan skicka flera filer över samma anslutning:

```xml
C-0-#000369 -> [Connection: Keep-Alive ]
```

Proxyservern är ett bra verktyg för att kontrollera om cookies är rätt inställda eller inte. Här ser du följande:

* cq3session cookie genererad av AEM
* den cookie för växling av visningsläge som genererats av CFC:n
* en cookie med namnet JSESSIONID. Den skapas automatiskt av JSP om den inte uttryckligen inaktiveras med &lt;%@ page session=&quot;false&quot; %>:

```xml
C-0-#000393 -> [Cookie: Show=ShowMode; cq3session=3bce15cf-1575-1b4e-8ea6-0d1a0c64738e; JSESSIONID=4161a56b-f193-d748-88a5-e09c5ff7ef2a ]
C-0-#000514 -> [ ]
S-0-#000000 -> [HTTP/1.0 200 OK ]
```

Servern stänger anslutningen 0 efter begäran. Det går inte att hålla sig vid liv eftersom begäran har ett frågetecken. Det innebär att servern inte kan returnera en cachelagrad version och därför inte kan fastställa innehållslängden just nu, vilket krävs för en keep-alive-anslutning.

```xml
S-0-#000017 -> [Connection: Close ]
S-0-#000036 -> [Server: Communique Servlet Engine/3.5.5 ]
S-0-#000077 -> [Content-Type: text/html;charset=iso-8859-1 ]
S-0-#000121 -> [Date: Tue, 14 Dec 2004 09:46:44 GMT ]
S-0-#000158 -> [Set-Cookie: JSESSIONID=4161a56b-f193-d8-88a5-e09c5ff7ef2a;Path=/author ]
S-0-#000232 -> [ ]
```

Här börjar servern skicka HTML-koden på anslutning 0:

```xml
S-0-#000234 -> [<html> ]
S-0-#000242 -> [.<head> ]
S-0-#000251 -> [..<title>Welcome</title> ]
S-0-#000277 -> [.</head> ]
S-0-#000287 -> [.<body> ]
S-0-#000296 -> [..Welcome to Playground<br> ]
S-0-#000325 -> [..<img src="/author/logo.gif"> ]
S-0-#000357 -> [.</body> ]
S-0-#000367 -> [</html>]
```

Anslutningen 0 stängs omedelbart efter att HTML-filen har använts:

```xml
C-0-Finished: 516 bytes (0.0 kb/s)
S-0-Finished: 374 bytes (0.0 kb/s)
```

Nu börjar utdata för anslutning 1, som laddar ned bilden som finns i HTML-koden:

```xml
C-1-#000000 -> [GET /author/logo.gif HTTP/1.1 ]
C-1-#000031 -> [Accept: */* ]
C-1-#000044 -> [Referer: http://localhost:4444/author/prox.html?CFC_cK=1102936796533 ]
C-1-#000114 -> [Accept-Language: de-ch ]
C-1-#000138 -> [Accept-Encoding: gzip, deflate ]
C-1-#000170 -> [User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0) ]
C-1-#000234 -> [Host: localhost:4444 ]
```

Återigen begär klienten en keep-alive-anslutning:

```xml
C-1-#000256 -> [Connection: Keep-Alive ]
C-1-#000280 -> [Cookie: Show=ShowMode; cq3session=3bce15cf-1575-1b4e-8ea6-0d1a0c64738e; JSESSIONID=4161a56b-f193-d748-88a5-e09c5ff7ef2a ]
C-1-#000401 -> [ ]
S-1-#000000 -> [HTTP/1.0 200 OK ]
```

För anslutning 1 kan servern ge dig&quot;keep-alive&quot; eftersom bilden är statisk och innehållslängden därför är känd.

```xml
S-1-#000017 -> [Connection: Keep-Alive ]
S-1-#000041 -> [Server: Communique Servlet Engine/3.5.5 ]
S-1-#000082 -> [Content-Type: image/gif ]
```

Servern returnerar bildens innehållslängd på anslutning 1:

```xml
S-1-#000107 -> [Content-Length: 124 ]
S-1-#000128 -> [Date: Tue, 14 Dec 2004 09:46:44 GMT ]
S-1-#000165 -> [ ]
```

Nu när innehållslängden är etablerad skickar servern bilddata vid anslutning 1:

```xml
S-1-#000167 -> [GIF87a..........................,.......
...I....0.A..8......YDA.W...1..`i.`..6...Z...$@.F..)`..f..A.....iu.........$..;]
```

När tidsgränsen för keep-alive har nåtts stängs även anslutning 1:

```xml
S-1-Finished: 291 bytes (0.0 kb/s)
C-1-Finished: 403 bytes (0.0 kb/s)
```

Exemplet ovan är relativt enkelt eftersom de två anslutningarna sker sekventiellt:

* först returnerar servern HTML-koden
* sedan begär webbläsaren bilden och öppnar en ny anslutning

I praktiken kan en sida generera många parallella förfrågningar för bilder, formatmallar, JavaScript-filer och så vidare. Detta innebär att loggarna har överlappande poster med parallella öppna anslutningar. I så fall rekommenderar Adobe att du använder alternativet -i för att förbättra läsbarheten.
