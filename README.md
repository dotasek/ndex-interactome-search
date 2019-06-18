
[jetty]: http://eclipse.org/jetty/
[maven]: http://maven.apache.org/
[java]: https://www.oracle.com/java/index.html
[git]: https://git-scm.com/

[make]: https://www.gnu.org/software/make

Interactome Search REST Service
===============================



Provides an interactome search REST service.
This service runs using an embedded [Jetty][jetty] server and is invoked
from the command line. 

Requirements
============

* Centos 7+, Ubuntu 12+, and most other linux distributions should work
* [Java][java] 8+ **(jdk to build)**
* [Make][make] **(to build)**
* [Maven][maven] 3.3 or higher **(to build)** -- tested with 3.6

Special Java modules to install (cause we haven't put these into maven central)

* [ndex-java-client](https://github.com/ndexbio/ndex-java-client) built and installed via `mvn install`

*TODO* requires **/opt/ndex/data/** and other pre-existing resources to exist.

Building the Interactome Search REST Service  
=================================

Interactome Search REST Service build requirements:

* [Java 8+][java] JDK
* [Make][make] **(to build)**
* [Maven][maven] 3.0 or higher **(to build)**


Commands below build the interactome REST Service assuming machine has [Git][git] command line tools 
installed and above Java modules have been installed:

```Bash
# In lieu of git one can just download repo and unzip it
git clone https://github.com/ndexbio/ndex-interactome-search.git

cd ndex-interactome-search
mvn clean install -DskipTests=true
```

The above command will create a jar file under **target/** named  
**interactomeSearch-\<VERSION\>.jar** that
is a command line application

Running Interactome Search REST Service
===============================

The following steps cover how to configure and run the Interactome Search REST service.
In the steps below **interactomeSearch.jar** refers to the jar
created previously named **interactomeSearch-\<VERSION\>.jar**

### Step 1 Create directories and copy template file

**/gitpath** is the directory into which you cloned **ndex-interactome-search**

```bash
# create directory
mkdir -p interactomedb/logs

# copy template.cx file to db directory
cp /gitpath/ndex-interactome-search/template.cx ./interactomedb/template.cx
```

### Step 2 Create genedb.mv.db file

*TODO* how do we handle /opt/ndex/data?

```bash
# create genedb.mv.db
java -classpath ./interactomedb/interactomeSearch.jar org.ndexbio.interactomesearch.GeneSymbolIndexer ./interactome/genedb /opt/ndex/data/ 
```

### Step 3 Run the service

**servicespath** is the global path at which **interactomedb** resides.

```bash
cd interactomedb
java -Xmx1g -Dndex.host="http://dev.ndexbio.org/v2" -Dndex.interactomehost=dev.ndexbio.org -Dndex.interactomedb=/servicespath/interactomedb -Dndex.queryport=8287 -jar interactomeSearch.jar
```

COPYRIGHT AND LICENSE
=====================

TODO

Acknowledgements
================

TODO
