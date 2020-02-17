# indexing-open-source-c-cpp-repositories
Researchers are heavily committed towards improving and developing new static analysis tools and algorithms.
Hence, it creates the necessity to evaluate their validity and performance. The best way to compare and test the effectiveness of such tools or algorithms, is to let them analyse the same code base, also called benchmark suite, and compare their results. Thus for the systematic evaluation of such tools benchmark suites are required that are representative of real world applications that are up-to-date. Yet current benchmark suites only exist for few research areas and are often insufficiently maintained.
This repository introduces a benchmark system that develops a knowledge database by collecting information of C/C++ open-source repositories using static code analysis. The focus is especially on the automation of the overall benchmark system's process.

All components of the benchmark system are implemented in Java. It facilitates the collection of repository metadata and code statistics through static analysis in an automatic manner. All information collected throughout the benchmark system's process, i.e. static analysis results, are stored into a database. These information serve as a starting point for creating current, targeted benchmark suites, and thus helps to evaluate static analysis tools and algorithms. It does so by automatically crawling for suitable open-source repositories published on GitHub. Subsequently, each repository is automatically downloaded, software dependencies are resolved with *Conan* and the source code is compiled with the *CMake* build system. Each successful compilation is followed by the extraction of the repository's *LLVM IR* code and static analysis of it using the static analysis framework *PhASAR*. Finally, all collected data is stored into an *Elasticsearch* database.

Tests show that more than 40\% of 176 crawled repositories are successfully built by the proposed benchmark system.  Consequently, the benchmark system collected metadata of 176 repositories and code statistics of 616 executables, 32 libraries and 90 archives.

## Required Software:
Elasticsearch\
Docker
## Basic Setup for Debian-based distributions:
### Installing Elasticsearch
#### 1. Download and install the public signing key:
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

#### 2. You may need to install the apt-transport-https package on Debian before proceeding:
$ sudo apt-get install apt-transport-https

#### 3. Save the repository definition to /etc/apt/sources.list.d/elastic-7.x.list:
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list\

#### 4. You can install the Elasticsearch Debian package with:
$ sudo apt-get update && sudo apt-get install elasticsearch

### Installing Docker
#### 1. You can install the Docker Debian package with:
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

## Start up
#### 1. Adjust the config.properties file first (paths, crawler settings etc.)

#### 2. Build the docker image:
$ docker build -t conan-cmake:clang .

#### 2. Start the elasticsearch server (By default localhost:9200):
$ sudo systemctl start elasticsearch.service

#### 3. Go to the Program folder and run the Crawler
$ java -jar Crawler.jar

#### 4. Go to the Program folder and run:
$ sudo java -jar CreateCollection.jar\
OR\
$ sudo java -jar CreateCollection.jar [NUMBER]
