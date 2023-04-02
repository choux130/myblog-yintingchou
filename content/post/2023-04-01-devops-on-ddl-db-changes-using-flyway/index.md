---
title: 'DevOps on DDL database changes using Flyway'
author: Yin-Ting
date: '2023-04-01'
slug: devops-on-ddl-db-changes-using-flyway
aliases: 
    - /posts/devops-on-ddl-db-changes-using-flyway/
categories:
    - DevOps
    - Data Engineering
tags:
    - MySQL
    - phpMyAdmin 
    - Jenkins
    - Gitbucket
    - Flyway
draft: no
---

Few days ago, I asked myself a question about how can you better manage all the database changes defined in SQL scripts with DDL (Data Definition Language)? Maybe try to use version control on the SQL code and then use DevOps pipelines to achieve continuous deployment. Then, I came up with a poc solution, [choux130/DevOps_In_DE/jenkins_mysql_flyway](https://github.com/choux130/DevOps_In_DE/tree/main/jenkins_mysql_flyway). 

This is the architecture overview, 
<p align="center">
<img src="https://raw.githubusercontent.com/choux130/DevOps_In_DE/main/jenkins_mysql_flyway/jenkins_mysql_flyway.png" width="600" title="architecture_diagram">
</p>

<br/>

Here are some undo works on this poc and I wish I can improve it in the future.
* Figure out how to automatically run the pipeline in `jenkins` by checking in the code to the repo in `gitbucket` instead of manually hitting `Build Now` on the ui.
    * For some reasons, the webhook to connect `jenkins` and `gitbucket` was not working as expected this time. 
        * [How to trigger auto build in Jenkins via Gitbucket's webhook?](https://stackoverflow.com/questions/49574298/how-to-trigger-auto-build-in-jenkins-via-gitbuckets-webhook)
        * [How to auto build a job in jenkins if there is any change in code on Github repository](https://www.edureka.co/community/49753/auto-build-job-jenkins-there-change-code-github-repository)
* Think about how to handle the DML (Data Manipulation Language) changes.
