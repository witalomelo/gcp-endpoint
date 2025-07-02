# Lab Endpoints - API de Aeroportos

Este projeto implementa uma API REST simples para consulta de informações de aeroportos utilizando códigos IATA, desenvolvida com Flask e hospedada no Google Cloud Platform (GCP) através do App Engine.

## 📋 Sobre o Projeto

A API permite consultar o nome de aeroportos através de seus códigos IATA (International Air Transport Association). Por exemplo, ao fornecer o código "SDU", a API retorna "Santos Dumont Airport".

### Funcionalidades

- **Consulta por código IATA**: Endpoint que recebe um código IATA e retorna o nome completo do aeroporto
- **Tratamento de erros**: Validação de parâmetros e códigos inexistentes
- **Base de dados**: Utiliza arquivo CSV com informações de aeroportos internacionais

## 🚀 Tecnologias Utilizadas

- **Python 3.13**: Linguagem de programação
- **Flask**: Framework web para criar a API REST
- **Gunicorn**: Servidor WSGI para produção
- **Google Cloud App Engine**: Plataforma de hospedagem
- **Google Cloud Endpoints**: Gerenciamento de APIs

## 📁 Estrutura do Projeto

```
endpoints/
├── app/
│   ├── main.py              # Aplicação Flask principal
│   ├── airports.py          # Classe para manipulação dos dados de aeroportos
│   ├── app.yaml            # Configuração do App Engine
│   ├── app_template.yaml   # Template de configuração
│   ├── requirements.txt    # Dependências Python
│   └── third_party/
│       └── airports.csv    # Base de dados dos aeroportos
├── deployment.yaml         # Configuração de deployment
├── openapi.yaml           # Especificação OpenAPI
├── openapi-gke.yaml       # Configuração para GKE
└── openapi-gke_autenticada.yaml
```

## 🔧 Configuração e Deploy

### Pré-requisitos

- Google Cloud SDK instalado e configurado
- Projeto no Google Cloud Platform criado
- App Engine habilitado no projeto

### Deploy no Google Cloud

1. **Clone o repositório e navegue até o diretório**:

   ```bash
   cd endpoints/app
   ```

2. **Configure o projeto no gcloud**:

   ```bash
   gcloud config set project lab-endpoints-464600
   ```

3. **Faça o deploy da aplicação**:

   ```bash
   gcloud app deploy
   ```

4. **Acesse a aplicação**:
   ```
   https://lab-endpoints-464600.ue.r.appspot.com
   ```

## 📚 Como Usar a API

### Endpoint Principal

**GET** `/airportName`

Retorna o nome do aeroporto baseado no código IATA fornecido.

#### Parâmetros

- `iataCode` (query parameter, obrigatório): Código IATA do aeroporto (3 letras)

#### Exemplos de Uso

**Requisição bem-sucedida:**

```bash
curl 'https://lab-endpoints-464600.ue.r.appspot.com/airportName?iataCode=SDU'
```

**Resposta:**

```
Santos Dumont Airport
```

**Outros exemplos:**

```bash
# Aeroporto Internacional do Rio de Janeiro
curl 'https://lab-endpoints-464600.ue.r.appspot.com/airportName?iataCode=GIG'

# Aeroporto de Congonhas
curl 'https://lab-endpoints-464600.ue.r.appspot.com/airportName?iataCode=CGH'

# Aeroporto Internacional de Guarulhos
curl 'https://lab-endpoints-464600.ue.r.appspot.com/airportName?iataCode=GRU'
```

#### Códigos de Status

- **200 OK**: Código IATA encontrado, retorna o nome do aeroporto
- **400 Bad Request**: Código IATA não fornecido ou não encontrado

#### Tratamento de Erros

**Parâmetro não fornecido:**

```bash
curl 'https://lab-endpoints-464600.ue.r.appspot.com/airportName'
```

Resposta: `No IATA code provided.`

**Código não encontrado:**

```bash
curl 'https://lab-endpoints-464600.ue.r.appspot.com/airportName?iataCode=XXX'
```

Resposta: `IATA code not found : XXX`

## ⚙️ Configuração Técnica

### App Engine (app.yaml)

```yaml
runtime: python313
env: flex
entrypoint: gunicorn -b :$PORT main:app

runtime_config:
  operating_system: "ubuntu22"
  runtime_version: "3.13"

endpoints_api_service:
  name: lab-endpoints-464600.appspot.com
  rollout_strategy: managed
```

### Dependências (requirements.txt)

```
Flask==2.3.3
gunicorn==21.2.0
```

## 🛠️ Desenvolvimento Local

Para executar o projeto localmente:

1. **Instale as dependências**:

   ```bash
   pip install -r requirements.txt
   ```

2. **Execute a aplicação**:

   ```bash
   python main.py
   ```

3. **Acesse localmente**:
   ```
   http://127.0.0.1:8080/airportName?iataCode=SDU
   ```

## 📝 Notas Importantes

- **Shell zsh**: Ao usar curl no zsh, coloque URLs com query parameters entre aspas simples para evitar problemas de interpretação
- **Códigos IATA**: A API aceita códigos de 3 letras conforme padrão internacional
- **Case sensitive**: Os códigos IATA devem ser fornecidos em maiúsculas

## 🔗 Links Úteis

- [Google Cloud App Engine](https://cloud.google.com/appengine)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Códigos IATA](https://www.iata.org/en/publications/directories/code-search/)

## 👥 Contribuição

Este é um projeto de laboratório para demonstração de conceitos de API REST e deploy no Google Cloud Platform.

---

**Autor**: Witalo  
**Projeto**: Lab Endpoints  
**Data**: Julho 2025
