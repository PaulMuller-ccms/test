# Clearchoice data warehouse
This repo contains files for the creation of database objects and configuration for the deployment of thsoe database object files.

# How it works
The file .github\workflows\main.yml defines a github action.  Github will run the action when it is triggered or when it is started manually.  Currently, I have the repo set up to trigger a action run when there is a commit to main.
You can look at .github\workflows\main.yml to see what exactly happens when the action runs; but to summarize: 
1. The action starts up an ephemeral ubuntu image.  Think of it like a linux virtual machine that will only exist for the duration of the action.
2. The first thing tht image does is to run a task called mattes/gce-cloudsql-proxy-action@v1.0.1.  This task is something someone (named mattes) has created as an easy frontend to a action which google provided.  As inputs, it takes an GCP postgres instance name and a credential which I created in GCP.  It provides a network path from the ephemeral instance to our GCP database.
3. The next thing that happens is that the ubuntu instance checks out this repo.  The files in the repo are what need to run in the database.  This is how they are accessible to the action.
4. Next, we install liquibase on the image.  This is done by running two unix commands - one to download the liquibase code, the next to extract it to the instance filesystem.
5. After that, I have a debug step, which i've sometimes commented out and other times have uncommented.  Its been useful to run commands on the instance so I can see how its configured and whats on its filesystem.
6. Finally, we run the liquibase command "update".  Thats that command which applies changes to the database.  There are plenty of other liquibase commandes; but update is the one we'll be using the most.
