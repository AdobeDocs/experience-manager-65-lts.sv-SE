---
title: Spåra studsade e-postmeddelanden
description: När du skickar ett nyhetsbrev till många användare finns det vanligtvis några ogiltiga e-postadresser i listan. Skickar nyhetsbrev till adresserna som studsar tillbaka. AEM kan hantera dessa studsar och kan sluta skicka nyhetsbrev till dessa adresser när den konfigurerade studsräknaren har överskridits.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization
role: User
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 0%

---

# Spåra studsade e-postmeddelanden{#tracking-bounced-emails}

>[!NOTE]
>
>Adobe planerar inte att ytterligare förbättra spårningen av öppnade/studsade e-postmeddelanden som skickas av AEM SMTP-tjänst.
>
>Rekommendationen är att [använda Adobe Campaign och dess AEM-integrering](/help/sites-administering/campaign.md).

När du skickar ett nyhetsbrev till många användare finns det vanligtvis några ogiltiga e-postadresser i listan. Skickar nyhetsbrev till adresserna som studsar tillbaka. AEM kan hantera dessa studsar och kan sluta skicka nyhetsbrev till dessa adresser när den konfigurerade studsräknaren har överskridits. Som standard är studsfrekvensen 3, men den kan konfigureras.

Om du vill konfigurera AEM för att spåra studsade e-postmeddelanden, ställer du in AEM för att ringa en befintlig postlåda där studsade e-postmeddelanden tas emot. Vanligtvis är den här platsen den&quot;från&quot;-e-postadress som du anger var du skickar nyhetsbrevet. AEM avsöker den här inkorgen och importerar alla e-postmeddelanden under sökvägen som anges i avsökningskonfigurationen. Ett arbetsflöde utlöses sedan för att söka efter de studsade e-postadresserna inom användarna och uppdaterar användarens egenskapsvärde bounceCounter i enlighet med detta. När de konfigurerade maxgränserna har överskridits tas användaren bort från nyhetsbrevslistan.

## Konfigurera flödesimporteraren {#configuring-the-feed-importer}

Med flödesimporteraren kan du importera innehåll från externa källor till din databas flera gånger. Med den här konfigurationen av flödesimporteraren kontrollerar AEM avsändarens postlåda om det finns utskickade e-postmeddelanden.

Så här konfigurerar du feed-importeraren för spårning av studsade e-postmeddelanden:

1. Välj Feed-importeraren i **Verktyg**.

1. Klicka på **Lägg till** för att skapa en konfiguration.

   ![chlimage_1](assets/chlimage_1a.png)

1. Lägg till en konfiguration genom att välja typ och lägga till information i avsöknings-URL:en så att du kan konfigurera värden och port. Lägg dessutom till vissa post- och protokollspecifika parametrar i URL-frågan. Ange att konfigurationen ska avfrågas minst en gång om dagen.

   Alla konfigurationer behöver information om följande i avsöknings-URL:

   `username`: Användarnamnet som används för anslutning

   `password`: Lösenordet som används för anslutning

   Beroende på vilket protokoll du använder kan du dessutom konfigurera vissa inställningar.

   **POP3-konfigurationsegenskaper:**

   `pop3.leave.on.server`: Definierar om meddelanden ska lämnas på servern eller inte. Ange som true om du vill lämna meddelanden på servern, annars false. Standardvärdet är true.

   **Exempel på POP3:**

   | pop3s://pop.gmail.com:995/INBOX?username=user&amp;password=secret | Använda pop3 över SSL för att ansluta till GMail på port 995 med användare/hemlighet, och lämna meddelanden på servern som standard |
   |---|---|
   | pop3s://pop.gmail.com:995/INBOX?username=user&amp;password=secret&amp;pop3.leave.on.server=false | pop3s://pop.gmail.com:995/INBOX?username=user&amp;password=secret&amp;pop3.leave.on.server=false |

   **IMAP-konfigurationsegenskaper:**

   Gör att du kan ange flaggor att söka efter.

   `imap.flag.SEEN`:Ange false för nytt/ej visat meddelande, true för redan lästa meddelanden

   Se [https://javaee.github.io/javamail/docs/api/index.html?javax/mail/Flags.Flag.html](https://javaee.github.io/javamail/docs/api/index.html?javax/mail/Flags.Flag.html) för en fullständig lista över flaggor.

   **IMAP-exempel:**

   | imaps://imap.gmail.com:993/inbox?username=user&amp;password=secret | Använda IMAP över SSL för att ansluta till GMail på port 993 med användar/hemlighet. Hämtar endast nya meddelanden som standard. |
   |---|---|
   | imaps://imap.gmail.com:993/inbox?username=user&amp;password=secret&amp;imap.flag.SEEN=true | Använda IMAP över SSL för att ansluta till GMail 993 med användar-/hemlighet, men få bara ett meddelande som redan visas. |
   | imaps://imap.gmail.com:993/inbox?username=user&amp;password=secret&amp;imap.flag.SEEN=true&amp;imap.flag.SEEN=false | Använda IMAP över SSL för att ansluta till GMail 993 med användar-/hemlighet och få redan lästa ELLER nya meddelanden. |

1. Spara konfigurationen.

## Konfigurera tjänstkomponenten för nyhetsbrev {#configuring-the-newsletter-service-component}

När du har konfigurerat feed-importeraren konfigurerar du Från-adressen och studsräknaren.

Så här konfigurerar du nyhetsbrevstjänsten:

1. Gå till **MCM Newsletter** på OSGi-konsolen på `<host>:<port>/system/console/configMgr`.

1. Konfigurera tjänsten och spara ändringarna när du är klar.

   ![chlimage_1-1](assets/chlimage_1-1a.png)

   Följande konfigurationer kan ställas in för att justera beteendet:

   | Högsta studsräknare (max.bounce.count) | Definierar antalet studsar tills en användare utelämnas när ett nyhetsbrev skickas. Om du anger värdet 0 inaktiveras studskontrollen helt. |
   |---|---|
   | Activity No Cache (skickad.activity.nocache) | Definierar cacheinställningen som ska användas för aktiviteten för skickade nyhetsbrev |

   När nyhetsbrevet har sparats gör MCM-tjänsten följande:

   * Skriver en aktivitet till användarens dolda ström när ett nyhetsbrev har skickats.
   * Skriver en aktivitet om en studsa upptäcks och användarnas studsräknare ändras.
