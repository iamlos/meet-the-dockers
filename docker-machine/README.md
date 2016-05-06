docker-machine 
docker-machine create 
docker-machine create --driver virtualbox -h
docker-machine create playground --driver=virtualbox

# View the ip of the VM
docker-machine ip playground

# View running images
docker ps

# View downloaded images
docker images

# Print environment variables that allow you to connect docker binary to docker daemon
docker-machine env playground
eval "$(docker-machine env playground)"
docker ps
docker images

docker-machine stop playground
docker-machine start playground
docker-machine stop playground
docker-machine rm playground
docker-machine ls

# when stopping and starting the vm, you may be asked to run
docker-machine regenerate-certs

# Shows help
docker-machine