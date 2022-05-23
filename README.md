# Steps to capture monitoring logs

Only run the instructions in *italics* if you want to add stress_test to the container.

## First steps
- Clone this repo.
- Make sure you have **docker** installed and the **docker** daemon is running (``systemctl start docker`` ).

## Running the service
First, we are going to run the app itself :
- *If you want to add stress-ng to a service (ts-food-service in this case), we need to build a custom docker image. FIrst step is to add stress_ng to our Java image, to do so run* :
```
./custom_docker/build_image.sh
```
		
*Then we need to build the custom ts-food-service image :*
```
./ts-food-service/build_image.sh
```
 
- Inside the repo, run the following command :
```
NAMESPACE=codewisdom TAG=0.2.0 docker-compose -f deployment/docker-compose-manifests/quickstart-docker-compose.yml up >> /logs/apps/_nameofthelogfileyouwant_
```
OR   this version if we want to run stress_ng 
:
```
NAMESPACE=codewisdom TAG=0.2.0 docker-compose -f deployment/docker-compose-manifests/quickstart-docker-compose-bis.yml up >> /logs/apps/_nameofthelogfileyouwant_
```

- To get the monitoring logs at the same time, run the following command in another terminal :
```
./collect_monitoring.sh >> /logs/monitoring/_nameofthelogfileyouwant_
```
   Note : you can change the interval of time between each monitoring log by editing the sleep command in the `collect_monitoring.sh` file. 

- Now you can play around with the website to generate logs.
- *To run stress_ng next to the food service, you need to identify the id of the container running it : Run* `docker container ls`, *find the line corresponding to `ts-food-service` and copy its id.* Then enter inside the container interactively by running :
 ```
 docker exec -it _id_of_the_container bash
``` 
- *Once you are in the container shell, run stress_ng via this command or similar :
```
stress-ng --vm 10 --vm-bytes 265M
```
For example, this will launch N workers with 256M of mmap memory each.
- To stop collecting the logs, simply 
