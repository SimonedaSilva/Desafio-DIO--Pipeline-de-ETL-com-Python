## Projeto DIO – Explorando IA Generativa em um pipeline de ETL com Python

Neste projeto foi criado um pipeline de ETL passando por todas etapas do ciclo (Extração, Transformação e Carregamento).

A primeira parte do desafio foi criar uma planilha simples em formato .CSV para extrair os IDs.

###  [Extract](url) :

Nesta etapa realizei a extração dos IDs dos usuários e o segundo passo foi fazer uma requisição na API da Santander Dev Week 2023 e obter mais dados detalhados.

### [Transform](url):

Na parte de transformação foi usado o Chatgpt, onde fiz a conexão com a API gpt-3.5-turbo-16k-0613 e transformei os dados em mensagens personalizadas para os usuários sobre logística, a importância de as entregas serem realizadas no prazo.

### [Load]([subdiretorio/README.md](https://github.com/SimonedaSilva/Desafio-DIO--Pipeline-de-ETL-com-Python/blob/main/SantanderDevWeek2023.ipynb)https://github.com/SimonedaSilva/Desafio-DIO--Pipeline-de-ETL-com-Python/blob/main/SantanderDevWeek2023.ipynb) :

Completando o ciclo do pipeline de ETL realizei o envio das informações atualizadas para API da Santander Dev Week 2023.
