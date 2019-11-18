# Variant Calling Workflow
Listed here are the steps that were taken with the Varint Calling workflow


## Preliminaries and setup
1. [Install Docker](https://docs.docker.com/docker-for-windows/install/)
2. Create a Docker volume with the BWA and Bowtie2 index files.  A bash script has been created that accomplishes these steps.
3. Pull Docker image. A writeup and README has been created for this image.


## Usage
The Docker container is set to run the analysisHelper executable, which acts as a delegate for starting and running analyses. The commands for the analysisHelper program are as follows:
