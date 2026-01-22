---
title: Skriptkörning misslyckas på AEM Forms 6.5 LTS med JBoss EAP 8 (Linux)
description: om du vill konfigurera JBoss EAP 8.0 (AEM Forms 6.5.1 LTS) i en Linux-miljö kan du råka ut för vissa fel när du kör gränssnittsskript eller startfiler
solution: Experience Manager
feature: Deploying
role: User,Admin,Developer
hide: true
index: false
hidefromtoc: true
source-git-commit: 5d020671efaa4527a5f6dbb4b779c7a3351888a4
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---


# Skriptkörning misslyckas på AEM Forms 6.5 LTS med JBoss EAP 8 (Linux)

## Problem

När du konfigurerar **JBoss EAP 8.0 (AEM Forms 6.5.1 LTS)** i en **Linux** -miljö kan något av följande fel uppstå när du kör gränssnittsskript eller startfiler:

```text
/bin/sh^M: bad interpreter
$'\r': command not found
```

Dessa fel inträffar när gränssnittsskript eller konfigurationsfiler skapas eller redigeras på ett **Windows** -system och innehåller **CRLF (Careting Return + Line Feed)** -radslut.
Linux-system stöder endast **LF-radslut (Line Feed)** och radslut i Windows-stil orsakar körningsfel för skript.

## Gäller för

* **JBoss EAP 8.0**
* **Linux/UNIX-baserade operativsystem**

## Felsökningssteg

1. **Identifiera den berörda filen**

   * Granska felutdata för att identifiera skriptet eller konfigurationsfilen `.sh` som orsakar felet.

2. **Konvertera filen till Unix-format**

   * Använd verktyget `dos2unix` för att konvertera radslut i Windows-stil till Unix-format:

     ```bash
     dos2unix <file_name>
     ```

   * Ersätt `<file_name>` med skriptet eller konfigurationsfilen som genererar felet.

3. **Upprepa om det behövs**

   * Om flera skript påverkas upprepar du konverteringen för alla relevanta `.sh`-filer (till exempel installationsskript, LCM- eller JBoss-startskript).

4. **Kör skriptet igen**

   * Efter konverteringen kör du skriptet igen för att bekräfta att problemet är löst.

När filerna har konverterats till LF-radslut (Unix) löses felen `/bin/sh^M` och `$'\r': command not found` och JBoss-skript körs korrekt i Linux.
