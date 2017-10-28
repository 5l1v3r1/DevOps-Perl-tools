Hari Sekhon Tools
=================
[![Build Status](https://travis-ci.org/HariSekhon/tools.svg?branch=master)](https://travis-ci.org/HariSekhon/tools)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/1769cc854b5246968ee2bae1818f771a)](https://www.codacy.com/app/harisekhon/tools)
[![GitHub stars](https://img.shields.io/github/stars/harisekhon/tools.svg)](https://github.com/harisekhon/tools/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/harisekhon/tools.svg)](https://github.com/harisekhon/tools/network)
[![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20OS%20X-blue.svg)](https://github.com/harisekhon/tools#hari-sekhon-tools)
[![DockerHub](https://img.shields.io/badge/docker-available-blue.svg)](https://hub.docker.com/r/harisekhon/tools/)
[![](https://images.microbadger.com/badges/image/harisekhon/tools.svg)](http://microbadger.com/#/images/harisekhon/tools)

### Hadoop, Hive, Solr, NoSQL, Web, Linux Tools ###

A few of the Hadoop, NoSQL, Web & Linux tools I've written over the years. All programs have --help to list the available options.

For many more tools see [PyTools](https://github.com/harisekhon/pytools) and the [Advanced Nagios Plugins Collection](https://github.com/harisekhon/nagios-plugins) which contains many more Hadoop, NoSQL and Linux/Web tools.

Hari Sekhon

Big Data Contractor, United Kingdom

https://www.linkedin.com/in/harisekhon

##### Make sure you run ```make update``` if updating and not just ```git pull``` as you will often need the latest library submodule and possibly new upstream libraries. #####

### Quick Start ###

#### Ready to run Docker image #####

All programs and their pre-compiled dependencies can be found ready to run on [DockerHub](https://hub.docker.com/r/harisekhon/tools/).

List all programs:
```
docker run harisekhon/tools
```
Run any given program:
```
docker run harisekhon/tools <program> <args>
```

#### Automated Build from source #####

```
git clone https://github.com/harisekhon/tools
cd tools
make
```

The 'make' command will initialize my library submodule and  use 'sudo' to install the required system packages and CPAN modules. If you want more control over what is installed you must follow the [Manual Setup](https://github.com/harisekhon/tools#manual-setup) section instead.

### Usage ###

All programs come with a ```--help``` switch which includes a program description and the list of command line options.

Environment variables are supported for convenience and also to hide credentials from being exposed in the process list eg. ```$PASSWORD```. These are indicated in the ```--help``` descriptions in brackets next to each option and often have more specific overrides with higher precedence eg. ```$SOLR_HOST``` takes priority over ```$HOST```.


### A Sample of cool Programs in this Toolbox ###

##### NOTE: Hadoop HDFS API Tools, Pig => Elasticsearch/Solr, Pig Jython UDFs and authenticated PySpark IPython Notebook have moved to my [PyTools](https://github.com/harisekhon/pytools) repo. #####

- ```hive_to_elasticsearch.pl``` - bulk indexes structured Hive tables in Hadoop to Elasticsearch clusters - includes support for Kerberos, Hive partitioned tables with selected partitions, selected columns, index creation with configurable sharding, index aliasing and optimization
- ```scrub.pl``` - anonymizes your configs / logs for pasting to online forums, Apache Jira tickets etc. Replaces hostnames/domains/FQDNs, email addresses, IP + MAC addresses, Kerberos principals, Cisco/Juniper passwords/shared keys and SNMP strings, as well as taking a configuration file of your Name/Company/Project/Database/Tables as regex to be able to also easily cover things like table naming conventions etc. Each replacement is replaced with a placeholder token indicating what was replaced (eg. ```<fqdn>```, ```<password>```, ```<custom>```), and there is even an --ip-prefix switch to leave the last IP octect to aid in cluster debugging to still see differentiated nodes communicating with each other to compare configs and log communications
- ```solr_cli.pl``` - Solr command line tool with shortcuts under ```solr/``` which make it much easier and quicker to use the Solr APIs instead of always using long tedious curl commands. Supports a lot of environments variables and tricks to allow for minimal typing when administering a Solr/SolrCloud cluster via the Collections and Cores APIs
- ```sqlcase.pl / *case.pl``` - fixes capitalization of keywords in files or stdin for SQL (Hive, Impala, Cassandra CQL, Couchbase N1QL, MySQL, PostgreSQL, Apache Drill, Microsoft SQL Server, Oracle), Pig Latin, Neo4j, InfluxDB and Docker. More specific ```*case.pl``` command calls limit case rewriting to the targeted platform for tighter control to avoid recasing things that may be keywords in other SQL[-like] dialects. Primarily written to help clean up docs and SQL scripts.
- ```watch_url.pl``` - watches a given url, outputting status code and optionally selected output, useful for debugging web farms behind load balancers and seeing the distribution to different servers (tip: set a /hostname handler to return which server you're hitting for each request in real-time)
- ```watch_nginx_stats.pl``` - watches nginx stats via the HttpStubStatusModule module
- ```diffnet.pl``` - print net line additions/removals from diff / patch files or stdin
- ```xml_diff.pl / hadoop_config_diff.pl``` - tool to help find differences between XML / Hadoop configs, can diff XML from HTTP addresses to diff live running clusters
- ```java_show_classpath.pl``` - shows java classpaths of a running Java program in a sane way
- ```datameer_config_git.pl``` - revision controls Datameer configurations from API to Git
- ```ibm_bigsheets_config_git.pl``` - revision controls IBM BigSheets configurations from API to Git
- ```ambari_freeipa_kerberos_setup.pl``` - Automates Hadoop cluster security Kerberos setup of FreeIPA principals and keytab distribution to the cluster nodes. Designed for Hortonworks HDP but now that other vendors such as IBM and Pivotal are standarizing on Ambari it should work the same for those distributions as well.

#### Manual Setup ####

Enter the tools directory and run git submodule init and git submodule update to fetch my library repo and then install the CPAN modules as mentioned further down:

```
git clone https://github.com/harisekhon/tools
cd tools
git submodule init
git submodule update
```

Then proceed to install the CPAN modules below by hand.

###### CPAN Modules ######

Install the following CPAN modules using the cpan command, use sudo if you're not root:

```
sudo cpan JSON LWP::Simple LWP::UserAgent Term::ReadKey Text::Unidecode Time::HiRes XML::LibXML XML::Validate
```

You're now ready to use these programs.

#### Configuration for Strict Domain / FQDN validation ####

Strict validations include host/domain/FQDNs using TLDs which are populated from the official IANA list. This is done via my [Lib](https://github.com/harisekhon/lib) submodule - see there for details on configuring this to permit custom TLDs like ```.local``` or ```.intranet``` (both supported by default).

### Updating ###

Run ```make update```. This will git pull and then git submodule update which is necessary to pick up corresponding library updates.

If you update often and want to just quickly git pull + submodule update but skip rebuilding all those dependencies each time then run ```make update-no-recompile``` (will miss new library dependencies - do full ```make update``` if you encounter issues).

### Testing

[Continuous Integration](https://travis-ci.org/HariSekhon/tools) is run on this repo with tests for success and failure scenarios:
- unit tests for the custom supporting [perl library](https://github.com/harisekhon/lib)
- integration tests of the top level programs using the libraries for things like option parsing
- [functional tests](https://github.com/HariSekhon/tools/tree/master/tests) for the top level programs using local test data and [Docker containers](https://hub.docker.com/u/harisekhon/)

To trigger all tests run:

```
make test
```

which will start with the underlying libraries, then move on to top level integration tests and functional tests using docker containers if docker is available.

### Contributions ###

Patches, improvements and even general feedback are welcome in the form of GitHub pull requests and issue tickets.

### See Also ###

* [PyTools](https://github.com/harisekhon/pytools) - 50+ tools for Hadoop, Spark (PySpark), Pig => Solr / Elasticsearch indexers, Pig Jython UDFs, Ambari Blueprints, AWS CloudFormation templates, HBase, Linux, IPython Notebook, Data converters between different data formats and syntactic validators for Avro, Parquet, CSV, JSON, INI (Java Properties), LDAP LDIF, XML, YAML...

* [The Advanced Nagios Plugins Collection](https://github.com/harisekhon/nagios-plugins) - 350+ programs for Nagios monitoring your Hadoop & NoSQL clusters. Covers every Hadoop vendor's management API and every major NoSQL technology (HBase, Cassandra, MongoDB, Elasticsearch, Solr, Riak, Redis etc.) as well as message queues (Kafka, RabbitMQ), continuous integration (Jenkins, Travis CI) and traditional infrastructure (SSL, Whois, DNS, Linux)

* [Perl Lib](https://github.com/harisekhon/lib) - my personal Perl library leveraged in this repo as a submodule

* [PyLib](https://github.com/harisekhon/pylib) - Python port of the above library

* [Spark => Elasticsearch](https://github.com/harisekhon/spark-to-elasticsearch) - Scala application to index from Spark to Elasticsearch. Used to index data in Hadoop clusters or local data via Spark standalone. This started as a Scala Spark port of ```pig-text-to-elasticsearch.pig``` from my [PyTools](https://github.com/harisekhon/pytools) repo.

You might also be interested in the following really nice Jupyter notebook for HDFS space analysis created by another Hortonworks guy Jonas Straub:

* https://github.com/mr-jstraub/HDFSQuota/blob/master/HDFSQuota.ipynb
