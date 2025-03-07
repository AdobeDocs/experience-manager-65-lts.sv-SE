---
title: Hur skapar och arbetar man med hasharna i dynamiska PDF forms?
description: Generera och arbeta med hash i dynamiska PDF forms.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 3fa1b6c9-fe73-4d76-aa72-20ce3e502941
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1182'
ht-degree: 0%

---

# Generera och arbeta med hash-tabeller i dynamiska PDF forms {#generate-work-with-hashes-dynamic-pdf-forms}

## Nödvändig kunskap {#prerequisite-knowledge}

Det krävs viss erfarenhet av AEM Forms på JEE Designer, liksom möjligheten att komma åt och anropa funktioner i skriptobjekt.

## Användarnivå {#user-level}

Början

När du vill dölja ett lösenord i ditt PDF-formulär och inte vill ha det i klartext inuti källkoden eller någon annanstans i PDF-dokumentet, är det viktigt att du vet hur man skapar och arbetar med hash-koderna MD4, MD5, SHA-1 och SHA-256.

Tanken är att dölja lösenordet genom att generera en unik hash och lagra hash-koden i PDF-dokumentet. Den här unika hashen kan genereras av olika hash-funktioner, och i den här artikeln visas hur du genererar dem i PDF-formuläret och hur du arbetar med dem.

En hash-funktion tar en lång sträng (eller ett meddelande) av valfri längd som indata och skapar en sträng med fast längd som utdata, vilket ibland kallas meddelandesammandrag eller ett digitalt fingeravtryck.

Med AEM Forms på JEE Designer kan du implementera de olika hash-funktionerna i skriptobjekt som JavaScript och köra dem i ett dynamiskt PDF-dokument. Exemplet på PDF-filer som ingår i exempelfilerna för den här artikeln använder implementeringar med öppen källkod för följande hash-funktioner:

* MD4 och MD5 - designade av Ronald Rivest

* SHA-1 och SHA-256 - enligt definition av NIST

Den största fördelen med hash-kodning är att du inte behöver jämföra lösenord direkt genom att jämföra tydliga textsträngar. Du kan i stället jämföra de två hash-koderna för de två lösenorden. Eftersom det är osannolikt att två olika strängar har samma hash, kan du anta att de jämförda strängarna (i det här fallet lösenorden) också är identiska om båda hash-strängarna är identiska.

>[!NOTE]
>
>Det finns några välkända säkerhetsproblem (s.k. hash-kollisioner) med MD4 eller MD5. På grund av dessa hash-kollisioner och andra SHA-1-attacker (inklusive regnbågstabeller) bestämde jag mig för att koncentrera mig på SHA-256-hash-funktionen i det andra exemplet. Mer information finns på sidorna [Kollision](https://en.wikipedia.org/wiki/Hash_collision) och [Regnbågstabell](https://en.wikipedia.org/wiki/Rainbow_table) från Wikipedia.

## Undersöka skriptobjekten {#examining-script-objects}

När du öppnar ett av de två exempelbilderna i AEM Forms på JEE Designer hittar du de fyra skriptobjekten på paletten Hierarki (se figur nedan).

![Variabler](assets/variables.jpg)

Om du vill se hur JavaScript implementerar hash-funktionerna i dessa skriptobjekt markerar du skriptobjektet och utforskar koden i skriptredigeraren. Du kan se hur följande hash-funktioner har implementerats:

* soHASHING_MD4.hex_md4()
* soHASHING_MD4.b64_md4()
* soHASHING_MD4.str_md4()
* soHASHING_MD5.hex_md5()
* soHASHING_MD5.b64_md5()
* soHASHING_MD5.str_md5()
* soHASHING_SHA1.hex_sha1()
* soHASHING_SHA1.b64_sha1( )
* soHASHING_SHA1.str_sha1( )
* soHASHING_SHA256.hex_sha256()
* soHASHING_SHA256.b64_sha256()
* soHASHING_SHA256.str_sha256()

Som du kan se i den här listan finns det olika funktioner tillgängliga för de olika utdatatyperna för hashen. Du kan välja mellan `hex_` för hexadecimala siffror, `b64_` för Base64-kodade utdata eller `str_` för enkel strängkodning.

Beroende på vilken hash-funktion du väljer varierar längden på hashen:

* MD4: 128 bitar
* MD5: 128 bitar
* SHA-1: 160 bitar
* SHA-256: 256 bitar

## Prova PDF forms {#try-sample-pdf-forms}

Exempelfilerna för den här artikeln innehåller två PDF forms. I det första exemplet kan du skriva in en sträng och sedan generera hash-värden för strängen MD4, MD5, SHA-1 och SHA-256. Det andra exemplet är ett enkelt formulär som låser upp textfält om rätt lösenord anges.

### Exempel 1: generera hash-värden {#generating-dashes}

Prova det första exemplet genom att följa stegen nedan:

1. När du har hämtat och packat upp exempelfilerna öppnar du hashing_forms_sample1.pdf med AEM Forms på JEE Designer. Du kan också använda Adobe Reader eller Adobe Acrobat Professional för att öppna och visa exemplet, men du kan inte se källkoden.
1. I textfältet med etiketten [!UICONTROL clear text] skriver du ett lösenord eller något annat meddelande som du vill ska hashas.
1. Klicka på någon av de fyra knapparna för att generera hash-taggen MD4, MD5, SHA-1 eller SHA-256. Beroende på vilken knapp du tryckte på anropas en av de fyra hash-funktionerna som skapar hexadecimala utdata och strängen eller meddelandet hashas.

Resultatet av hash-åtgärden visas i fältet [!UICONTROL hash]. Hash-längden varierar beroende på vilken hash-funktion du väljer.

Alla samplingar använder hexadecimala siffror som utdatatyp. Du kan använda skriptredigeraren för att ändra exemplen och ändra utdatatypen till Base64 eller simple String.

### Exempel 2: matcha lösenord {#matching-passwords}

Det andra exemplet visar hur hash-värden jämförs i bakgrunden utan att det riktiga lösenordet behöver avslöjas. Lösenordet du anger är hashas. Det riktiga lösenordet, som lagras i ett osynligt fält, hashas också. Lösenordet är säkert, inte för att det är osynligt, utan för att det har hashats. Eftersom det är omöjligt att rekonstruera lösenordet från det hash-kodade värdet är det säkert att visa lösenordet i hash-kodad form. Jämförelsen görs bara mellan hasharna, inte mellan lösenorden i klartext. Om båda hash-koderna är desamma kan du anta att lösenorden är identiska.

Prova det andra exemplet genom att följa stegen nedan:

1. Öppna `hashing_forms_sample2.pdf` med AEM Forms på JEE Designer. Du kan också använda Adobe Reader eller Adobe Acrobat Professional för att öppna och visa exemplet, men du kan inte se källkoden.
1. Välj ett av de två lösenordsfälten [!UICONTROL Password MAN] eller [!UICONTROL Password WOMAN] och skriv in lösenorden:
   1. Lösenordet för mannen är `bob`
   1. Lösenordet för kvinnan är `alice`
1. När du flyttar bort fokus från lösenordsfälten eller trycker på Retur genereras hash-värdet för lösenordet som du har angett automatiskt och jämförs med den lagrade hash-koden för det korrekta lösenordet i bakgrunden. De korrekta, hashas-lösenorden lagras i de osynliga textfälten `passwd_man_hashed` och `passwd_woman_hashed`. Om du anger rätt lösenord för mannen görs textfälten `Man 1` och `Man 2` tillgängliga så att du kan skriva text i dem. Samma beteende gäller för kvinnans fält.
1. Du kan också klicka på knappen med namnet&quot;delete passwords&quot;, som inaktiverar textfälten och ändrar deras kantlinje.

Koden för att jämföra de två hash-kodade värdena och aktivera textfälten är okomplicerad:

```xml
if (soHASHING_SHA256.hex_sha256(this.rawValue) == passwd_man_hashed.rawValue){
     VAL_man_1.access = "open";
     VAL_man_2.access = "open";
     VAL_man_1.borderColor = "0,255,0";
     VAL_man_2.borderColor = "0,255,0";
}
```

## Vart ska du gå härifrån? {#next-steps}

Var behöver du något sådant här? Överväg ett PDF-formulär som innehåller fält som endast ska fyllas i av behöriga personer. Genom att skydda fälten med ett lösenord, som inte kan visas i klartext någonstans i dokumentet som i Sample_2.pdf, kan du se till att dessa fält bara är tillgängliga för användare som känner till lösenordet.

Jag rekommenderar att du fortsätter utforska de två exempelfilerna för PDF.  Du kan generera nya hash-värden med Sample_1.pdf och använda de genererade värdena för att ändra antingen lösenordet eller hash-funktionen som används i Sample_2.pdf.  Resurserna som listas i avsnittet Attribut innehåller även ytterligare information om hashning och de specifika JavaScript-implementeringar som används i den här artikeln.

## Attribut {#attributions}

* [Ronald Rivest](https://en.wikipedia.org/wiki/Ron_Rivest)
* [NIST](https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines)
* [Hash-kollision](https://en.wikipedia.org/wiki/Hash_collision)
* [Regnbågstabell](https://en.wikipedia.org/wiki/Rainbow_table)
* [JavaScript MD5 - startsida för projekt](https://pajhome.org.uk/crypt/md5/)
* [jsSHA2-projektstartsida](https://anmar.eu.org/projects/jssha2/)
