# Modernize Apps using IBM Transformation Advisor on IBM Cloud Pak for Applications

In this code pattern, we will use Transformation Advisor from IBM Cloud Pak for Application to evaluate an on-premise traditional WebSphere application for deployment on OpenShift Cluster. We'll use Transformation Advisor, download the generated migration bundle and use its recommendations to deploy the app in a Liberty container running on OpenShift. 

A sample web app is provided to demonstrate migration from on-premise to the cloud (OpenShift).

When the reader has completed this code pattern, they will understand how to:

* Use Transformation Advisor to create a custom Data Collector
* Run the custom Data Collector to analyze a traditional WebSphere app
* Review the Transformation Advisor reports to see migration complexity, cost, and recommendations
* Generate artifacts to containerize the app
* Move the modernized app to IBM managed OpenShift Cluster using a generated migration bundle

## Flow

![architecture](doc/source/images/architecture.png)

1. Developer accesses IBM Transformation Advisor on IBM Cloud Pak for Applications
2. Developer downloads a custom Data Collector from IBM Transformation Advisor
3. Developer runs the Data Collector on the traditional WebSphere Application Server host where application(to be migrated) is running
4. Data Collector analysis is uploaded (automatically or manually)
5. Developer reviews recommendations in Transformation Advisor and creates a migration bundle
6. Developer downloads migration bundle
7. Developer uses Docker to build an image and upload it to OpenShift Docker Registry
8. Developer creates an app using the pushed image and runs the containerized app


## Pre-requisites

* [IBM Cloud account](https://cloud.ibm.com/) 
* [IBM managed OpenShift Cluster instance](https://cloud.ibm.com/kubernetes/catalog/create?platformType=openshift&bss_account=46816354d9cb773bae86c226aa74c8cc&ims_account=2001776)
* [OpenShift CLI](https://cloud.ibm.com/docs/openshift?topic=openshift-openshift-cli)
* [Docker](https://www.docker.com/)

## Steps

1. [Install IBM Cloud Pak for Applications](#1-install-ibm-cloud-pak-for-applications)
2. [Get started with the Transformation Advisor](#2-get-started-with-the-transformation-advisor)
3. [Download and run the Data Collector](#3-download-and-run-the-data-collector)
4. [Upload results, if necessary](#4-upload-results-if-necessary)
5. [View the recommendations and cost estimates](#5-view-the-recommendations-and-cost-estimates)
6. [Complete your migration bundle](#6-complete-your-migration-bundle)
7. [Deploy your application on CP4A](#7-deploy-your-application-on-cp4a)

## 1. Install IBM Cloud Pak for Applications

Please refer to this [video](https://www.youtube.com/watch?v=gBI0ApHUFSs) to install IBM Cloud Pak for Applications.

## 2. Get started with the Transformation Advisor

To get started with the Transformation Advisor:

* Select the project `cp4a` on the Openshift console, and open `Cloud Pak for Applications`.
 ![open_cloudpak](doc/source/images/open_cloudpak.png)
 
* Open `Transformation Advisor`
  ![open_ta](doc/source/images/open_ta.png)

* On the welcome screen, click the `+` to add a workspace.

  ![welcome_to_ta](doc/source/images/welcome_to_ta.png)

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

![screen11](doc/source/images/screen11.png)

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

Select `View migration plan` for the Application you wish to migrate.

![add_dependencies](doc/source/images/plan.png)

Transformation Advisor will automatically generate the artifacts you need to get your application deployed and running in a Liberty container on OpenShift Cluster, including...

* server.xml 
* pom.xml
* Dockerfile
* Operator Resources
* ..

![add_dependencies](doc/source/images/added_war.png)

Click on `Download bundle` to download the bundle.

Unzip the bundle contents into a folder `migrated_app`. Now, let us add the source code and other dependencies to complete the bundle.

Clone this repo by running the below command:
```
git clone https://github.com/IBM/migrate-app-to-openshift-using-cp4a
```
This creates a folder `migrate-app-to-openshift-using-cp4a` with all the contents from the repo.

Let us now copy the sources and dependencies to the `migrated-app` folder:
- Copy the folder `migrate-app-to-openshift-using-cp4a/src` with contents to `migrated_app` folder.
- Copy the file `migrate-app-to-openshift-using-cp4a/pom.xml` to `migrated_app` folder.
- Copy the contents under the folder `migrate-app-to-openshift-using-cp4a/WebContent` to `migrated_app/src/main/webapp`.
- Modify the file `location` attribute of the application tag in the file `migrated_app/src/main/liberty/config/server.xml` as shown below:
```
<application id="modresorts" location="modresorts-1.0.war" name="modresorts-1_0_war" type="war"/>
```
Now the migration bundle is complete and is ready to be deployed on IBM Cloud Pak for Applications.

## 7. Deploy your application on CP4A

***Login to OpenShift Cluster using CLI***

Go to `IBM Cloud Dashboard > Clusters > Click on your OpenShift Cluster > OpenShift web console` as shown.

![openshift-web-console](doc/source/images/openshift-web-console.png)

It will open a OpenShift web console for you. Get the login command from console as shown in the below snapshot.

![copy-login-command](doc/source/images/copy-login-command.png)

Go to terminal and paste the copied login command. You will get logged into your OpenShift cluster.

```
   $ oc login https://xxx.containers.cloud.ibm.com:xxx --token=xxxx
   Logged into "https://xxx.containers.cloud.ibm.com:xxx" as "xxxx" using the token provided.
   
   # Create a new project to run your application
   $ oc new-project <project-name>
```

***Create/Get a route for the docker-registry***

```
   $ oc project default
   $ oc get route docker-registry
    # If it does not show any route for docker-registry, follow the below steps to create route for deocker-registry.
    
   $ oc create route reencrypt --service=docker-registry
   $ oc get route docker-registry
   NAME              HOST/PORT                                              PATH   SERVICES     PORT   TERMINATION   WILDCARD
   docker-registry   docker-registry-default.xxxx.containers.appdomain.cloud  docker-registry   5000-tcp   reencrypt     None
    
```
   
   The docker-registry URL is `docker-registry-default.<cluster_name>-<ID_string>.<region>.containers.appdomain.cloud`.
   
   Make a note of the Docker registry URL. Set it as a variable. 
   
    ```
    export IMAGE_REGISTRY=docker-registry-default.<cluster_name>-<ID_string>.<region>.containers.appdomain.cloud
    ```
    
***Build and Tag the docker image***

   Change your directory to the directory where you have downloaded artifacts from Transformatoin Advisor. Unzip the downloaded bundle and then change directory to it.
   
   ```
      $ cd <unzipped-downloaded-bundle-directory-name>
      $ ls
      Dockerfile	docs		operator	pom.xml		src
      
      # Build the docker image using the Dockerfile provided
      $ docker build -t modapp:latest .
      $ docker images |head -2       ## it will show the latest build image using the previous command
      
      $ docker tag modapp:latest $IMAGE_REGISTRY/<project-name>/<image_name>:<image_tag>
      
   ```
   Here, 
   * docker-registry URL - the URL which was noted in previous step
   * project-name - the new project created before to run your migrated application
   * image_name - you can choose any name for the image
   * image_tag - you can give any image tag. If you do not provide any tag for your image then default tag `latest` wil be taken.
   
***Push the docker image to OpenShift docker-registry***
   
   To push the image, you need to login to the OpenShift docker-registry using the following command.
   
    ```
     docker login -u $(oc whoami) -p $(oc whoami -t) $IMAGE_REGISTRY
    ```
   
   where -
   * Username is the name displayed on OpenShift web console at the top-right corner after login
   * Use the same token which was used in your OpenShift cluster login command
   * Use the Docker registry URL noted earlier.
 
 After successful login to docker-registry, push the image as:
 
 ```
   $ oc project <project-name>
   $ docker push $IMAGE_REGISTRY/<project-name>/<image_name>:<image_tag>
 ```
 
 ***Deploy the image to OpenShift***
 
 Run the following commands to create an application using the image and to expose it as a service.
 
 ```
   $ oc new-app --image-stream=<image_name>:<image_tag> --name=modapp-openshift
   $ oc expose svc/modapp_openshift
   
   # Verify the pods and services
   $ oc get pods       ## it will show a pod running with modapp-openshift-** name
   $ oc get services   ## it will show a service running with modapp-openshift name
 ```

***Access the migrated app***

   To access the migrated app on OpenShift, get the URL of the app from OpenShift web console.
   
   `OpenShift Web Console > <Go to your project> > Overview`
   
   Access the URL displayed against `modapp-openshift` application on the OpenShift web console.
   It will show you the WebSphere Liberty console by default. Append the context-root for your app at the end of the URL and then your application will be accessible. To find the context root of your application, just check the logs of your application pod as shown. In this case context-root is `modresorts-1_0_war`.
   
   ```
    $ oc logs <pod-name>
    
    For example,
    $ oc logs modapp-openshift-**
    ## Ouput should be like this...
    ...
    [AUDIT   ] CWWKT0016I: Web application available (default_host): ...
    [AUDIT   ] CWWKT0016I: Web application available (default_host): ...
    [AUDIT   ] CWWKT0016I: Web application available (default_host): ...
    [AUDIT   ] CWWKZ0001I: Application modresorts-1_0_war started in 0.904 seconds.
    ...
   ```

## Learn More

* [Build a secure microservices based banking application](https://developer.ibm.com/patterns/build-a-secure-microservices-based-application-with-transactional-flows/)
* [Java EE Application Modernization with OpenShift](https://developer.ibm.com/patterns/jee-app-modernization-with-openshift/)
* [Learn more about IBM Cloud Pak for Application](https://developer.ibm.com/series/ibm-cloud-pak-for-applications-video-series/)

<!-- keep this -->
## License

This code pattern is licensed under the Apache Software License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)


