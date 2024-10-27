---
title: "Desbravando o Power BI: Usando o ChatGPT para Criar Colunas e Tabelas no Power BI"
description: Neste guia, mostrarei como enviar prompts do ChatGPT dentro de scripts do Power Query e salvar os resultados diretamente como novas colunas ou tabelas em seus dados.
date: 2024-10-24
categories: [Power BI, ChatGPT]
tags: [power bi, chatgpt, development, table synthesis, column synthesis]     # TAG names should always be lowercase
image: 
  path: /assets/images/24-10-2024/Capa-24-04-2024.webp
---

O ChatGPT tem muitos usos para melhorar a produtividade e a eficiência de um profissional de dados. Com casos de uso que variam desde assistência no desenvolvimento, como recomendações de código, solução de problemas e formatação, até casos mais simples, como redação de documentação, elaboração de planos de projeto e criação de programas de aprendizado.

Hoje, vou apresentar um guia prático sobre como enviar prompts do ChatGPT dentro de scripts do Power Query e salvar os resultados diretamente como novas colunas ou tabelas em seus dados. Essa integração oferece uma maneira de enriquecer suas análises, permitindo que você aproveite um pouco mais a inteligência artificial.

<iframe src="https://capture.dropbox.com/embed/JHusE0SOwu8hrG8B?source=copy-embed" width="728" height="410" frameborder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Neste guia, mostrarei uma possível forma de se utilizar a API da OpenAI para criar novas colunas e tabelas no Power BI. Apresentando os princípios básicos da integração, exemplos práticos e uma discussão sobre suas limitações.  

---
## Obtendo uma chave de segurança da API
O primeiro passo do processo é obter uma chave de segurança da API da OpenAI, esta serve para autenticar e autorizar seu acesso aos seus serviços, incluindo o ChatGPT, e nos permitindo acessar o mecanismo de forma programática.

A chave de segurança tem as seguintes funções:

◼**Autenticação:** A chave identifica sua conta e valida que você tem permissão para acessar os serviços da API.

◼**Autorização:** Ela garante que você tenha os direitos necessários para realizar solicitações específicas, como geração de texto ou análise de dados.

◼**Controle de uso:** A chave permite à OpenAI monitorar e controlar o uso da API, incluindo o número de solicitações feitas e o volume de dados processados. Isso ajuda a evitar abusos e garante que você permaneça dentro dos limites de uso estabelecidos.

◼**Faturamento:** A chave está vinculada à sua conta de pagamento e/ou a quantidade de crédito na plataforma, permitindo que a OpenAI cobre pelo uso da API de acordo com a estrutura de preços.

Para criação da chave os seguintes passos são necessários:

1.Acesse o site [🔗platform.openai.com](https://platform.openai.com/docs/overview) e clique em “Sign Up” se você não tiver uma conta. Se já tiver uma conta, basta fazer o login na plataforma.

![Imagem 2](/assets/images/24-10-2024/2.webp)

2.Após o login navegue até a seção de API Keys da plataforma, selecionando “Dashboard” no canto superior direito e depois a opção “Api Keys”.

![Imagem 3](/assets/images/24-10-2024/3.webp)

3.Selecione o botão “Create a new secret key”, escolha um nome, projeto, permissão e por último o botão “Create secret key”.

![Imagem 4](/assets/images/24-10-2024/4.webp)

4.A seguir copie e salve a sua chave. Por motivos de segurança, você não poderá visualizá-la novamente através da sua conta da OpenAI. 

Se você perder essa chave, precisará gerar uma nova.
{: .prompt-danger }

![Imagem 5](/assets/images/24-10-2024/5.webp)

Depois de obter sua chave, você pode então começar a integrar a API da OpenAI em suas aplicações, incluindo seu uso dentro do Power Query.

---
## Conectando o ChatGPT ao Power Query

Estamos prontos para continuar com a segunda parte do processo.
Nesta etapa, utilizaremos a função baseada na [🔗documentação](https://platform.openai.com/docs/api-reference/introduction) da API.

1.Va para o Power Query via “Transform Data”.

![Imagem 6](/assets/images/24-10-2024/6.webp)

2.Crie uma nova “consulta em braco”.

![Imagem 7](/assets/images/24-10-2024/7.webp)

3.Abra o “editor avançado”.

![Imagem 8](/assets/images/24-10-2024/8.webp)

4.Copie e cole o seguinte código no seu editor avançado:

```ts
let
    //A função é criada com um parâmetro de entrada chamado prompt do tipo text. 
    //Essa função retorna um valor de texto, que é o conteúdo da resposta gerada pelo ChatGPT para o prompt fornecido.
    Output = (prompt as text) as text =>
    let
        //A variável url define o endpoint da API da OpenAI para chat/completions, que é o caminho usado para interagir com o modelo de chat.
        url = "https://api.openai.com/v1/chat/completions",
        //A variável apiKey recebe o valor da chave de segurança que você gerou
        apiKey = "---SUA CHAVE DE SEGURANÇA AQUI---",
        //A variável headers define os cabeçalhos HTTP necessários para a requisição
        headers = [
            //Requisição em JSON.
            #"Content-Type" = "application/json",
            //Autentica a requisição utilizando a variável contendo a chave de API.
            #"Authorization" = "Bearer " & apiKey
        ],
        //Especifica o modelo a ser usado e envia uma lista de mensagens, onde o prompt passado é a entrada principal
        body = Json.FromValue([model = "gpt-4o-mini",messages = {[role = "user", content = prompt]}]),
        response = Web.Contents(url, [Headers=headers, Content=body]),
        //Converte a resposta da API em um objeto JSON e extrai o texto da resposta armazenado na variável content
    in
        content
in
    Output
```
Altere o valor da variável `apiKey` para a chave de segurança que você gerou e altere o modelo em `model = "gpt-4o-mini"` caso necessário.

Os diferentes modelos representam versões distintas da tecnologia. Cada modelo possui características e capacidades que o tornam mais adequado para diferentes tipos de tarefas como:

◼**Precisão e complexidade:** Modelos mais recentes, como o `gpt-4o`, geralmente têm uma capacidade melhor de entender e gerar respostas complexas, pois foram treinados em conjuntos de dados maiores e mais diversificados. Isso permite que lidem melhor com detalhes em consultas complexas.

◼**Velocidade e custo:** Modelos menores, como o `gpt-4o-mini`, são otimizados para oferecer respostas rápidas e com menor custo de processamento. Eles são ideais para automações ou consultas rápidas onde não é necessária a precisão máxima de modelos mais complexos.

◼**Contexto e tamanho do prompt:** Modelos mais simples, como o `gpt-3.5-turbo`, podem ter limitações quanto ao tamanho do prompt e à profundidade da análise, o que pode ser uma consideração importante em alguns projetos.

◼**Aplicações especificas:** Dependendo da aplicação, pode ser mais eficiente usar um modelo otimizado. Como por exemplo, projetos que envolvem análise técnica complexa, produção de conteúdo extenso ou geração de código sofisticado o `gpt-4o` pode ser mais adequado.

Para mais detalhes sobre os modelos existentes acesse o [🔗site oficial](https://platform.openai.com/docs/models) da plataforma

![Imagem 9](/assets/images/24-10-2024/9.webp)

5.Teste a API invocando a função criada.

![Imagem 10](/assets/images/24-10-2024/10.webp)

Se tudo ocorrer como o planejado a função invocada deve retornar um resultado do prompt.

![Imagem 11](/assets/images/24-10-2024/11.webp)

>A capital do brasil é Brasilia. Foi Inaugurada em 221 de abril de 1960 e é conhecida por sua arquitetura modernista e planejamento urbano.

---
## Criando novas colunas baseando se em um prompt

Usar o ChatGPT para criar novas colunas no modelo de dados do Power BI pode ser uma abordagem poderosa para enriquecer suas análises de dados, automatizar processos repetitivos e popular tabelas de dimensões. Ao integrar a API do ChatGPT no Power Query, é possível gerar colunas personalizadas que atendem a requisitos específicos do negócio.

Podemos testar e colocar em prática a função criada previamente para gerar novas colunas da seguinte forma:

1.Crie uma tabela com os prompts a testar:

![Imagem 12](/assets/images/24-10-2024/12.webp)

2.Utilize a função criada para gerar novas colunas, na aba “add column” selecione a opção de invocar uma função, adicione um nome para a nova coluna e marque qual coluna contém o prompt (No meu caso a coluna Perguntas).

![Imagem 13](/assets/images/24-10-2024/13.webp)

Obtendo então o seguinte retorno:

![Imagem 14](/assets/images/24-10-2024/14.webp)

---
## Criando novas colunas com base em colunas já existentes no modelo

Outra forma de utilizarmos a função criada é a de adicionarmos informações de colunas já existentes na tabela ao prompt para se criar uma nova. Desta forma, podemos ter colunas mais especificas e mais relacionadas a tabela na qual a nova coluna vai ser criada. Por exemplo:

1.A seguinte tabela já existe no meu modelo e gostaria de adicionar uma nova coluna com base em informações das seguintes colunas:
- nome_produto;
- categoria_produto;
- preço_unitario;
- status_estoque;
- Marca;

![Imagem 15](/assets/images/24-10-2024/15.webp)

2.Invoque a função criada da seguinte forma:

```ts
= Table.AddColumn(#"Changed Type", "Marketing", each FxOpenAiApi(
"Com base no nome do produto= "&[nome_produto]&", 
na categoria do produto= "&[categoria_produto]&", 
no status do estoque= "&[status estoque]&", 
no status da promoção= "&[status promoção]&", 
da marca= "&[Marca]&",
com o preço= "&Text.From([preço_unitario])&". 
Crie um texto com no máximo 5 linhas contendo uma campanha de marketing para os produtos"
))
```
Obteremos uma nova coluna chamada “Marketing” que contém o resultado do prompt enviado.

![Imagem 16](/assets/images/24-10-2024/16.webp)

Exemplo de retorno para o produto da linha 3 — Playstation 5:

> 🌟 Aproveite a oportunidade exclusiva! 🌟 A PlayStation 5 está com **últimos 10% em estoque** e em **promoção imperdível** por apenas **R$ 3.499**! Garanta já o seu console da **Sony** e mergulhe em uma nova dimensão de jogos. Não perca essa chance de elevar a sua experiência gamer! 🕹️✨

---
## Criando tabelas inteiras com base em um prompt

Uma última forma de utilizarmos a API da OpenAI é a de gerar tabelas inteiras com base em uma requisição, podendo gerar então bases inteiras para testes ou melhorar as já existentes dentro do modelo. Para isso devemos alterar a função criada para a seguinte forma:

```ts
let
    //A função é criada com um parâmetro de entrada chamado prompt do tipo text. 
    //Essa função retornará agora uma tabela do tipo table para o prompt fornecido.
    Output = (prompt as text) as table =>
    let
        //A variável url define o endpoint da API da OpenAI para chat/completions, que é o caminho usado para interagir com o modelo de chat.       
        url = "https://api.openai.com/v1/chat/completions",
        //A variável apiKey recebe o valor da chave de segurança que você gerou
        apiKey = "---SUA CHAVE DE SEGURANÇA AQUI---",
        //A variável headers define os cabeçalhos HTTP necessários para a requisição
        headers = [
            //Requisição em JSON.
            #"Content-Type" = "application/json",
            //Autentica a requisição utilizando a variável contendo a chave de API.
            #"Authorization" = "Bearer " & apiKey
        ],
        //Especifica o modelo a ser usado e envia uma lista de mensagens, onde o prompt passado é a entrada principal
        body = Json.FromValue([model = "gpt-4o-mini", messages = {[role = "user", content = prompt]}]),
        response = Web.Contents(url, [Headers=headers, Content=body]),
        content = Json.Document(response)[choices]{0}[message][content],
        //Remove os caracteres de marcação retornados pela API.
        //Faz uma limpeza de espaços brancos.
        cleanText = Text.Trim(Text.Remove(content, {"`,"})),
        //Divide o texto em linhas.
        linhas = Lines.FromText(cleanText),
        //Define os cabeçalhos da tabela dividindo a primeira linha com base no delimitador.
        cabeçalhos = Text.Split(linhas{0}, ","),
        //processa cada linha após os cabeçalhos, dividindo o texto em valores usando o delimitador e convertendo-os em registros com base nos cabeçalhos.
        registros = List.Transform(List.Skip(linhas, 1), each 
           let
                valores = Text.Split(_, ",")
            in
            Record.FromList(valores, cabeçalhos)
            ),
        //cria a tabela final a partir dos registros usando
        tabela = Table.FromRecords(registros)
    in
        tabela
in
    Output
```

Desta forma, podemos solicitar uma tabela em algum formato especifico, fazer o tratamento no Power Query e obtendo uma tabela inteira a partir de um único prompt. Utilizando o seguinte prompt:

>Gere uma lista de produtos reais e com marcas reais contendo 10 linhas no formato CSV com as colunas: nome do produto, categoria do produto, data cadastro, preço unitário, marca e quantidade estoque.

Obtemos a tabela:

![Imagem 17](/assets/images/24-10-2024/17.webp)

![Create table](/assets/images/24-10-2024/1.gif)
_Gere uma lista de produtos com 100 linhas no formato CSV com as colunas: nome do produto, categoria do produto, data cadastro, preço unitário, marca e quantidade estoque._

---
## Limitações
Existe três grandes limitações utilizando a API do ChatGpt para síntese de novas tabelas e colunas em um modelo de dado no Power BI. Sendo elas:

◼**Velocidade🛵:** Chamar a API repetidamente é extremamente lento, mesmo para algumas centenas de linhas, sem falar em milhares. Isso ocorre em parte porque a API não apenas recupera dados, mas também os gera. Além disso, o Power Query executa uma chamada de API por vez, sem suporte para paralelização. Portanto, não tem viabilidade em usá-la para grandes volumes de dados.

◼**Consistência📈:** Cada vez que o modelo é atualizado, as respostas do ChatGPT são solicitadas novamente, e o resultado pode variar de uma execução para outra.

◼**Custo💲:** O custo da API do ChatGPT varia dependendo do modelo utilizado e da quantidade de tokens processados (tokens são as unidades de texto que o modelo utiliza para processar e gerar texto) e pode se tornar elevado dependendo da quantidade de requisições feitas. Para conhecimento os testes que executei no desenvolvimento desse guia consumiu **$0.02 USD** dos **$5.00 USD** adicionados como “crédito” na plataforma.

![Imagem 18](/assets/images/24-10-2024/18.webp)

![Imagem 19](/assets/images/24-10-2024/19.webp)

---
## Conclusão

É isso! Espero que este guia tenha sido útil. Se você tiver alguma dúvida ou precisar de mais esclarecimentos, sinta-se à vontade para entrar em contato comigo no [🔗LinkedIn](https://www.linkedin.com/in/lucas-barbosa-517259169). Ficarei feliz em ajudar ou aprimorar qualquer parte do guia para uma melhor compreensão e contribuir com a comunidade.
