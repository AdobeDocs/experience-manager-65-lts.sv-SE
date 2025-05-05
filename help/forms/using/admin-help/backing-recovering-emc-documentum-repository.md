---
title: Säkerhetskopiera och återställa EMC Documentum-databasen
description: I det här dokumentet beskrivs de uppgifter som krävs för att säkerhetskopiera och återställa EMC Documentum-databasen som konfigurerats för din AEM-formulärmiljö.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 054d31c3-bd58-4596-8c06-4909d75e9569
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# Säkerhetskopiera och återställa EMC Documentum-databasen {#backing-up-and-recovering-the-emc-documentum-repository}

I det här avsnittet beskrivs de uppgifter som krävs för att säkerhetskopiera och återställa EMC Documentum-databasen som konfigurerats för din AEM-formulärmiljö.

>[!NOTE]
>
>I dessa anvisningar förutsätts att AEM-formulär med kopplingar för ECM och EMC Documentum Content Server installeras och konfigureras efter behov.

För både säkerhetskopiering och återställning finns det två huvudsakliga uppgifter:

* Säkerhetskopiera (eller återställa) AEM-formulärmiljön.
* Säkerhetskopiera (eller återställa) EMC Documentum Content Server.

>[!NOTE]
>
>Säkerhetskopiera dina AEM-formulärdata innan du säkerhetskopierar EMC Documentum-systemet och återställer sedan EMC Documentum-systemet innan du återställer AEM formulärmiljö.

## Programvarukrav {#software-requirements}

Om du vill utföra de nödvändiga säkerhetskopieringsåtgärderna på EMC Documentum Content Server köper du ett tredjepartsverktyg som EMC NetWorker från EMC eller CYA SmartRecovery för EMC Documentum från CYA. Följande instruktioner beskriver hur du använder EMC NetWorker-modulen version 7.2.2.

Du behöver följande EMC NetWorker-moduler:

* NetWorker-modul
* Konfigurationsguiden för NetWorker
* Konfigurationsguiden för NetWorker-enhet
* NetWorker-modul för den databastyp som används av Content Server
* NetWorker-modul för Documentum

## Förbereda EMC Document Content Server för säkerhetskopiering och återställning {#preparing-the-emc-document-content-server-for-backup-and-recovery}

I det här avsnittet beskrivs hur du installerar och konfigurerar EMC NetWorker-programmet på innehållsservern.

**Förbered EMC Documentum-servern för säkerhetskopiering**

1. Installera EMC NetWorker-modulerna på EMC Documentum Content Server och acceptera alla standardvärden.

   Under installationsprocesserna uppmanas du att ange servernamnet för innehållsserverdatorn som *NetWorker-serverns namn*. När du installerar EMC NetWorker-modulen för din databas väljer du en fullständig installation.

1. Med exempelinnehållet nedan skapar du en konfigurationsfil med namnet *nsrnmd_win.cfg* och sparar den på en tillgänglig plats på innehållsservern. Filen anropas av kommandona för säkerhetskopiering och återställning.

   Följande text innehåller formateringstecken för radbrytningar. Om du kopierar den här texten till en plats utanför det här dokumentet kopierar du en del i taget och tar bort formateringstecknen när du klistrar in den på den nya platsen.

   ```shell
    ################################################
    # NetWorker Module for Documentum v1.2 nsrnmd_win.cfg D5.3+ example with
    # typical set of working parameters.  THIS FILE MUST BE SITE-CUSTOMISED.
    #
    # Parameters not shown can be set in this file (as per site customisation) #or from the command-line.
    #
    # See the user Guides for details on all parameters, including
    # those not listed below.
    # Note: DCTM environment for D6 is slightly different from D5, refer to D6
    # Installation Guide to update the values.
    ################################################
    #Can get values for most of below from doing as the dctm inst owner: cmd> set DOCUMENTUM=C:\Documentum
    DM_HOME=C:\Documentum\product\6.0
    JAVA_HOME=C:\Progra~1\Documentum\java\1.5.0_12
    JAVA_PATH=C:\Progra~1\Documentum\java\1.5.0_12\bin
    PATH=C:\WINNT\system32;C:\WINNT;C:\WINNT\system32\WBEM;C:\Documentum\product\6.0\bin;C:\Documentum\fulltext\fast;C:\Documentum\product\6.0\fusion;C:\Program Files\Documentum\Shared;C:\Program Files\Legato\nsr\bin;C:\Program Files\Microsoft SQL Server\80\Tools\Binn;C:\Program Files\Microsoft SQL Server\90\DTS\Binn\;C:\Program Files\Microsoft SQL Server\90\Tools\binn;C:\Program Files\Microsoft SQL Server\90\Tools\Binn\VSShell\Common7\IDE;C:\Program Files\Documentum\java\1.5.0_12\bin;C:\Documentum\config;C:\Documentum
    #See also manifest dctm.jar file for class path locations
    CLASSPATH=.;C:\Program Files\Legato\nsr\bin;C:\Program Files\Legato\nsr\bin\nsrnmdde.jar;C:\Program Files\Documentum\java\1.5.0_12\lib\tools.jar;C:\Program Files\Documentum\Shared\dfc.jar;C:\Program Files\Documentum\Shared\aspectjrt.jar;C:\Program Files\Documentum\dctm.jar;C:\Program Files\Documentum\Shared\workflow.jar;C:\Program Files\Documentum\Shared\log4j.jar;C:\Program Files\Documentum\java\1.5.0_12\lib\dt.jar;C:\Documentum\config
   
    ################################################
    #If not using nsrnmdsv -m ALL|DB|DB_LOG|FTI|FTI_ALL|ICF|SA|SA_ALL, set #for backup
    NMD_SCOPE=ALL
    #Mandatory when scope (backup or restore) is FTI/SA without -a option
    #NMD_OBJECT_NAME=filestore_01
    ################################################
    NMDDE_DM_DOCBASE=Docbase
    NMDDE_DM_USER=Administrator
    #NMDDE_DM_PASSWD must be set via running: nsrnmdsv -f <nmdcfg> -P <pwd>.
    ################################################
    #DB related hooks to invoke arbitrary scripts:
    #Set if DB is on a remote host
    #NMD_DB_HOST=dbhost
    #Pure basename implies remote host execution; absolute path ... local
    #execution as in NMD v1.0.
    #
    #Remote execution requires script be put in remote nsrexecd bin directory
    #and D5.3+ host be added to remote nsr\res\servers file w/ nsrexecd recycled
    #
    #Refer to user Guides for sample script code.
    NMD_DB_FULL_BACKUP_CMD=C:\DocumentumBackup\Scripts\nsrnmddbf.bat
    NMD_DB_LOG_BACKUP_CMD=C:\DocumentumBackup\Scripts\nsrnmddbl.bat
    NMD_DB_INCR_BACKUP_CMD=C:\DocumentumBackup\Scripts\nsrnmddbi.bat
    ################################################
    #For D5.3+ only: NMD_FTI_INCLUDED, NMD_FTI_HOST, NMD_FTI_USER,
    #NMD_FTI_PASSWD & NMD_FTI_SUBDIRS.
    #Optional for remote D5.3+ FTI server
    NMD_FTI_INCLUDED=no
    #NMD_FTI_HOST=ftihost
    #Recommended for D5.3+ FTI server quiesce/unquiesce
    #NMD_FTI_USER=ftiadmin
    #The index name: optional for D5.3+ FTI server, used with -M FTI_ALL or
    #-M ALL
    #NMD_FTI_NAME=rep_name_ftindex_01
    #Recommended for D5.3+ FTI server quiesce/unquiesce
    #NMDDE_FTI_PASSWD must be set via running: nsrnmdsv -f <nmdcfg> -P <pwd>
    #-M FTI.
    #Pure basename implies remote host execution; absolute path ... local execution
    #as in NMD v1.0.
    #
    #Remote execution requires script be put in remote nsrexecd bin directory
    #and D5.3+ host be added to remote nsr\res\servers file w/ nsrexecd
    #recycled.
    #
    #See example nsrnmdfti*.bat examples.
    #
    #Mandatory for D5.3+
    #NMD_BACKUP_FTI_QUIESCE=nsrnmdftiq.bat
    #NMD_BACKUP_FTI_UNQUIESCE=nsrnmdftiu.bat
    #Used for D5.3+ to get InstallProfile.xml FTI file in multinode
    #discovery.
    #Optional, if not specified, will treat as single-node FTI.
    #NMD_FTI_GET_INSTPROF=nsrnmdfti_instprof.bat
    #Mandatory for D5.3+. No spaces in paths or around comma separators.
    #For remote FTI, paths must be valid at the FTI host.
    #NMD_FTI_SUBDIRS=F:\Documentum_idx\data\fulltext\fixml,F:\Documentum_idx
    #\data\fulltext\index
    ################################################
    #Mandatory. No spaces in paths or before comma
    #separators in NMD_ICF_SUBDIRS_xxx:
    #NMD_ICF_INCLUDED=yes
    #NMD_ICF_SUBDIRS=C:\Documentum\dba,C:\Documentum\product\5.3
    ################################################
    NMD_FILEREPORT_INCLUDED=yes
    NMDDE_METADATA_OUTPUT_DEST=C:\docbase_freports\
    ################################################
    #Other misc recommended NMD_xxx parameters
    #Recommended to get more meaningful saveset names
    NMD_USE_DEFAULT_SAVESET_NAMES=yes
    #Use following to skip unwanted ICF, FTI and non-SnapImage SA dirs/files.
    #For example, "<</>> +skip: dm_ftwork_dir" line will skip non-data
    #files.
    #
    #The path will be the same and must exist on D5.3+, remote FTI host, and
    #RCS hosts correspondingly if used.
    #NMD_DIRECTIVES_FILE=E:\Program Files\Legato\nsr\res\nsrnmddirectives.txt
    #For non-SnapImage SA backup
    #NMD_SA_FULL_NUM_SAVESET=16
    #NMD_SA_INCR_NUM_SAVESET=4
    #NMD_USE_SNAPIMAGE=yes
    ################################################
    # DSA setup
    ################################################
    # Name of the config file at the remote sites;
    # Mandatory, listed in the config file at the primary host.
    # (if skipped, backup is treated as local)
    # NMD_RCS_CFG_FILE=rep_name_rcs.cfg
   
    # SA-host mapping add, optional, will override far-store list info.
    # No space around comma.
    # NMD_HOST_SAS_MAP=host01,sa_01,sa_02
    # NMD_HOST_SAS_MAP=host02,sa_03
    ################################################
    NSR_SERVER=con-dctm
    #NSR_CLIENT=d5svrhost
    NSR_GROUP=Default
    NSR_DATA_VOLUME_POOL=Default
    #NSR_SNAPIMAGE_DATA_VOLUME_POOL=Default
    #Relocation dir will be the same on D5+ and remote FTI/SA hosts.
    NSR_RELOCATION=C:\reloc
    #NSR_PARALLELISM=16
    NSR_DEBUG_FILE=C:\Program Files\Legato\nsr\applogs\nmd.log
    NSR_DEBUG_LEVEL=9
    ################################################
    NMDDE_DM_PASSWD=XAtup9pl
   ```

   Behåll konfigurationsfilens lösenordsfält `NMDDE_DM_PASSWD` tomt. Du anger lösenordet i nästa steg.

1. Ange lösenordet för konfigurationsfilen enligt följande:

   * Öppna en kommandotolk och ändra till `[NetWorker_root]\Legato\nsr\bin`.
   * Kör följande kommando: `-nsrnmdsv.exe -f`*&lt;sökväg_till_cfg_fil> -P &lt;lösenord>*

1. Skapa de körbara batchfiler (.bat) som används för att säkerhetskopiera databasen. (Se dokumentationen för NetWorker.) Ange information i gruppfilerna enligt installationen.

   * Fullständig säkerhetskopiering av databasen (nsrnmddbf.bat):

     `NetWorker_database_module_root` `-s`*&lt;NetWorker_Server_Name>* `-U` `[username]` `-P`*[password ]*`-l full`*&lt;database_name>*

   * Inkrementell säkerhetskopiering av databas (nsrnmddbi.bat):

     `[NetWorker_database_module_root]` `-s`*&lt;NetWorker_Server_Name>* `-U` `[username]` `-P` `[password]` `-l 1 -R`*&lt;database_name>*

   * Säkerhetskopiering av databaslogg (nsrnmddbl.bat):

     `[NetWorker_database_module_root]` `-s` `<NetWorker_Server_Name>` `-U` `[username]` `-P` `[password]` `-l incr -R`*&lt;database_name>*

     Var:

     `[NetWorker_database_module_root]` är installationskatalogen för NetWorker-modulen. Standardinstallationskatalogen för NetWorker-modulen för SQL Server är till exempel C:\Program Files\Legato\nsr\bin\nsrsqlsv.

     `NetWorker_Server_Name` är den server som NetWorker är installerat på.

     `username` &amp; `password` är användarnamn och lösenord för databasadministratörsanvändaren.

     `database_name` är namnet på databasen som ska säkerhetskopieras.

**Skapa en säkerhetskopieringsenhet**

1. Skapa en katalog på EMC Documentum-servern och dela mappen genom att ge fullständig behörighet till alla användare.
1. Starta EMC NetWorker-administratören och klicka på Mediehantering > Enheter.
1. Högerklicka på Enheter och välj Skapa.
1. Ange följande värden och klicka på OK:

   **Namn:** Den fullständiga sökvägen till den delade katalogen

   **Medietyp:** `File`

1. Högerklicka på den nya enheten och välj Åtgärder.
1. Klicka på Etikett, ange ett namn, klicka på OK och sedan på Montera.

En enhet läggs till som de säkerhetskopierade filerna sparas i. Du kan lägga till flera enheter med olika format.

## Säkerhetskopiera EMC Documentum Content Server {#back-up-the-emc-documentum-content-server}

Utför följande uppgifter när du har slutfört en fullständig säkerhetskopiering av dina AEM-formulärdata. (Se [Säkerhetskopiera AEM-formulärdata](/help/forms/using/admin-help/backing-aem-forms-data.md#backing-up-the-aem-forms-data).)

>[!NOTE]
>
>Kommandoskripten kräver den fullständiga sökvägen till filen nsrnmd_win.cfg som du skapade i [Förbereda EMC Document Content Server för säkerhetskopiering och återställning](backing-recovering-emc-documentum-repository.md#preparing-the-emc-document-content-server-for-backup-and-recovery).

1. Öppna en kommandotolk och ändra till `[NetWorker_root]\Legato\nsr\bin`.
1. Kör följande kommando:

   ```shell
    - nsrnmdsv.exe -f <path_to_cfg_file>
   ```

## Återställ EMC Documentum Content Server {#restore-the-emc-documentum-content-server}

Utför följande åtgärder innan du återställer AEM-formulärdata. (Se [Återställa AEM-formulärdata](/help/forms/using/admin-help/recovering-aem-forms-data.md#recovering-the-aem-forms-data).)

>[!NOTE]
>
>Kommandoskripten kräver den fullständiga sökvägen till filen nsrnmd_win.cfg som du skapade i [Förbereda EMC Document Content Server för säkerhetskopiering och återställning](backing-recovering-emc-documentum-repository.md#preparing-the-emc-document-content-server-for-backup-and-recovery).

1. Stoppa den Docbase-tjänst som du återställer.
1. Starta NetWorker-användarverktyget för din databasmodul (till exempel *NetWorker-användare för SQL Server*).
1. Klicka på återställningsverktyget och välj sedan Normal.
1. Till vänster på skärmen väljer du databasen för din Docbase och klickar på knappen Start i verktygsfältet.
1. Starta om Docbase-tjänsten när databasen har återställts.
1. Öppna en kommandotolk och ändra till *[NetWorker_root]*\Legato\nsr\bin
1. Kör följande kommando:

   ```shell
    - nsrnmdrs.exe -B <docbase_name> -f <path_to_cfg_file> -C SA
   ```
