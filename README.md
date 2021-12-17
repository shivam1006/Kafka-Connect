######################################## Kafka Source Connector##########################################

# 1) Source connectors
# Start our kafka cluster
docker-compose up kafka-cluster

# We start a hosted tools, mapped on our code
# Linux / Mac
docker run --rm -it -v "$(pwd)":/tutorial --net=host landoop/fast-data-dev:cp3.3.0 bash

# create the topic we're going to write to
docker run --rm -it --net=host landoop/fast-data-dev:cp3.3.0 bash
kafka-topics --create --topic demo-2-distributed --partitions 3 --replication-factor 1 --zookeeper 127.0.0.1:2181

# Now that the configuration is launched, we need to create the file demo-file.txt
docker ps
docker exec -it <containerId> bash
touch demo-file.txt
echo "hi" >> demo-file.txt
echo "hello" >> demo-file.txt
echo "from the other side" >> demo-file.txt

# Read the topic data
docker run --rm -it --net=host landoop/fast-data-dev:cp3.3.0 bash
kafka-console-consumer --topic demo-2-distributed --from-beginning --bootstrap-server 127.0.0.1:9092


######################################## Kafka Sink Connector##########################################

# Start our kafka cluster
docker-compose up kafka-cluster elasticsearch postgres

# We make sure elasticsearch is working. Replace 127.0.0.1 by 192.168.99.100 if needed
 
http://127.0.0.1:9200/
# Go to the connect UI and apply the configuration at :
sink/demo-elastic/sink-elastic-twitter-distributed.properties
# Visualize the data at:
http://127.0.0.1:9200/_plugin/dejavu
# http://docs.confluent.io/3.1.1/connect/connect-elasticsearch/docs/configuration_options.html
# Counting the number of tweets:
http://127.0.0.1:9200/demo-3-twitter/_count
# You can download the data from the UI to see what it looks like
# We can query elasticsearch for users who have a lot of friends, see query-high-friends.json
![image](https://user-images.githubusercontent.com/15971678/146482183-de0f3653-feef-4079-809a-0a3241fb569c.png)
