# XL Release Custom Report Plugin


[![Travis (.org)](https://img.shields.io/travis/xebialabs-community/xlr-custom-report-plugin)](https://travis-ci.org/xebialabs-community/xlr-custom-report-plugin)
[![GitHub](https://img.shields.io/github/license/xebialabs-community/xlr-custom-report-plugin)](https://opensource.org/licenses/MIT)
[![GitHub All Releases](https://img.shields.io/github/downloads/xebialabs-community/xlr-custom-report-plugin/total)](https://github.com/xebialabs-community/xlr-custom-report-plugin/releases/latest)


## Preface
This document describes the functionality provided by the 'xlr-custom-report-plugin'

## Overview
This plugin allows the user to generate all sorts of custom reports and extract them out using a custom REST Endpoint or through a Simple User Inteface. The goal of the plugin is to provide a framework so that users capable of writing jython/python scripting to pull data out of XL Release can provide a script and that is then utilized to pull a report. 

This also gives an opportunity to users to experience the power of XL Release extension framework that how easily it can be extended using   
-  Custom UI  
-  XML DSL   
-  Custom REST Endpoints  
-  Jython/Python  

**!! IMPORTANT !!**   

- This plugin is a pure community effort at this point and the script that you provide to pull out a report should be validated such that it doesn't slow down your system.  
- The UI csv download is generated using javascript so it might be limited by the size of data being pulled out by the report and hasn't been tested for the limits.

**!! Benefits !!** 
 
- It helps to pull data faster as all the scripting runs on server side and only one REST API call made to pull that data.  
- One can be creative in writing the script and generating any type of report
- The report from the UI can be downloaded as a CSV report that can be directly used in Excel
- The report from the custom REST Endpoint would be pulled out as json.  rows of comma separated values


## Installation
- Copy the plugin JAR fole into the *'SERVER_HOME/plugins'* directory of XL Release.

## Prerequisites 
Knowledge of Jython/Python scripting and XL Release jython API.  Please refer here for API [Jython API](https://docs.xebialabs.com/jython-docs/#!/xl-release/9.0.x/)

## Configuration
User can configure custom reports by writing their own scripts. A new shared configuration type **report.CustomReport** is introduced by the plugin that gives a text area to provide a custom jython script

**!! IMPORTANT !!**  

- Please do not have any print statements in your report script  
- The script should keep adding comma separated values in a variable called `report`
- Every row being appended should have a new line separator

#### Snapshot 1

![](images/snap1.png)

#### Snapshot 2

![](images/snap2.png)

## GUI Usage
Click on the top level custom menu **CustomReport** and you will see the following UI. Enter a folder path if desired and click Refresh.  The dropdown pulls the list of all reports configured under Shared Configuration and all reports visible from the folder path if present.
Click Refresh each time you change the folder path input.

#### Snapshot 3a

![](images/snap3a.png)


Click on the **Test Report** to validate the execution and it also shows the output or errors

#### Snapshot 4a

![](images/snap4a.png)

Click on the **Generate CSV** and the system would download a CSV file that contains that output. you can open it up in Excel and use it to your convenience.

#### Snapshot 5

![](images/snap5.png)

## REST Endpoint Usage

Here's how you can also call the custom REST API to pull the data externally.  The format for the folder path is MyFolder/MySubfolder/etc., using the path visible in the Folder tree on the Design panel.


**REST API** : http://localhost:5516/api/extension/reportlist?folderPath=< folder path> ( GET, basicauth ).

Pulls the list of custom reports

#### Snapshot 6
![](images/snap6.png)

**REST API** : http://localhost:6516/api/extension/report?folderPath=< folder path >&type=< reportname > ( GET, basicauth ).

Pulls the actual report

#### Snapshot 7
![](images/snap7.png)







## Example

#### Simple dummy report
```
report = "1,2,3,4,5\n6,7,8,9,10\n"
```

#### Pull custom fields from a release
```
report =  "Template,Phase Name,Task Title,Owner,Team\n"
release = releaseApi.searchReleasesByTitle("Configure XL Release")[0]
for p in release.phases:
  for t in p.tasks:
    report = "%s%s,%s,%s,%s,%s\n" % (report,release.title, p.title,t.title, t.owner, t.team)
```


