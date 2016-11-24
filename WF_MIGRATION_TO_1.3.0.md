# Migration from version 1.2.0 to 1.3.0

Before starting the migration, we strongly invite you to take a backup of:
- the current D-Net webapp you want to migrate
- your property file `cnr.override.properties`, typically stored in `$yourTomcatHomeDirectory$/common/classes` (e.g. `/var/lib/tomcat7/common/classes`)
- the xmldb and the subscriptions from `http://<host>:<port>/<webapp>/mvc/inspector/backups.do`
- the mongo database

The most important change from 1.2.0 to 1.3.0 is the adoption of a refactored module for the management of the OAI Publisher.
The new module fixes some important bugs in OAI-PMH management and requires that:
- the OAI stored located in the mongo database `oaistore_dnet` MUST be deleted. 
- the old workflows assigned to data sources MUST be re-created or patched.
   
Please follow one of the two migration options described here:
1. Start the aggregator from scratch: this is the easiest and safest option but you will loose all metadata collected so far.
The procedure will guide you on how to re-import the D-Net profiles you might have configured (e.g. data sources, indexing instructions, transformation rules).
2. Patch the aggregation workflows: this option will allow you to keep the metadata collected so far.
The procedure will guide you on how to patch the existing workflows by performing a list of XUpdate on the D-Net Information system based on the XML database existDB.
The XUpdates have been tested on a test machine, NOT in a production environment.

## Option 1: Start the aggregator from scratch
1. Ensure you have backed-up:
- the current D-Net webapp you want to migrate (the file `.war` deployed on your tomcat)
- the xmldb and the subscriptions from `http://<host>:<port>/<webapp>/mvc/inspector/backups.do`. By default the backup is stored in `dnet.data.path` (see your
property file). Ensure you download the latest backups in your machine as well because we will delete that folder in step 3.
(this backup is needed not only for safety, but also to re-import existing profiles in the new aggregator)
- your property file `cnr.override.properties`, typically stored in `$yourTomcatHomeDirectory$/common/classes` (e.g. `/var/lib/tomcat7/common/classes`)
- mongodb (please see the mongo manual for reference: https://docs.mongodb.com/manual/reference/program/mongodump/)
2. Stop tomcat
3. Delete the `dnet.data.path` folder. Please note: this is the folder where the backups of the xmldb and subscriptions are stored by default. 
This is why you MUST download a copy of the backups in Step 1.
4. Delete the content of the mongo database. Before proceeding, please check in your property file if you have overridden any of the following properties
 and change the suggested command accordingly:
	~~~~   
	services.mdstore.mongodb.db = mdstore_minimal
	dnet.logger.mongo.db = dnet_logs_minimal
	~~~~
	Assuming you have not changed the default values, from the mongo shell please type the following:
	~~~~   
    use oaistore_dnet
    db.dropDatabase()
    use mdstore_minimal
    db.dropDatabase()
    ~~~~
	if you are not interested in keeping the old history of workflows, you can delete also the `dnet_logs_minimal` database:
	~~~~
	use dnet_logs_minimal
	db.dropDatabase()
	~~~~
5. Deploy the new D-Net webapp 1.3.0 (`.war` file)
6. Start tomcat and verify the webapp started properly by looking at the tomcat logs and accessing to `http://<host>:<port>/<webapp>/mvc/ui/repoApis.do`
At this step, you should have no registered data sources except for the `Mock` one.
7. You can add your data sources by using the available GUIs (suggested) or by importing the old data source profiles (suggested if you have a lot of data sources):
	1. using the GUI form: "Datasource Management" -> "Add new datasource". 
	APIs for aggregation can be added via the form at "Datasource Management" -> "Add new API".
	Please check the file CREATING_NEW_DATASOURCE.md for details.
	2. to import the existing data sources and APIs please follow these steps:
		1. Copy the backup zip of xmldb downloaded in step 1 into a server folder (e.g. `/tmp`) and unzip it. You will see a folder named `db`.
		2. Go to `http://<host>:<port>/<webapp>/mvc/ui/isManager.do#/bulk`
		3. Fill in the following path in the "BaseURL Directory" text box: `file:///tmp/db/DRIVER/RepositoryServiceResources`
		4. Ensure ONLY "Import profiles" is checked
		5. Click on "Import"
	When ready, you will see a report of the import telling how many profiles were added or discarded.
8. If you had configured other profiles, you can proceed in the same way of step 7.ii, but selecting the directory of the back-up containing the configured profiles.
	Names of directories are self-explanatory in most of the cases, however this is where you can find the profiles you might want to re-import:
	- Transformation rules: `file:///tmp/db/DRIVER/TransformationRuleDSResources`
	- Vocabularies: `file:///tmp/db/DRIVER/VocabularyDSResources`
	- Solr index configuration: `file:///tmp/db/DRIVER/MDFormatDSResources`
	- OAI publisher configuration: `file:///tmp/db/DRIVER/OAIPublisherConfigurationDSResources`
9. Now you should be ready to assign the aggregation workflows to the data sources and run them to collect their metadata records and populate the index and the OAI store.


## Option 2: Patch the aggregation workflow
We strongly recommend to use Option 1. Option 2 should only be used if it is impossible to collect again the metadata from the original data sources.
Please note that the mongodb database used by the OAI-publisher MUST be deleted (see step 3 below)

1. Ensure you have backed-up:
- the current D-Net webapp you want to migrate (the file `.war` deployed on your tomcat)
- the xmldb and the subscriptions from `http://<host>:<port>/<webapp>/mvc/inspector/backups.do`. Ensure you download the latest backups in your machine as well.
- your property file `cnr.override.properties`, typically stored in `$yourTomcatHomeDirectory$/common/classes` (e.g. `/var/lib/tomcat7/common/classes`)
- mongodb (please see the mongo manual for reference: https://docs.mongodb.com/manual/reference/program/mongodump/)
2. Set the aggregator in "Prepare for shutdown" by clicking on the button here: http://<host>:<port>/<webapp>/mvc/ui/prepareShutdown.do
3. Drop the mongo database called 'oaistore_dnet': this will delete ALL the content currently exposed via OAI-PMH. 
   ~~~~   
   use oaistore_dnet
   db.dropDatabase()
	~~~~
4. Follow the migration instructions in the next sections, then come back here
5. If you are here, then you have run all the XUpdates with success: Congratulations! 
The next steps are needed to deploy the new version of D-Net and check if everything is ok.
6. Stop tomcat
7. Deploy the new D-Net webapp 1.3.0 
8. Start tomcat and verify the webapp started properly by looking at the tomcat logs and accessing to `http://<host>:<port>/<webapp>/mvc/ui/repoApis.do`
9. Select one of the data source and run its aggregation workflow to verify everything is working. Eventually the OAI-PMH publisher should offer only records from this data source.
10. Run the `oai` and `oaiPostFeed` workflows (the second should start automatically after the first) for all data sources. This final step can be avoided if you 
set the scheduled aggregation for your data sources. In this case the aggregation workflows will start as specified by the scheduling instructions.

### Migrating the OAI workflows
You MUST set the aggregator in "Prepare for shutdown", by clicking on the button here: http://<host>:<port>/<webapp>/mvc/ui/prepareShutdown.do
You can proceed with the migration only if there are no workflows running.
Please note that if there are still workflows running, the menu bar is orange. When there are no workflows running, the menu bar is RED.
The following xupdates must be run in sequence. If any xupdate fails, please contact us.

~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oai'] return update value $x//NODE[./@name='readMDStore']/@type with 'FetchMDStoreRecords'
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oai']//NODE[@name='readMDStore']
  let $mdid := $x//PARAM[./@name='mdstoreId']/text()
  return update replace $x/PARAMETERS with
  <PARAMETERS>
     <PARAM name="eprParam" managedBy="system" type="string" required="true">oai_epr</PARAM>
     <PARAM name="mdId" managedBy="system" type="string" required="true">{$mdid}</PARAM>
  </PARAMETERS>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oai']
    let $dsname := $x//NODE[@name='SET_INFO']//PARAM[@name='providerName']/text()
    return update replace $x//NODE[@name='oaiSync']/PARAMETERS with
    <PARAMETERS>
            <PARAM name="eprParam" managedBy="system" type="string" required="true">oai_epr</PARAM>
            <PARAM name="oaiSource" managedBy="system" type="string" required="true">{$dsname}</PARAM>
            <PARAM name="oaiDbName" managedBy="user" type="string" required="true">oaistore_dnet</PARAM>
            <PARAM name="oaiFormat" managedBy="user" type="string" required="true">DMF</PARAM>
            <PARAM name="oaiLayout" managedBy="user" type="string" required="true">store</PARAM>
            <PARAM name="oaiInterpretation" managedBy="user" type="string" required="true">transformed</PARAM>
    </PARAMETERS>
~~~~
### Migrating the OAI Post-feed workflows
The following xupdates must be run in sequence. If any xupdate fails, please contact us.

~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed'] return update replace $x//ARC[@to='setFormat'] with <ARC to="prepareOAI"/>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']//NODE[@name='setFormat'] return update delete $x
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']//NODE[@name='ConfigSets'] return update delete $x
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']
  let $dsname := $x//PARAM[@name='providerName']/text()
  return update replace $x//NODE[@name='prepareOAI']/PARAMETERS with
  <PARAMETERS>
          <PARAM managedBy="user" name="oaiDbName" required="true" type="string">oaistore_dnet</PARAM>
          <PARAM managedBy="user" name="oaiFormat" required="true" type="string">DMF</PARAM>
          <PARAM managedBy="user" name="oaiLayout" required="true" type="string">store</PARAM>
          <PARAM managedBy="user" name="oaiInterpretation" required="true" type="string">transformed</PARAM>
          <PARAM name="oaiSource" managedBy="system" type="string" required="true">{$dsname}</PARAM>
  </PARAMETERS>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']//NODE[@name='CompoundIndexes']
    let $fields := $x//PARAM[@name='fieldNames']/text()
    return update replace $x/PARAMETERS with
    <PARAMETERS>
    		<PARAM managedBy="user" name="fieldNames" required="true" type="string">{$fields}</PARAM>
    </PARAMETERS>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']//NODE[@name='ConfigIndexes']
    return update replace $x/PARAMETERS with <PARAMETERS></PARAMETERS>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed'] return update replace $x//ARC[@to='SetsCount'] with <ARC to="RefreshConfig"/>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']//NODE[@name='ConfigIndexes'] return update insert 
  <NODE name="RefreshConfig" type="OAIRefreshConfiguration">
  	<DESCRIPTION>Reads the current OAI configuration and updates the OAI sets accordingly</DESCRIPTION>
  		<PARAMETERS></PARAMETERS>
  		<ARCS>
  			<ARC to="SetsCount" />
  		</ARCS>
   </NODE>
   following $x
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_NAME='oaiPostFeed']//NODE[@name='SetsCount'] return update replace $x with 
<NODE name="SetsCount" type="OAISetsCountUpdate">
     <DESCRIPTION>Count records in each OAI set, for each exported metadata format linked to the given oai collection</DESCRIPTION>
     <PARAMETERS>
		<PARAM managedBy="system" name="configuredOnly" required="true" type="boolean">true</PARAM>
     </PARAMETERS>
     <ARCS>
		<ARC to="success"/>
     </ARCS>
</NODE>
~~~~
### Migrating the repo-bye workflows 
The repo-bye workflow is automatically created when you assign a workflow to a datasource.
Its goal is to release all resources created and used by a data source when you delete its aggregation workflow.
One bug, now fixed, was that the repo-by workflows were not deleting the OAI set of the data source.

~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_TYPE='REPO_BYE'] return update replace $x//ARC[@to='RemoveApiExtraFields'] with <ARC to="prepareOAI"/>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_TYPE='REPO_BYE']
  let $dsname := $x//PARAM[@name='providerName']/text()
  let $indexNode := $x//NODE[@name='DeleteIndex']
  return update insert 
  <NODE name="prepareOAI" type="PrepareOaiJob">
       <DESCRIPTION>Prepare oai feeding</DESCRIPTION>
       <PARAMETERS>
  		<PARAM managedBy="user" name="oaiDbName" required="true" type="string">oaistore_dnet</PARAM>
  		<PARAM managedBy="user" name="oaiFormat" required="true" type="string">DMF</PARAM>
  		<PARAM managedBy="user" name="oaiLayout" required="true" type="string">store</PARAM>
  		<PARAM managedBy="user" name="oaiInterpretation" required="true" type="string">transformed</PARAM>
  		<PARAM name="oaiSource" managedBy="system" type="string" required="true">{$dsname}</PARAM>
       </PARAMETERS>
       <ARCS>
  		<ARC to="DropOAIStore"/>
       </ARCS>
  </NODE>
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_TYPE='REPO_BYE']
      let $dsname := $x//PARAM[@name='providerName']/text()
      let $node := $x//NODE[@name='prepareOAI']
      return update insert 
  <NODE name="DropOAIStore" type="OAIDropStore">
  	<DESCRIPTION>Delete the OAI store for this repo</DESCRIPTION>
  	<PARAMETERS>
          <PARAM name="oaiSource" managedBy="system" type="string" required="true">{$dsname}</PARAM>
  	</PARAMETERS>
  	<ARCS>
  		<ARC to="OAISetsCountUpdate"/>
  	</ARCS>
  </NODE>
  following $node
~~~~
~~~~
for $x in //RESOURCE_PROFILE[.//WORKFLOW_TYPE='REPO_BYE']
  let $node := $x//NODE[@name='DropOAIStore']
  return update insert 
  <NODE name="OAISetsCountUpdate" type="OAISetsCountUpdate">
  	<DESCRIPTION>Update counts on OAI configuration sets</DESCRIPTION>
  	<PARAMETERS>
  		<PARAM managedBy="user" name="configuredOnly" required="true" type="boolean">true</PARAM>
  	</PARAMETERS>
  	<ARCS>
  		<ARC to="RemoveApiExtraFields"/>
  	</ARCS>
  </NODE>
  following $node
~~~~

    
