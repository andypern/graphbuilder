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

Run the script:

pig examples/rdf_example.pig

Will take a minute or so, output should look like:
	
	Success!
	
	Job Stats (time in seconds):
	JobId	Maps	Reduces	MaxMapTime	MinMapTIme	AvgMapTime	MedianMapTime	MaxReduceTime	MinReduceTime	AvgReduceTime	MedianReducetime	Alias	Feature	Outputs
	job_201408261432_0003	1	1	5	5	5	5	4	4	4	4	employees,employees_with_valid_ids,macro_MERGE_DUPLICATE_ELEMENTS_grouped_0,macro_MERGE_DUPLICATE_ELEMENTS_labeled_0,merged,pge,rdf_triples	GROUP_BY	/tmp/rdf_triples,
	
	Input(s):
	Successfully read 13 records (38240 bytes) from: "/mapr/drillram/projects/graphbuilder/examples/data/employees.csv"
	
	Output(s):
	Successfully stored 72 records in: "/tmp/rdf_triples"
	
	Counters:
	Total records written : 72
	Total bytes written : 0
	Spillable Memory Manager spill count : 0
	Total bags proactively spilled: 0
	Total records proactively spilled: 0
	
	Job DAG:
	job_201408261432_0003
	
	
	2014-08-27 16:32:26,532 [main] WARN  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - Encountered Warning UDF_WARNING_1 1 time(s).
	2014-08-27 16:32:26,532 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - Success!
	
	
You can also tail the part-000 file:

	 tail /mapr/drillram/tmp/rdf_triples/part-r-00000 
	http://www.w3.org/2002/07/owl#00009 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00011 .
	http://www.w3.org/2002/07/owl#00009 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00013 .
	http://www.w3.org/2002/07/owl#00010 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00001 .
	http://www.w3.org/2002/07/owl#00010 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00003 .
	http://www.w3.org/2002/07/owl#00010 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00004 .
	http://www.w3.org/2002/07/owl#00010 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00005 .
	http://www.w3.org/2002/07/owl#00010 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00007 .
	http://www.w3.org/2002/07/owl#00010 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00009 .
	http://www.w3.org/2002/07/owl#00011 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00009 .
	http://www.w3.org/2002/07/owl#00013 http://www.w3.org/2002/07/owl#worksUnder http://www.w3.org/2002/07/owl#00009 .		


##Data

	cd /mapr/drillram/projects
	wget http://download.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles.xml.bz2
	