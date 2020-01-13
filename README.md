# indexing-open-source-c-cpp-repositories

Adjust the config.properties file first (paths etc.)

Build the docker image:  
$ docker build -t conan-cmake:clang .  

Start the elasticsearch server (localhost:9200 default):
$ sudo systemctl start elasticsearch.service   

Go to the Program folder and run:\
$ java -jar CreateCollection.jar\
OR\
$ java -jar CreateCollection.jar [NUMBER]
