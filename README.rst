Snowflake Hive Metastore Connector
**********************************

.. image:: http://img.shields.io/:license-Apache%202-brightgreen.svg
    :target: http://www.apache.org/licenses/LICENSE-2.0.txt
.. image:: https://travis-ci.com/snowflakedb/snowflake-hive-metastore-connector.svg?branch=master
    :target: https://travis-ci.com/snowflakedb/snowflake-hive-metastore-connector
.. image:: https://codecov.io/gh/snowflakedb/snowflake-hive-metastore-connector/branch/master/graph/badge.svg
    :target: https://codecov.io/gh/snowflakedb/snowflake-hive-metastore-connector
.. image:: https://maven-badges.herokuapp.com/maven-central/net.snowflake/snowflake-hive-metastore-connector/badge.svg?style=plastic   
    :target: http://repo2.maven.org/maven2/net/snowflake/snowflake-hive-metastore-connector/

The Snowflake Hive metastore connector provides an easy way to query Hive-managed data via Snowflake. Once installed, the connector listens to Hive metastore events and creates the equivalent Snowflake objects. See also: https://docs.snowflake.net/manuals/user-guide/tables-external-hive.html

Installation:
=============

#. Create a new file named 'snowflake-config.xml' to the same directory that contains hive-site.xml. This will be the configuration file for the connector. This configuration file should look like:

   .. code-block:: xml
 
     <configuration>
       <property>
         <name>snowflake.jdbc.username</name>
         <value>...</value>
       </property>
       <property>
         <name>snowflake.jdbc.password</name>
         <value>...</value>
       </property>
       <property>
         <name>snowflake.jdbc.role</name>
         <value>...</value>
       </property>
       <property>
         <name>snowflake.jdbc.account</name>
         <value>...</value>
       </property>
       <property>
         <name>snowflake.jdbc.db</name>
         <value>...</value>
       </property>
       <property>
         <name>snowflake.jdbc.schema</name>
         <value>...</value>
       </property>
       <property>
         <name>snowflake.jdbc.connection</name>
         <value>jdbc:snowflake://account.snowflakecomputing.com</value>
       </property>
       <property>
         <name>snowflake.hive-metastore-listener.integration</name>
         <value>...</value>
       </property>
     </configuration>

#. Package the jar by running:

   .. code-block:: bash

     mvn package
   
   Or download the jar from Maven: https://repo1.maven.org/maven2/net/snowflake/

#. Copy the jar into the Hive classpath

#. Update the Hive metastore configuration to point to the listener. To do this, simply add the following section to hive-site.xml:

   .. code-block:: xml

     <configuration>
       ...
       <property>
         <name>hive.metastore.event.listeners</name>
         <value>net.snowflake.hivemetastoreconnector.SnowflakeHiveListener</value>
       </property>
     </configuration>
    
#. Restart the Hive metastore.

Usage:
======

#. In Hive, touch partitions to be used in Snowflake:

   .. code-block::

     alter table <table_name> touch partition <partition_spec>;

   Or for non-partitioned Hive tables:

   .. code-block::

     alter table <table_name> touch;

#. Query the table in Snowflake:

   .. code-block::

     select * from <table_name>;
