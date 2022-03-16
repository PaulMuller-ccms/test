# Clearchoice data warehouse
This repo contains files for the creation of database objects and configuration for the deployment of thsoe database object files.

# how it works
We are using a github action, which has a liquibase task.  This is very convenient as we don't have to worry about installing liquibase anywhere.  Instead, github spinds up an ephemeral container which has liquibase isntalled, it copies the repo there, and the plugin issues the command(s) in the config file.

