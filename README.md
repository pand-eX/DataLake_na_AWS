# Implementando um Data Lake na AWS !!!!

[![NPM](https://img.shields.io/npm/l/react)](https://github.com/pand-eX/DwNuvem/blob/main/LICENSE) 

# About the project

-Nesse Projeto irei criar as Instância EC2 para o Nodo Master e os Slaves, irei cria o usuário no cluster, autenticar o SSH sem senha para a comunicação do cluster, instalar e configurar o JDK e o Hadoop. Também irei cria nossa fonte de dados que será um banco de dados Relacional AWS RDS com engine PostgreSQL e Também vou implementar a primeira camada de Aquisição em Batch com Sqoop.

## Why?

porque sabemos que a Cloud Computing é o futuro da TI. 

- Este projeto faz parte do meu portfólio pessoal, então ficarei feliz se você puder me dar algum feedback sobre o projeto, código, estrutura ou qualquer coisa que você possa relatar que possa me fazer um melhor engenheiro de dados!

Email-me: henricao_7@yahoo.com.br

Connect with me at [LinkedIn](https://www.linkedin.com/in/henrique-castro-484269203//).

## Arquitetura Data Lake na AWS.


![1](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/1.png)


Essa será a arquitetura quando todo o projeto estiver pronto, mas irei dividir o projeto para não ficar muito grande então cada camada irei implementar e separar por projeto.

Todos os scripts estarão em anexo.

## Iniciando o Projeto


Irei cria 3 Instancia EC2 uma para o Name Node é 2 para o Data Node.


![2](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/2.png)



Basicamente ficará assim o cluster você pode adicionar mais node se você quiser.


![3](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/3.png)



Essa é a criação do Cluster. Agora iremos conectar nas Instâncias EC2 via SSH e cria o usuário de configuração, instalar o JDK nas máquinas do Cluster, Autenticar SSH sem senha para comunicação dos Nodes e Instalar e configurar o Hadoop no Node Master e no Nodes Slaves.
Não irei mostrar essa etapa porque já fiz isso no projeto Data Lake On-Premises então caso você fique perdido nesse processo de alguns passos para trás e procure o meu projeto que tem o passo a passo.

Lembrando que o Script está em anexo basta seguir que terá o mesmo resultado.
Finalizando a última etapa e testando o Data Lake na Nuvem AWS.



![4](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/4.png)



Proximo passo é a criação do banco de dados por que estou simulando todo o ambiente e não tenho um banco de dados para fazer a carga no Data Lake então irei usar o RDS da amazo na que está na camada gratuita bastar criar baixar o pgadmin usarei o postgresql e fazer a conexão. Na criação do RDS você recebe um endpoint que é ele que você usa para fazer a conexão se você tiver outro banco de dados pode utilizar também, oracle, mysql e etc... 
Fiz a criação da tabela e coloquei alguns dados para simular um ambiente de banco de dados relacional.



![5](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/5.png)



Agora iremos implementar uma camada de aquisição dos dados em Batch utilizando o Sqoop.
Na a camada de aquisição de dados que vai permitir levar os dados da fonte para o destino nesse caso do banco de dados relacional para o nosso Data Lake usaremos o apache sqoop.
Conectando no sqoop pelo DL aws lembrando que você precisa do JDBC do postgresql para acessar é so você seguir o scrip então fazemo um teste para ver se a tabela que criamos no pgadmin será visto pelo sqoop esse é o teste.



![6](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/6.png)



Comando para ver tabelas no sqoop. >
sqoop list-tables --connect jdbc:(Coloca o seu banco de dados)://(e aqui o seu endpoint) /(nome do seu banco de dados que voce deseja entrar) --username (nome do admin) -P
-Aqui ele encontra a tabela customer que criamos no postgresql
E depois para importar os arquivos do banco relacional para o Data Lake comando > 
sqoop import --connect jdbc:(Coloca o seu banco de dados)://(e aqui o seu endpoint) /(nome do seu banco de dados que voce deseja entrar) --table (nome da tabela que vc quer importar) -- usarname(nome do admin) –P



![7](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/7.png)



Sqoop faz o mapeamento ele então cria um processo e traz os dados lá do banco relacional ou a fonte que você estiver trabalhando cria esse mapeamento de dados e na sequencia leva isso através de um job map reduce, basicamente ele usa metade do processo só o mapeamento e leva isso para dentro do cluster hadoop veja que aqui ele coletou 7 linhas ou 7 registros que foi o que a gente colocou no postgresql e com isso nós conseguimos importar os dados do banco relacional para o Data Lake.


![8](https://github.com/pand-eX/DataLake_na_AWS/blob/main/Datalakenaws/assets/8.png)



Esses são os dados distribuído pelo cluster hadoop não importa em qual Data Node está quem toma conta de tudo isso quem administra é o name node ele vai pegar quantos nodes você tem e vai enviando os blocos de dados de modo que os dados estão sendo distribuído pelo cluster isso facilita na hora que você for fazer as leituras dos dados.

Fim.

No próximo projeto irei implementar a camada de Aquisição em Streaming com Apache Flume e um mini-projeto com Apache NiFi.
