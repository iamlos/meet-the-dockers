docker-machine ls
docker-machine create 
docker-machine create --driver virtualbox -h
docker-machine create playground --driver=virtualbox

docker-machine ls

# View the ip of the VM
docker-machine ip playground

# Print environment variables that allow you to connect docker binary to docker daemon
docker-machine env playground

# Set the environment variables to allow you to connect to docker daemon
eval "$(docker-machine env playground)"
docker ps

docker-machine stop playground
docker-machine start playground
docker-machine stop playground
docker-machine rm playground
docker-machine ls

# when stopping and starting the vm, you may be asked to run
docker-machine regenerate-certs

# Shows help
docker-machine