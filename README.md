# Projeto-Pipiline-de-ETL-com-Python
Projeto Realizado no curso de Ciência de dados com Python



Santander Dev Week 2023 (ETL com Python)
Contexto: Você é um cientista de dados no Santander e recebeu a tarefa de envolver seus clientes de maneira mais personalizada. Seu objetivo é usar o poder da IA Generativa para criar mensagens de marketing personalizadas que serão entregues a cada cliente.

Condições do Problema:

Você recebeu uma planilha simples, em formato CSV ('SDW2023.csv'), com uma lista de IDs de usuário do banco:
UserID
1
2
3
4
5
Seu trabalho é consumir o endpoint GET https://sdw-2023-prd.up.railway.app/users/{id} (API da Santander Dev Week 2023) para obter os dados de cada cliente.
Depois de obter os dados dos clientes, você vai usar a API do ChatGPT (OpenAI) para gerar uma mensagem de marketing personalizada para cada cliente. Essa mensagem deve enfatizar a importância dos investimentos.
Uma vez que a mensagem para cada cliente esteja pronta, você vai enviar essas informações de volta para a API, atualizando a lista de "news" de cada usuário usando o endpoint PUT https://sdw-2023-prd.up.railway.app/users/{id}.
[ ]
# Utilize sua própria URL se quiser ;)
# Repositório da API: https://github.com/digitalinnovationone/santander-dev-week-2023-api
sdw2023_api_url = 'https://sdw-2023-prd.up.railway.app'
Extract
Extraia a lista de IDs de usuário a partir do arquivo CSV. Para cada ID, faça uma requisição GET para obter os dados do usuário correspondente.

[ ]
import pandas as pd

df = pd.read_csv('Projeto ETL.csv')
user_ids = df['UserID'].tolist()
print(user_ids)
[2462, 2463, 2464]
[ ]
import requests
import json

def get_user(id):
  response = requests.get(f'{sdw2023_api_url}/users/{id}')
  return response.json() if response.status_code == 200 else None

users = [user for id in user_ids if (user := get_user(id)) is not None]
print(json.dumps(users, indent=2))
[
  {
    "id": 2462,
    "name": "Ssimonoi",
    "account": {
      "id": 2591,
      "number": "01557-3",
      "agency": "0012-1",
      "balance": 1.0,
      "limit": 23.0
    },
    "card": {
      "id": 2385,
      "number": "**** 1548 **** 5348",
      "limit": 48.0
    },
    "features": [],
    "news": [
      {
        "id": 5154,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Entregas pontuais: chave para o sucesso log\u00edstico!"
      }
    ]
  },
  {
    "id": 2463,
    "name": "Madruga Silva",
    "account": {
      "id": 2592,
      "number": "01559-3",
      "agency": "0014-1",
      "balance": 1.0,
      "limit": 24.0
    },
    "card": {
      "id": 2386,
      "number": "**** 1546 **** 5248",
      "limit": 48.0
    },
    "features": [],
    "news": [
      {
        "id": 5155,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Entregas no prazo: essencial para a satisfa\u00e7\u00e3o do cliente!"
      }
    ]
  },
  {
    "id": 2464,
    "name": "BetoPedri",
    "account": {
      "id": 2593,
      "number": "02559-3",
      "agency": "0024-1",
      "balance": 1.0,
      "limit": 24.0
    },
    "card": {
      "id": 2387,
      "number": "**** 1346 **** 5149",
      "limit": 48.0
    },
    "features": [],
    "news": [
      {
        "id": 5156,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Entregas no prazo: fator essencial para o sucesso do seu neg\u00f3cio. N\u00e3o deixe seus clientes esperando!"
      }
    ]
  }
]
Transform
Utilize a API do OpenAI GPT-4 para gerar uma mensagem de marketing personalizada para cada usuário.

[ ]
!pip install openai
Collecting openai
  Downloading openai-0.28.0-py3-none-any.whl (76 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 76.5/76.5 kB 1.8 MB/s eta 0:00:00
Requirement already satisfied: requests>=2.20 in /usr/local/lib/python3.10/dist-packages (from openai) (2.31.0)
Requirement already satisfied: tqdm in /usr/local/lib/python3.10/dist-packages (from openai) (4.66.1)
Requirement already satisfied: aiohttp in /usr/local/lib/python3.10/dist-packages (from openai) (3.8.5)
Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (3.2.0)
Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (3.4)
Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (2.0.4)
Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (2023.7.22)
Requirement already satisfied: attrs>=17.3.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (23.1.0)
Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (6.0.4)
Requirement already satisfied: async-timeout<5.0,>=4.0.0a3 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (4.0.3)
Requirement already satisfied: yarl<2.0,>=1.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (1.9.2)
Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (1.4.0)
Requirement already satisfied: aiosignal>=1.1.2 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (1.3.1)
Installing collected packages: openai
Successfully installed openai-0.28.0
[ ]
# Documentação Oficial da API OpenAI: https://platform.openai.com/docs/api-reference/introduction
# Informações sobre o Período Gratuito: https://help.openai.com/en/articles/4936830

# Para gerar uma API Key:
# 1. Crie uma conta na OpenAI
# 2. Acesse a seção "API Keys"
# 3. Clique em "Create API Key"
# Link direto: https://platform.openai.com/account/api-keys

# Substitua o texto TODO por sua API Key da OpenAI, ela será salva como uma variável de ambiente.
openai_api_key = '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'
[ ]
from openai.api_resources.completion import Completion

import openai

openai.api_key = openai_api_key

def generate_ai_news(user):
  completion = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-16k-0613",
  messages=[
      {
          "role": "system",
          "content": "Você é um especialista de Logística."
      },
      {
          "role": "user",
          "content": f"Crie uma mensagem para {user['name']} sobre a importância das entregas no prazo (máximo de 100 caracteres)"
      }
    ]
  )
  return completion.choices[0].message.content.strip('\"')

for user in users:
  news = generate_ai_news(user)
  print(news)
  user['news'].append({
      "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
      "description": news
  })
Entregas pontuais: chave para o sucesso logístico!
Entregas no prazo: essencial para a satisfação do cliente!
Entregas no prazo: fator essencial para o sucesso do seu negócio. Não deixe seus clientes esperando!
Load
Atualize a lista de "news" de cada usuário na API com a nova mensagem gerada.

[ ]
def update_user(user):
  response = requests.put(f"{sdw2023_api_url}/users/{user['id']}", json=user)
  return True if response.status_code == 200 else False

for user in users:
  success = update_user(user)
  print(f"User {user['name']} updated? {success}!")
User Ssimonoi updated? True!
User Madruga Silva updated? True!
User BetoPedri updated? True!

