
<head>
<!-- Start Styles. Move the 'style' tags and everything between them to between the 'head' tags -->
<style type="text/css">
.myTable { background-color:#eee;border-collapse:collapse; }
.myTable th { background-color:#000;color:white; }
.myTable td, .myTable th { padding:5px;border:1px solid #000; }
</style>

</head>
# Properties

This file contains the list of properties that can be set to configure a D-Net instance.

To get the list of currently used properties please go to the Admin UI of your running instance and select Configuration > Container Properties

## D-Net webapp properties
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>container.hostname</td>  <td>localhost</td> <td>String</td> <td>the host name where the web app will be running</td>
    </tr>
    <tr>
        <td>container.port</td>  <td>8280</td>  <td>int</td> <td>the port where the web app will be running</td>
    </tr>
    <tr>
        <td>container.context</td>  <td>app</td>  <td>String</td> <td>name of the web app</td>
    </tr>
    <tr>
        <td>dnet.data.path</td>  <td>/tmp/dnet</td>  <td>String</td> <td>path to the directory where all D-Net related resources will be saved.  An embedded existDB will be automatically installed in this directory during the first start-up. The directory must be writable by the user running tomcat.</td>
    </tr>
</table>

## D-Net properties for enabling services

### ResultSet Service
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>services.is.resultset.client.pagesize</td>  <td>10</td> <td>int</td> <td>Default number of records in each page of  result set</td>
    </tr>
     <tr>
        <td>services.is.resultset.push.maxElementsInMemory</td>  <td>500</td> <td>int</td> <td>Push resultset stores records temporary on a cache. This is the max number of records in memory. After this number they will be stored on disk</td>
    </tr>
     <tr>
        <td>services.is.resultset.push.maxElementsOnDisk</td>  <td>5000000</td> <td>int</td> <td>Push resultset stores records temporary on a cache. This is the max number of records in memory. After this number new records will override older records (those will be lost if not yet read)</td>
    </tr>
    <tr>
        <td>services.resultset.data.path</td>  <td>${dnet.data.path}/resultset</td> <td>String</td> <td>Path of the ResultSet cache (for Push ResultSet)</td>
    </tr>
     <tr>
        <td>services.is.resultset.push.timeToIdle</td>  <td>36000</td> <td>int</td> <td>Seconds before a push result set expires if no read/write operations are performed on it.</td>
    </tr>
     <tr>
        <td>services.is.resultset.resultSetRegistryCache.timeToIdle</td>  <td>240</td> <td>int</td> <td>Seconds before a result set expires if no read/write operations are performed on it.</td>
    </tr>
     <tr>
        <td>services.is.resultset.resultSetRegistryMaxIdleTimeCache.timeToIdle</td>  <td>240</td> <td>int</td> <td>Seconds before a result set expires if no read/write operations are performed on it.</td>
    </tr>
</table>

### IS Store Service
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type/ Allowed values</th>
<th>Description</th>
</tr>
    <tr>
        <td>services.is.store.database.bean</td>  <td>temporaryExistDatabase</td> <td>temporaryExistDatabase, persistentExistDatabase</td> <td>The D-Net Information Service manages the resources of the infrastructure that are described by XML profiles. Those profiles are stored in an embedded existdb. In dev mode you may want to use a non persistent store, while in prod mode you have to set this to 'persistentExistDatabase'</td>
    </tr>
    <tr>
        <td>services.is.store.bulk.validation</td>  <td>true</td> <td>boolean</td> <td>True to activate automatic XML validation of resource profiles upon registration</td>   </tr>
      <tr>
        <td>default.service.comparator</td>  <td>preferLocalRunningInstanceComparator</td>  <td>preferLocalRunningInstanceComparator</td> <td>Bean that selects a service instance among a set of services of the same type. Currently the only option is 'preferLocalRunningInstanceComparator', which prefers a service instance running on localhost.</td>
    </tr>
</table>

### Resource Registration
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type/ Allowed values</th>
<th>Description</th>
</tr>
    <tr>
        <td>services.schemas</td>  <td>classpath*:/eu/dnetlib/test/schemas/**/*.xsd</td> <td>String</td> <td>Classpath containing the XSD schemata of the resource profiles</td>
    </tr>
    <tr>
        <td>services.registration.disabled</td>  <td>false</td> <td>boolean</td> <td>True to prevent the registration of new services to the IS</td>   </tr>
      <tr>
        <td>default.service.comparator</td>  <td>preferLocalRunningInstanceComparator</td>  <td>preferLocalRunningInstanceComparator</td> <td>Bean that selects a service instance among a set of services of the same type. Currently the only option is 'preferLocalRunningInstanceComparator', which prefers a service instance running on localhost.</td>
    </tr>
     
</table>


# D-Net properties for access control
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type/ Allowed values</th>
<th>Description</th>
</tr>
    <tr>
        <td>dnet.modular.ui.authorization.default.superAdmin</td>  <td>admin</td>  <td>string</td> <td>Name of the admin user that can perform every operation via the D-Net admin UI</td>
    </tr>
    <tr>
        <td>dnet.admin.password</td>  <td>f98c5d6f0e68b251f78bffac722fd0b9 (dnet-minimal)</td> <td>md5(password)</td> <td>MD5 of the password for the user admin</td>
    </tr>
    <tr>
        <td>dnet.modular.ui.authorization.manager</td>  <td>simpleAuthenticationManager</td>  <td>simpleAuthenticationManager,mockUserAuthenticationManager</td> <td>Authorization Manager used by the D-Net admin UI</td>
    </tr>
    <tr>
        <td>dnet.modular.ui.authorization.dao</td>  <td>mongoAuthorizationDao</td>  <td>mongoAuthorizationDao</td> <td>name of the bean in charge of the authorization mechanism. mongoAuthorizationDao keeps authorization info about users in a mongodb server.</td>
    </tr>
    <tr>
        <td>dnet.modular.ui.authorization.mongo.host</td>  <td>localhost</td> <td>String</td> <td>host that runs a mongodb to be used for user authorization management</td>
    </tr>
    <tr>
        <td>dnet.modular.ui.authorization.mongo.port</td>  <td>27017</td> <td>int</td> <td>port of the mongodb server to be used for user authorization management</td>
    </tr>
    <tr>
        <td>dnet.modular.ui.authorization.mongo.db</td>  <td>dnet_auth</td> <td>String</td> <td>mongodb database to be used for user authorization management</td>
    </tr>
</table>


# D-Net properties for metadata collection
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>collector.oai.http.defaultDelay</td>  <td>120</td> <td>seconds</td> <td>Seconds to wait between two subsequent OAI-PMH ListRecord requests</td>
    </tr>
    <tr>
        <td>collector.oai.http.readTimeOut</td>  <td>120</td>  <td>int</td> <td>Seconds to wait for an OAI-PMH publisher response.</td>
    </tr>
    <tr>
        <td>collector.oai.http.maxNumberOfRetry</td>  <td>6</td>  <td>int</td> <td>Number of times to retry in case of an OAI-PMH request timeout.</td>
    </tr>
     <tr>
        <td>repo.ui.compatibilityLevels.vocabulary</td>  <td>dnet:compatibilityLevel</td>  <td>Vocabulary code</td> <td>Code of a D-Net vocabulary containing the terms about datasource compatibility (or compliance)</td>
    </tr>
      <tr>
        <td>repo.ui.contentDescriptions.vocabulary</td>  <td>dnet:content_description_typologies</td>  <td>Vocabulary code</td> <td>Code of a D-Net vocabulary containing the terms about the typologies of collectable items</td>
    </tr>
     <tr>
        <td>repo.ui.datasourceTypes.vocabulary</td>  <td>dnet:typologies</td>  <td>Vocabulary code</td> <td>Code of a D-Net vocabulary containing the terms about the typologies of datasources</td>
    </tr>
    <tr>
        <td>repo.ui.protocols.vocabulary</td>  <td>dnet:protocols</td>  <td>Vocabulary code</td> <td>Code of a D-Net vocabulary containing the terms about the supported protocols for collection</td>
    </tr>
     <tr>
        <td>services.aggregator.country</td>  <td>EU</td>  <td>String</td> <td>your country code</td>
    </tr>
     <tr>
        <td>services.aggregator.name</td>  <td>D-NET</td>  <td>String</td> <td>the name of your aggregator</td>
    </tr>
    
</table>


#D-Net properties for metadata transformation and harmonisation
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type/ Allowed values</th>
<th>Description</th>
</tr>
    <tr>
        <td>dnet.modular.ui.vocabulary.dao</td>  <td>registryServiceVocabularyDAO</td> <td>bean name</td> <td>name of the bean that manages vocabulary resources</td>
    </tr>
    <tr>
        <td>services.transformation.defaultschema</td>  <td>/eu/dnetlib/data/collective/transformation/schema/DMFSchema_vTransformator.xsd</td>  <td>/eu/dnetlib/data/collective/transformation/schema/DMFSchema_vTransformator.xsd, /eu/dnetlib/data/collective/transformation/schema/OAFSchema_vTransformator.xsd</td> <td>Path to the Transformator Service schema. Two options are available: one for DMF, one for OAF. The template property below must be set accordingly.</td>
    </tr>
    <tr>
        <td>services.transformation.defaulttemplate</td>  <td>/eu/dnetlib/data/collective/transformation/engine/template.xsl</td>  <td>/eu/dnetlib/data/collective/transformation/engine/template.xsl,/eu/dnetlib/data/collective/transformation/engine/oaftemplate.xsl</td> <td>Template for the Transformator Service.  Two options are available: one for DMF, one for OAF. The schema property above must be set accordingly.</td>
    </tr>
    <tr>
        <td>services.transformation.vocabularyproperties.json </td>  <td>{"map":{"AccessRights":{"name":"dnet:rights", "caseSensitive":"false", "delimiter":"/"}, "Languages":{"name":"dnet:languages", "caseSensitive":"false", "delimiter":"/"}, "TextTypologies":{"name":"dnet:texttypologies", "caseSensitive":"false", "delimiter":"/"}}}</td>  <td>json map</td> <td>Instructs the Transformator Service about the D-Net vocabularies to apply for specific fields</td>
    </tr>
</table>

#D-Net properties for Metadata Store Service (MDStore)
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>services.mdstore.dao</td>  <td>mongodbMDStoreDao</td> <td>bean name</td> <td>name of the bean that manages mdstore. Currently there is only one option to work with a mongodb server.</td>
    </tr>
    <tr>
        <td>services.mdstore.discardrecords</td>  <td>true</td>  <td>boolean</td> <td>True to save non well-formed records in a discarded store.</td>
    </tr>
    <tr>
        <td>services.mdstore.mongodb.checkmetadata.onstart</td>  <td>true</td>  <td>boolean</td> <td>True to perform automatic checks on existing mdstore at startup.</td>
    </tr>
        <tr>
        <td>services.mdstore.mongodb.checkmetadata.startdelay</td>  <td>30000</td>  <td>millisecond</td> <td>Millisecond to wait after a webapp restart before running the metadata check. Only valid if 'services.mdstore.mongodb.checkmetadata.onstart' is true.</td>
    </tr>
      <tr>
        <td>services.mdstore.mongodb.ensureindex.cron</td>  <td>0 0 23 * * ?</td>  <td>Cron expression</td> <td>When to run a re-indexing on the mdstores. Only valid if 'services.mdstore.mongodb.ensureindex.enable' is true.</td>
    </tr>
      <tr>
        <td>services.mdstore.mongodb.ensureindex.enable</td>  <td>false</td>  <td>boolean</td> <td>Enables the automatic re-indexing of mdstores.</td>
    </tr>
     <tr>
        <td>services.mdstore.mongodb.host</td>  <td>localhost</td> <td>String</td> <td>host that runs a mongodb to be used for metadata storage</td>
    </tr>
    <tr>
        <td>services.mdstore.mongodb.port</td>  <td>27017</td> <td>int</td> <td>port of the mongodbserver to use for metadata storage</td>
    </tr>
    <tr>
        <td>services.mdstore.mongodb.db</td>  <td>mdstore_minimal</td> <td>String</td> <td>mongodb database to use for metadata storage</td>
    </tr>
     <tr>
        <td>services.mdstore.mongodb.connectionsPerHost</td>  <td>20</td> <td>int</td> <td>max number of connections per host allowed for the mdstore db used for metadata storage</td>
    </tr>
</table>

#D-Net properties for workflows
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>dnet.logger.mongo.host</td>  <td>localhost</td> <td>String</td> <td>host that runs a mongodb to be used for D-Net workflow history management</td>
    </tr>
    <tr>
        <td>dnet.logger.mongo.port</td>  <td>27017</td> <td>int</td> <td>port of the mongodbserver to use for D-Net workflow history management</td>
    </tr>
    <tr>
        <td>dnet.logger.mongo.db</td>  <td>dnet_logs_minimal</td> <td>String</td> <td>mongodb database to use for D-Net workflow history management</td>
    </tr>
</table>


#D-Net properties for mail notifications
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>infrastructure.name</td>  <td>development</td> <td>String</td> <td>Name of the infrastructure to appear in the mail subject</td>
    </tr>
    <tr>
        <td>msro.wf.mail.smtp.host</td>  <td>localhost</td> <td>String</td> <td>SMTP server host</td>
    </tr>
    <tr>
        <td>msro.wf.mail.smtp.port</td>  <td>587</td> <td>String</td> <td>SMTP server port</td>
    </tr>
     <tr>
        <td>msro.wf.mail.smtp.user</td>  <td></td> <td>String</td> <td>SMTP server user</td>
    </tr>
     <tr>
        <td>msro.wf.mail.smtp.password</td>  <td></td> <td>String</td> <td>SMTP server user password</td>
    </tr>
    <tr>
        <td>msro.wf.mail.from</td>  <td>dnet-noreply@research-infrastructures.eu</td> <td>e-mail</td> <td>Mail sender</td>
    </tr>
    <tr>
        <td>msro.wf.mail.fromName</td>  <td>D-NET Workflow Manager</td> <td>String</td> <td>Name of the e-mail sender</td>
    </tr>
    <tr>
        <td>msro.wf.mail.cc</td>  <td></td>  <td>comma separated list of e-mails</td> <td>e-mail addresses that will always receive each mail in cc</td>
    </tr>
    <tr>
        <td>dnet.guest.password</td>  <td>084e0343a0486ff05530df6c705c8bb4</td>  <td>md5(password)</td> <td>MD5 of the password for the user guest</td>
    </tr>
</table>


#D-Net properties for D-Net OAI-PMH Publisher
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type/ Allowed values</th>
<th>Description</th>
</tr>
    <tr>
        <td>services.oai.publisher.db.xquery</td>  <td>//RESOURCE_PROFILE[.//RESOURCE_TYPE/@value = 'OAIPublisherConfigurationDSResourceType']//CONFIGURATION/CURRENTDB/string() applicationContext-dnet-oai-utils.properties
</td> <td>xquery string</td> <td>xquery to fetch the OAI configuration</td>
    </tr>
    <tr>
        <td>services.oai.publisher.deletedrecords</td>  <td>TRANSIENT</td> <td>TRANSIENT, YES, NO</td> <td>See OAI-PMH specifications about the Identify verb response</td></tr>
    <tr>
        <td>services.oai.publisher.dnet.pagesize</td>  <td>100</td> <td>int</td> <td>Number of OAI records in a a page</td>
    </tr>
    <tr>
        <td>services.oai.publisher.repo.earliestdatestamp</td>  <td>2013-10-03T15:53:06Z</td>  <td>Date</td> <td>See OAI-PMH specifications about the Identify verb response</td></tr>
    <tr>
        <td>services.oai.publisher.repo.email</td>  <td>dnet-admin@mock.it</td>  <td>e-mail</td> <td>See OAI-PMH specifications about the Identify verb response</td></tr>
    </tr>
    <tr>
        <td>services.oai.publisher.repo.granularity</td>  <td>YYYY-MM-DDThh:mm:ssZ</td>  <td>date pattern</td> <td>See OAI-PMH specifications about the Identify verb response</td></tr>
    </tr>
    <tr>
        <td>services.oai.publisher.repo.name</td>  <td>D-Net OAI-PMH Publisher</td>  <td>String</td> <td>See OAI-PMH specifications about the Identify verb response</td></tr>
    </tr>
     <tr>
        <td>services.oai.publisher.baseUrl</td>  <td></td>  <td>OAI-PMH Publisher base URL</td> <td>See OAI-PMH specifications about the Identify verb response</td></tr>
    </tr>
     <tr>
        <td>services.publisher.oai.host</td>  <td>localhost</td>  <td>host that runs a mongodb to be used to store records served by the OAI-PMH publisher</td>
        </tr>
 	<tr>
        <td>services.publisher.oai.port</td>  <td>27017</td>  <td>port of the mongodb to be used to store records served by the OAI-PMH publisher</td>
    </tr>

</table>

#D-Net properties for D-Net Index Service over Solr
<table class="myTable">
<tr>
<th>Property name</th>
<th>Default value</th>
<th>Value type</th>
<th>Description</th>
</tr>
    <tr>
        <td>service.solr.index.jsonConfiguration</td>  <td>
        <code>{
        "id":"solr", "address":"localhost:9983","port":"8983", 
        "host":"localhost", "webContext":"solr",
        "numShards":"1","replicationFactor":"1",
        "feedingShutdownTolerance":"30000", "feedingBufferFlushThreshold":"1000","feedingSimulationMode":"false",
        "luceneMatchVersion":"4.9", "serverLibPath":"../../../../contrib/extraction/lib",
        "filterCacheSize":"512", "filterCacheInitialSize":"512",
        "queryCacheSize":"512","queryCacheInitialSize":"512",
        "documentCacheSize":"512","documentCacheInitialSize":"512",
        "ramBufferSizeMB":"960",
        "mergeFactor":"40",
        "autosoftcommit":"-1","autocommit":"15000",
        "termIndexInterval":"1024","maxIndexingThreads":"8",
        "queryResultWindowSize":"20", "queryResultMaxDocCached":"200"}
         </code>
</td> <td>json map</td> <td>information about the Solr instance to be used. See Solr documentation for details</td>
    </tr>
</table>