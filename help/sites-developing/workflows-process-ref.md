---
title: Referens för arbetsflödesprocess
description: Använd den här processreferensen för arbetsflöden i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 20fd27e2-0eb7-4fab-a56a-f83816947579
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 0%

---

# Referens för arbetsflödesprocess{#workflow-process-reference}

AEM innehåller flera steg som kan användas för att skapa arbetsflödesmodeller. Anpassade processsteg kan också läggas till för uppgifter som inte omfattas av de inbyggda stegen (se [Skapa arbetsflödesmodeller](/help/sites-developing/workflows-models.md)).

## Processegenskaper {#process-characteristics}

Följande egenskaper beskrivs för varje steg i processen.

### Java™-klass eller ECMA-sökväg {#java-class-or-ecma-path}

Processstegen definieras antingen av en Java™-klass eller av ett ECMAScript.

* För Java™-klassprocesser anges det fullständiga, kvalificerade klassnamnet.
* För ECMAScript-processerna anges sökvägen till skriptet.

### Nyttolast {#payload}

Nyttolasten är den enhet som en arbetsflödesinstans agerar på. Nyttolasten väljs implicit av den kontext i vilken en arbetsflödesinstans startas.

Om ett arbetsflöde till exempel används på en AEM-sida, *P*, skickas *P* från steg till steg allt eftersom arbetsflödet fortskrider, där varje steg kan agera på *P* på något sätt.

I det vanligaste fallet är nyttolasten en JCR-nod i databasen (till exempel en AEM-sida eller resurs). En JCR-nodnyttolast skickas som en sträng som antingen är en JCR-sökväg eller en JCR-identifierare (UUID). Ibland kan nyttolasten vara en JCR-egenskap (skickas som en JCR-sökväg), en URL, ett binärt objekt eller ett generiskt Java™-objekt. Enskilda processsteg som fungerar på nyttolasten förväntar sig vanligtvis en nyttolast av en viss typ, eller fungerar på olika sätt beroende på nyttolasttypen. För varje process som beskrivs nedan beskrivs den förväntade nyttolasttypen, om sådan finns,.

### Argument {#arguments}

Vissa arbetsflödesprocesser accepterar argument som administratören anger när arbetsflödessteget ställs in.

Argument anges som en enskild sträng i egenskapen **Processargument** i rutan **Egenskaper** i arbetsflödesredigeraren. För varje process som beskrivs nedan beskrivs argumentsträngens format i en enkel EBNF-grammatik. Följande indikerar till exempel att argumentsträngen består av ett eller flera kommaavgränsade par, där varje par består av ett namn (som är en sträng) och ett värde, avgränsade med ett dubbelkolon:

```
    args := name '::' value [',' name '::' value]*
    name := /* A string */
    value := /* A string */
```


### Timeout {#timeout}

Efter den här tidsgränsen är arbetsflödessteget inte längre i drift. Vissa arbetsflödesprocesser respekterar tidsgränsen, medan andra inte gäller och ignoreras.

### Behörigheter {#permissions}

Sessionen som skickas till `WorkflowProcess` backas upp av tjänstanvändaren för arbetsflödets processtjänst, som har följande behörigheter i arkivets rot:

* `jcr:read`
* `rep:write`
* `jcr:versionManagement`
* `jcr:lockManagement`
* `crx:replicate`

Om den behörighetsuppsättningen inte är tillräcklig för din `WorkflowProcess`-implementering måste den använda en session med de behörigheter som krävs.

Det rekommenderade sättet att göra detta är att använda en tjänstanvändare som har skapats med den nödvändiga, men minimala, underuppsättningen behörigheter som krävs.

>[!CAUTION]
>
>Om du uppgraderar från en tidigare version än AEM 6.2 kan du behöva uppdatera implementeringen.
>
>I tidigare versioner skickades administratörssessionen till `WorkflowProcess`-implementeringarna och kunde sedan ha fullständig åtkomst till databasen utan att behöva definiera specifika åtkomstkontrollistor.
>
>Behörigheterna definieras nu enligt ovan ([Behörigheter](#permissions)). Som den metod som rekommenderas för att uppdatera implementeringen.
>
>En kortsiktig lösning finns också tillgänglig för bakåtkompatibilitet när kodändringar inte är möjliga:
>
>* Använda webbkonsolen ( `/system/console/configMgr`) för att hitta **Adobe Granite Workflow Configuration Service**
>
>* aktivera **äldre arbetsflödesprocessläge**
>
>Detta återgår till det gamla beteendet att tillhandahålla en administratörssession till implementeringen av `WorkflowProcess` och ger obegränsad åtkomst till hela databasen igen.

## Processer för arbetsflödeskontroll {#workflow-control-processes}

Följande processer utför inga åtgärder på innehåll. De styr själva arbetsflödet.

### AbsoluteTimeAutoAdvance (autoförskott för absolut tid) {#absolutetimeautoadvancer-absolute-time-auto-advancer}

Processen `AbsoluteTimeAutoAdvancer` (autoavancerat för absolut tid) fungerar på samma sätt som **AutoAdvanced**, förutom att den inträffar vid en viss tidpunkt och ett visst datum i stället för efter en viss tidslängd.

* **Java™-klass**: `com.adobe.granite.workflow.console.timeout.autoadvance.AbsoluteTimeAutoAdvancer`
* **Nyttolast**: Ingen.
* **Argument**: Inga.
* **Timeout**: Processen tar slut när den angivna tiden och datumet nås.

### AutoAdvantager (automatiskt avancerat) {#autoadvancer-auto-advancer}

Processen `AutoAdvancer` flyttar automatiskt arbetsflödet till nästa steg. Om det finns mer än ett möjligt nästa steg (till exempel om det finns en OR-delning) kommer den här processen att flytta arbetsflödet längs *standardvägen*, om en sådan har angetts, annars kommer arbetsflödet inte att avanceras.

* **Java™-klass**: `com.adobe.granite.workflow.console.timeout.autoadvance.AutoAdvancer`

* **Nyttolast**: Ingen.
* **Argument**: Inga.
* **Timeout**: Processtiden är slut efter angiven tidslängd.

### ProcessAssembler (Process Assembler) {#processassembler-process-assembler}

Processen `ProcessAssembler` kör flera underprocesser sekventiellt i ett enda arbetsflödessteg. Om du vill använda `ProcessAssembler` skapar du ett enda steg av den här typen i arbetsflödet och anger dess argument för att ange namn och argument för de underprocesser som du vill köra.

* **Java™-klass**: `com.day.cq.workflow.impl.process.ProcessAssembler`

* **Nyttolast**: En DAM-resurs, AEM-sida eller ingen nyttolast (beror på kraven för underprocesser).
* **Argument**:

```
        args := arg [',' arg]
        arg := processname ['::' processargs]
        processname := /* A fully qualified Java Class or absolute
        repository path to an ECMAScript */
        processargs := processarg [';' processarg]*
        processarg := '[' nobracketprocessarg ']' | nobracketprocessarg
        nobracketprocessarg := listitem [':' listitem]*
        listitem := /* A string */
```

* **Timeout**: Förväntat.

Till exempel:

* Hämta metadata från resursen.
* Skapa tre miniatyrbilder av de tre angivna storlekarna.
* Skapa en JPEG-bild från resursen, förutsatt att resursen ursprungligen inte är en GIF eller PNG (då skapas ingen JPEG).
* Ange det senast ändrade datumet för tillgången.

```shell
com.day.cq.dam.core.process.ExtractMetadataProcess,
    com.day.cq.dam.core.process.CreateThumbnailProcess::[140:100];[48:48];[319:319:false],
    com.day.cq.dam.core.process.CreateWebEnabledImageProcess::dimension:1280:1280;mimetype:image/jpeg,
    com.day.cq.dam.core.process.AssetSetLastModifiedProcess
```

## Grundläggande processer {#basic-processes}

Följande processer utför enkla uppgifter eller fungerar som exempel.

>[!CAUTION]
>
>Ändra ingenting i sökvägen `/libs`.
>
>Detta beror på att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan skrivas över när du använder en snabbkorrigering eller ett funktionspaket).

### delete {#delete}

Objektet vid den angivna sökvägen tas bort.

* **ECMAScript-sökväg**: `/libs/workflow/scripts/delete.ecma`

* **Nyttolast**: JCR-sökväg
* **Argument**: Inga
* **Timeout**: Ignorerad

### noop {#noop}

Detta är null-processen. Ingen åtgärd utförs, men ett felsökningsmeddelande loggas.

* **ECMAScript-sökväg**: `/libs/workflow/scripts/noop.ecma`

* **Nyttolast**: Ingen
* **Argument**: Inga
* **Timeout**: Ignorerad

### rule-false {#rule-false}

Detta är en null-process som returnerar `false` för metoden `check()`.

* **ECMAScript-sökväg**: `/libs/workflow/scripts/rule-false.ecma`

* **Nyttolast**: Ingen
* **Argument**: Inga
* **Timeout**: Ignorerad

### exempel {#sample}

Detta är ett exempel på ECMAScript-process.

* **ECMAScript-sökväg**: `/libs/workflow/scripts/sample.ecma`

* **Nyttolast**: Ingen
* **Argument**: Inga
* **Timeout**: Ignorerad

### LockProcess {#lockprocess}

Låser arbetsflödets nyttolast.

* **Java™-klass:** `com.day.cq.workflow.impl.process.LockProcess`

* **Nyttolast:** JCR_PATH och JCR_UID
* **Argument:** Ingen
* **Timeout:** ignoreras

Stegen har ingen effekt under följande omständigheter:

* Nyttolasten är redan låst
* Noden nyttload innehåller inte en underordnad jcr:content-nod

### UnlockProcess {#unlockprocess}

Låser upp arbetsflödets nyttolast.

* **Java™-klass:** `com.day.cq.workflow.impl.process.UnlockProcess`

* **Nyttolast:** JCR_PATH och JCR_UID
* **Argument:** Ingen
* **Timeout:** ignoreras

Stegen har ingen effekt under följande omständigheter:

* Nyttolasten är redan upplåst
* Noden nyttload innehåller inte en underordnad jcr:content-nod

## Versionshantering {#versioning-processes}

Följande process utför en versionsrelaterad uppgift.

### CreateVersionProcess {#createversionprocess}

Skapar en version av arbetsflödets nyttolast (AEM-sida eller DAM-resurs).

* **Java™-klass**: `com.day.cq.wcm.workflow.process.CreateVersionProcess`

* **Nyttolast**: En JCR-sökväg eller UUID som refererar till en sida eller en DAM-resurs
* **Argument**: Inga
* **Timeout**: Respekt
