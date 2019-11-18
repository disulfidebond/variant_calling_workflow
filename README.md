# Variant Calling Workflow
Listed here are the steps that were taken with the Varint Calling workflow


## Preliminaries and setup
1. [Install Docker](https://docs.docker.com/docker-for-windows/install/)
2. Create a Docker volume with the BWA and Bowtie2 index files.
3. Pull Docker image, then run the container.


## Usage

The commands for the [analysisHelper](https://github.com/disulfidebond/variant_calling_workflow/blob/master/analysisHelper.md) program are described in a markdown file in this repository. Briefly, the analysisHelper script can be called as a part of the Docker *run* command, using *-c* to indicate that arguments and parameters will be passed as a single text string, or *-l* to indicate that arguments and parameters will be passed as a text file. Note that if you use *-l*, you must copy this text file ahead of time to the Docker container or volume, and provide the full path to the file.

The analysisHelper executable acts as a delegate for starting and running analyses. At a basic level, the requirements to run are a set of commands, and a valid path to all data sources. The Bowtie2 and BWA index files should already be present in the Docker volume, and the recommended setup is to also add data files to the same Docker volume for simplicity. For example, the following could be done to run an alignment of BWA or Bowtie2:

        # copy the fastq files to a Docker volume. See the DockerVolumes.md file within this repo for more information
        docker cp fqfile1.fastq fqfile2.fastq dataVolume:/data/
        # create or start the docker container, then pass the arguments to start the analysis
        # the arguments in this case are in a newline-separated text file called 'commands.txt'
        docker run -it --name Bowtie2Run --mount source=dataVolume,target=/data datacram/bioctools:version2 /usr/bin/analysisHelper '-l commands.txt'




