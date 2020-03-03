# ***** Work in progress ****
# Modernize a web-app to OpenShift using IBM Cloud Pak for Application

In this code pattern, we will use Transformation Advisor from IBM Cloud Pak for Application to evaluate an on-premise traditional WebSphere application for deployment on OpenShift Cluster. We'll use Transformation Advisor, download the generated migration bundle and use its recommendations to deploy the app in a Liberty container running on OpenShift. 

A sample web app is provided to demonstrate migration from on-premise to the cloud (OpenShift).

When the reader has completed this code pattern, they will understand how to:

* Use Transformation Advisor to create a custom Data Collector
* Run the custom Data Collector to analyze a traditional WebSphere app
* Review the Transformation Advisor reports to see migration complexity, cost, and recommendations
* Generate artifacts to containerize the app
* Move the modernized app to IBM managed OpenShift Cluster using a generated migration bundle

## Flow

1. Developer downloads a custom Data Collector from Transformation Advisor
2. Developer runs the Data Collector on the traditional WebSphere Application Server host
3. Data Collector analysis is uploaded (automatically or manually)
4. Developer reviews recommendations in Transformation Advisor and creates a migration bundle
5. Developer downloads migration bundle
6. Developer uses Docker to build an image and upload it to OpenShift Cluster Registry
7. Developer creates an app using the pushed image and runs the containerized app.

## Included components

* Transformation Advisor: Not every workload should move to cloud. The right choice can yield large cost savings and speed time to market. The Transformation Advisor tool can help you decide.
   
* WebSphere Liberty: A dynamic and easy-to-use Java EE application server, with fast startup times, no server restarts to pick up changes, and a simple XML configuration.

## Featured technologies

* OpenShift:
* Cloud: Accessing computer and information technology resources through the Internet.
* Containers: Virtual software objects that include all the elements that an app needs to run.
* Java: A secure, object-oriented programming language for creating applications.

## Pre-requisite

* IBM Cloud account 
* IBM managed OpenShift Cluster instance

## Steps


## Learn More

<!-- keep this -->
## License

This code pattern is licensed under the Apache Software License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)


