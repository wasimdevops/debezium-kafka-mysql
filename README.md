====================================================================================
# Note:
With this project we can setup connection with debezium and mysql using docker compose.

====================================================================================
1) Get the docker and docker compose installation ready:

Reference Link: https://docs.docker.com/compose/install/  

-----------------------------------------------------------------------------------------
2)Clone a repository:

 git clone git@github.com:wasimdevops/debezium-kafka-mysql.git

-----------------------------------------------------------------------------------------
3)Specify the debezium version and run docker compose setup:

 export DEBEZIUM_VERSION=2.1

 docker compose -f docker-compose-mysql.yaml up -d

-----------------------------------------------------------------------------------------
4)Mysql connector HTTP POST connection:

  curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mysql.json

-----------------------------------------------------------------------------------------
5)Start kafka console consumer to view database message: (open in separate terminal)

  docker compose -f docker-compose-mysql.yaml exec kafka /kafka/bin/kafka-console-consumer.sh     --bootstrap-server kafka:9092     --from-beginning     --property print.key=true     --topic dbserver1.inventory.customers

-----------------------------------------------------------------------------------------
6)Add or modify records in mysql DB via MYSQL client: (open in separate terminal)

  docker compose -f docker-compose-mysql.yaml exec mysql bash -c 'mysql -u $MYSQL_USER -p$MYSQL_PASSWORD inventory'

-----------------------------------------------------------------------------------------
Notes:

a)With step 6 ,add or modify database.table contents and observe the changes in kafka console consumer.
b)Here in above case database used is inventory and table is customers.  
c)One can add new database as per their requirement:
  i) To modify the db name change db values in register-mysql.json & docker-compose-mysql.yaml.
 ii) Change db name and table name during runtime with kafka topic and mysql connection client i.e with step 5 & 6.

-----------------------------------------------------------------------------------------
7)To Shutdown the setup:

 docker compose -f docker-compose-mysql.yaml down

-----------------------------------------------------------------------------------------
