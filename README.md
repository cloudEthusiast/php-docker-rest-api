SOLUTIONS TO THE ANSWER:
- A tier 2 infrastructure is a highly available infra,  when a application is deployed to tow availability zone, One VPC, Two se[erate subnets one each of different zone
- To cut cost, is to ahave an auto-scaler that will and HPA pods. this will scale up when traffic is up, and scale down during idle period
- An ingress controller with an HPA will help solve the traffic issue
- A Hpa cloud-sql by GCP is a managed MySql instance that is fully handled . This will take the MySql management budden from the company.

### PRACTICAL ILLUSTRATION

##IAC
We automate the infrastructure with terraform for GKE
- create an accout with gcloud
- create a project  "playground"
- create a service account with BASIC "EDITORS" (for this practice sake) ROLE to playgound project
- Download gcloud sdk to your terninala dn activate the GCLOUD with the service account, Europe-west4 region
- install terraform 
- clone this repo https://github.com/cloudEthusiast/php-docker-rest-api.git
- cd  /php-docker-rest-api.git/IAC-terraform-GKE-KCR-CloudSQL/terraform-google-gke/GKE-MYSQL-TERRAFORM/gke-private-clusster
- terraform init 
- terraform apply -auto-approve #his will deploy -a teir 2 GKE and  one GCR
## create a teir 2 MYSQL DB
- cd ../cloudsql/terraform-google-sql-db/MySql-terraform
- terraform init
- terraform apply -auto-approve


##gcloud and kubectl 
  # required for gcloud and kubectl to authenticate correctly
            echo $GCLOUD_SERVICE_KEY | gcloud auth activate-service-account --key-file=-
            gcloud --quiet config set project ${GOOGLE_PROJECT_ID}
            gcloud --quiet config set compute/zone ${GOOGLE_COMPUTE_ZONE}
            # required for terraform and terratest to authenticate correctly
            echo $GCLOUD_SERVICE_KEY > /tmp/gcloud.json
            export GOOGLE_APPLICATION_CREDENTIALS="/tmp/gcloud.json"
            # run the tests
            mkdir -p /tmp/logs
            run-go-tests --path test --timeout 2h | tee /tmp/logs/all.log


# PHP REST APIs



Get Laravel 5.8.x database for your non laravel projects. Built on top of illuminate/database to provide migration, seeding and artisan support 

## Dependencies used in the application
```
Apache 2.0
PHP 7.3.*
MySQL-Server 5.6
Composer 1.9.*
Laravel 5.8.*
```

Available artisan commands
```
dump-autoload        Regenerate framework autoload files
  env                  Display the current framework environment
  help                 Displays help for a command
  list                 Lists commands
  migrate              Run the database migrations
  serve                Serve the application on the PHP development server
  tinker               Interact with your application
 db
  db:seed              Seed the database with records
 ide-helper
  ide-helper:eloquent  Add \Eloquent helper to \Eloquent\Model
  ide-helper:generate  Generate a new IDE Helper file.
  ide-helper:meta      Generate metadata for PhpStorm
  ide-helper:models    Generate autocompletion for models
 make
  make:command         Create a new Artisan command
  make:controller      Create a new controller class
  make:factory         Create a new model factory
  make:migration       Create a new migration file
  make:model           Create a new Eloquent Model
  make:seed            Create a new seeder class
  make:test            Create a new test class
 migrate
  migrate:fresh        Drop all tables and re-run all migrations
  migrate:install      Create the migration repository
  migrate:refresh      Reset and re-run all migrations
  migrate:reset        Rollback all database migrations
  migrate:rollback     Rollback the last database migration
  migrate:status       Show the status of each migration
 vendor
  vendor:publish       Publish any publishable assets from vendor packages
```

## Steps for running application on development machine
* Run below command for running application on default port 8000
```
php artisan serve
``` 

* Run below command for running application on specific port and host
```
php artisan serve --port=8080 --host=apiserver.dev
```

## Steps for running application using docker
* Make sure all the folders inside `app/storage` have write permission for the application to write the needed files for caching
  
* Create `.env` by cloning `.env.example`
 
```
cp .env.example .env
```
* Run below docker-compose command to build the container

```
docker-compose up -d
```

## Steps for running application on GKE


* Run below docker-compose command to build the container with HTTP(S)
```
docker-compose -f docker-compose.local.yml up -d
```

## Swagger documentation
```
http://localhost:3000/apidocs/

https://localhost:8085/apidocs/
```

## Execute unit and integration tests using PHPUnit
```
docker-compose exec -T restapi php ./vendor/bin/phpunit --log-junit test-results.xml
```

![image](https://github.com/abhishek70/php-docker-rest-api/blob/master/screenshots/apidocs.png)

## Official documentation
[Luracast/Laravel-Database](https://github.com/Luracast/Laravel-Database)

[Luracast/Restler](http://restler3.luracast.com/index.html#quick-start-guide)

[Laravel](https://laravel.com/docs/5.8)

[PHP-Docker](https://hub.docker.com/_/php)

[Composer-Docker](https://hub.docker.com/_/composer)

[MySQL-Docker](https://hub.docker.com/r/mysql/mysql-server)

## PUSH DOCKER IMAGE TO GCR

## GCR  ---this will push the docker file to the gcr artifactory
            gcloud auth activate-service-account --key-file PLAYGROUND.json ##add Artifactory role and the project to the service accoundt.
            gcloud auth configure-docker europe-docker.pkg.dev
            cat NG_REGISTRY_SA.json | docker login -u _json_key --password-stdin https://europe-docker.pkg.dev
            docker push $IMAGE_NAME
## DEPLOY TO K8S WITH BASTION HOST 
  command: |
            echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
            curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
            sudo apt-get update && sudo apt-get install google-cloud-sdk
            gcloud config set project playground
            gcloud auth activate-service-account --key-file playground.json
            gcloud container clusters get-credentials example-gke --zone europe-west4-a  command: |
            echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
            curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
            sudo apt-get update && sudo apt-get install google-cloud-sdk
            echo -n $PLAYGROUND | base64 --decode > PLAYGROUND.json
            gcloud config set project playground
            gcloud auth activate-service-account --key-file PLAYGROUND.json
            gcloud container clusters get-credentials example-gke --zone europe-west4-a

#DEPLOY 
- Create a MySql dbase name "abc-db" and password
- fill the MYSQL db and ip addres on the kubernetes deployment files


## The deployment files will create
- Nmae-space ---dev-abc
- secret # please convert the db- password to base64, and fill it on the ecret "dbase-password"
- Create a deployemtn file
- Create a Load balance service
- create an HPA # to address the traffic and latency issue


- 
