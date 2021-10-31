# Mulesoft Certified Integration Architect - Notes

## Object Store V2
 - Available in every aws region where deployment can happen.
 - Mue4 supports OSV2 but not OSV1
 - On-Prem apps can use via REST API
 - **It persists in same region of deployed app.**
 > In order to move both apps and object store to different region, need to delete and reupload, this action will lost the existing data.
 -To use Object Store v2, ensure that the Persistent option is selected for the object store configuration reference in the Global Elements page. If the XML includes persistent="false", the app does not use Object Store v2.
 
 ### What are the limits on an object store?
      The total size of an object store is not limited, but each value is limited to 10 MB.

      Every API call (via the connector or API) to Object Store v2 counts toward the API rate limiting (transactions per second (TPS)):

      Base subscription: 10 TPS per app

      Premium add-on subscription: 100 TPS per app

 ### What is the maximum number of keys that can persist in Object Store v2?
      In Object Store v2, there is no limit on the number of keys per app.

 ### How many characters can a key have?
      The maximum number of characters in a key is 256.

      Object Store v2 doesn’t support using pipe (|) characters in keys.
 
### How long can data persist in Object Store v2?
      The maximum TTL (time to live) is 2592000 seconds (30 days).

      The entryTtl value, specified in the global configuration parameters for the connector, determines when to evict key-value pairs from the store.

      The TTL can be either rolling or static.
      Mule versions 4.2.1 and later:

        - Object stores have a rolling TTL by default.

        - If the data is accessed at least once a week, the TTL is extended for 30 days. Otherwise, it is evicted in the next 7 to 30 days from the most recent expiration date.

        Rolling TTL is available for all apps using Mule version 4.2.1 and later.
      Mule versions earlier than 4.2.1:

        - Object stores have a static TTL of 30 days by default.

        - Updating the data (for example, by overwriting the key) resets the TTL.
        
## Data Integration Patterns
  ### Migration
  Migration is the act of moving data from one system to the other. A migration contains a source system where the data resides at prior to execution, a criteria which determines the scope of the data to be migrated, a transformation that the data set will go through, a destination system where the data will be inserted, and an ability to capture the results of the migration to know the final state vs the desired state.
  
  ### Broadcast
  Broadcast can also be called “one way sync from one to many,” and it refers to moving data from a single source system to many destination systems in an ongoing and real-time (or near real-time), basis.
  
  ### Bi-directional sync 
  The bi-directional sync data integration pattern is the act of combining two datasets in two different systems so that they behave as one, while respecting their need to exist as different datasets. This type of integration need comes from having different tools or different systems for accomplishing different functions on the same dataset.
  
  ### Correlation
  The correlation data integration pattern is a design that identifies the intersection of two data sets and does a bi-directional synchronization of that scoped dataset only if that item occurs in both systems naturally. This is similar to how the bi-directional pattern synchronizes the union of the scoped dataset, correlation synchronizes the intersection.
 
  ### Aggregation
  Aggregation is the act of taking or receiving data from multiple systems and inserting into one. For example, customer data integration could reside in three different systems, and a data analyst might want to generate a report which uses data from all of them. One could create a daily migration from each of those systems to a data repository and then query that against that database. But then there would be another database to keep track of and keep synchronized.
  
  
## Semantic Versioning
 Given a version number MAJOR.MINOR.PATCH, increment the:

  MAJOR version when you make incompatible API changes,
  MINOR version when you add functionality in a backwards compatible manner, and
  PATCH version when you make backwards compatible bug fixes.
  Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.
  Example: 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0.
        
## Build Lifecycle Basics
  validate - validate the project is correct and all necessary information is available
  compile - compile the source code of the project
  test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
  package - take the compiled code and package it in its distributable format, such as a JAR.
  verify - run any checks on results of integration tests to ensure quality criteria are met
  install - install the package into the local repository, for use as a dependency in other projects locally
  deploy - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.
  
## Business Groups
 - Business groups provide more fine-grained control over access to resources.
 - Because business groups are hierarchical, the owner of a parent business group automatically has and retains administrator permissions for any child business group of that parent, even it they make another Organization Administrator user owner of a child business group.

   Conversely, owners of child business groups cannot:

   Access or modify the parent business group or root organization settings

   View the parent business group’s client ID and client secret

- Allocating redistributable resources to a business group makes those resources available only to that business group, which makes them unavailable to the parent organization.
- Users who are assigned the Organization Administrator permission at any level are automatically assigned that same role in any child business group created within that level, so are automatically visible in the child. This does not apply retroactively to users who are assigned the Organization Administrator permission after a child is created.
- All users, regardless of which business groups they are in, belong to the root organization.

## HA Clusture
 ### Prerequisites
  Keep the ports configured for the Mule cluster open.
  If you configure your cluster through Runtime Manager and you use the default ports, keep TCP ports 5701, 5702, and 5703 open.
  If you configure custom ports instead, keep the custom ports open.
  Ensure communication between nodes is open through port 5701.
  Verify the firewall rules in your network to ensure that two-way communication between nodes is enabled.
  If you enable multicast for your cluster, the following requirements also apply in addition to the previous prerequisites:

   - Keep UDP port 54327 open.

   - Enable the multicast IP address: 224.2.2.3.
  There are a number of recommended practices related to clustering. These include:

   - As much as possible, organize your application into a series of steps where each step moves the message from one transactional store to another.

   - If your application processes messages from a non-transactional connector, use a reliability pattern to move them to a transactional store such as a VM or JMS store.

   - Use transactions to process messages from a transactional connector. This ensures that if an error is encountered, the message reprocesses.

   - Use distributed stores such as those used with the VM or JMS connector – these stores are available to an entire cluster. This is preferable to the non-distributed stores used with connectors such as File, FTP, and JDBC – these stores are read by a single node at a time.

   - Use the VM connector to get optimal performance. Use the JMS connector for applications where data needs to be saved after the entire cluster exits.

   Implement reliability patterns to create high reliability applications.
