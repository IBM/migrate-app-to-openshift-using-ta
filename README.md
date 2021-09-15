# Transform your traditional on-premises app and deploy it as a containerized app on Red Hat OpenShift Cluster

> **Modernize your Applications using IBM Cloud Transformation Advisor on a Red Hat OpenShift cluster**

IBM Cloud Transformation Advisor, can help modernize IBM WebSphere, Red Hat JBoss, Oracle WebLogic, Apache Tomcat, IBM MQ, WebSphere Message Broker, and IBM Integration Bus applications.

This repo contains the sample web app that is used in the IBM Developer tutorial, "[Transform a traditional on-premises app and deploy it as a containerized app on IBM Cloud or Red Hat OpenShift.](https://developer.ibm.com/tutorials/modernize-apps-with-ibm-transformation-advisor/)."

## Flow

![architecture](doc/source/images/architecture.png)

1. Developer accesses IBM Transformation Advisor on the OpenShift cluster.
2. Developer downloads a custom Data Collector from IBM Transformation Advisor.
3. Developer runs the Data Collector on the traditional WebSphere Application Server host where application(to be migrated) is running.
4. Data Collector analysis is uploaded (automatically or manually).
5. Developer reviews recommendations in Transformation Advisor and creates a migration bundle.
6. Developer downloads migration bundle.
7. Developer deploys the containerized application using migration bundle and accesses the application on cloud.


## License

This code pattern is licensed under the Apache Software License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
