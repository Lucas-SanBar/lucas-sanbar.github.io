---
title: "Desbravando o Power BI: Como Criar Seu Primeiro Custom Visual do Zero"
description: Neste guia, mostrarei como começar do zero no desenvolvimento de custom visuals, passando pela configuração do ambiente, ferramentas necessárias e criando um gráfico no estilo lollipop chart.
date: 2024-10-18
categories: [Power BI, Custom Visual]
tags: [power bi, custom visual, development, chart]     # TAG names should always be lowercase
---

Se você já domina o Power BI e está buscando dar o próximo passo, criar seus próprios custom visuals pode ser a chave para levar suas análises a um novo patamar. Custom visuals permitem que você personalize ainda mais suas visualizações, criando gráficos e componentes que atendam a alguma necessidade específica sua e oferecendo maior flexibilidade na apresentação dos dados.

Neste guia, mostrarei como começar do zero no desenvolvimento de custom visuals, passando pela configuração do ambiente, ferramentas necessárias e criando um gráfico **simples** no estilo lollipop chart 🍭 utilizando a biblioteca [D3](https://d3js.org).

![Lollipop Chart](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/lollipop-chart-1.gif)
_Lollipop chart criado utilizando a biblioteca D3_

## Ferramentas necessárias
1 **(Node.js) :** O primeiro passo para começar o desenvolvimento de custom visuals é a instalação do Node.js, permitindo que você execute scripts Java Script fora do navegador, o que é fundamental para o processo de criação dos seus visuais personalizados. Para instalá-lo, basta acessar o 🔗site oficial do Node.js, baixar a versão recomendada para o seu sistema operacional e seguir as instruções de instalação.

![imagem 1](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/1.webp)

Certifique-se de marcar a ✅checkbox para que seja feita a instalação automática das ferramentas para módulos nativos, inclusive do [🔗Chocolatey](https://chocolatey.org), que é um gerenciador de pacotes para o Windows que facilita o processo de atualização e download dessas ferramentas através de linha de comando.

![imagem 2](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/2.webp)

2 **(VS Code) :** A segunda etapa é a instalação do Visual Studio Code (VS Code), um editor de código leve e extremamente poderoso. O VS Code é a ferramenta ideal para editar e gerenciar os arquivos do seu projeto, oferecendo suporte nativo a JavaScript e TypeScript, além de diversas extensões que facilitam o processo de desenvolvimento. Para instalação, basta baixar o VS Code do [🔗site oficial](https://code.visualstudio.com/download) e instalá-lo em seu sistema. Uma vez configurado, o VS Code permitirá que você escreva, depure e teste o código dos seus custom visuals de forma mais eficiente.

![imagem 3](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/3.webp)

Certifique-se de marcar a ✅checkbox para adicionar o VS code as variáveis do sistema PATH e de reiniciar a máquina após a instalação.

![imagem 4](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/4.webp)

3 **(Power BI Visual CLI Tool — PBIVIZ) :** A terceira etapa é a instalação do Power BI Visual Tools (PBIVIZ) via [🔗npm](https://www.npmjs.com/package/powerbi-visuals-tools) (Node Package Manager). O PBIVIZ é a ferramenta oficial que permite a criação, desenvolvimento e empacotamento de visuais personalizados para o Power BI. Para instalá-la, após ter o Node.js configurado, você precisa abrir o Node.js command prompt e executar o seguinte comando: `npm install -g powerbi-visuals-tools`. Esse comando vai instalar o PBIVIZ globalmente no seu sistema, permitindo que você acesse o conjunto de ferramentas necessárias para iniciar novos projetos, pré-visualizar seus visuais e gerar os pacotes para serem usados no Power BI.

![imagem 5](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/5.webp)

Se tudo ocorrer como planejado e nenhum problema na instalação ocorrer, basta agora executar o seguinte comando para verificar as funcionalidades: pbiviz . O Seguinte resultado deve ser retornado:

![imagem 6](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/6.webp)

## Configurando o ambiente de desenvolvimento
Se as três etapas anteriores foram seguidas corretamente, estamos prontos para finalmente configurar o ambiente de desenvolvimento.

1 **(Criar um certificado para teste local) :** O primeiro passo na configuração do ambiente de desenvolvimento é garantir que você possa testar seus custom visuals de forma segura e funcional no Power BI. Para isso, é necessário gerar e instalar o certificado SSL, que permitem que o Power BI reconheça e execute seus visuais localmente durante o desenvolvimento. Para gerar esse certificado, basta rodar o seguinte comando no terminal: `pbiviz install-cert`. Esse comando cria o certificado necessário e faz a instalação do mesmo para que você possa visualizar seus custom visuals diretamente no Power BI durante o processo de criação e depuração.

![imagem 7](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/7.webp)

Certifique-se de marcar a ✅checkbox para apenas o Usuário Atual.

![imagem 8](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/8.webp)

Avance até a seguinte etapa, e preencha a senha da chave privada com o valor fornecido pelo comando utilizado anteriormente, no meu caso a senha é **_93133_**.

![imagem 9](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/9.webp)

Avance até a seguinte etapa, selecione o ✅checkbox para “Colocar todos os certificados no repositório a seguir de certificados” e selecione a opção “Autoridades de Certificação Raiz Confiáveis” como mostrado a seguir e avance até concluir a instalação do certificado.

![imagem 10](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/10.webp)

2 **(Uma conta do Power BI Pro, PPU ou de avaliação gratuita) :** Para dar continuidade ao desenvolvimento, é importante ter uma conta no Power BI, seja na versão Power BI Pro, Power BI Premium por Usuário (PPU) ou até mesmo uma de avaliação gratuita. Caso não tenha nenhuma, basta se cadastrar no [🔗site oficial](https://www.microsoft.com/pt-br/power-platform/products/power-bi/pricing) da Microsoft para uma conta gratuita.

![imagem 11](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/11.webp)

A conta é necessária para que você possa acessar e utilizar todos os recursos da plataforma e testar “on the fly” o visual criado.

3 **(Ativar o modo Power BI Developer Mode) :** Para desenvolver um visual do Power BI, é necessário habilitar as opções de “Modo de desenvolvedor” no Power BI desktop e no Power BI Service.

◼Para habilitar a opção no Power BI desktop, basta ir no relatório .PBVIZ a ser publicado -> File -> Options and Settings -> Options e ativar o seguinte ✅checkbox:

![imagem 12](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/12.webp)

◼Para habilitar a opção no Power BI service, basta acessar a opção de “Developer settings” no canto superior direito e ativar a opção “Developer mode” para 🟢**On**

![imagem 13](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/13.webp)

4 (Criar a estrutura de projeto do visual do Power Bi) : Com o ambiente de desenvolvimento configurado, certificado instalada e sua conta no Power BI ativa, a próxima etapa é criar a estrutura do seu projeto. Para gerar automaticamente a estrutura básica de arquivos e pastas para o seu visual personalizado. Basta abrir o terminal, navegar até o diretório onde deseja criar o projeto, e executar o comando `pbiviz new <nome-do-projeto>`. Esse comando criará uma nova pasta com o nome do projeto e incluirá todos os arquivos essenciais, como o manifesto do visual, arquivos de configuração, além dos templates de código HTML, CSS e TypeScript. A partir dessa estrutura inicial, você pode começar a desenvolver e personalizar o seu visual conforme as necessidades do seu projeto.

![imagem 14](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/14.webp)

Para abrir o seu novo projeto no VS Code, execute o comando `code .` dentro do projeto criado.

A estrutura de arquivos e pastas criada segue o seguinte modelo:

![imagem 15](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/15.webp)

◼package.json — Usado pelo npm para gerenciamento de pacotes e informação do projeto;

◼pbiviz.json — Arquivo principal para seu projeto, contem os metadados do mesmo;

◼capabilities.json — Definições das principais capacidades e propriedades do visual criado;

◼tsconfig.json — Arquivo de configuração para Typescript;

Para um entendimento mais detalhado da estrutura do projeto acesse a [🔗pagina oficial](https://learn.microsoft.com/en-us/power-bi/developer/visuals/visual-project-structure) no Microsoft Learn.

## Criando o primeiro custom visual

Com todas as ferramentas instaladas e o ambiente de desenvolvimento devidamente configurado, chegou a hora de colocar a mão na massa e começar a criar seus custom visuals no Power BI. Você já tem a estrutura do projeto pronta e todas as dependências instaladas.

Agora que tudo está pronto, vamos começar criando um visual simples como exemplo para entender o fluxo básico de desenvolvimento de um custom visual no Power BI.

### Modificar o arquivo Visual.ts

1.No VS Code, no painel do Explorer, expanda a pasta src e selecione o arquivo visual.ts.

![imagem 16](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/16.webp)

2.Remova todo o código já criado e importe as bibliotecas e módulos necessários e defina a seleção de tipo para a biblioteca d3:
```javascript
import * as d3 from "d3";
import powerbi from "powerbi-visuals-api";
import IVisualHost = powerbi.extensibility.IVisualHost;
import VisualUpdateOptions = powerbi.extensibility.visual.VisualUpdateOptions;
import VisualConstructorOptions = powerbi.extensibility.visual.VisualConstructorOptions;
import DataView = powerbi.DataView;
```

Os seguintes itens importados são:

1. IVisualHost — uma coleção de propriedades e serviços usados para interagir com o host do visual (Power BI).
2. Biblioteca D3 — biblioteca JavaScript para criar documentos controlados por dados.

3.Abaixo das importações, crie uma classe `visual` vazia. A classe `visual` implementa a interface IVisual em que todos os visuais começam:
```javascript
export class Visual implements powerbi.extensibility.visual.IVisual {

}
```

4.Adicione métodos privados de nível de classe no início da classe `visual`:
```javascript
    private target: HTMLElement;
    private svg: d3.Selection<SVGElement, any, HTMLElement, any>;
    private host: IVisualHost;
```

5.Defina o método `constructor` dentro da classe `visual`. Esse método é chamado sempre que o visual é instanciado.
```javascript
    constructor(options: VisualConstructorOptions) {
        this.target = options.element;
        this.host = options.host;

        // Criação do elemento SVG
        this.svg = d3.select(this.target)
            .append('svg')
            .classed('lollipop-chart', true);
    }
```

O método `constructor` configura o ambiente inicial para o visual. Ele salva referências importantes (`element` e `host`) e cria um elemento SVG onde o gráfico será desenhado. Ele faz isso uma única vez quando a classe é instanciada, garantindo que o gráfico tenha um lugar apropriado para ser renderizado.

6.Adicione o método `update` logo após o método `constructor`. Este é o responsável por renderizar ou atualizar o gráfico sempre que houver mudanças nos dados ou no tamanho da visualização. Ele é chamado sempre que os dados ou as configurações do visual são alterados, permitindo que o gráfico seja redesenhado conforme necessário.
```javascript
public update(options: VisualUpdateOptions) {
        // Limpa o SVG para evitar duplicação
        this.svg.selectAll("*").remove();

        const width = options.viewport.width;
        const height = options.viewport.height;

        // Ajuste do tamanho do SVG
        this.svg
            .attr('width', width)
            .attr('height', height);

        const dataView: DataView = options.dataViews[0];
        if (!dataView || !dataView.categorical || !dataView.categorical.categories || !dataView.categorical.values) {
            return;
        }

        const categorical = dataView.categorical;
        const category = categorical.categories[0];
        const values = categorical.values[0];

        const data = category.values.map((d, i) => ({
            category: d as string,
            value: values.values[i] as number
        }));

        // Recupera a cor escolhida para os círculos
        const objects = dataView.metadata.objects;

        // Margens para o gráfico
        const margin = { top: 20, right: 100, bottom: 50, left: 100 };
        const chartWidth = width - margin.left - margin.right;
        const chartHeight = height - margin.top - margin.bottom;

        // Escalas
        const y = d3.scaleBand()
            .domain(data.map(d => d.category))
            .range([0, chartHeight])
            .padding(1); // Mais espaçamento para o gráfico

        const x = d3.scaleLinear()
            .domain([0, d3.max(data, d => d.value)]) // Definir o domínio do eixo x conforme os dados
            .nice() // Garante que o eixo tenha intervalos mais "limpos"
            .range([0, chartWidth]);

        // Grupo principal para o gráfico
        const chart = this.svg.append('g')
            .attr('transform', `translate(${margin.left},${margin.top})`);

        // Eixo Y (categorias)
        chart.append('g')
            .attr('class', 'y-axis')
            .style('font-size', '15px')
            .call(d3.axisLeft(y));

        // Eixo X (valores)
        chart.append('g')
            .attr('class', 'x-axis')
            .style('font-size', '15px')
            .attr('transform', `translate(0, ${chartHeight})`)
            .call(d3.axisBottom(x));

        // Linhas do gráfico
        chart.selectAll('myline')
            .data(data)
            .enter()
            .append('line')
            .attr('x1', 0)
            .attr('x2', 0)
            .attr('y1', d => y(d.category))
            .attr('y2', d => y(d.category))
            .attr('stroke', 'black')
            .attr('stroke-width', 1)
            .attr('stroke-dasharray', '4,2') // Define um padrão de traços e espaços
            .transition()  // Inicia a transição
            .duration(2000)  // Define a duração de 2000ms (2 segundos)
            .attr('x1', d => x(d.value));  // Define o valor final de x1 com a transição

        // Ponto do lollipop (círculos após as linhas)
        chart.selectAll('mycircle')
            .data(data)
            .enter()
            .append('circle')
            .attr('cx', 0)
            .attr('cy', d => y(d.category) + y.bandwidth() / 2)
            .attr('r', 6)
            .style("fill", '#69b3a2')  // Aplica a cor escolhida no painel de formatação
            .attr("stroke", "black")
            .transition()  // Inicia a transição
            .duration(2000)  // Define a duração de 2000ms (2 segundos)
            .attr('cx', d => x(d.value));  // Define o valor final de cx com a transição

        // Texto com os valores da medida
        chart.selectAll('mytext')
            .data(data)
            .enter()
            .append('text')
            .attr('x', 0)  // Inicializa a posição X no começo (0)
            .attr('y', d => y(d.category) + y.bandwidth() / 2 + 5)  // Centraliza o texto verticalmente
            .text(d => d.value.toFixed(2))  // Exibe o valor numérico
            .style('fill', 'black')  // Define a cor do texto
            .style('font-size', '12px')  // Tamanho da fonte
            .transition()  // Inicia a transição
            .duration(2000)  // Define a duração de 2000ms (2 segundos)
            .attr('x', d => x(d.value) + 12);  // Move o texto ao lado do círculo
    }
```

### Modificar o arquivo Capabilities.json
1.No VS Code, no painel do Explorer, selecione o arquivo capabilities.json.

![imagem 17](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/17.webp)

2.Remova todo o código já criado e preencha da seguinte forma:
```json
{
  "dataRoles": [
      {
          "name": "category",
          "kind": "Grouping",
          "displayName": "Category"
      },
      {
          "name": "measure",
          "kind": "Measure",
          "displayName": "Measure"
      }
  ],
  "dataViewMappings": [
      {
          "categorical": {
              "categories": {
                  "for": {
                      "in": "category"
                  }
              },
              "values": {
                  "for": {
                      "in": "measure"
                  }
              }
          }
      }
  ],
  "privileges": []
}
```

Este json é responsável por definir as propriedades e funcionalidades do visual, incluindo quais dados podem ser usados, as configurações disponíveis para o usuário e como esses dados serão mapeados visualmente. Ele também configura os painéis de formatação, as interações entre os elementos visuais e o comportamento do visual no Power BI. Neste exemplo o nosso lollipop chart contará com uma medida e uma categoria (A qual a medida selecionada será agrupada).

As seguintes estruturas são permitidas dentro do capabilities.json:
1. dataRoles— Define os tipos de dados que o visual pode aceitar. Por exemplo, categorias (dimensão) ou medidas.
2. dataViewMappings — Mapeia os dados que são vinculados aos papéis definidos em dataRoles para o gráfico e descreve como os dados serão estruturados.
3. object — Define as opções de personalização, como cores, tamanhos, rótulos, entre outros. Essas opções aparecem no painel de formatação do Power BI e permitem que o usuário final personalize a aparência e comportamento do visual.

### Testando o visual criado
Após fazer todas as alterações e salvar os arquivos do projeto podemos então seguir para o processo de teste do visual customizado no Power BI.

1.O primeiro passo é, compilar e iniciar o servidor de testes, no diretório do projeto ou no terminal do VS Code apenas execute o seguinte comando `pbiviz start` . Caso tudo ocorra como o planejado o seguinte log será retornado:

![imagem 18](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/18.webp)

Pronto, agora o código foi compilado e o comando inicia um servidor local na sua máquina. Esse servidor fornece uma URL (geralmente http://localhost:8080), que pode ser acessada pelo Power BI para carregar o visual em desenvolvimento.

2.No painel publicado com o modo de desenvolvedor ativo, adicione o visual chamado Developer Visual.

![imagem 19](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/19.webp)

3.Adicione as capacidades criadas no capabilities.json, dimensão e medida:

![imagem 20](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/20.webp)

4.O seguinte visual deve finalmente aparecer:

![imagem 21](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/21.webp)
![imagem 22](https://raw.githubusercontent.com/Lucas-SanBar/lucas-sanbar.github.io/refs/heads/main/_posts/postimages/18-10-2024/lollipop-chart-2.gif)

# Conclusão
É isso! Espero que este guia tenha sido útil. Se você tiver alguma dúvida ou precisar de mais esclarecimentos, sinta-se à vontade para entrar em contato comigo no [🔗LinkedIn](https://www.linkedin.com/in/lucas-barbosa-517259169). Ficarei feliz em ajudar ou aprimorar qualquer parte do guia para uma melhor compreensão e contribuir com a comunidade.
