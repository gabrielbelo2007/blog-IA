# Documentação do Blog IA - BIA
> Usando Github pages e Jkelly

### Ferramentas jkelly localmente (Testes do Projeto)
> Requer instalação do Ruby e do Jkelly na máquina

```bash
    # Instalar dependencias
    bundle install
    
    # Executar servidor local
    bundle exec jekyll serve
    
    # Limpar registro do servidor local
    bundle exec jekyll clean
```

### Configuração do servidor

> Arquivo _config.yml 

```yaml

# Define titulo principal do site
title:  IA BLOG - UFRPE

# Endereco de contato
email:  gabrielbelo.dev@gmail.com

# Descricao do site
description:  >- (define que nao deve ter quebra de linha no texto abaixo)
Esse e um blog de IA, onde serão postados artigos e tutoriais sobre Inteligência Artificial,
Aprendizado de Máquina e Ciência de Dados.

# Define o diretorio no qual o site sera servido
baseurl:  "/blog-IA"  # the subpath of your site, e.g. /blog

# Dominio padrao do Github Pages
url:  "gabrielbelo2007.github.io" 

# Informacao adicional para ser anexada ao site caso necessario
github_username:  gabrielbelo2007

# Processador de .md para HTML
markdown:  kramdown

# Listas de plugins do Jkelly
plugins:
- jekyll-sass-converter
```

### Estrutura do diretório

#### Pasta Principal (root)

- _includes (Armazena elementos portáteis para as páginas)
	- footer.html
	- header.html

- _ layouts (Estrutura das páginas)
	- default.html
	- post.html
	- 404.html

- assets (Estilização das páginas)
	- style.css

> Precisam seguir uma convenção de nomenclatura:  ANO-MÊS-DIA-titulo-do-post
- _posts (Pasta especial que armazena as postagens)
	- 2025-06-16-post.md 
	
- pages (Armazena as paginas principais do site)
	- index.md
	- about.md
	- posts.md
	- 404.md

### Estrutura dos Posts

> Front Matter: Define os metadados da página e as variáveis personalizadas
```yaml
---
layout:  post
title:  Utilizando a API do Google Gemini
subtitle:  Use o Modelo de IA do Google em Python
date:  2024-09-10 14:30:00 -0300
author:  Vinicius B. Pessoa
categories: [Inteligência Artificial, Deep Learning]
tags: [IA Generativa, Chatbots, Criação de Conteúdo, DALL-E]
---
```
---
> Exemplo: conteúdo do post

O **Gemini** é um novo modelo de linguagem que veio competir com o **ChatGPT/GPT** da OpenAI. A versão Gemini 1.5 (e subversões) veio competir diretamente com o GPT 4 (e subversões).

Neste artigo, não vamos usar a [interface do Gemini para usuários em geral](https://gemini.google.com/). O que queremos, aqui, é mostrar como acessar o _Gemini_ por meio do código de um programa Python, usando uma **API** (Interface de Programação de Aplicações) própria do Gemini.

**_E o que podemos fazer assim?_**

Podemos criar programas que usam o Gemini adaptado ao contexto específico da sua aplicação. Por exemplo:

-   Para acessar a dados específicos do usuário, como documentos ou atividades agendadas.
-   Para criar resumos, classificar textos, ou fazer outro tipo de raciocínio em linguagem natural.
-   Para fazer o Gemini disparar código Python criado por você.

Enfim, você pode mesclar a capacidade de linguagem e raciocínio do Gemini com a capacidade de programação Python tradicional para criar programas inovadores!

Neste documento, você saberá como acessar e utilizar o **Google Gemini** para esse fim.

# 1 - Configurações Iniciais

## 1.1 - Pré-requisitos

1 - Python 3.9 ou superior.

2 - Possuir o módulo  `google-generativeai`  instalado. Para isso, basta rodar o comando abaixo na linha de comando:

```bash
pip install google-generativeai
```

## 1.2 - Obtendo uma Chave para a API

Para acessar os modelos Gemini, você vai precisar de uma chave privada. Para gerá-la, siga estes passos:

1 - Acesse o seguinte site: [Google API Key.](https://aistudio.google.com/app/apikey?hl=pt-br)

2 - Caso ainda não esteja logado, faça o login utilizando sua conta Google e aceite os termos de serviço.

3 - Clique no botão **Criar chave de API** para criar uma chave para um novo projeto.

4 - Nesse momento será exibido uma mensagem com os termos de serviço, basta clicar em **OK** para prosseguir.

5 - Uma caixa contendo a opção de **Criar uma chave de API em um novo projeto**. Basta clicar para prosseguir.

6 - Parabéns sua chave de API já foi gerada. Agora copie a chave e salve-a em algum lugar seguro.

**Observação:** A chave da API é um elemento crítico para acessar os serviços fornecidos pela Google API. Essa chave funciona como uma _senha_ que autentica suas solicitações para usar os modelos Gemini. Por esse motivo, deve-se prestar muita atenção para que a mesma não fique disponível publicamente.

## 1.3 - Armazenando a Chave da API

Para garantir que a chave da API não fique disponível publicamente, é recomendado salvá-la em uma variável de ambiente  `GOOGLE_API_KEY`.

Você pode fazer isso de uma maneira prática criando um arquivo  `.env`  no seu projeto e carregando-o por meio de código Python.

Para isso, siga estes passos

1.  Crie um arquivo chamado exatamente  `".env"`
2.  Abra-o como arquivo de texto e escreva este conteúdo:

```
GOOGLE_API_KEY=<Sua chave da API>.
```

3 - Lembrar de substituir o trecho  `<Sua chave da API>`  pela sua chave.

**Atenção:** Nunca faça o _commit_ do arquivo  `.env`  para um repositório git público! É grande o risco de que algum _hacker_ a descubra e use.

## 1.4 - Carregando a Chave em Python

Agora, usaremos a biblioteca  `python-dotenv`  para carregar as variáveis de ambiente salvas em arquivo  `.env`. Porém, para deixar mais fácil, criamos duas funções auxiliares:

-   **carrega_chave()** : Carrega a chave da API armazenada no arquivo .env e configura a biblioteca Gemini com essa chave.
-   **verifica_chave()**: Verifica a existência do arquivo .env no sistema.

```python
def carrega_chave(): # Carrega uma chave do arquivo .env
    _ = load_dotenv(find_dotenv())
    chave = os.getenv("GOOGLE_API_KEY")
    gemini.configure(api_key=os.getenv('GOOGLE_API_KEY'))

    print(f"Chave API carregada com sucesso!")
    return chave

def verifica_chave(): # Verifica a existência de um arquivo .env 
    return find_dotenv()
```

Agora, basta fazer a chamada abaixo e…

```python
chave = carrega_chave()
```

…sua chave estará carregada!

# 2 - Verificando modelos disponíveis

Primeiramente devemos verificar quais são todos os modelos disponíveis para utilização dsiponibilizados pelo  **Google Gemini**.

Para isso basta utilizar o seguinte comando para receber a lista dos modelos existentes.

```python
modelos = client.get_default_model_client().list_models()
```

Agora a variável **modelos** contém uma lista contendo todos os modelos existentes.

Para listar apenas os modelos generativos, utilize um loop  `for`  para filtrar os dados em que  `model.supported_generation_methods`  seja igual a  `'generateContent'`  como demonstra o código a seguir.

```python
for model in modelos:
    if 'generateContent' in model.supported_generation_methods:
        print(model.name)
```

Principalmente podemos trabalhar com os modelos mais atualizados, que são estes:

-   **gemini-1.5-flash**  - O de melhor custo benefício.  **Será utilizado nesta publicação**
-   **gemini-1.5-pro**  - O melhor modelo (ou seja, que gera o melhor conteúdo).

**Dica**: Antes de programar com os modelos, você pode testar todos eles usando uma interface amigável: o  [Google AI Studio](https://aistudio.google.com/)!

# 3 - Usando apenas Texto

Depois dessa longa preparação, vamos usar o modelo!

Este código instancia uma classe para acessar o modelo remotamente (lembrando que o modelo não é carregado localmente).

```py
model = gemini.GenerativeModel('gemini-1.5-flash')
```

Agora, vamos usá-lo em dois exemplos

## 3.1 - Gerando resumo

Netse caso vou solicitar ao modelo para fazer um resumo de um trecho da  [documentação da função  `statistics.median_high()`](https://docs.python.org/pt-br/3/library/statistics.html#statistics.median_high)  de Python.

Agora, vamos receber nossa primeira resposta do modelo via código, assim:

```py
resposta = model.generate_content("Faça um resumo em uma linha desse texto:"
    + "statistics.median_high(data) Retorna a mediana superior de dados numéricos. Se data for vazio, "
    + "a exceção StatisticsError é levantada. data pode ser uma sequência ou um iterável. "
    + "A mediana superior sempre é um membro do conjunto de dados. Quando o número de elementos for ímpar, "
    + "o valor intermediário é retornado. "
    + "Se houver um número par de elementos, o maior entre os dois valores centrais é retornado.")

```

Agora, você pode investigar algumas informações da  `resposta`  recebida com este código:

```py
print(resposta.text) # Resposta textual
print(resposta.usage_metadata) # Informações como número de tokens usados
```

A saída deve ser algo como:

```py
# `statistics.median_high(data)` retorna o maior dos dois valores médios em um conjunto de dados, ou o único valor médio se o número de elementos for ímpar. 

prompt_token_count: 102
candidates_token_count: 37
total_token_count: 139
```

A saída retornada pelo campo  `.usage_metadata`  indica quantos “tokens” foram usados do modelo. Os tokens são proporcionais à quantidade de palavras e são usados para definir o valor a ser cobrado pelo uso. (Mas vamos usá-lo dentro dos limites gratuitos aqui).

## 3.2 - Criando receitas

Neste segundo exemplo, queremos dar uma ideia de como usar esse modelo dentro de uma aplicação. A ideia seria criar um  _app_  que recomenda receitas para o usuário baseando-se nos ingredientes possuídos pelo usuário.

O código abaixo vai ler os ingredientes digitados e montar um  **prompt**  (ou  _query_), ou seja, uma pergunta ou comando para o modelo.

```py
# Pegando o input do suario (simulando uma aplicação).
ingredientes = input("Liste os ingredientes que você possui? ")

# Montando a string completa de 'prompt' ou 'query' (pergunta)
prompt = f"Com os seguintes ingredientes: {ingredientes}, escreva uma receita completa que utilize esses e apenas esses ingredientes."
```

Agora, podemos enviar o  _prompt_  ao modelo, receber a resposta e imprimir o resultado assim:

```py
# Gera a resposta que estamos buscando
resposta = model.generate_content(prompt)

# Imprime a resposta textual
print(resposta.text) 

# Informações sobre quantidades de tokens usados
print(resposta.usage_metadata) 
```

Caso o usuário tenha digitado  `'ovos, farinha de trigo, leite, açúcar, sal, manteiga'`, a saída seria algo assim:

```markdown
## Bolo Simples de Leite

**Ingredientes:**

* 3 ovos grandes em temperatura ambiente
* 1 xícara (120g) de manteiga sem sal, derretida e levemente resfriada
* 1 xícara (200g) de açúcar granulado
* 2 xícaras (250g) de farinha de trigo
* 1 colher de chá de fermento em pó
* 1/2 colher de chá de bicarbonato de sódio
* 1/2 colher de chá de sal
* 1 xícara (240ml) de leite integral

**Instruções:**

1. Pré-aqueça o forno a 180°C e unte e enfarinhe uma forma redonda com furo no centro de 23cm de diâmetro.
2. Em uma tigela grande, bata os ovos com a manteiga derretida e o açúcar até obter um creme claro e fofo.
3. Em outra tigela, misture a farinha, o fermento, o bicarbonato e o sal.
4. Adicione gradualmente os ingredientes secos à tigela com os ovos, alternando com o leite, começando e terminando com os ingredientes secos. Misture até incorporar todos os ingredientes, mas não bata demais.
5. Despeje a massa na forma preparada e asse por 30 a 35 minutos, ou até que um palito inserido no centro saia limpo.
6. Deixe o bolo esfriar na forma por 10 minutos antes de virá-lo em uma grade para esfriar completamente.

**Dicas:**

* Para um bolo mais úmido, adicione 1/2 xícara de frutas secas ou 1/2 xícara de chocolate picado à massa.
...
prompt_token_count: 33
candidates_token_count: 467
total_token_count: 500
```

Como você pode perceber, uma receita nova foi gerada pelo modelo!

## 4 - Fornecendo Imagens

Agora, vamos ver vamos utilizar o serviço passando uma imagem como entrada.

Primeiramente devemos carregar uma imagem em uma variável, para isso vou utilizar a biblioteca PLT a imagem está armazenada em “Imagens\animal.jpg” a partir do diretório que esse script está escrito.

Vamos usar o módulo  `PIL`  em um notebook Jupyter para exibir uma imagem localizada em  `Imagens\animal.jpg`. Crie uma célula de código e escreva o código abaixo para exibir a imagem:

```py
from IPython.display import Image as IPImage
import PIL.Image

img = PIL.Image.open(r'Imagens\animal.jpg')
img
```

Saída:

![Imagem de uma Capivara](https://github.com/ViniciusBPessoa/Repositorio_Academico_Multidisciplinar/blob/main/Extencao/Post_01/Imagens/animal.jpg?raw=true)

A partir desse momento já é possível enviar essa imagem como entrada para o modelo, e obter a resposta dele.

```py
resposta = model.generate_content(img)
print(resposta.text)
```

Saída:

```
This is a capybara. It is the largest rodent in the world and is native to South America.
Capybaras are semi-aquatic and are often found near bodies of water. 
They are herbivores and are known for their docile nature.
```

Vamos agora ver como enviar  **uma imagem e um texto**  para alguma finalidade específica. Por exemplo, vamos pedir para o modelo simplesmente identificar o animal presente na imagem de forma mais direta.

Para isso, basta que enviemos uma lista  **`[A, B]`**  contendo:

A - Texto de entrada, nesse caso  `"Identifique esse o animal nessa imagem"`.

B - Variável contendo a imagem.

```py
resposta = model.generate_content(["Identifique esse o animal nessa imagem", img])
print(resposta.text)
```

Saída:

```
Esse é um capivara.
```

No final deste artigo tem o link para o notebook com todo o código mostrado aqui.

# 5 - Utilizando para Conversar (Chat)

Agora, veremos como usar no modo de conversa (_chat_), permitindo uma interação contínua com um usuário.

Para isso, vamos usar uma classe especializada que armazena o histórico de mensagens trocadas. Você pode instanciá-la assim:

```py
chat_model = model.start_chat(history=[])
```

Agora, você pode enviar uma mensagem e ver a resposta assim:

```py
resposta = chat_model.send_message("Escreva uma linha de uma historia sobre o cachorro bernardo")
print(resposta.text)
```

Agora é possível ver todo o histórico de mensagens usando o seguinte comando:

```py
chat_model.history
```

Em um notebook Jupyter, você veria esta saída:

```py
[parts {
   text: "Escreva uma linha de uma historia sobre o cachorro bernardo"
 }
 role: "user",
 parts {
   text: "A cauda de Bernardo, geralmente um borrão feliz, caiu ao chão quando ele percebeu que o bebê estava desaparecido. \n"
 }
 role: "model"]

```

Vamos enviar outra mensagem e examinar a resposta:

```py
resposta = chat_model.send_message("Troque o nome de bernardo para severino")
print(resposta.text)
```

Saída:

```
A cauda de Severino, geralmente um borrão feliz, caiu ao chão quando ele percebeu que o bebê estava desaparecido.
```

A forma de acessar o modelo acima garante que haja uma continuidade no envio de mensagens para o modelo, permitindo que cada nova mensagem seja analisada no contexto da conversa anterior (como demonstrado pela troca do nome do cachorro).

Para finalizar, vamos ver como acessar os detalhes da conversa:

```py
for mensagem in chat_model.history:
  print(f'{mensagem.role}: {mensagem.parts[0].text}')
```

Cada item de  `chat_model.history`  (acessado pela variável  `mensagem`) tem:

-   o campo  `.role`  para indicar quem mandou a mensagem (“user” ou “model”),
-   o campo  `.parts`  para indicar cada parte da mensagem, que pode ser texto ou imagem (na maioria dos casos, tem apenas uma parte, contendo texto).

Agora você deve ser capaz de usar os modelos  [Gemini](https://gemini.google.com/)  para criar projetos inovadores!

---
