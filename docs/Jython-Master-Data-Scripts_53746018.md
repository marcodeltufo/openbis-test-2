::: {#page}
::: {#main .aui-page-panel}
::: {#main-header}
::: {#breadcrumb-section}
1.  [openBIS Documentation Rel. 20.10](index.html)
:::

[ openBIS Documentation Rel. 20.10 : Jython Master Data Scripts ]{#title-text} {#title-heading .pagetitle}
==============================================================================
:::

::: {#content .view}
::: {.page-metadata}
Created by [ Fuentes Serna Juan Mariano (ID)]{.author}, last modified by
[ Kovtun Viktor (ID)]{.editor} on Mar 10, 2023
:::

::: {#main-content .wiki-content .group}
::: {.toc-macro .rbtoc1678781405410}
-   [Introduction](#JythonMasterDataScripts-Introduction)
    -   [API Basics](#JythonMasterDataScripts-APIBasics)
    -   [Simple example](#JythonMasterDataScripts-Simpleexample)
    -   [Command line tools](#JythonMasterDataScripts-Commandlinetools)
        -   [Executing master data
            scripts](#JythonMasterDataScripts-Executingmasterdatascripts)
        -   [Exporting master
            data](#JythonMasterDataScripts-Exportingmasterdata)
:::

Introduction {#JythonMasterDataScripts-Introduction}
============

openBIS defines as \"Master data\" all metadata configurations needed
before the import of the real raw data. Master data includes
experiment/sample/data set/property/file types, vocabularies and
property assignments.

API Basics {#JythonMasterDataScripts-APIBasics}
----------

Similarly to the [Jython Dropbox
API](https://unlimited.ethz.ch/pages/viewpage.action?pageId=53746029)
the script can access a global variable named `service`, which can be
used to create transactions.

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre syntaxhighlighter-params="brush: python; gutter: false; theme: Confluence" theme="Confluence"}
transaction = service.transaction()
```
:::
:::

The transactions are a focal API concept offering to create new types
(e.g. `createNewSampleType`, `createNewDataSetType`) and new property
assignments (e.g. `assignPropertyType`).

The complete Javadoc for the API is available at

::: {.table-wrap}
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------
  [IMasterDataRegistrationService](https://openbis.ch/javadoc/20.10.x/javadoc-openbis/ch/systemsx/cisd/openbis/generic/server/jython/api/v1/IMasterDataRegistrationService.html){.external-link}           The global service variable
  [IMasterDataRegistrationTransaction](https://openbis.ch/javadoc/20.10.x/javadoc-openbis/ch/systemsx/cisd/openbis/generic/server/jython/api/v1/IMasterDataRegistrationTransaction.html){.external-link}   The public API of the transaction objects
  [All classes](https://openbis.ch/javadoc/20.10.x/javadoc-openbis/ch/systemsx/cisd/openbis/generic/server/jython/api/v1/package-summary.html){.external-link}                                             Javadocs for the complete API package
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------
:::

Simple example {#JythonMasterDataScripts-Simpleexample}
--------------

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre syntaxhighlighter-params="brush: python; gutter: false; theme: Confluence" theme="Confluence"}
import ch.systemsx.cisd.openbis.generic.client.jython.api.v1.DataType as DataType

tr = service.transaction()

expType = tr.createNewExperimentType('EXPERIMENT-TYPE')
expType.setDescription('Experiment type description.')

sampleType = tr.createNewSampleType('SAMPLE-TYPE')
sampleType.setDescription('Sample type description.')
sampleType.setSubcodeUnique(True)
sampleType.setAutoGeneratedCode(True)
sampleType.setGeneratedCodePrefix("G_");

dataSetType = tr.createNewDataSetType('DATA-SET-TYPE')
dataSetType.setContainerType(True)
dataSetType.setDescription('Data set type description.')

materialType = tr.createNewMaterialType('MATERIAL-TYPE')
materialType.setDescription('Material type description.')

stringPropertyType = tr.createNewPropertyType('VARCHAR-PROPERTY-TYPE', DataType.VARCHAR)
stringPropertyType.setDescription('Varchar property type description.')
stringPropertyType.setLabel('STRING')

materialPropertyType = tr.createNewPropertyType('MATERIAL-PROPERTY-TYPE', DataType.MATERIAL)
materialPropertyType.setDescription('Material property type description.')
materialPropertyType.setLabel('MATERIAL')
materialPropertyType.setMaterialType(materialType)
materialPropertyType.setManagedInternally(False)

# assigns the newly created property 'MATERIAL-PROPERTY-TYPE'
# as a mandatory property for 'SAMPLE-TYPE'
materialAssignment = tr.assignPropertyType(sampleType, materialPropertyType)
materialAssignment.setMandatory(True)

# assigns the newly created property 'VARCHAR-PROPERTY-TYPE'
# as an optional property for 'EXPERIMENT-TYPE' with default value 'FOO_BAR'
stringAssignement = tr.assignPropertyType(expType, stringPropertyType)
stringAssignement.setMandatory(False)
stringAssignement.setDefaultValue('FOO_BAR')
```
:::
:::

Command line tools {#JythonMasterDataScripts-Commandlinetools}
------------------

### Executing master data scripts {#JythonMasterDataScripts-Executingmasterdatascripts}

Make sure openBIS AS is up and running prior script execution. Go to the
openBIS AS installation folder. Assuming your script is
`/local/master-data-script.py` and openBIS AS is started on the URL
`     http://localhost:8888/openbis   ` execute the command

::: {.preformatted .panel style="border-width: 1px;"}
::: {.preformattedContent .panelContent}
    > cd /local0/openbis/servers/openBIS-server/jetty/bin
    > /register-master-data.sh -s http://localhost:8888/openbis/openbis -f /local/master-data-script.py
:::
:::

You will be prompted for username/password before the script execution.
Please note that the second \'openbis\' is needed in the server address,
so that you connect via the API.

### Exporting master data {#JythonMasterDataScripts-Exportingmasterdata}

You can export the master data from a running openBIS system as script
by running the command

::: {.preformatted .panel style="border-width: 1px;"}
::: {.preformattedContent .panelContent}
    > cd /local0/openbis/servers/openBIS-server/jetty/bin
    > /export-master-data.sh -s http://localhost:8888/openbis/openbis
:::
:::

This command will create a folder `exported-master-data-DATE` which will
contain the exported master data script - `master-data.py`
:::
:::
:::

::: {#footer role="contentinfo"}
::: {.section .footer-body}
Document generated by Confluence on Mar 14, 2023 09:10

::: {#footer-logo}
[Atlassian](https://www.atlassian.com/)
:::
:::
:::
:::