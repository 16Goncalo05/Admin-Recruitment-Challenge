# Admin-Recruitment-Challenge
This is a task where I need to create a dockerized deployment.



# Step-by-step
First of all after installing the Ubuntu Server version 22.04, we are going to update after entering in root mode;
    ( sudo su | apt update )


Then we are going to install CA certificates to verify SSL connections and adds and manages software sources;
    ( apt install apt-transport-https ca-certificates curl software-properties-common )


Adding Docker's Officel GPG Key
    ( curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - )


Set up Docker Reporsitory
    ( add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" )

Install Docker CE
    ( apt install docker-ce )

Create Project Directory and Dockerfile
    ( mkdir my-tomcat | cd my-tomcat | nano Dockerfile)


Add the Following Content to the Dockerfile

    # Use Ubuntu base image
    FROM ubuntu:20.04

    # Set environment variables
    ENV CATALINA_HOME /opt/tomcat
    ENV PATH $CATALINA_HOME/bin:$PATH
    ENV TOMCAT_VERSION 8.5.81

    # Install dependencies
    RUN apt-get update && apt-get install -y \
    openjdk-11-jdk \
    wget \
    && rm -rf /var/lib/apt/lists/*

    # Download and extract Tomcat
    RUN wget https://archive.apache.org/dist/tomcat/tomcat-8/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
    && mkdir -p /opt/tomcat \
    && tar xzvf apache-tomcat-$TOMCAT_VERSION.tar.gz -C /opt/tomcat --strip-components=1 \
    && rm apache-tomcat-$TOMCAT_VERSION.tar.gz

    # Modify Tomcat configuration to listen on port 4041
    RUN sed -i 's/port="8080"/port="4041"/' /opt/tomcat/conf/server.xml

    # Expose port 4041
    EXPOSE 4041

    # Start Tomcat
    CMD ["catalina.sh", "run"]



Build the Docker Image:
    ( docker build -t my-tomcat:8.5 . )

Run the Docker Container:
    ( docker run -d -p 4041:4041 --name tomcat-server my-tomcat:8.5 )

After that we are going to test the setup by verifying if the containers is running:
    ( docker ps )

Test the Tomcat server by navigating to https://localhost:4041 in your browser or using curl:
    ( curl -Ik https://localhost:4041 ) 

Just to confirm tomcat is dockerized wea re going to open the browser and search:
    ( "ip address:4041" )
