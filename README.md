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

1. [Install IBM Cloud Pak for Application](#1-install-ibm-cloud-pak-for-application)
2. [Get started with the Transformation Advisor](#2-get-started-with-the-transformation-advisor)
3. [Download and run the Data Collector](#3-download-and-run-the-data-collector)
4. [Upload results, if necessary](#4-upload-results-if-necessary)
5. [View the recommendations and cost estimates](#5-view-the-recommendations-and-cost-estimates)
6. [Complete your migration bundle](#6-complete-your-migration-bundle)
7. [Deploy your application](#7-deploy-your-application)

## 1. Install IBM Cloud Pak for Application


## 2. Get started with the Transformation Advisor

To get started with the Transformation Advisor:

* Go to your OpenShift Cluster web-console.
//TODO

* Select `Platform ▷ Transformation`.

  ![run_ta](doc/source/images/run_ta.png)

* On the welcome screen, click the `+` to add a workspace.

  ![welcome_to_ta](doc/source/images/welcome_to_ta.jpg)

* Create a new workspace that will be used to house your recommendations. The workspace name can be any string you want, such as the project name or the name for the portfolio of applications you will be analysing -- basically anything that will help you to easily identify your work when you return to it at a later date.

  ![new_workspace](doc/source/images/new_workspace.png)

* You will then be asked to enter a collection name. This is an opportunity for you to subdivide your work even further into a more focused grouping. It would typically be associated with a single run of the Data Collector and may be the name of the individual WAS server that you will be running the Data Collector on. It can be any string and can be deleted later -– so don’t be afraid to get creative!

  ![new_collection](doc/source/images/new_collection.png)

* Hit `Let’s Go`.

## 3. Download and run the Data Collector

> If you don't want to run our sample app and the Data Collector in your own WAS environment, you can use the files that we already collected and saved in [data/examples](data/examples). Just upload them in [the next step](#4-upload-results-if-necessary) to continue.

The Data Collector identifies which profiles are associated with the WebSphere installation along with the installed WebSphere and Java versions. It also identifies all WebSphere applications within each deployment manager and standalone profile. The tool generates one folder per profile and places analysis results within that directory.

> Note: The Data Collector will collect configuration information in WAS installations at version 7 or later.

### Download the Data Collector

The Data Collector tab should now display the screen shown below. The Data Collector is a downloadable zip file that needs to be extracted and run on your target server where the applications you wish to migrate are located (i.e., your WAS application server machine). You should choose the correct Data Collector for your target server’s operating system.

* Download the zip file to your browser's download directory.

  ![dc_download](doc/source/images/dc_download.png)

### Install and run

> **WARNING:** The Data Collector is likely to consume a significant amount of resources while gathering data. Therefore, we recommend you run the tool in a pre-production environment. Depending on the number, size and complexity of your applications the Data Collector may take quite some time to execute and upload results.

Once downloaded, follow these steps:

* Copy/FTP from your download directory to your target server. Put the zip file in a directory where you have read-write-execute access.

* Decompress the downloaded file. Your file name will be specific to your platform/version/collection.
  ```
  tar xvfz transformationadvisor-2.1_Linux_example.tgz
  ```

* Go to the Data Collector directory.
  ```
  cd transformationadvisor-2.1
  ```
* Perform analysis of app, .ear and .war files on IBM WebSphere applications.
  ```
  ./bin/transformationadvisor -w <WEBSPHERE_HOME_DIR> -p <PROFILE_NAME> <WSADMIN_USER> <WSADMIN_PASSWORD> -no-version-check
  ```

## 4. Upload results, if necessary

If there is a connection between your system and your new collection, the Data Collector will send your application data for you. Use the `Recommendations` tab to see the results and continue with the following section:
[5. View the recommendations and cost estimates](#5-view-the-recommendations-and-cost-estimates).

If there is no connection, the Data Collector will return a .zip file containing your application data. Use the `Recommendations` tab to upload the .zip file(s).

* Find the results for each profile. These are zip file(s) created by the Data Collector with the same name as the profile. You will find the zip file(s) in the transformationadvisor directory of the Data Collector.

* Copy the zip file(s) to your local system and select them use the `Drop or Add File` button.

* Use the `Upload` button to upload the files.

## 5. View the recommendations and cost estimates

Selecting the `Recommendations` tab after the Data Collector has completed and uploaded results should display a screen similar to that shown below. Please be aware that any cost estimates displayed by the tool are high-level estimates only and may vary widely based on skills and other factors not considered by the tool.

> Note: You can use the `Advanced Settings` gear icon to change the `Dev cost multiplier` and `Overhead cost` and adjust the estimates for your team.

![recommendations](doc/source/images/recommendations.png)

The recommendations tab shows you a table with a summary row for each application found on your application server. Each row contains the following information:

| Column | Description |
| ------ | ----------- |
| | *A drop-down arrow lets you expand the summary row to see the analysis for other targets.* |
| | *Alert icons may appear to indicate apps that are incompatible with a target.* |
| Application | *The name of the EAR/WAR file found on the application server.* |
| | *An indicator to show how complex Transformation Advisor considers this application to be if you were to migrate it to the cloud.* |
| Tech match | *This is a percentage and if less than 100% it indicates that there may be some technologies that are not suitable for the recommended platform. You should investigate the details and ensure your application is actually using the technologies.* |
| Dependencies | *This shows potential external dependencies detected during the scan. Work may be needed to configure access to these external dependencies.* |
| Issues | *This indicates the number and severity of potential issues migrating the application.* |
| Est. dev cost | *This is an estimate in days of the development effort to perform the migration.* |
| Total effort | *This is the total estimate in days of the overhead and development costs in migration up to the point of functional testing.* |
| | *The `Migration plan` button will take you to the Migration page for the application.* |

Each column in the table is sortable. There is also a `Search items` box which allows you to filter out rows of data. You can use the `+` symbol to see only rows that match all your terms (e.g., `Liberty+Simple`). You can filter by complexity using the filter button.

Clicking on your application name will take you to more information about the discovered `Complexity` and `Application Details`. For starters, the complexity rating is explained for you.

![complexity](doc/source/images/complexity.png)

You will also see details for each issue and dependency discovered:

![app_details](doc/source/images/app_details.png)

There will be additional sections to show any technology issues, external dependencies, and additional information related to your application transformation.

Scroll to the end of the recommendations screen to find three links to further detailed reports.

![screen11](doc/source/images/screen11.jpg)

The three reports are described as follows:

### Analysis Report

The binary scanner’s detailed migration report digs deeper to understand the nitty-gritty details of the migration. The detailed report helps with migration issues like deprecated or removed APIs, Java SE version differences, and Java EE behavior differences. Please note that the Transformation Advisor uses a rule system based on common occurring events seen in real applications to enhance the base reports and to provide practical guidance. As a result of this some items may show a different severity level in Transformation Advisor than they do in the detailed binary scanner reports.

![analysis](doc/source/images/analysis.png)

### Technology Report

The binary scanner can examine your application and generate the Application Evaluation Report, which shows which editions of WebSphere Application Server are best suited to run the application. The report provides a list of Java EE programming models that are used by the application, and it indicates on which platforms the application can be supported.

![evaluation](doc/source/images/evaluation.png)

### Inventory Report

The binary scanner has an inventory report that helps you examine what’s in your application including the number of modules and the technologies in those modules. It also gives you a view of all the utility JAR files in the application that tend to accumulate over time. Potential deployment problems and performance considerations are also included.

![inventory](doc/source/images/inventory.png)

## 6. Complete your migration bundle

Select the Application you wish to migrate from the `Recommendations` tab and hit the `Migration plan` button.

Transformation Advisor will automatically generate the artifacts you need to get your application deployed and running in a Liberty container on IBM Cloud Private, including...

* pom.xml
* Dockerfile
* Operator
* Docs
* Source code

**You will need to add the application binary itself (EAR/WAR file) and any external dependencies that may be particular to your application such as database drivers. These files can easily be added on the migration plan page at the click of a button.

![add_dependencies](doc/source/images/added_war.png)

**Once all required application dependencies are uploaded, you will be able to use the buttons to `Download bundle` and/or `Deploy Bundle`.**

Download the bundle using the `Deploy Bundle` button and continue with next step to deploy the app on OpenShift.

## 7. Deploy your application





Access the migrated app on ICP by going to this address (note the fragment for the resorts app example):
```
http://<Ingress IP>:<TCP PORT>/resorts/
```


## Learn More

<!-- keep this -->
## License

This code pattern is licensed under the Apache Software License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)


