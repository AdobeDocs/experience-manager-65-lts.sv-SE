---
title: Fördefinierade rapporter i processrapportering
description: Fråga efter AEM Forms om JEE-processdata om du vill skapa rapporter om långvariga processer, processens varaktighet och arbetsflödets volym
content-type: reference
topic-tags: process-reporting
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 3bf65798-a8ce-4864-9d77-952bb8d8da43
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 0%

---

# Fördefinierade rapporter i processrapportering {#pre-defined-reports-in-process-reporting}

## Fördefinierade rapporter i processrapportering {#pre-defined-reports-in-process-reporting-1}

AEM Forms Process Reporting levereras med följande *färdiga*-rapporter:

* **[Långa processer](#long-running-processes)**: En rapport över alla AEM Forms-processer som tog mer än en angiven tid att slutföra
* **[Processvaraktighetsdiagram](#process-duration-report)**: En rapport över en angiven AEM Forms-process utifrån varaktighet
* **[Arbetsflödesvolym](#workflow-volume-report)**: En rapport över pågående och slutförda instanser av angiven process per datum

## Långa processer {#long-running-processes}

Rapporten Långa processer visar de AEM Forms-processer som tagit mer än en angiven tid att slutföra.

### Så här kör du en rapport över en process som körs länge {#to-execute-a-long-running-process-report}

1. Klicka på noden **Rapporter** i trädvyn **Processrapportering** om du vill visa listan med fördefinierade rapporter i processrapportering.
1. Klicka på rapportnoden **Långa processer**.

   ![long_running_node](assets/long_running_node.png)

   När du väljer en rapport visas panelen **Rapportparametrar** till höger om trädvyn.

   ![lång körningsprocess, rapportparameterpanel](assets/report_parameters_panel.png)

   Parametrar:

   * **Varaktighet** (*obligatorisk*): Ange varaktighet och tidsenhet. Visa alla AEM Forms-processer som har körts under mer än den angivna varaktigheten.
   * **Startades efter** (*valfritt*): Välj ett datum. Filtrera rapporten så att processinstanser som har startats efter det angivna datumet visas.
   * **Startades före** (*valfritt*): Välj ett datum. Filtrera rapporten så att processinstanser som startats före det angivna datumet visas.

1. Klicka på **Gå** för att köra rapporten.

   Rapporten visas på panelen **Rapport** till höger om fönstret **Processrapportering**.

   ![long_running_processes](assets/long_running_processes.png)

   Använd alternativen i det övre högra hörnet av panelen **Rapport** om du vill utföra följande åtgärder för rapporten.

   * **Uppdatera**: Uppdaterar rapporten med de senaste data som finns i lagringen
   * **Ändra förklaringsfärg**: Markera och ändra färgen på rapportförklaringen
   * **Exportera till CSV**: Exportera och hämta data från rapporten till en kommaavgränsad fil

## Processvaraktighet - rapport  {#process-duration-report}

Processvaraktighetsrapporten visar antalet instanser av en Forms-process i antal dagar som varje instans har körts.

### Så här kör du en rapport för processvaraktighet {#to-execute-a-process-duration-report}

1. Om du vill visa de fördefinierade rapporterna i processrapportering klickar du på noden **Rapporter** i trädvyn **Processrapportering**.
1. Klicka på rapportnoden **Bearbetar varaktighet**.

   ![process_duration_node](assets/process_duration_node.png)

   När du väljer en rapport visas panelen **Rapportparametrar** till höger om trädvyn.

   ![lång körningsprocess, rapportparameterpanel](assets/process_duration_params.png)

   Parametrar:

   * **Välj process** (*obligatoriskt*): Välj en AEM Forms-process.

1. Klicka på **Gå** för att köra rapporten.

   Rapporten visas på panelen **Rapport** till höger om fönstret Processrapportering.

   ![process_duration_report](assets/process_duration_report.png)

   Använd alternativen i det övre högra hörnet av panelen **Rapport** om du vill utföra följande åtgärder för rapporten.

   * **Uppdatera**: Uppdaterar rapporten med de senaste data som finns i lagringen
   * **Ändra förklaringsfärg**: Markera och ändra färgen på rapportförklaringen
   * **Exportera till CSV**: Exportera och hämta data från rapporten till en kommaavgränsad fil

## Rapport över arbetsflödesvolym {#workflow-volume-report}

Volymrapporten Arbetsflöde visar antalet pågående och slutförda instanser av en AEM Forms-process per kalenderdag.

### Så här kör du en arbetsflödesvolymrapport {#to-execute-a-workflow-volume-report}

1. Om du vill visa de fördefinierade rapporterna i processrapportering klickar du på noden **Rapporter** i trädvyn **Processrapportering**.
1. Klicka på rapportnoden **Arbetsflödesvolym**.

   ![workflow_volume_node](assets/workflow_volume_node.png)

   När du väljer en rapport visas panelen **Rapportparametrar** till höger om trädvyn.

   ![lång körningsprocess, rapportparameterpanel](assets/workflow_volume_params.png)

   Parametrar:

   * **Välj process** (*obligatoriskt*): Välj en AEM Forms-process.

   * **Startades efter** (*valfritt*): Välj ett datum. Filtrerar rapporten så att processinstanser som startats efter det angivna datumet visas.

   * **Startades före** (*valfritt*): Välj ett datum. Filtrerar rapporten så att processinstanser som påbörjats före det angivna datumet visas.

1. Klicka på **Gå** för att köra rapporten.

   Rapporten visas på panelen **Rapport** till höger om fönstret **Processrapportering**.

   ![workflow_volume_report](assets/workflow_volume_report.png)

   Använd alternativen i det övre högra hörnet av panelen **Rapport** om du vill utföra följande åtgärder för rapporten.

   * **Uppdatera**: Uppdaterar rapporten med de senaste data som finns i lagringen
   * **Ändra förklaringsfärg**: Markera och ändra färgen på rapportförklaringen
   * **Exportera till CSV**: Exportera och hämta data från rapporten till en kommaavgränsad fil
