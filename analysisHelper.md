### Overview
At a basic level, this workflow functions by starting the Docker container, and then passing arguments to it for the desired analyses:

![](https://github.com/disulfidebond/variant_calling_workflow/blob/master/images/workflow_basic.png)

The [analysisHelper](https://github.com/disulfidebond/variant_calling_workflow/blob/master/src/analysisHelper) script acts as a delegate for handling data locations and parameters for workflows.

The usage for the [analysisHelper](https://github.com/disulfidebond/variant_calling_workflow/blob/master/src/analysisHelper) is as follows:

        ./analysisHelper.sh [-c || -l]

        -c : accepts arguments from commandline, i.e. passed into the docker container as an argument on commandline. 
        Arguments must be separated by whitespace, and optionally single quotes can be used for special characters embedded in the command.
        A this point, variables cannot be passed, with double quotes or otherwise. Note that this option should be used for development/testing purposes only.
        Example:
          ./analysisHelper.sh -c 'echo hello world ; echo apples'

        -l : accepts arguments from a newline-delimited text file, which must be copied into the docker container.
        The arguments will be read sequentially, and quotes are not required for strings. A hash '#' will be ignored, and can be used to separate out commands
        Example from a text file named 'textfile.txt'
        
        echo
        hello world

        The analysisHelper script can call executables that will be launched within the Docker container. If no absolute or relative path is provided, then the environment path will be used, which may or may not be desirable.

        Example 1:
        ./analysisHelper.sh -c /bin/echo, 'hello world'
        ./analysisHelper.sh -c echo,'hello world'
        # the above two commands will produce the same result

        Example 2a, do not add '#' in the actual text file that will be used:
        
        # ############
        # textfile.txt
        /bin/echo
        hello world
        # ############
        
        ./analysisHelper.sh -l textfile.txt


        Example 2b, A more complicated example would be
        ./analysisHelper.sh -l more-complicated.txt
        
        
        /opt/bin/bowtie2-build
        /home/scientist/hg19.fa
        /home/scientist/hg19.fa
        #
        sleep 2
        echo 
        'job done'
        hello world



Although not strictly required, it is **strongly** recommended to use the analysisHelper script to manage data analyses. If you opt not to use the *analysisHelper* script, then it is assumed that you know what you are doing, and that you know how to interpret the inevitable error messages that will appear. 
