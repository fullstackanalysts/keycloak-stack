- This project houses the code for a multi compose file configuration
  with traefik as the reverse proxy and Keycloak serving as an authentication
  service. The prebuild_image folder contains the code to build a flask image
  which is used to act as the forward auth middleware end point for traefik
  and provide a sign in screen.

- For the stack to function correctly ensure all host used within the configs
  have been added to the system host file if running locally.

- Ensure your docker instance has a bridge/overlay network called traefik
  * if it doesn't run:
  
$docker network create -d bridge[or overlay] traefik

- If the flaskauth images has not been creates cd into the docker file
  directory and run:

$docker build -t flaskauth .

NOTE: the "." at the end is required as part of the cli command

To initiate the stack run:

$docker compose -f a_traefik-auth-compose.yml -p "demostack" up -d

NOTE: the "-p" defines the project name as "demostack" if you want to 
shut down all container and remove you can run: $docker compose -p demostack down

- After starting the Keycloak service login via the url defined in the yml env variable
  and then setup a new realm, client and user for authentication. Change the client secret
  in the auth-mw-compose.yml file to match the client secret provided in the Keycloak admin UI

- Now you're ready to run the remaining compse files.

$docker compose -f b_auth-mw-compose.yml -p "authstack" up -d
$docker compose -f c_services-compose.yml -p "servicestack" up -d

NOTE: you can now add as many service as needed by simply adding them to the
service-compose yml file. you will need to down the compose file 
(i.e. $docker compose services-compose.yml down or docker compose -p servicestack down if configured) 
if additional services are added then rerun the compose up command.
