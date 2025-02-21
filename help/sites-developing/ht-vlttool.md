---
title: Så här använder du VLT-verktyget
description: Jackrabbit FileVault-verktyget (VLT) har utvecklats av Apache Foundation som mappar innehållet i en Jackrabbit/AEM-instans till ditt filsystem
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2687'
ht-degree: 0%

---

# Så här använder du VLT-verktyget {#how-to-use-the-vlt-tool}

Jackrabbit FileVault-verktyget (VLT) är ett verktyg som utvecklats av [Apache Foundation](https://www.apache.org/) och som mappar innehållet i en Jackrabbit/AEM-instans till ditt filsystem. VLT-verktyget har liknande funktioner som en VLT-klient (till exempel en Subversion-klient), med normala in-, utchecknings- och hanteringsåtgärder samt konfigurationsalternativ för flexibel representation av projektinnehållet.

Du kör VLT-verktyget från kommandoraden. I det här dokumentet beskrivs hur du använder verktyget, inklusive hur du kommer igång och får hjälp, och en lista över alla [kommandon](#vlt-commands) och tillgängliga [alternativ](#vlt-global-options).

## Koncept och arkitektur {#concepts-and-architecture}

Se sidan [Fillevaöversikt](https://jackrabbit.apache.org/filevault/overview.html) och [Vault FS](https://jackrabbit.apache.org/filevault/vaultfs.html) från den officiella [dokumentationen för Apache Jackrabbit Filevault](https://jackrabbit.apache.org/filevault/index.html) för en grundlig översikt över koncept och struktur för verktyget Filevault.

## Komma igång med VLT {#getting-started-with-vlt}

Om du vill börja använda VLT måste du göra följande:

1. Installera VLT, uppdatera miljövariabler och uppdatera globala ignorerade underversionsfiler.
1. Konfigurera AEM-databasen (om du inte redan har gjort det).
1. Ta en titt på AEM-databasen.
1. Synkronisera med databasen.
1. Testa om synkroniseringen fungerade.

### Installera VLT-verktyget {#installing-the-vlt-tool}

Om du vill använda VLT-verktyget måste du först installera det. Det installeras inte som standard eftersom det är ett extra verktyg. Dessutom måste du ange systemmiljövariabeln.

1. Hämta arkivfilen FileVault från arkivdatabasen [Maven-felaktigheter.](https://repo1.maven.org/maven2/org/apache/jackrabbit/vault/vault-cli/)
   >[!NOTE]
   >
   >VLT-verktygets källa är [tillgänglig på GitHub.](https://github.com/apache/jackrabbit-filevault)
1. Hämta arkivet.
1. Lägg till `<archive-dir>/vault-cli-<version>/bin` i miljön `PATH` så att kommandofilerna `vlt` eller `vlt.bat` nås efter behov. Till exempel:

   `<aem-installation-dir>/crx-quickstart/opt/helpers/vault-cli-3.1.16/bin>`

1. Öppna ett kommandoradsskal och kör `vlt --help`. Kontrollera att utdata liknar följande hjälpskärm:

   ```shell
   vlt --help
   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   Jackrabbit FileVault [version 3.1.16] Copyright 2013 by Apache Software Foundation. See LICENSE.txt for more information.
   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   Usage:
     vlt [options] <command> [arg1 [arg2 [arg3] ..]]
   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   
   Global options:
   
     -Xjcrlog <arg>           Extended JcrLog options (omit argument for help)
     -Xdavex <arg>            Extended JCR remoting options (omit argument for help)
     --credentials <arg>      The default credentials to use
     --update-credentials     if present the credentials-to-host list is updated in the ~/.vault/auth.xml
     --config <arg>           The JcrFs config to use
     -v (--verbose)           verbose output
     -q (--quiet)             print as little as possible
     --version                print the version information and exit
     --log-level <level>      the log4j log level
     -h (--help) <command>    print this help
   ```

När du har installerat den måste du uppdatera globala ignorerade underversionsfiler. Redigera SVN-inställningarna och lägg till följande:

```xml
[miscellany]
### Set global-ignores to a set of whitespace-delimited globs
### which Subversion will ignore in its 'status' output, and
### while importing or adding files and directories.
global-ignores = .vlt
```

### Konfigurera tecknet för radslut {#configuring-the-end-of-line-character}

VLT hanterar automatiskt End Of Line (EOF) enligt följande regler:

* rader med filer som checkats ut i Windows avslutas med en `CRLF`
* rader med filer utcheckade på Linux/Unix slutar med en `LF`
* rader med filer som har implementerats i databasen avslutas med en `LF`

För att garantera att konfigurationen för VLT och SVN matchar bör du ställa in egenskapen `svn:eol-style` på `native` för filtillägget som lagras i databasen. Redigera SVN-inställningarna och lägg till följande:

```xml
[auto-props]
*.css = svn:eol-style=native
*.cnd = svn:eol-style=native
*.java = svn:eol-style=native
*.js = svn:eol-style=native
*.json = svn:eol-style=native
*.xjson = svn:eol-style=native
*.jsp = svn:eol-style=native
*.txt = svn:eol-style=native
*.html = svn:eol-style=native
*.xml = svn:eol-style=native
*.properties = svn:eol-style=native
```

### Checka ut databasen {#checking-out-the-repository}

Checka ut databasen med hjälp av källkontrollssystemet. I svn skriver du till exempel följande (ersätter URI och sökväg med din databas):

```shell
svn co https://svn.server.com/repos/myproject
```

### Synkronisera med databasen {#synchronizing-with-the-repository}

Du måste synkronisera filnivå med databasen. Så här gör du:

1. Navigera till `content/jcr_root` på kommandoraden.
1. Checka ut databasen genom att skriva följande (ditt portnummer ersätts med **4502** och dina administratörslösenord):

   ```shell
   vlt --credentials admin:admin co --force http://localhost:4502/crx
   ```

   >[!NOTE]
   >
   >Inloggningsuppgifterna behöver bara anges en gång efter din första utcheckning. De lagras sedan i din hemkatalog i `.vault/auth.xml`.

### Testar om synkroniseringen fungerade {#testing-whether-the-synchronization-worked}

När du har checkat ut databasen och synkroniserat den bör du testa att se till att allt fungerar som det ska. Ett enkelt sätt att göra detta är att redigera en **.jsp**-fil och se om ändringarna återspeglas när ändringarna har verkställts.

Så här testar du synkroniseringen:

1. Navigera till `.../jcr_content/libs/foundation/components/text`.
1. Redigera något i `text.jsp`.
1. Visa de ändrade filerna genom att skriva `vlt st`
1. Visa ändringarna genom att skriva `vlt diff text.jsp`
1. Genomför ändringarna: `vlt ci test.jsp`.
1. Läs in en sida som innehåller en textkomponent igen och se om ändringarna finns där.

## Få hjälp med VLT-verktyget {#getting-help-with-the-vlt-tool}

När du har installerat VLT-verktyget kommer du åt dess hjälpfil från kommandoraden:

```shell
vlt --help
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Jackrabbit FileVault [version 3.1.16] Copyright 2013 by Apache Software Foundation. See LICENSE.txt for more information.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Usage:
  vlt [options] <command> [arg1 [arg2 [arg3] ..]]
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Global options:
  -Xjcrlog <arg>           Extended JcrLog options (omit argument for help)
  -Xdavex <arg>            Extended JCR remoting options (omit argument for help)
  --credentials <arg>      The default credentials to use
  --update-credentials     if present the credentials-to-host list is updated in the ~/.vault/auth.xml
  --config <arg>           The JcrFs config to use
  -v (--verbose)           verbose output
  -q (--quiet)             print as little as possible
  --version                print the version information and exit
  --log-level <level>      the log4j log level
  -h (--help) <command>    print this help
Commands:
  export                   Export the Vault filesystem
  import                   Import a Vault filesystem
  checkout (co)            Checkout a Vault file system
  status (st)              Print the status of working copy files and directories.
  update (up)              Bring changes from the repository into the working copy.
  info                     Displays information about a local file.
  commit (ci)              Send changes from your working copy to the repository.
  revert (rev)             Restore pristine working copy file (undo most local edits).
  resolved (res)           Remove 'conflicted' state on working copy files or directories.
  propget (pg)             Print the value of a property on files or directories.
  proplist (pl)            Print the properties on files or directories.
  propset (ps)             Set the value of a property on files or directories.
  add                      Put files and directories under version control.
  delete (del,rm)          Remove files and directories from version control.
  diff (di)                Display the differences between two paths.
  rcp                      Remote copy of repository content.
  sync                     Control vault sync service
  console                  Run an interactive console
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

Om du vill ha hjälp med ett visst kommando skriver du hjälpkommandot följt av namnet på kommandot. Till exempel:

```shell
vlt --help export
Usage:
 export -v|-t <arg>|-p <uri> <jcr-path> <local-path>

Description:
  Export the Vault filesystem mounted at <uri> to the local filesystem at <local-path>. An optional <jcr-path> can be specified to export just a sub tree.
  Example:
    vlt export http://localhost:4502/crx /apps/geometrixx myproject

Options:
  -v (--verbose)          verbose output
  -t (--type) <arg>       specifies the export type. either 'platform' or 'jar'.
  -p (--prune-missing)    specifies if missing local files should be deleted.
  <uri>                   mountpoint uri
  <jcr-path>              the jcr path
  <local-path>            the local path
```

## Vanliga uppgifter utförda i VLT {#common-tasks-performed-in-vlt}

Nedan följer några vanliga uppgifter som utförs i VLT. Mer information om varje kommando finns i de enskilda [kommandona](#vlt-commands).

### Checka ut ett underträd {#checking-out-a-subtree}

Om du bara vill checka ut ett underträd i databasen, till exempel `/apps/geometrixx`, kan du göra det genom att skriva följande:

```shell
vlt co http://localhost:4502/crx/-/jcr:root/apps/geometrixx geo
```

Om du gör det skapas en ny exportrot `geo` med katalogen `META-INF` och `jcr_root` och alla filer placeras under `/apps/geometrixx` i `geo/jcr_root`.

### Utföra en filtrerad utcheckning {#performing-a-filtered-checkout}

Om du har ett befintligt arbetsytefilter och vill använda det för utcheckning kan du antingen skapa katalogen `META-INF/vault` och placera filtret där, eller ange det på kommandoraden enligt följande:

```shell
$ vlt co --filter filter.xml http://localhost:4502/crx/-/jcr:root geo
```

Ett exempelfilter:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<workspaceFilter version="1.0">
    <filter root="/etc/designs/geometrixx" />
    <filter root="/apps/geometrixx"/>
</workspaceFilter>
```

### Använda Importera/exportera i stället för VLT-kontroll {#using-import-export-instead-of-vlt-control}

Du kan importera och exportera innehåll mellan en JCR-databas och det lokala filsystemet utan att använda kontrollfiler.

Så här importerar och exporterar du innehåll utan att använda kontrollen `.vlt`:

1. Inledningsvis konfigurera databasen:

   ```shell
   $ cd /projects
   $ svn mkdir https://svn.server.com/repos/myproject
   $ svn co https://svn.server.com/repos/myproject
   $ vlt export -v http://localhost:4502/crx /apps/geometrixx geometrixx
   $ cd geometrixx/
   $ svn add META-INF/ jcr_root/
   $ svn ci
   ```

1. Ändra fjärrkopian och uppdatera JCR:

   ```shell
   $ cd /projects/geometrixx
   $ vlt -v import http://localhost:4502/crx . /
   ```

1. Ändra fjärrkopian och uppdatera filservern:

   ```shell
   $ cd /projects/geometrixx
   $ vlt export -v http://localhost:4502/crx /apps/geometrixx .
   $ svn st
   M      META-INF/vault/properties.xml
   M      jcr_root/apps/geometrixx/components/contentpage/.content.xml
   $ svn ci
   ```

## Använda VLT {#using-vlt}

Om du vill skicka kommandon i VLT skriver du följande på kommandoraden:

```shell
vlt [options] <command> [arg1 [arg2 [arg3] ..]]  
```

Alternativ och kommandon beskrivs i detalj i följande avsnitt.

## Globala VLT-alternativ {#vlt-global-options}

Nedan följer en lista över VLT-alternativ som är tillgängliga för alla kommandon. Mer information om ytterligare tillgängliga alternativ finns i de enskilda kommandona.

|  |  |
|--- |--- |
| Alternativ | Beskrivning |
| `-Xjcrlog <arg>` | Utökade JCRLog-alternativ |
| `-Xdavex <arg>` | Utökade alternativ för JCR-fjärrkommunikation |
| `--credentials <arg>` | Standardautentiseringsuppgifterna som ska användas |
| `--config <arg>` | JcrFs-konfigurationen som ska användas |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | skriv ut så lite som möjligt |
| `--version` | Skriver ut versionsinformation och avslutar VLT |
| `--log-level <level>` | Anger loggnivån, till exempel loggnivån log4j. |
| `-h (--help) <command>` | Skriver ut hjälp för det aktuella kommandot |

## VLT-kommandon {#vlt-commands}

I följande tabell beskrivs alla tillgängliga VLT-kommandon. I de enskilda kommandona finns detaljerad information om syntax, tillgängliga alternativ och exempel.

|  |  |  |
|--- |--- |--- |
| Kommando | Förkortat kommando | Beskrivning |
| `export` |  | Exporterar från en JCR-databas (valv) till det lokala filsystemet utan kontrollfiler. |
| `import` |  | Importerar ett lokalt filsystem till en JCR-databas (valvfilsystem). |
| `checkout` | `co` | Checkar ut ett valvfilsystem. Använd detta för en inledande JCR-databas till det lokala filsystemet. (Obs! Checka först ut databasen i subversion.) |
| `analyze` |  | Analyserar paket. |
| `status` | `st` | Skriver ut status för arbetskopia av filer och kataloger. |
| `update` | `up` | Importerar ändringar från databasen till arbetskopian. |
| `info` |  | Visar information om en lokal fil. |
| `commit` | `ci` | Skickar ändringar från din arbetskopia till databasen. |
| `revert` | `rev` | Återställer arbetskopiefilen till det ursprungliga läget och ångrar de flesta lokala redigeringar. |
| `resolved` | `res` | Tar bort konfliktstatus för arbetsfiler eller kataloger. |
| `propget` | `pg` | Skriver ut värdet för en egenskap på filer eller kataloger. |
| `proplist` | `pl` | Skriver ut egenskaperna för filer eller kataloger. |
| `propset` | `ps` | Anger värdet för en egenskap i filer eller kataloger. |
| `add` |  | Placerar filer och kataloger under versionskontroll. |
| `delete` | `del` eller `rm` | Tar bort filer och kataloger från versionskontrollen. |
| `diff` | `di` | Visar skillnaderna mellan två banor. |
| `console` |  | Kör en interaktiv konsol. |
| `rcp` |  | Kopierar ett nodträd från en fjärrdatabas till en annan. |
| `sync` |  | Låter dig styra tjänsten för valvsynkronisering. |

### Exportera {#export}

Exporterar Vaultfilsystemet som är monterat på &lt;uri> till det lokala filsystemet på &lt;local-path>. Du kan ange ett valfritt &lt;jcr-path> om du bara vill exportera ett underträd.

#### Syntax {#syntax}

```shell
export -v|-t <arg>|-p <uri> <jcr-path> <local-path>
```

#### Alternativ {#options}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-t (--type) <arg>` | anger exporttypen, antingen plattform eller jar. |
| `-p (--prune-missing)` | anger om saknade lokala filer ska tas bort |
| `<uri>` | montpoint uri |
| `<jcrPath>` | JCR-sökväg |
| `<localPath>` | lokal sökväg |

#### Exempel {#examples}

```shell
vlt export http://localhost:4502/crx /apps/geometrixx myproject
```

### Importera {#import}

Importerar det lokala filsystemet (med början vid `<local-path>` till valvfilsystemet vid `<uri>`. Du kan ange en `<jcr-path>` som importrot. Om `--sync` anges hamnar de importerade filerna automatiskt under vaultkontroll.

#### Syntax {#syntax-1}

```shell
import -v|-s <uri> <local-path> <jcr-path>
```

#### Alternativ {#options-1}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-s (-- sync)` | placerar de lokala filerna under vaultkontrollen |
| `<uri>` | montpoint uri |
| `<jcrPath>` | JCR-sökväg |
| `<localPath>` | lokal sökväg |

#### Exempel {#examples-1}

```shell
vlt import http://localhost:4502/crx . /
```

### Utcheckning (co) {#checkout-co}

Utför en första utcheckning från en JCR-databas till det lokala filsystemet från &lt;uri> till det lokala filsystemet på &lt;local-path>. Du kan också lägga till ett &lt;jcrPath>-argument för att checka ut en underkatalog till fjärrträdet. Workspace-filter kan anges som kopieras till META-INF-katalogen.

#### Syntax {#syntax-2}

```shell
checkout --force|-v|-q|-f <file> <uri> <jcrPath> <localPath>  
```

#### Alternativ {#options-2}

|  |  |
|--- |--- |
| `--force` | tvingar utcheckning att skriva över lokala filer om de redan finns |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `-f (--filter) <file>` | anger automatiska filter om inga har definierats |
| `<uri>` | montpoint uri |
| `<jcrPath>` | fjärrsökväg (valfritt) |
| `<localPath>` | (valfritt) lokal sökväg |

#### Exempel {#examples-2}

Använda JCR Remoting:

```shell
vlt --credentials admin:admin co http://localhost:8080/crx/server/crx.default/jcr_root/
```

Med standardarbetsytan:

```shell
vlt --credentials admin:admin co http://localhost:8080/crx/server/-/jcr_root/
```

Om URI:n är ofullständig utökas den:

```shell
vlt --credentials admin:admin co http://localhost:8080/crx
```

### Analysera {#analyze}

Analyserar paket.

#### Syntax {#syntax-3}

```shell
analyze -l <format>|-v|-q <localPaths1> [<localPaths2> ...]
```

#### Alternativ {#options-3}

|  |  |
|--- |--- |
| `-l (--linkFormat) <format>` | utskriftsformat för snabbkorrigeringslänkar (namn,id), till exempel `[CQ520_HF_%s|%s]` |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `<localPaths> [<localPaths> ...]` | lokal sökväg |

### Status {#status}

Skriver ut status för arbetskopia av filer och kataloger.

Om `--show-update` anges kontrolleras varje fil mot fjärrversionen. Den andra bokstaven anger sedan vilken åtgärd som ska utföras av en uppdateringsåtgärd.

#### Syntax {#syntax-4}

```shell
status -v|-q|-u|-N <file1> [<file2> ...]
```

#### Alternativ {#options-4}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `-u (--show-update)` | visar uppdateringsinformation |
| `-N (--non-recursive)` | arbetar i en enda katalog |
| `<file> [<file> ...]` | fil eller katalog som ska visa status |

### Uppdatera {#update}

Kopierar ändringar från databasen till arbetskopian.

#### Syntax {#syntax-5}

```shell
update -v|-q|--force|-N <file1> [<file2> ...]
```

#### Alternativ {#options-5}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `--force` | tvingar överskrivning av lokala filer |
| `-N (--non-recursive)` | arbetar i en enda katalog |
| `<file> [<file> ...]` | fil eller katalog som ska uppdateras |

### Info {#info}

Visar information om en lokal fil.

#### Syntax {#syntax-6}

```shell
info -v|-q|-R <file1> [<file2> ...]
```

#### Alternativ {#options-6}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `-R (--recursive)` | används rekursivt |
| `<file> [<file> ...]` | fil eller katalog för att visa information |

### Verkställ {#commit}

Skickar ändringar från din arbetskopia till databasen.

#### Syntax {#syntax-7}

```shell
commit -v|-q|--force|-N <file1> [<file2> ...]
```

#### Alternativ {#options-7}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `--force` | Tvingar att implementera även om fjärrkopian ändras |
| `-N (--non-recursive)` | arbetar i en enda katalog |
| `<file> [<file> ...]` | fil eller katalog som ska implementeras |

### Återställ {#revert}

Återställer arbetskopiefilen till ursprungligt läge och ångrar de flesta lokala redigeringarna.

#### Syntax {#syntax-8}

```shell
revert -q|-R <file1> [<file2> ...]
```

#### Alternativ {#options-8}

|  |  |
|--- |--- |
| `-q (--quiet)` | så lite som möjligt |
| `-R (--recursive)` | nedtonar rekursivt |
| `<file> [<file> ...]` | fil eller katalog som ska implementeras |

### Löst {#resolved}

Tar bort **konfliktläge** för arbetskopiefiler eller kataloger.

>[!NOTE]
>
>Det här kommandot löser inte konflikter semantiskt och tar inte bort konfliktmarkörer. Det tar bara bort konfliktrelaterade artefaktfiler och tillåter att PATH verkställs igen.

#### Syntax {#syntax-9}

```shell
resolved -q|-R|--force <file1> [<file2> ...]  
```

#### Alternativ {#options-9}

|  |  |
|--- |--- |
| `-q (--quiet)` | så lite som möjligt |
| `-R (--recursive)` | nedtonar rekursivt |
| `--force` | löses, även om det finns konfliktmarkörer |
| `<file> [<file> ...]` | fil eller katalog som ska tolkas |

### Propget {#propget}

Skriver ut värdet för en egenskap på filer eller kataloger.

#### Syntax {#syntax-10}

```shell
propget -q|-R <propname> <file1> [<file2> ...]
```

#### Alternativ {#options-10}

|  |  |
|--- |--- |
| `-q (--quiet)` | så lite som möjligt |
| `-R (--recursive)` | nedtonar rekursivt |
| `<propname>` | egenskapsnamnet |
| `<file> [<file> ...]` | fil eller katalog som egenskapen ska hämtas från |

### Proplist {#proplist}

Skriver ut egenskaperna för filer eller kataloger.

#### Syntax {#syntax-11}

```shell
proplist -q|-R <file1> [<file2> ...]
```

#### Alternativ {#options-11}

|  |  |
|--- |--- |
| `-q (--quiet)` | så lite som möjligt |
| `-R (--recursive)` | nedtonar rekursivt |
| `<file> [<file> ...]` | fil eller katalog som egenskaperna ska listas från |

### Propset {#propset}

Anger värdet för en egenskap i filer eller kataloger.

>[!NOTE]
>
>VLT känner igen följande egenskaper med specialversioner:
>
>`vlt:mime-type`
>
>Filens mime-typ. Används för att avgöra om filen ska sammanfogas. En mime-text som börjar med &quot;text/&quot; (eller en mime-typ som saknas) behandlas som text. Allt annat behandlas som binärt.

#### Syntax {#syntax-12}

```shell
propset -q|-R <propname> <propval> <file1> [<file2> ...]
```

#### Alternativ {#options-12}

|  |  |
|--- |--- |
| `-q (--quiet)` | så lite som möjligt |
| `-R (--recursive)` | nedtonar rekursivt |
| `<propname>` | egenskapsnamnet |
| `<propval>` | egenskapsvärdet |
| `<file> [<file> ...]` | fil eller katalog som egenskapen ska anges till |

### Lägg till {#add}

Placerar filer och kataloger under versionskontroll och schemalägger dem så att de kan läggas till i databasen. De kommer att läggas till vid nästa implementering.

#### Syntax {#syntax-13}

```shell
add -v|-q|-N|--force <file1> [<file2> ...]
```

#### Alternativ {#options-13}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `-N (--non-recursive)` | arbetar i en enda katalog |
| `--force` | tvingar operationen att köras |
| `<file> [<file> ...]` | lokal fil eller katalog som ska läggas till |

### Ta bort {#delete}

Tar bort filer och kataloger från versionskontrollen.

#### Syntax {#syntax-14}

```shell
delete -v|-q|--force <file1> [<file2> ...]
```

#### Alternativ {#options-14}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift |
| `-q (--quiet)` | så lite som möjligt |
| `--force` | tvingar operationen att köras |
| `<file> [<file> ...]` | lokal fil eller katalog som ska tas bort |

### Diff {#diff}

Visar skillnaderna mellan två banor.

#### Syntax {#syntax-15}

```shell
diff -N <file1> [<file2> ...]
```

#### Alternativ {#options-15}

|  |  |
|--- |--- |
| `-N (--non-recursive)` | arbetar i en enda katalog |
| `<file> [<file> ...]` | fil eller katalog som skillnaderna ska visas från |

### Konsol {#console}

Kör en interaktiv konsol.

#### Syntax {#syntax-16}

```shell
console -F <file>
```

#### Alternativ {#options-16}

|  |  |
|--- |--- |
| `-F (--console-settings) <file>` | anger konsolinställningsfilen. Standardfilen är console.properties. |

### Rcp {#rcp}

Kopierar ett nodträd från en fjärrdatabas till en annan. `<src>` pekar på källnoden och `<dst>` anger målsökvägen, där den överordnade noden måste finnas. Rcp bearbetar noderna genom att direktuppspela data.

#### Syntax {#syntax-17}

```shell
rcp -q|-r|-b <size>|-t <seconds>|-u|-n|-e <arg1> [<arg2> ...] <src> <dst>
```

#### Alternativ {#options-17}

|  |  |
|--- |--- |
| `-q (--quiet)` | Skriver ut så lite som möjligt. |
| `-r (--recursive)` | Nedbrytning rekursivt. |
| `-b (--batchSize) <size>` | Antal noder som ska bearbetas innan en mellanliggande spara sparas. |
| `-t (--throttle) <seconds>` | Antal sekunder som ska förflyta efter en mellanliggande sparning. |
| `-u (--update)` | Skriv över/ta bort befintliga noder. |
| `-n (--newer)` | Respektera egenskaperna lastModified för uppdatering. |
| `-e (--exclude) <arg> [<arg> ...]` | Regexp för uteslutna källsökvägar. |
| `<src>` | Källträdets databasadress. |
| `<dst>` | Målnodens databasadress. |

#### Exempel {#examples-3}

```shell
vlt rcp http://localhost:4502/crx/-/jcr:root/content  https://admin:admin@localhost:4503/crx/-/jcr:root/content_copy  
```

>[!NOTE]
>
>Alternativen `--exclude` måste följas av ett annat alternativ före argumenten `<src>` och `<dst>`. Till exempel:
>
>`vlt rcp -e ".*\.txt" -r`

### Synkronisera {#sync}

Låter dig styra tjänsten för valvsynkronisering. Utan argument försöker det här kommandot synkronisera den aktuella arbetskatalogen. Om den körs inom en giltig utcheckning används respektive filter och värd för att konfigurera synkroniseringen. Om den körs utanför en giltig utcheckning registreras den aktuella mappen endast för synkronisering om katalogen är tom.

#### Syntax {#syntax-18}

```shell
sync -v|--force|-u <uri> <command> <localPath>
```

#### Alternativ {#options-18}

|  |  |
|--- |--- |
| `-v (--verbose)` | utförlig utskrift. |
| `--force` | tvinga vissa kommandon att köras. |
| `-u (--uri) <uri>` | Anger URI:n för synkroniseringsvärden. |
| `<command>` | synkroniseringskommando som ska köras. |
| `<localPath>` | lokal mapp att synkronisera. |

### Statuskoder {#status-codes}

Statuskoderna som används av VLT är:

* &#39; inga ändringar
* A har lagts till
* C i konflikt
* D har tagits bort
* I ignorerades
* M har ändrats
* R har ersatts
* ? objektet är inte under versionskontroll
* &#39;!&#39; objektet saknas (tas bort av ett icke-svn-kommando) eller är ofullständigt
* Versionsobjektet &#39;~&#39; hindrades av något objekt av en annan typ

## Konfigurera FileVault-synkronisering {#setting-up-filevault-sync}

Tjänsten vault sync används för att synkronisera databasinnehåll med en lokal filsystemrepresentation och omvänt. Detta uppnås genom att installera en OSGi-tjänst som avlyssnar databasändringar och regelbundet söker igenom filsystemets innehåll. Det använder samma serialiseringsformat som vault för att mappa databasinnehållet till disken.

>[!NOTE]
>
>Tjänsten för vaultsynkronisering är ett utvecklingsverktyg och bör inte användas i ett produktivt system. Tjänsten kan bara synkroniseras med det lokala filsystemet och kan inte användas för fjärrutveckling.

### Installera tjänsten med VLT {#installing-the-service-using-vlt}

Kommandot `vlt sync install` kan användas för att installera tjänstpaketet för vaultsynkronisering och konfigurationen automatiskt.

Paketet installeras under `/libs/crx/vault/install` och konfigurationsnoden skapas på `/libs/crx/vault/com.day.jcr.sync.impl.VaultSyncServiceImpl`. Tjänsten är till att börja med aktiverad, men inga synkroniseringsrötter har konfigurerats.

I följande exempel installeras synkroniseringstjänsten på den CRX-instans som är tillgänglig för den angivna uri.

```shell
$ vlt --credentials admin:admin sync --uri http://localhost:4502/crx install
```

### Visa tjänstens status {#displaying-the-service-status}

Kommandot `status` kan användas för att visa information om den synkroniseringstjänst som körs. &quot;

```shell
$ vlt sync status --uri http://localhost:4502/crx
Connecting via JCR remoting to http://localhost:4502/crx/server
Listing sync status for http://localhost:4502/crx/server/-/jcr:root
- Sync service is enabled.
- No sync directories configured.
```

>[!NOTE]
>
>Kommandot `status` hämtar inga livedata från tjänsten utan läser konfigurationen på `/libs/crx/vault/com.day.jcr.sync.impl.VaultSyncServiceImpl`.

### Lägga till en synkroniseringsmapp {#adding-a-sync-folder}

Kommandot `register` används för att lägga till en mapp som ska synkroniseras med konfigurationen.

```shell
$ vlt sync register
Connecting via JCR remoting to http://localhost:4502/crx/server
Added new sync directory: /tmp/workspace/vltsync/jcr_root
```

>[!NOTE]
>
>Kommandot `register` utlöser inte någon synkronisering förrän du konfigurerar `sync-once`-konfigurationen.

### Ta bort en synkroniseringsmapp {#removing-a-sync-folder}

Kommandot `unregister` används för att ta bort en mapp som ska synkroniseras från konfigurationen.

```shell
$  vlt sync unregister
Connecting via JCR remoting to http://localhost:4502/crx/server
Removed sync directory: /tmp/workspace/vltsync/jcr_root
```

>[!NOTE]
>
>Du måste avregistrera en synkroniseringsmapp innan du tar bort själva mappen.

### Konfigurerar synkronisering {#configuring-synchronization}

#### Tjänstkonfiguration {#service-configuration}

När tjänsten körs kan den konfigureras med följande parametrar:

* `vault.sync.syncroots`: En eller flera lokala filsystemsökvägar som definierar synkroniseringsrötterna.

* `vault.sync.fscheckinterval`: Täthet (i sekunder) som filsystemet ska genomsökas efter ändringar. Standardvärdet är 5 sekunder.
* `vault.sync.enabled`: Allmän flagga som aktiverar/inaktiverar tjänsten.

>[!NOTE]
>
>Tjänsten kan konfigureras med webbkonsolen eller en `sling:OsgiConfig`-nod (med namnet `com.day.jcr.sync.impl.VaultSyncServiceImpl`) i databasen.
>
>När du arbetar med AEM finns det flera metoder för att hantera konfigurationsinställningarna för sådana tjänster. Mer information finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

#### Synkronisera mappkonfiguration {#sync-folder-configuration}

I varje synkroniseringsmapp lagras konfiguration och status i tre filer:

* `.vlt-sync-config.properties`: konfigurationsfil.

* `.vlt-sync.log`: loggfil som innehåller information om de åtgärder som utfördes under synkroniseringen.
* `.vlt-sync-filter.xml`: filter som definierar vilka delar av databasen som synkroniseras. Filens format beskrivs i avsnittet [Utföra en filtrerad utcheckning](#performing-a-filtered-checkout).

Med filen `.vlt-sync-config.properties` kan du konfigurera följande egenskaper:

**inaktiverad** Aktiverar eller inaktiverar synkroniseringen. Som standard är den här parametern inställd på false för att tillåta synkronisering.

**sync-once** Om nästa sökning inte är tom kommer att synkronisera mappen i den angivna riktningen, kommer parametern att rensas. Två värden stöds:

* `JCR2FS`: exporterar allt innehåll i JCR-databasen och skriver till den lokala disken.
* `FS2JCR`: importerar allt innehåll från disken till JCR-databasen.

**sync-log** definierar loggfilens namn. Som standard är värdet .vlt-sync.log

### Använda VLT-synkronisering för utveckling {#using-vlt-sync-for-development}

Så här konfigurerar du en utvecklingsmiljö baserad på en synkroniseringsmapp:

1. Checka ut databasen med VLT-kommandoraden:

   ```shell
   $ vlt --credentials admin:admin co --force http://localhost:4502/crx dev
   ```

   >[!NOTE]
   >
   >Du kan bara använda filter för att checka ut lämpliga sökvägar. Mer information finns i avsnittet [Utföra en filtrerad utcheckning](#performing-a-filtered-checkout).

1. Gå till rotmappen för arbetskopian:

   ```shell
   $ cd dev/jcr_root/
   ```

1. Installera synkroniseringstjänsten i din databas:

   ```xml
   $ vlt sync install
   Connecting via JCR remoting to http://localhost:4502/crx/server
   Preparing to install vault-sync-2.4.24.jar...
   Updated bundle: vault-sync-2.4.24.jar
   Created new config at /libs/crx/vault/config/com.day.jcr.sync.impl.VaultSyncServiceImpl
   ```

1. Initiera synkroniseringstjänsten:

   ```shell
   $ vlt sync
   Connecting via JCR remoting to http://localhost:4502/crx/server
   Starting initialization of sync service in existing vlt checkout /Users/colligno/Applications/cq5/vltsync/sandbox/dev/jcr_root for http://localhost:4502/crx/server/-/jcr:root
   Added new sync directory: /Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root
   
   The directory /Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root is now enabled for syncing.
   You might perform a 'sync-once' by setting the
   appropriate flag in the /Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root/.vlt-sync-config.properties file.
   ```

1. Redigera den dolda filen `.vlt-sync-config.properties` och konfigurera synkronisering för att synkronisera innehållet i databasen:

   ```xml
   sync-once=JCR2FS
   ```

   >[!NOTE]
   >
   >I det här steget hämtas hela databasen enligt din filterkonfiguration.

1. Kontrollera loggfilen `.vlt-sync.log` för att se förloppet:

   ```xml
   ***
   30.04.2017 14:39:10 A file:///Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root/apps/geometrixx-outdoors/src/core/src/main/java/info/geometrixx/outdoors/
   30.04.2017 14:39:10 A file:///Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root/apps/geometrixx-outdoors/src/core/src/main/java/info/geometrixx/outdoors/core/
   30.04.2017 14:39:10 A file:///Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root/apps/geometrixx-outdoors/src/core/src/main/java/info/geometrixx/outdoors/core/product/
   30.04.2017 14:39:10 A file:///Users/trushton/Applications/aem/vltsync/sandbox/dev/jcr_root/apps/geometrixx-outdoors/src/core/src/main/java/info/geometrixx/outdoors/core/product/GeoProduct.java
   ***
   ```

Din lokala mapp är nu synkroniserad med databasen. Synkroniseringen är dubbelriktad, så ändringar från databasen tillämpas på din lokala synkroniseringsmapp och omvänt.

>[!NOTE]
>
>Funktionen för VLT-synkronisering stöder endast enkla filer och mappar men identifierar särskilda serialiserade vault-filer (.content.xml, dialog.xml och så vidare) och ignorerar dem. Det är därför möjligt att använda valvsynkronisering i en standardvärdeutcheckning.
