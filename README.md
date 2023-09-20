# Implementando Apache Kafka 

- configuração, conceito e prática <br>

Ordem funcionamento :  <br>

zookeeper-server-start.bat  C:\Kafka\config\zookeeper.properties  <br>
kafka-server-start.bat C:\kafka\config\server.properties  <br>
kafka-topics --bootstrap-server localhost:9092 --create --topic JavaTest  <br>

Observação :  <br>

1 - sempre subir primeiro o  zookeeper-server-start.bat  C:\kafka\config\zookeeper.properties  <br>
2 - kafka-server-start.bat  C:\kafka\config\server.properties  <br>
3 - Criar topicos exe :   kafka-topics --bootstrap-server localhost:9092 --create --topic teste6  <br>
4 - Listar :  kafka-topics --bootstrap-server localhost:9092 --list  <br>
5 - OBS: eu uso "Linux" e com docker,  foi um experimento com windows  <br>

Kafka puro entendendo seus conceitos, prática e configuração no windows 10  <br>

---------------------------------------   INÍCIO  ----------------------------------------------------------------------------------------------------------------------- 
1° Passo ->  <br>

Download no site https://kafka.apache.org/downloads  <br>

 Extrair arquivo baixado e copiar conteudo  <br>

-------------------------------------------------------------------------------------------------------------------------------------------------------------- 
2° Passo ->  <br>

 Criar estrutura de pastas pasta
Caso o seu Disco Local não seja particionado, e você apenas possua um, utilize " C: "	 <br>

-------------------------------------------------------------------------------------------------------------------------------------------------------------- 
3° Passo ->  <br>

# Abrir pasta C:\Kafka\config\  <br>
copiar arquivo server.properties e colar duas vezes o mesmo conteudo e renomear com os nomes abaixo  <br>
- server1.properties   <br>
- server2.properties  <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------  
4° Passo ->  <br>

Editar arquivo C:\Kafka\application\config\server1.properties  <br>
- Setar broker id = 1   <br>
broker.id=1  <br>

- Descomentar a linha e setar porta 9092 para o server1  <br>
listeners=PLAINTEXT://:9092  <br>

- Setar diretório de logs para o diretório criado para o server1  <br>
C:/Kafka/tmp/kafka-logs/1  <br>

- Numero de partições por topico setar 2   <br>
num.partitions=2   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------  
5° Passo ->  <br>

Editar arquivo C:\Kafka\application\config\server2.properties   <br>
- Setar broker id = 2   <br>
broker.id=2   <br>

- Descomentar a linha e setar porta 9093 para o server1   <br>
listeners=PLAINTEXT://:9093   <br>

- Setar diretório de logs para o diretório criado para o server1   <br>
C:/Kafka/tmp/kafka-logs/2   <br>

- Numero de partições por topico setar 2   <br>
num.partitions=2    <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------  
6° Passo ->   <br>

Editar o arquivo C:\Kafka\application\config\zookeeper.properties   <br>

- Setar o diretorio de dados(datadir) para o zookeeper   <br>
dataDir=C:/Kafka/data/zookeeper   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------  
7° Passo ->   <br>

Abra o Prompt de comando e execute os comandos a seguir:   <br>

# Batch para Inicializar o Zookeeper e Kafka cluster   

echo start C:\Kafka\application\bin\windows\zookeeper-server-start.bat C:\Kafka\application\config\zookeeper.properties > C:\Kafka\batch\Start_Zookeeper.bat   <br>

# Batch para Inicializar o Servidor 1 Kafka  
echo start C:\Kafka\application\bin\windows\kafka-server-start.bat C:\Kafka\application\config\server1.properties > C:\Kafka\batch\Start_Kafka_Server1.bat   <br>

# Batch para Inicializar o Servidor 2 Kafka   
echo start C:\Kafka\application\bin\windows\kafka-server-start.bat C:\Kafka\application\config\server2.properties > C:\Kafka\batch\Start_Kafka_Server2.bat   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------  
8° Passo ->   <br>

# Abra as batchs criadas para inicializar o cluster   

- Start_Zookeper.bat   <br>
- Start_Kafka_Server1.bat    <br>
- Start_Kafka_Server2.bat   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------

9° Passo ->

# Vamos criar dois topicos para teste

# Batch para criar topico customer

echo start C:\Kafka\application\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 2 --partitions 2 --topic customer_topic > C:\Kafka\batch\Create_customer_topic.bat   <br>

# Batch para criar topico product

echo start C:\Kafka\application\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 2 --partitions 2 --topic product_topic > C:\Kafka\batch\Create_product_topic.bat    <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------

10° Passo ->   <br>

# Vamos agora, iniciar o serviço de producer e consumer, para assim podermos enviar e receber mensagens atraves do kafka broker (customer_topic)

# Batch para inicializar o serviço de producer para o topico customer_topic

echo start C:\Kafka\application\bin\windows\kafka-console-producer.bat --topic customer_topic --broker-list localhost:9092,localhost:9093 > C:\Kafka\batch\Producer_customer_topic.bat   <br>

# Vamos agora, criar a batch para iniciar o serviço de consumer, par aassim podermos receber mensagens do kafka broker (customer_topic)

echo start C:\Kafka\application\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --from-beginning --topic customer_topic > C:\Kafka\batch\Consumer_customer_topic.bat   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------

11° Passo -> 

# Vamos, iniciar o serviço de producer e consumer, para assim podermos enviar e receber mensagens atraves do kafka broker (product_topic)

# Batch para inicializar o serviço de producer para o topico product_topic

echo start C:\Kafka\application\bin\windows\kafka-console-producer.bat --topic product_topic --broker-list localhost:9092,localhost:9093 > C:\Kafka\batch\Producer_product_topic.bat    <br>

# Vamos, criar a batch para iniciar o serviço de consumer, par aassim podermos receber mensagens do kafka broker (product_topic)

echo start C:\Kafka\application\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --from-beginning --topic product_topic > C:\Kafka\batch\Consumer_product_topic.bat   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------

12° Passo ->

# Vamos Verificar e descrever o status dos topicos do kafka

# Batch para descrever o status do kafka para o topico customer_topic

echo start C:\Kafka\application\bin\windows\kafka-topics.bat --describe --zookeeper localhost:2181 --topic customer_topic > C:\Kafka\batch\Status_customer_topic.bat   <br>

# Batch para descrever o status do kafka para o topico product_topic

echo start C:\Kafka\application\bin\windows\kafka-topics.bat --describe --zookeeper localhost:2181 --topic product_topic > C:\Kafka\batch\Status_product_topic.bat   <br>

--------------------------------------------------------------------------------------------------------------------------------------------------------------

Fim !! 

