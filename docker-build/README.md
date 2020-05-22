This container contains a prebuilt Clang+LLVM and the modified SVF for artifact evaluation of Temporal Specialization.

To run the analysis for a single application, in the docker container simply run:

docker run -d --name artifact-eval --volume <local_results_dir_full_path>:/results taptipalit/temporal-specialization-artifacts:1.0  ./run.sh <bitcode_name> 

For example, to run the analysis for the memcached.libevent bitcode, with the
function worker_libevent as the entrypoint for the serving phase, and
./results directory as results directory, run the following command: 

docker run -d --name artifact-eval --volume mnt/Projects/temporal-specialization-artifacts/docker-build/results:/results taptipalit/temporal-specialization-artifacts:1.0 ./run.sh memcached.libevent 

If you want to run the analysis for all the applications, first launch the
container, and then use docker exec to run each analysis.

docker run -it --name artifact-eval --volume <local_results_dir_full_path>:/results taptipalit/temporal-specialization-artifacts:1.0

docker exec -it artifact-eval ./run.sh memcached.libevent
docker exec -it artifact-eval ./run.sh httpd.apr
docker exec -it artifact-eval ./run.sh nginx
docker exec -it artifact-eval ./run.sh lighttpd 
docker exec -it artifact-eval ./run.sh redis-server

If you wish to build the container yourself from the Dockerfile

1. Build the container:

Select the number of cores you want to use to build clang+llvm, in the ENV NUM_CORES option in the Dockerfile.

docker build --tag temporal-specialization-artifacts:1.0 ./

2. Run the container

docker run -it --name artifact-eval --volume <local_results_dir_full_path>:/results temporal-specialization-artifacts:1.0  ./run.sh <bitcode_name> 

e.g. 
docker run -it --name artifact-eval --volume mnt/Projects/temporal-specialization-artifacts/docker-build/results:/results temporal-specialization-artifacts:1.0 ./run.sh  memcached.libevent 

3. Other helpful commands

a. Stop the artifact-eval containter

docker stop artifact-eval

b. Delete the artifact-eval container

docker rm artifact-eval

a. Stop all containers

docker stop $(docker ps -a -q)

b. Delete all containers

docker rm $(docker ps -a -q)

c. Delete all docker images

docker rmi $(docker images -a -q)

d. Run commands on an existing container

docker -exec -it <container_name> <command> 

docker -exec -it ./run.sh httpd.wapr 
