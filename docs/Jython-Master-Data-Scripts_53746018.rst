.. container::
   :name: page

   .. container:: aui-page-panel
      :name: main

      .. container::
         :name: main-header

         .. container::
            :name: breadcrumb-section

            #. `openBIS Documentation Rel. 20.10 <index.html>`__

         .. rubric:: openBIS Documentation Rel. 20.10 : Jython Master
            Data Scripts
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Fuentes Serna Juan Mariano (ID), last modified by
            Kovtun Viktor (ID) on Mar 10, 2023

         .. container:: wiki-content group
            :name: main-content

            .. container:: toc-macro rbtoc1678781405410

               -  `Introduction <#JythonMasterDataScripts-Introduction>`__

                  -  `API Basics <#JythonMasterDataScripts-APIBasics>`__
                  -  `Simple
                     example <#JythonMasterDataScripts-Simpleexample>`__
                  -  `Command line
                     tools <#JythonMasterDataScripts-Commandlinetools>`__

                     -  `Executing master data
                        scripts <#JythonMasterDataScripts-Executingmasterdatascripts>`__
                     -  `Exporting master
                        data <#JythonMasterDataScripts-Exportingmasterdata>`__

            .. rubric:: Introduction
               :name: JythonMasterDataScripts-Introduction

            openBIS defines as "Master data" all metadata configurations
            needed before the import of the real raw data. Master data
            includes experiment/sample/data set/property/file types,
            vocabularies and property assignments.

            .. rubric:: API Basics
               :name: JythonMasterDataScripts-APIBasics

            Similarly to the `Jython Dropbox
            API <https://unlimited.ethz.ch/pages/viewpage.action?pageId=53746029>`__
            the script can access a global variable named ``service``,
            which can be used to create transactions.

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     transaction = service.transaction()

            The transactions are a focal API concept offering to create
            new types (e.g. ``createNewSampleType``,
            ``createNewDataSetType``) and new property assignments (e.g.
            ``assignPropertyType``).

            The complete Javadoc for the API is available at

            .. container:: table-wrap

               +----------------------------------+----------------------------------+
               | `IMasterDataRegis                | The global service variable      |
               | trationService <https://openbis. |                                  |
               | ch/javadoc/20.10.x/javadoc-openb |                                  |
               | is/ch/systemsx/cisd/openbis/gene |                                  |
               | ric/server/jython/api/v1/IMaster |                                  |
               | DataRegistrationService.html>`__ |                                  |
               +----------------------------------+----------------------------------+
               | `IMasterDataRegistrationT        | The public API of the            |
               | ransaction <https://openbis.ch/j | transaction objects              |
               | avadoc/20.10.x/javadoc-openbis/c |                                  |
               | h/systemsx/cisd/openbis/generic/ |                                  |
               | server/jython/api/v1/IMasterData |                                  |
               | RegistrationTransaction.html>`__ |                                  |
               +----------------------------------+----------------------------------+
               | `All                             | Javadocs for the complete API    |
               | classes <h                       | package                          |
               | ttps://openbis.ch/javadoc/20.10. |                                  |
               | x/javadoc-openbis/ch/systemsx/ci |                                  |
               | sd/openbis/generic/server/jython |                                  |
               | /api/v1/package-summary.html>`__ |                                  |
               +----------------------------------+----------------------------------+

            .. rubric:: Simple example
               :name: JythonMasterDataScripts-Simpleexample

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

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

            .. rubric:: Command line tools
               :name: JythonMasterDataScripts-Commandlinetools

            .. rubric:: Executing master data scripts
               :name: JythonMasterDataScripts-Executingmasterdatascripts

            Make sure openBIS AS is up and running prior script
            execution. Go to the openBIS AS installation folder.
            Assuming your script is ``/local/master-data-script.py`` and
            openBIS AS is started on the URL
            ``http://localhost:8888/openbis`` execute the command

            .. container:: preformatted panel

               .. container:: preformattedContent panelContent

                  ::

                     > cd /local0/openbis/servers/openBIS-server/jetty/bin
                     > /register-master-data.sh -s http://localhost:8888/openbis/openbis -f /local/master-data-script.py

            You will be prompted for username/password before the script
            execution. Please note that the second 'openbis' is needed
            in the server address, so that you connect via the API.

            .. rubric:: Exporting master data
               :name: JythonMasterDataScripts-Exportingmasterdata

            You can export the master data from a running openBIS system
            as script by running the command

            .. container:: preformatted panel

               .. container:: preformattedContent panelContent

                  ::

                     > cd /local0/openbis/servers/openBIS-server/jetty/bin
                     > /export-master-data.sh -s http://localhost:8888/openbis/openbis

            This command will create a folder
            ``exported-master-data-DATE`` which will contain the
            exported master data script - ``master-data.py``

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 14, 2023 09:10

         .. container::
            :name: footer-logo

            `Atlassian <https://www.atlassian.com/>`__
