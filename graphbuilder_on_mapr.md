#Graphbuilder on MapR

Loosely following:

* https://github.com/01org/graphbuilder/tree/2.0.alpha
* https://01.org/graphbuilder/documentation/how-run-demo-application



##cluster
* 3.1.1
* hbase-master
* hbase-region-server on all nodes
* mapr-pig on all nodes



##pre-req's
	
	yum install -y git wget
	
install maven
	
	
##proceedure


	cd /mapr/graphbuilder
	mkdir projects
	cd projects
	
	git clone https://github.com/01org/graphbuilder -b 2.0.alpha
>branch is important, it grabs example files.

	
	cd graphbuilder
	

	mvn clean install -DskipTests

> it'll error out otherwise...


## pig scripts

modify rdf_example.pig to fix pathnames:

	employees = LOAD '/mapr/drillram/projects/graphbuilder/examples/data/employees.csv' USING PigStorage(',')


	


##Data

	cd /mapr/drillram/projects
	wget http://download.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles.xml.bz2
	