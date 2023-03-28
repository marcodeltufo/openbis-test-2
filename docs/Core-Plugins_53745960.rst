.. container::
   :name: page

   .. container:: aui-page-panel
      :name: main

      .. container::
         :name: main-header

         .. container::
            :name: breadcrumb-section

            #. `openBIS Documentation Rel. 20.10 <index.html>`__

         .. rubric:: openBIS Documentation Rel. 20.10 : Core Plugins
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Fuentes Serna Juan Mariano (ID), last modified by
            Kovtun Viktor (ID) on Aug 22, 2022

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Core Plugins
               :name: CorePlugins-CorePlugins

            .. container:: toc-macro rbtoc1678781405463

               -  `Core Plugins <#CorePlugins-CorePlugins>`__

                  -  `Motivation <#CorePlugins-Motivation>`__
                  -  `Core Plugins Folder
                     Structure <#CorePlugins-CorePluginsFolderStructure>`__
                  -  `Merging Configuration
                     Data <#CorePlugins-MergingConfigurationData>`__
                  -  `Enabling Modules and Disabling
                     Plugins <#CorePlugins-EnablingModulesandDisablingPlugins>`__

                     -  `Enabling
                        Modules <#CorePlugins-EnablingModules>`__
                     -  `Disabling Core Plugins by
                        Property <#CorePlugins-DisablingCorePluginsbyProperty>`__
                     -  `Disabling Core Plugins by Marker
                        File <#CorePlugins-DisablingCorePluginsbyMarkerFile>`__

                  -  `Core Plugin
                     Dependency <#CorePlugins-CorePluginDependency>`__
                  -  `Rules for Plugin
                     Writers <#CorePlugins-RulesforPluginWriters>`__
                  -  `Using Java libraries in Core
                     Plugins <#CorePlugins-UsingJavalibrariesinCorePlugins>`__

            .. rubric:: Motivation
               :name: CorePlugins-Motivation

            The ``service.properties`` file of openBIS Application
            Server (AS) and Data Store Server (DSS) can be quite big
            because of all the configuration data for maintenance tasks,
            drop-boxes, reporting and processing plugins, etc. Making
            this configuration more modular will improve the structure.
            It would also allow to have core plugins shipped with
            distribution and customized plugins separately. This makes
            maintenance of these plugins more independent. For example,
            a new maintenance task plugin can be added in an update
            without any need for an admin to put the configuration data
            manually into the ``service.properties`` file.

            .. rubric:: Core Plugins Folder Structure
               :name: CorePlugins-CorePluginsFolderStructure

            All plugins whether they are a part of the distribution or
            added and maintained are stored in the folder usually called
            ``core-plugins``. Standard (i.e. core) plugins are part of
            the distribution. During installation the folder
            ``core-plugins`` is unpacked as a sibling folder of
            ``openBIS-server`` and\ ``datastore_server``.

            The folder structure is organized as follows:

            -  The file ``core-plugins.properties`` containing the
               following properties:

               -  ``enabled-modules``: comma-separated list of regular
                  expressions for all enabled modules.
               -  ``disabled-core-plugins``: comma-separated list of
                  disabled plugins. All plugins are disabled for which
                  the beginning of full plugin ID matches one of the
                  terms of this list. To disable initialization of
                  master data of a module - disable it's core plugin
                  "initialize-master-data"

            -  The children of ``core-plugins`` are folders denoting
               modules like the standard technologies, ``proteomics``
               and ``screening``. For customization, any module can be
               added.
            -  Each module folder has children which are numbered
               folders. The number denotes the version of the plugins of
               that module. The version with the largest number will be
               used. Different modules can have different largest
               version numbers.
            -  Every version folder has the subfolder ``as``
               and/or\ ``dss``\ which have subfolders for the various
               types of plugins. The types are different for AS and DSS:

               -  AS:

                  -  ``maintenance-tasks``: Maintenance tasks triggered
                     by some time schedule.\ ````\ Property ``class``
                     denotes fully-qualified class name of a class
                     implementing
                     ``ch.systemsx.cisd.common.maintenance.IMaintenanceTask``.
                     For more details see `Maintenance
                     Tasks <https://unlimited.ethz.ch/display/openBISDoc2010/Maintenance+Tasks>`__.
                  -  ``dss-data-sources``: Definition of data sources
                     with corresponding data source definitions for DSS.
                     For more details see `Installation and
                     Administrator Guide of the openBIS
                     Server <https://unlimited.ethz.ch/display/openBISDoc2010/Installation+and+Administrator+Guide+of+the+openBIS+Server>`__.
                  -  ``query-databases``: Databases for SQL queries. For
                     more details see `Custom Database
                     Queries <https://unlimited.ethz.ch/display/openBISDoc2010/Custom+Database+Queries>`__.
                  -  ``custom-imports``: Custom file imports to DSS via
                     Web interface. For more details see `Custom
                     Import <Custom-Import_53746004.html>`__.
                  -  ``services``: Custom services. For more details
                     see `Custom Application Server
                     Services <https://unlimited.ethz.ch/display/openBISDoc2010/Custom+Application+Server+Services>`__.
                  -  ``webapps``: HTML5 applications that use the
                     openBIS API. For more details see `openBIS
                     webapps <openBIS-webapps_53745961.html>`__.
                  -  ``miscellaneous``: Any additional properties.

               -  ``DSS:``

                  -  ``drop-boxes``: ETL server threads for registration
                     of data sets.\ ````
                  -  ``reporting-plugins``: Reports visible in openBIS.
                     Property ``class`` denotes fully-qualified class
                     name of a class implementing
                     ``ch.systemsx.cisd.openbis.dss.generic.server.plugins.tasks.IReportingPluginTask``.
                     For more details see `Reporting
                     Plugins <https://unlimited.ethz.ch/display/openBISDoc2010/Reporting+Plugins>`__.
                  -  ``processing-plugins``: Processing tasks triggered
                     by users.\ ````\ Property ``class`` denotes
                     fully-qualified class name of a class implementing
                     ``ch.systemsx.cisd.openbis.dss.generic.server.plugins.tasks.IProcessingPluginTask``.\ ````\ For
                     more details see `Processing
                     Plugins <https://unlimited.ethz.ch/display/openBISDoc2010/Processing+Plugins>`__.\ ````
                  -  ``maintenance-tasks``: Maintenance tasks triggered
                     by some time schedule.\ ````\ Property ``class``
                     denotes fully-qualified class name of a class
                     implementing
                     ``ch.systemsx.cisd.common.maintenance.IMaintenanceTask``.\ ````\ For
                     more details see `Maintenance
                     Tasks <https://unlimited.ethz.ch/display/openBISDoc2010/Maintenance+Tasks>`__.
                  -  ``search-domain-services``: Services for variaous
                     search domains (e.g. search on sequence databases
                     using BLAST).
                     Property ``class`` denotes fully-qualified class
                     name of a class implementing
                     ``ch.systemsx.cisd.openbis.dss.generic.shared.api.internal.v2.ISearchDomainService``.
                  -  ``data-sources``: Internal or external database
                     sources.
                  -  ``services``: Services based on servlets.
                     Property ``class`` denotes fully-qualified class
                     name of a class implementing
                     ``javax.servlet.Servlet``.
                  -  ``imaging-overview-plugins``: Data set type
                     specific provider of the overview image of a data
                     set.
                     Property ``class`` denotes fully-qualified class
                     name of a class implementing
                     ``ch.systemsx.cisd.openbis.dss.generic.server.IDatasetImageOverviewPlugin``.
                  -  ``file-system-plugins``: Provider of a custom DSS
                     file system (FTP/SFTP) view hierarchy.
                     Property ``class`` denotes fully-qualified class
                     name of a class
                     implementing ``ch.systemsx.cisd.openbis.dss.generic.server.fs.IResolverPlugin``
                     Property code denotes the name of the top-level
                     directory under which the custom hierarchy will be
                     visible
                  -  ``miscellaneous``: Any additional properties.\ ````

            -  Folders of each of these types can have an arbitrary
               number of subfolders. But if the type folder is present
               it should have at least one subfolder. Each defining one
               plugin. The name of these subfolders define the plugin
               ID. It has to be unique over all plugins independent of
               module and plugin type. It should not contain the
               characters space ' ', comma '``,``', and equal sign
               '``=``'.
            -  Each plugin folder should contain at least the file
               ``plugin.properties``. There could be additional files
               (referred in ``plugin.properties``) but no subfolders.

            Here is an example of a typical structure of a core plugins
            folder:

            .. container:: preformatted panel

               .. container:: preformattedContent panelContent

                  ::

                     core-plugins
                       core-plugins.properties
                       proteomics
                         1
                           as
                             initialize-master-data.py
                           dss
                             drop-boxes
                               ms-injection
                                 plugin.properties
                             maintenance-tasks
                               data-set-clean-up
                                 plugin.properties
                       screening
                         1
                           core-plugin.properties
                           as
                             initialize-master-data.py
                             maintenance-tasks
                               material-reporting
                                 mapping.txt
                                 plugin.properties
                             custom-imports
                               myCustomImport
                                 plugin.properties
                           dss
                             drop-boxes
                               hcs-dropbox
                                 lib
                                   custom-lib.jar
                                 hcs-dropbox.py
                                 plugin.properties

            You might noticed the file ``initialize-master-data.py`` in
            AS core plugins sections  in this example. It is a script to
            register master data in the openBIS core database. For more
            details see `Installation and Administrator Guide of the
            openBIS
            Server <https://unlimited.ethz.ch/display/openBISDoc2010/Installation+and+Administrator+Guide+of+the+openBIS+Server>`__.

            Each plugin can refer to any number of files. These files
            are part of the plugin folder. In ``plugin.properties`` they
            are referred relative to the plugin folder, that is by file
            name. Example:

            .. container:: preformatted panel

               .. container:: preformattedHeader panelHeader

                  **plugin.properties**

               .. container:: preformattedContent panelContent

                  ::

                     incoming-dir = ${incoming-root-dir}/incoming-hcs
                     incoming-data-completeness-condition = auto-detection
                     top-level-data-set-handler = ch.systemsx.cisd.openbis.dss.etl.jython.JythonPlateDataSetHandler
                     script-path = hcs-dropbox.py
                     storage-processor = ch.systemsx.cisd.openbis.dss.etl.PlateStorageProcessor
                     storage-processor.data-source = imaging-db
                     storage-processor.define-channels-per-experiment = false

            .. rubric:: Merging Configuration Data
               :name: CorePlugins-MergingConfigurationData

            At start up of AS and DSS merges  the content of 
            ``service.properties`` with the content of all
            ``plugin.properties`` of the latest version per enabled
            module. Plugin properties can be deleted by adding
            ``<plugin ID>.<plugin property key> = __DELETED__`` to
            service.properties. Example:

            .. container:: preformatted panel

               .. container:: preformattedContent panelContent

                  ::

                     simple-dropbox.incoming-data-completeness-condition = __DELETED__

            This leads to a deletion of the property
            ``incoming-data-completeness-condition`` specified in
            ``plugins.properties`` of the plugin ``simple-dropbox``.

            Merging is done by injection the properties of
            ``plugin.properties`` into ``service.properties``\ by adding
            the plugin ID as a prefix to the property key (not for
            ``miscellaneous).``\ For example, the property
            ``script-path`` of plugin ``hcs-dropbox`` becomes
            ``hcs-dropbox.script-path``. References to files inside the
            plugin are replaced by a path relative to the working
            directory. For the various plugin types (except
            ``miscellaneous``) the plugin ID is appended to the related
            property in  ``service.properties`` for this plugin type.
            For example, plugins of type ``drop-boxes`` are added to the
            property ``inputs``.

            .. rubric:: Enabling Modules and Disabling Plugins
               :name: CorePlugins-EnablingModulesandDisablingPlugins

            There are three methods to control which plugins are
            available and witch not:

            -  enabling by property ``enabled-modules``
               in\ ``core-plugins.properties``: This enables all plugins
               of certain modules.
            -  disabling by property ``disabled-core-plugins`` in
               ``core-plugins.properties`` : This allows to disable on a
               fine grade level specific plugins.
            -  disabling by marker file: Plugin developers should use
               this method when developing new plugins.

            .. rubric:: Enabling Modules
               :name: CorePlugins-EnablingModules

            The property ``enabled-modules`` in
            ``core-plugins.properties`` is a comma-separated list of
            regular expressions denoting modules. All plugins in a
            module folder of ``core-plugins`` folder are enabled if the
            module name matches one of these regular expressions. If
            this list is empty or the property hasn't been specified no
            core-plugin will be used. Note, that this property is
            manipulated by openBIS Installer for Standard Technologies.
            Example:

            .. container:: preformatted panel

               .. container:: preformattedHeader panelHeader

                  **service.properties**

               .. container:: preformattedContent panelContent

                  ::

                     enabled-modules = screening, proteomics, dev-module-.*

            .. rubric:: Disabling Core Plugins by Property
               :name: CorePlugins-DisablingCorePluginsbyProperty

            The property ``disabled-core-plugins`` in
            ``core-plugins.properties`` allows to disable plugins
            selectively either by module name, module combined with
            plugin type or full plugin ID. Example:

            .. container:: preformatted panel

               .. container:: preformattedHeader panelHeader

                  **service.properties**

               .. container:: preformattedContent panelContent

                  ::

                     disabled-core-plugins = screening, proteomics:reporting-plugins, proteomics:maintenance-tasks:data-set-clean-up

            .. rubric:: Disabling Core Plugins by Marker File
               :name: CorePlugins-DisablingCorePluginsbyMarkerFile

            The empty marker file ``disabled`` in a certain plugin
            folder disables the particular plugin.

            .. rubric:: Core Plugin Dependency
               :name: CorePlugins-CorePluginDependency

            A core plugin can depend on another core plugin. The
            dependency is specified
            in ``<module>/<version>/core-plugin.properties``. It has a
            property named ``required-plugins``. Its value is a
            comma-separated list of core-plugins on which it depends.
            The dependency can be specified selectively either by module
            name, module combined with plugin type or full plugin ID.
            Example:

            .. container:: preformatted panel

               .. container:: preformattedHeader panelHeader

                  **core-plugin.properties**

               .. container:: preformattedContent panelContent

                  ::

                     required-plugins = module-a, module-b:initialize-master-data, module-b:reporting-plugins, module-a:drop-boxes:generic

            .. rubric:: Rules for Plugin Writers
               :name: CorePlugins-RulesforPluginWriters

            As a consequence of the way plugins are merged with 
            ``service.properties`` writers of plugins have to obey the
            following rules:

            -  Plugin IDs have to be unique among all plugins whether
               they are defined in ``service.properties`` or as core
               plugins. The only exceptions are plugins of type
               ``miscellaneous``.
            -  In ``plugin.properties`` other properties can be referred
               by the usual ``${<property key>``} notation. The referred
               property can be in ``service.properties`` or in any
               ``plugin.properties``.
            -  As convention use ``${incoming-root-dir``} when defining
               the incoming folder for a drop box.
            -  Refer files in ``plugin.properties`` only by names and
               add them as siblings of ``plugin.properties`` to the
               plugin folder. Note, that different plugins can refer
               files with the same name. There will be no ambiguity
               which file is meant.
            -  In order to be completely independent from updates of the
               core plugins which are part of the distribution create
               your own module, like ``my-plugins``, and put all your
               plugins there. Do not forget to add your module to the
               property ``enabled-modules`` in
               ``core-plugins.properties``.

            .. rubric:: Using Java libraries in Core Plugins
               :name: CorePlugins-UsingJavalibrariesinCorePlugins

            OpenBIS allows you to include Java libraries in core plugin
            folders. The \*.jar files have to be stored in "<code plugin
            folder>/lib" folder. For instance, in order to use
            "my-lib.jar" in "my-dropbox" a following file structure is
            needed:

            .. container:: preformatted panel

               .. container:: preformattedHeader panelHeader

                  **service.properties**

               .. container:: preformattedContent panelContent

                  ::

                     my-technology
                         1
                           dss
                             drop-boxes
                               my-dropbox
                                 lib
                                   my-lib.jar
                                 dropbox.py
                                 plugin.properties

            Having this structure, Java classes from "my-lib.jar" can be
            imported and used in "dropbox.py" script.

            NOTICE: Currently this feature is only supported for DSS
            core plugins. Under the hood, a symbolic link to a jar file
            is created in "datastore_server/lib" folder during DSS
            startup.

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 14, 2023 09:10

         .. container::
            :name: footer-logo

            `Atlassian <https://www.atlassian.com/>`__
