---
title: Extraherar strängar för översättning
description: Använd xgettext-maven-plugin för att extrahera strängar från källkoden som behöver översättas
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: components
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---

# Extraherar strängar för översättning{#extracting-strings-for-translating}

Använd xgettext-maven-plugin för att extrahera strängar från källkoden som behöver översättas. Plugin-programmet Maven extraherar strängar till en XLIFF-fil som du skickar för översättning. Strängar extraheras från följande platser:

* Java-källfiler
* JavaScript källfiler
* XML-representationer av SVN-resurser (JCR-noder)

## Konfigurerar strängextrahering {#configuring-string-extraction}

Konfigurera hur xgettext-maven-plugin-verktyget extraherar strängar för ditt projekt.

```xml
/filter { }
/parsers {
   /vaultxml { }
   /javascript { }
   /regexp {
      /files {
         /java { }
         /jsp { }
         /extjstemplate { }
      }
   }
}
/potentials { }
```

| Avsnitt | Beskrivning |
|---|---|
| /filter | Identifierar de filer som tolkas. |
| /parsers/vaultxml | Konfigurerar parsning av vaultfiler. Identifierar de JCR-noder som innehåller externa strängar och lokaliseringstips. Identifierar också JCR-noder som ska ignoreras. |
| /parsers/javascript | Identifierar de JavaScript-funktioner som externaliserar strängar. Du behöver inte ändra det här avsnittet. |
| /parsers/regexp | Konfigurerar tolkningen av Java-, JSP- och ExtJS-mallfiler. Du behöver inte ändra det här avsnittet. |
| /potentials | Formeln för identifiering av strängar som ska internationaliseras. |

### Identifiera filerna som ska tolkas {#identifying-the-files-to-parse}

Avsnittet /filter i i18n.any identifierar filerna som xgettext-maven-plugin-verktyget tolkar. Lägg till flera inkluderings- och exkluderingsregler som identifierar filer som tolkas respektive ignoreras. Du bör inkludera alla filer och sedan exkludera de filer som du inte vill analysera. Vanligtvis utesluter du filtyper som inte bidrar till användargränssnittet, eller filer som definierar användargränssnittet men som inte översätts. Reglerna include och exclude har följande format:

```
{ /include "pattern" }
{ /exclude "pattern" }
```

Mönsterdelen av en regel används för att matcha namnen på de filer som ska inkluderas eller exkluderas. Mönsterprefixet anger om du matchar en JCR-nod (dess representation i Vault) eller filsystemet.

| Prefix | Effekt |
|---|---|
| / | Anger en JCR-sökväg. Det innebär att det här prefixet matchar filer under katalogen jcr_root. |
| &amp;ast; | Anger en vanlig fil i filsystemet. |
| ingen | Inget prefix, eller ett mönster som börjar med en mapp eller ett filnamn, anger att filen är en vanlig fil i filsystemet. |

När det används i ett mönster anger tecknet / en underkatalog och tecknet &amp;ast; matchar alla. I följande tabell visas flera exempelregler.

<table>
 <tbody>
  <tr>
   <th>Exempelregel</th>
   <th>Effekt</th>
  </tr>
  <tr>
   <td><code>{ /include "*" }</code></td>
   <td>Inkludera alla filer.</td>
  </tr>
  <tr>
   <td><code>{ /exclude "*.pdf" }</code></td>
   <td>Uteslut alla PDF-filer.</td>
  </tr>
  <tr>
   <td><code> { /exclude "*/pom.xml" }</code></td>
   <td>Uteslut POM-filer.</td>
  </tr>
  <tr>
   <td><code class="code">{ /exclude "/content/*" }
      { /include "/content/catalogs/geometrixx/templatepages" }
      { /include "/content/catalogs/geometrixx/templatepages/*" }</code></td>
   <td><p>Uteslut alla filer under noden /content.</p> <p>Inkludera noden /content/catalogs/geometrixx/templatesPages.</p> <p>Inkludera alla underordnade noder för /content/catalogs/geometrixx/templates.</p> </td>
  </tr>
 </tbody>
</table>

### Extrahera strängarna  {#extracting-the-strings}

ingen POM:

```shell
mvn -N com.adobe.granite.maven:xgettext-maven-plugin:1.2.2:extract  -Dxgettext.verbose=true -Dxgettext.target=out -Dxgettext.rules=i18n.any -Dxgettext.root=.
```

Med POM: Lägg till detta i POM:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.adobe.granite.maven</groupId>
            <artifactId>xgettext-maven-plugin</artifactId>
            <version>1.1</version>
            <configuration>
                <rules>i18n.any</rules>
                <root>jcr_root</root>
                <xliff>cq.xliff</xliff>
                <verbose>true</verbose>
            </configuration>
        </plugin>
    </plugins>
</build>
```

kommandot:

```shell
mvn xgettext:extract
```

### Utdatafiler {#output-files}

* `raw.xliff`: extraherade strängar
* `warn.log`: varningar (om sådana finns), om `CQ.I18n.getMessage()` API används felaktigt. De behöver alltid en fix och sedan en omstart.

* `parserwarn.log`: parservarningar (om sådana finns), till exempel problem med JS-parsern
* `potentials.xliff`:&quot;potentiella&quot; kandidater som inte extraheras, men kan vara läsbara strängar som behöver översättas (kan ignoreras, men ändå skapa en enorm mängd falska positiva resultat)
* `strings.xliff`: förenklad xliff-fil som ska importeras till ALF
* `backrefs.txt`: tillåter snabb sökning av källkodsplatser för en given sträng
