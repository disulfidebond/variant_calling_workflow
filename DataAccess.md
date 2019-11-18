## README
This readme will describe the basics of how data can be stored and accessed for this workflow.

### Docker Volume
This approach is strongly recommended. The index and other data will first be copied to a Docker Volume, and then accessed by container(s).


        # create a Docker volume
        docker volume create analysisData
        # optional, inspect the volume
        # docker inspect analysisData
        # docker volume ls
        
        # create a temporary hello-world container to communicate with the Docker volume
        docker container create --name tempDataIO -v analysisData:/data hello-world
        # copy files to Docker Volume via container, assumes run from same directory as files
        docker cp *.fastq tempDataIO:/data/
        # remove temporary docker container
        docker rm tempDataIO
        # finally, start container and mount Docker volume
        docker run -it --name bt2Analysis --mount source=analysisData,target=/data datacram/bioctools:version2 /usr/bin/analysisHelper '-l /data/analysisCommands.txt'
        
        # to retrieve the contents of the analysis at a later time, use something similar to the following **from the host**:
        docker cp bt2Analysis:/home/scientist/*.bam ./
        
        # to access the container while it's running, do something similar to:
        # continuing above, CONTAINERNAME='bt2Analysis'
        docker exec -it CONTAINERNAME /bin/bash
        
### Copy to or from the Docker Container
Another option is to copy the data to or from the Docker Container:

        # copy data to the stopped container
        docker cp *.txt CONTAINERNAME:/home/scientist/
        
        # copy data from the stopped container
        docker cp CONTAINERNAME:/home/scientist/someFile.bam ./
        
Then, start the Docker Container and run a command.

        # start container from image named IMAGENAME, then run a command 
        # docker container run IMAGENAME [COMMAND]
        # example:
        docker container run datacram/bioctools:version2 analysisHelper -c echo 'hello world'

Unless you specified a command with *container run*, the container will immediately stop. 
If you modified the container to automagically run a command when starting up, then it will run the command and then exit. 
Otherwise, you must specify some way for the container to persist. Albeit inefficient, one way to do this would be:

        docker container run datacram/bioctools:version2 tail -f /dev/null
        
