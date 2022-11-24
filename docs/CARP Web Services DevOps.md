This page contain documentation on how to do different DevOps tasks for the server-side part of CARP – CARP Web Services (CAWS).

## Overall Architecture

### Hosting Servers

Currently we are using two "physical" (virtual) servers:

| Name | URL | Who   |
|------|------|------|
| CANS | [https//cans.cachet.dk](https//cans.cachet.dk) | [Digital Ocean](https://www.digitalocean.com) |
| Computerome | [https://carp.computerome.dk](https://carp.computerome.dk) | [DTU Computerome ](https://computerome.dk)|



### CAWS Servers

| Name | Git Branch | Role | Description  |
|------|------|------|--------------|
|`dev` | `develop` | internal development & testing | iterative development and teting |
|`test` | `test`|  external testing of development | testing from external clients, e.g. phones |
|`stage` | `master` | external testing of studies | a production server *identical* to `prod` used for testing protocols and data upload - cannot contain any real data, since this database can be cleared at any time |
|`prod` | `master` | live production | live production where all data is saved and backed up |

> **Note** : the `stage` and `prod` servers must be *identical* and are both to be considered as production servers. Hence any deployment to production must update **BOTH** servers at the same time.

## Deployment

### Building the CARP Webservices to Docker Hub

Once a version runs locally on the developers machine (runs and all test succeeds):

1. Use the commands to build the docker image locally with a prober tag, like `dev`:

 * `docker build -f Dockerfile -t carp-webservices-docker:<tag> .`
 * `docker image ls` 
 * `docker tag <image_id> cphcachet/carp.webservices-docker:<tag>`

2. Push the build to docker hub with a prober tag, like `dev`.

 * `docker push cphcachet/carp.webservices-docker:<tag>`

3. Check if it has been pushed correctly here [docker hub](https://hub.docker.com/repositories)

Remember to use `docker login` if you are not authenticated yet.

### Building the CARP Dashboard

See the [`carp.webservices.dashboard`](https://github.com/cph-cachet/carp.webservices.dashboard#deployment) for a detailed description of this.

### Deployment to Servers

1. Go to the [cans portainer](https://cans.cachet.dk/portainer/#!/1/docker/containers) or [computerome portainer](https://carp.computerome.dk/portainer/#!/1/docker/containers). If the container is already running then just click "Recreate" and toggle the "Pull latest image" button. Portainer will automatically restart the container with the new image. Else, go to step 2.

2. Deploy from [docker hub](https://hub.docker.com/orgs/cphcachet/repositories)
  * name container following this naming convention: `carp-<projectName>-<env>`
  * name image following this naming convention: `cphcachet/carp.<projectName>-docker:<env>`
    * where `<env>` is the environment - `dev | test | stage | prod`
  * use network = `host`
  * set Restart Policy = `always`
  * for Spring services, set Env name=`SPRING_PROFILES_ACTIVE` to one of `development | testing | staging | production`
  * bind a writable volume from container `/home/carp/storage/development/<storage-folder>` to the same folder on host, where `<storage-folder>` refers to one of - `development | testing | staging | production`

3. Deploy the following repositories from docker hub (these may exists for both dev, test and staging/development)
  * cphcachet/carp.config-server
  * cphcachet/carp.api-gateway
  * cphcachet/carp.webservices-docker
  * cphcachet/carp-webservice-dashboard

4. Clean up by removing both container and images

### CARP Jenkins

A new instance of [CARP Jenkins](http://146.190.234.94:8080/) has been deployed and has a multibranch pipeline. For the installation procedure [this](https://www.jenkins.io/doc/book/installing/docker/) guide was used. Each time you push a commit to any of the branches that derives from `develop | test | stage | master`, a build takes place and runs through the following stages: `checkout | install | test | deploy | post actions`. You can check the declarative pipeline used via the Jenkinsfile in the root of the repository.

1. Checkout: checks out the changes made
2. Install
  * Runs `yarn install`
  * Copies everything from non_npm_dependencies to node_modules except for @types
  * Copies @types from non_npm_dependencies to node_modules/types
3. Test: runs `npm run test`
4. Deploy:
  * Reads the .env file from the config files stored in Jenkins based on the branch-name
  * Creates the .env file in the root of the repository
  * Changes the base name in package.json and in src/index.html
  * Runs `yarn build`
  * Builds the docker image and tags it based on the name of the branch. Every commit to master will create a docker image tagged `prod_jenkins`, so if you want to deploy to production, you have to tag the image as prod.
  * Pushes to DockerHub based on the credentials stored in Jenkins
5. Post actions: a webhook will send us a notification to the [CARP Jenkins Slack channel](https://cph-cachet.slack.com/archives/C03BB80L24A) of the result of the pipeling

### Changing configuration properties on the server
Please refer to [`carp.config-server`](https://github.com/cph-cachet/carp.config-server) on updating the properties. Currently every environment is fetching its properties from a Spring Cloud Config server.

## Grafana

Runs on `https://cans.cachet.dk/grafana/`.


## Server Endpoints (NGINX)

To add/edit URL endpoints to the web-api edit the file `/etc/nginx/site-available/default`.

All error logs are compiled to `/var/log/nginx/cans.error.log`.

To spin up several production servers (for load balancing):

 * deploy another prod container from the portainer
 * assign a separate port number
 * in the nginx config file (`default`) list the prod containers running as specified below:

```
upstream production {
   least_conn;
   server localhost:8089;
   server localhost:8090;
}
```



## Q&A

### What are the deployment regulations to Production Servers?   

Always ask Bardram when and what to deploy to production (not dev and test).


### How to check the status of my deployment?

Most can be accessed in the portainer overview. Each container has its own status and logs. The log file can be downloaded for further analysis.

### How to handle rollback/redeploy operations in case of bugs?

Make sure to tag the docker hub image with the commit id. In this manner you can always go back and pull and deploy the previous image via the portainer interface.

Tag the docker hub image like this: `carp.<projectName>-docker:<env>-<git-commit-number>`

### How do you restart a server

1. Login as `root` on the server (using e.g., ZenTermLite). 
2. Run the command `sudo apt-get update -y && sudo apt-get upgrade -y` to update and upgrade server packages like Postgres, security, .. (on Computerome, this should be handled by them, though).
3. Run the command `reboot`.



### How to clear the database?

* Stop all the server containers 
* In a database tool (e.g., DataGrip) **drop** the database
* Start all the server containers (on start, all needed database tables are created)

Alternatively log in as a root on the server and use the `deployment.sh` script.

* log in as root
* go to `/home/git/carp/`
* go to the service you want to update (e.g., `carp.webservices-prod`)
* do a `git pull` (if needed)
* usr `bash deployment.sh testing down` to stop the server
* usr `bash deployment.sh testing up` to restart the server
* use `bash deployment.sh testing reset_db` to reset the DB



### How to clean up studies, participants, data?

This is not supported by CARP (yet - will be part of carp-core v. 35+ upgrade). So - for now, this has to be done via the database.

#### Studies

On the database you can find studies with a specific study id by:

```
SELECT
*
FROM studies t
WHERE t.snapshot ->> 'studyId' IN
(
       '855355e8-2125-4f13-a2ee-f4606d479cef',
);
```

This will return a list of studies (rows). From this take the PostGres `id` (not the CARP UUID), and run:


```
DELETE FROM study_participants WHERE study_id = <study_id>;
DELETE FROM studies WHERE id = <study_id>;
```

Note that this will not delete deployments or data - only the study.

Also - it is best to stop the deployments (i.e., the participants enrolled in the study), before deleting the study. In this way, there are no pending invitations to a study which no longer exists.

You can also delete invitations and deployments for a study by:

```
DELETE FROM participation_invitations p WHERE deployment_id=<deployment_id>;
DELETE FROM deployments WHERE deployed_from_study_id = <study_id>;
```

Note that the `<deployment_id>` and `<study_id>` are the PostGres IDs (integers) and **NOT** the CARP UUIDs (strings).

#### Accounts

Delete an account with <account_id> (not email) by:

```
DELETE FROM accounts_roles WHERE accounts_id = '<account_id>';
DELETE FROM accounts WHERE id = '<account_id>';
```

#### Data Points

To get / delete data point use statement like:

```
SELECT * FROM data_points d WHERE deployment_id = '<deployment_id>';
```



### How to unblock a blocker user?

There is a Postman endpoint for this. Alternatively go to the database and run:

```
DELETE FROM login_attempts WHERE email = '<account_email>';
```




### How to see the server logs? 

Go to the portainer interface and see the logs for each container running. Download from here for offline analysis.




