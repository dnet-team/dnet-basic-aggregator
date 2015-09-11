# D-Net Software Toolikt

This is a minimal instance of the D-Net software toolkit, a software framework for the realization of aggregative data infrastructures.

Official Web Site: http://www.d-net.research-infrastructures.eu/

Need support? Contact us via email at: dnet-team@isti.cnr.it

This webapp contains the minimal set of services needed to feature:

- Collection of metadata records in oai_dc format via OAI-PMH, FTP, local file system, HTTP.

- Transformation of the collected metadata records into an internal format named DMF (Driver Metadata Format)

- Indexing of DMF records in a Solr full-text index

- OAI-PMH export of aggregated metadata records in DMF and oai_dc formats. More formats can be added at runtime by providing a dedicated XSLT from DMF to the desired target format.

# Installation requirements
This minimal instance can be run on a single machine as web application to be deployed on a Tomcat container. 
## Hardware requirements

Suggested minimal hardware requirements:

- Operating system: almost anything but Windows
- HARD DISK space: mostly depends on the quantity and size of records you are going to collect. A couple of GBs for a small repository (<10K metadata recods) should be fine. See suggestions on installing mongodb below.

## Software requirements
Software required:

* Apache Tomcat 7: the webapp container
* Mongodb >= 2.4: used to store the collected and transformed metadata records. Each collected record will be stored in three separate "versions": original, transformed, pmh-ready, hence enough disk space should be available for mongoDB.
* Solr 4.9.x or 4.10.x: used to make the documents searchable. The solr server should be run using the option '-DzkRun' to instruct solr to start the zookeeper server. 

Note that Tomcat, Solr and Mongodb can be installed in the same machine or in dedicated nodes, although this requires to change some default system properties.

#Running the D-Net web app with Maven
## Maven settings

Either if you want to run the D-Net web app with the Tomcat7 plugin for maven, or you want to build the .war file to deploy on a running tomcat, 
you need maven3 and you must add the following repository into your <code>settings.xml</code>:

```
 <repository>
          <id>dnet-bootstrap-releases</id>
          <name>D-Net Bootstrap Releases</name>
          <url>http://maven.research-infrastructures.eu/nexus/content/repositories/dnet4-bootstrap-release/</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
          <layout>default</layout>
 </repository>
```

We also suggest to add the Tomcat plugin to the plugins group at the bottom of the same file:

```
<pluginGroups>
    <pluginGroup>org.apache.tomcat.maven</pluginGroup>
</pluginGroups>
```

## Testing on local machine:
The D-Net Software is developed in Java using Maven. You can try out the D-Net web app on your local machine with the tomcat7 plugin, provided you are also running a mongodb and a solr server on localhost that are listening to the relative standard ports.

Please note that the solr client used in D-Net needs to interact with the zookeeper server. For simplicity we suggest to use the embedded zookepper instance provided within the solr distribution. By default solr listens on the 8983 port and its embedded zookeeper server on the 9983 port. 

To override properties, you can modify <code>dnet-basic-aggregator/src/main/resources/eu/dnetlib/cnr-site.properties</code>. Please check the Section D-Net Configuration and the PROPERTIES.md file for more information about D-Net properties.

```
> cd dnet-basic-aggregator

> mvn tomcat7:run
```

When you see a log like:
```
52665 [Thread-7] INFO  eu.dnetlib.enabling.is.store.TestContentInitializerJob  - INITIALIZED
```

The webapp should be ready and running at http://localhost:8280/app , where 'app' is the value of the property <code>container.hostname</code> ('app' is the default).


# Deployment on a Tomcat instance

In this distribution you will find a ready-to-deploy war package.

Copy the war file into the Tomcat 7 <code>webapps</code> directory, ensure you have overridden the properties as explained in the D-Net configuration section and restart Tomcat.

When you see a log like:
```
52665 [Thread-7] INFO  eu.dnetlib.enabling.is.store.TestContentInitializerJob  - INITIALIZED
```

The webapp should be ready and running at 

```
http://${container.hostname}:${container.port}/${container.context}
```

If you want to build the web app yourself, then keep reading...


## Building the D-Net web app
The D-Net Software is developed in Java with Maven.

To build the war to use in a Tomcat 7 web app container:

```
 > cd dnet-basic-aggregator

 > mvn package
```

The <code>.war</code> file is then created into the <code>target</code> directory.

#D-Net configuration
Before you start the web application, you need to configure at least the following properties.
For the full list of available properties and their values, check PROPERTIES.md.

Create a file named <code>cnr.override.properties</code> in <code>$yourTomcatHomeDirectory$/common/classes</code> (<code>$yourTomcatHomeDirectory$</code> will likely be something similar to <code>/var/lib/tomcat7</code>)

- <code>container.hostname</code>: the host name where the web app will be running. Default value is <code>localhost</code>. The default value should *only* be used in local development scenarios.
</br>Example: <code>container.hostname = dnet-host.dnet.eu</code>
- <code>container.port</code>: the port where the web app will be running. Default is 8280.
</br>Example: <code>container.port = 8080</code>
- <code>container.context</code>: the name of the web app (i.e. the name of the war file). Default is "app". The default value should *only* be used in local development scenarios.
</br>Example: <code>container.context = is</code>
- <code>dnet.data.path</code>: path to the directory where all D-Net related resources will be saved. An embedded existDB will be automatically installed in this directory during the first start-up. The directory must be writable by the user running tomcat. Default value is <code>/tmp/dnet</code>. The default value should *only* be used in local development scenarios.
</br>Example: <code>dnet.data.path = /var/lib/dnet</code>
- <code>services.aggregator.country</code>: your country code. Default is <code>EU</code> (Europe).
</br>Example: <code>services.aggregator.country = IT</code>
- <code>services.aggregator.name</code>: the name of your aggregator. Default is "D-NET"
</br>Example: <code>services.aggregator.name = TEST_Aggregator</code>. 
- <code>services.mdstore.mongodb.host</code>: the machine hosting mongodb for the storage of metadata records (M[eta]D[ata]Store). Default is <code>localhost</code>.
</br>Example: <code>services.mdstore.mongodb.host = mongodb.dnet.eu</code>
- <code>services.mdstore.mongodb.db</code>: name of the mongodb database to be used for the storage of metadata records. Default is <code>mdstore_minimal</code>.
</br>Example: <code>services.mdstore.mongodb.db = mdstore_1</code>
- <code>dnet.logger.mongo.host</code>: the machine hosting mongodb for the storage of workflow logs. Default is localhost.
</br>Example: <code>dnet.logger.mongo.host = mongo.dnet.eu</code>
- <code>dnet.logger.mongo.db</code>: name of the mongodb database to be used for the storage of workflow logs. Default is "dnet_logs_minimal".
</br>Example: <code>dnet.logger.mongo.db = dnet_logs_1</code>
- <code>services.oai.publisher.repo.name</code>: name of the OAI-PMH Publisher, as it will appear in the OAI Identify response. Default is "D-Net OAI-PMH Publisher".
</br>Example: <code>services.oai.publisher.repo.name = TEST_Aggregator OAI-PMH Publisher</code>
- <code>services.oai.publisher.repo.email</code>: email of the OAI-PMH Publisher administrator, as it will appear in the OAI Identify response. Default is "dnet-admin@mock.it". The default *must not* be used in beta or production system for it is a mock email.
</br>Example: <code>name.surname@valid.mail.com</code>
- <code>dnet.admin.password</code>: md5sum of the password that will allow the user "admin" to login to the D-Net Admin UI. To generate the new password: <code>echo -n "thePassword" | md5sum</code>. Default is "dnet-minimal" (without double quotes). The default value *should always be overridden*.
</br>Example: <code>dnet.admin.password = 9003d1df22eb4d3820015070385194c8</code>, where 9003d1df22eb4d3820015070385194c8 is the md5 for the string "pwd" obtained via the command <code>echo -n "pwd" | md5sum</code>.
- <code>service.solr.index.jsonConfiguration</code>: information about the Solr instance to be used to create full-text indices on the aggregated metadata records. Default value assumes a local Solr instance. Specifically:
<code>
{"id":"solr", "address":"localhost:9983", "port":"8983", "webContext":"solr", "numShards":"1", "replicationFactor":"1", "host":"localhost",	"feedingShutdownTolerance":"30000",	"feedingBufferFlushThreshold":"1000", "feedingSimulationMode":"false", "luceneMatchVersion":"4.9",	"serverLibPath":"../../../../contrib/extraction/lib", "filterCacheSize":"512","filterCacheInitialSize":"512",	"queryCacheSize":"512","queryCacheInitialSize":"512", "documentCacheSize":"512", "documentCacheInitialSize":"512", "ramBufferSizeMB":"960","mergeFactor":"40",	"autosoftcommit":"-1","autocommit":"15000", "termIndexInterval":"1024","maxIndexingThreads":"8", "queryResultWindowSize":"20","queryResultMaxDocCached":"200"} 
</code>

If you are not running the Solr service on the same machine where Tomcat runs, then you need to override the above configuration according to your Solr server installation.
Typically, changing <code>address</code> and <code>host</code> is enough if your Solr server is not configured for sharding and replication.
For more details refer to the Solr documentation.

#Using D-Net

Under the root folder of the project you can find the folder `mock-repository-content`. 
It contains 150 `oai_dc` metadata records you can use to test the functionality of the D-Net software with a Mock Datasource.

* Place the folder in a location that is readable from tomcat 
* Start the container
* Access the Admin UI (`http://${container.hostname}:${container.port}/${container.context}/mvc/ui/index.do`)
  * If you are running via the maven tomcat plugin with the default properties the URL is: `http://localhost:8280/app/mvc/ui/index.do`
* Go on Datasource Management --> Overview and search for "mock"
* Click on "Add metaworkflow" and select the "Collection and Transformation" meta-workflow. This action will associate a meta-workflow (i.e., a workflow of workflows) to the datasource and will create all needed metadata stores.
* Click on the "access params" button on the top right and change the base url to the location where you saved the sample folder (e.g. `file:///dnet/test/mock-repository-content`)
* Click on the meta-workflow "Collection and Transformation" and configure its workflows with the missing parameter for the transformation rule 
  * click on the yellow "parameters" button of the trasnformation workflow and select the rule `dc2dmf_DRIVER`
* Ensure the launch mode is set to "Auto" for each workflow 
* Click on the Launch button of the first ("collect")
* Wait for all the workflows to complete: collect, transform, index, oai, and oaiPostFeed
* Verify that the records get transformed and indexed: click on MD Inspectors --> D-Net content checker and perform some queries
* Verify that the aggregated records are correctly exposed via the built-in OAI-PMH publisher at: 
  * `http://${container.hostname}:${container.port}/${container.context}/mvc/oai/oai.do?verb=ListRecords&metadataPrefix=dmf` for the DMF metadata format
  * `http://${container.hostname}:${container.port}/${container.context}/mvc/oai/oai.do?verb=ListRecords&metadataPrefix=oai_dc` for the OAI_DC metadata format
	
#Need support?
Do not hesitate to contact dnet-team@isti.cnr.it
