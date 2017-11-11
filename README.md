![alt text](http://imageshack.com/a/img922/5924/DMrAzb.png)

## Roadmap

* [Versões](#versões)
* [Glossário](#glossário)
* [Interface Web](#interface-web)
* [Pré Requisitos](#pré-requisitos)
* [Usando o Visual Studio](#usando-o-visual-studio)
* [Por Linha de Comando](#por-linha-de-comando)
* [Arquivos de Configuração](#arquivos-de-configuração)

## Versões

* 2013
* **2015** [ 1 | 2 | **3** | 4 ]
* 2017
* 2018.RC

## Glossário

* `template` - Modelo de projeto utilizado na criação de um novo projeto TFS. _Templates_ são vinculados a uma _collection_ e passam ser apresentados como uma opção no momento de criar um novo projeto dentro dessa _collection_. Uma vez criado um projeto com um template, ele **nunca** mais poderá ser alterado, permitindo apenas customizações.

* `collection` - É uma coleção de projetos TFS. Na CWI, cada cliente possui uma _collection_ (LojasRenner) com vários projetos dentro (Portal Realize). Para adicionar ou remover templates e projetos é necessário ter a permissão de _collection administrator_.

* `project` - Cada projeto TFS. Dentro de um projeto podem ser definidas _areas_ e _teams_, de forma que seja possível organizar frentes de trabalho dentro de cada projeto. Para gerenciar _teams_, _areas_ e _iterations_, conceder permissão de acesso, customizar, adicionar e remover work-items, é necessário ter a permissão _project administrator_.

* `team` - É um sub-projeto. Todo projeto nasce com um _team_ \<Nome do Projeto>Team e esse será usado quando a raiz do projeto for acessada. Mas é possível definir outros sub-projetos. É muito fácil confundir _team_ com _area_. **O time é um sub-projeto e nada mais**: ele pode possuir uma configuração específica de usuários, grupos e permissões de acesso e é o time que irá aparecer na interface quando estiver procurando por projetos. Quando se está usando times, a url, então, fica [url-tfs/collection/project/team]().

* `area` - Uma _area_ é um conjunto de trabalho a ser feito. **Enquanto times estão ligados a projeto e usuários, as áreas estão unicamente ligadas a trabalho e aos _work-items_**. Os times podem ser associados a mais de uma área, o que indicaria que um determinado time poderia atuar sobre mais de um grupo de trabalho. Então podemos ter uma area X com suas _epics_, _features_, _user stories_, _tasks_, enfim, e uma area Y com outros _work-items_ desses tipos. Associando um time à área A, permitiria que esse time enxergasse todos _work-items_ dessa área em seu backlog, mas não os definidos na area Y (que não foi associada ao time). Geralmente se usa uma única _area_ principal associada a um time, mas é possível criar _areas_ filhas para agrupar seções de trabalho (não lembro de alguém já ter usado dessa forma). 

* `iteration` - A _iteration_ é a **sprint**. A _iteration_ define um intervalo de trabalho onde _work-items_ serão associados para que sejam concluídos. Assim como as _areas_, _iterations_ podem ser associadas a _teams_ e diferentes _teams_ podem ter uma mesma _iteration_. Possui apenas um nome (obrigatório), data de início e fim (opcionais). Geralmente se usa uma _iteration_ principal associada a um time e outras _iterations_ filhas com o nome desejado das sprints.

> Nota I: perceba que não existe uma ligação direta entre _team_, _area_ e _iteration_. Assim sendo, é possível que existam _areas_ e _iterations_ não associadas a _team_ algum - essas ficarão no limbo. A associação é feita sempre a partir do time.


> Nota II: é altamente recomendável nomear a raiz da _area_ e a raiz da _iteration_ com nomes adequados ao _team_ (geralmente a relação é um para um). Isso evita casos como no passado em que o time era "Análise da Proposta Team", a área era "Análise da Proposta" e a iteration raiz era "Team Análise da Proposta". Padrão, por favor.

* `work-item` - Um _work-item_ é um ítem de trabalho ( ¯\\\_(ツ)\_/¯ ). Cada elemento passível de ser entregue é um _work-item_: _tasks_, _user stories_, _features_, enfim. Os _work-items_ são visualizados no _backlog_ e no _board_ (desde que estejam em uma _iteration_) do projeto.

## Interface Web

Apesar de poucas, o TFS permite que algumas configurações sejam feias pela própria ferramenta web. Dentre essas configurações, podemos:

### Taskboard

* Exibir ou não o _ID_ do card
* Exibir ou não o _remaining work_
* Exibir ou não as _tags_
* Alterar modo de visualização do _assigned to_
* Exibir campos customizados aos cards
* Agrupar por _user story_ ou por _assigned to_
* Destacar cards por _assigned to_
* Ocultar campos com valores vazio ³
* Alterar o estilo do card de acordo com o valor de um campo ³
* Alterar visibilidade de _user story_, _feature_ e _epic_ ³
* Configurar _working days_ ³
* Comportamento de _bugs_ ³

> Nota: é possível definir essas configurações de forma independente para cada _work-item_

> ³ Disponível no **Update 3**

### Board

* Exibir ou não o _ID_ do card
* Exibir ou não o _story points_
* Exibir ou não as _tags_
* Alterar modo de visualização do _assigned to_
* Exibir campos customizados aos cards
* Separar as US em raias - _swimlanes_
* Definir as colunas que aparecem no board ¯\\\_(ツ)\_/¯
* Definir cores para as _tags_ ³ ¯\\\_(ツ)\_/¯
* Configurar ordenação de _user stories_ ³
* Configurar _cumulative flow_ ³

> ³ Disponível no **Update 3**

## Customização de Projetos

### Pré-requisitos

Para customizar um projeto do TFS é necessário:

* Windows - né? :)
* Visual Studio - versão **maior ou igual** à versão do TFS
* Productivity Power Tools - versão **igual** a do Visual Studio

### Usando o Visual Studio

Vamos recorrer ao Visual Studio quando for necessário alterar campos de um item, alterar as regras de preenchimento dos campos ou ainda o workflow do mesmo. Tudo isso pode ser feito apenas editando e importando arquivos `xml`, mas é muito mais fácil pelo Visual Studio nesses casos.

É possível alterar tanto ítens direto do projeto, quanto arquivos xml em disco. Também é possível exportar e importar _work-items_ pela interface do Visual Studio, mas não creio que seja possível exportar o _process-config_ e o _categories_.

Para alterar um _work-item_ existente no servidor, clique em _Tools > Process Editor > Open WITD from Server_. Se for a primeira vez, será necessário informar um usuário e senha válidos - lembrando que é necessário ter permissão de _project administrator_ para ter acesso às custmizações. Após logado, selecione o projeto desejado e, então, o item desejado.

* **Fields** - Primeira aba. Permite adicionar novas propriedades aos ítens, além de definir regras de preenchimento para os campos, para que seja possível estipular quais informações são default, obrigatórias, opcionais, disponíveis em uma lista, enfim. Também é possível criar campos novos customizados para atender particularidades do projeto.

![Customizando usando o Visual Studio na aba Fields](http://imageshack.com/a/img922/218/QySrBu.jpg)

> Nota: Uma vez criado um novo campo, é **impossível** alterar seu _reference name_ ou _data type_. Ou seja, uma vez criado um campo como `String`, ele será para sempre `String`.

* **Layout** - Segunda aba. Permite customizar a interface gráfica de criação e edição do _work-item_. É possível adicionar, na interface, campos customizados criados na primeira aba, alterar os labels dos campos e estrutura de layout da modal de edição do ítem. 

![Customizando usando o Visual Studio na aba Layout](http://imageshack.com/a/img924/9210/7pXoVg.png)

* **Workflow** - Segunda aba. Permite definir os status e transições entre status de um item. É possível, também, definir regras de preenchimento de campos específicos para cada status do item, como definir que para o status _closed_ é necessário ter apropriado horas no campo _completed work_. Nas transições é possível definir que campos terão valores alterados automaticamente após o item passar para determinado status.

![Customizando usando o Visual Studio na aba Workflow](http://imageshack.com/a/img923/3535/K2OcAV.png)

> Nota: Se for criado um status novo, diferente de todos que existam em outros ítens, será necessário alterar o _process-config_ para adicionar esse status.

### Por Linha de Comando

A primeira coisa a ter em mente é que os comandos não funcionam em qualquer terminal. Precisa usar o terminal instalado junto com a instalação do Visual Studio. Para encontrá-lo, digite `prompt` na busca do Windows e escolha a opção _Developer Command Prompt for {version}_. 

Esse processo é comumente utilizados quando queremos copiar uma customização de um projeto para outro. Nesse caso, exportamos os arquivos _process-config_  e _categories_ , e todos os _work-items_ de um projeto e importamos em outro. Outro caso em que essa abordagem é utilizada, é quando queremos criar um novo _work-item_ baseado em um pré-existente. Para isso, exportamos o item existente, alteramos o nome e importamos novamente, criando uma cópia com outro nome. 

Os principais comandos utilizados são:

* Exportar o **process-config** atual do projeto:

```
witadmin exportprocessconfig /collection: "URLDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Importar um arquivo de **process-config** e substituir o do projeto:

```
witadmin importprocessconfig /collection:"URLDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Exportar o **categories** atual do projeto:

```
witadmin exportcategories /collection: "URLDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Importar um arquivo de **categories** e substituir o do projeto:

```
witadmin importcategories /collection:"URLDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Exportar algum **work-item** existente no projeto:

```
witadmin exportwitd /collection: "URLDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo" /n:"NomeDoWorkItem"
```

* Importar um novo **work-item** ou substituir um existente no projeto:

```
witadmin importwitd /collection:"URLDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo" /n:"NomeDoWorkItem"
```
 
* Listar todos os **work-items** existentes no projeto:

```
witadmin listwitd /collection:"URLDaCollection" /p:"NomeDoProjeto"
```
 
* Remover do projeto um **work-item** existente:

```
witadmin importwitd /collection:"URLDaCollection" /p:"NomeDoProjeto" /n:"NomeDoWorkItem"
```

* Renomear um **work-item** existente no projeto:

```
witadmin renamewitd /collection:"URLDaCollection" /p:"NomeDoProjeto" /n:"NomeDoWorkItem" /new:NovoNome
```

> Nota: para documentação completa sobre esses e outros comandos, acesso a [documentação oficial](https://docs.microsoft.com/en-us/vsts/work/customize/reference/witadmin/witadmin-import-export-manage-wits).



### Arquivos de Configuração

* `process-config.xml` - é o principal arquivo do projeto. Nele estão contidas as informações básicas do projeto, fluxos de trabalho, os possíveis estados de cada item, as definições de campos, enfim. Editar esse arquivo pode causar inúmeros problemas e é comum deixar o projeto todo inacessível se houver algum erro no arquivo. Esse arquivo será editado quando for adicionado ou removido algum estado do workflow de algum item ou quando for necessário a cor de algum item no board.

* `categories.xml` -  esse arquivo serve para definir as categorias de _work-items_. Na prática, só vamos editar esse arquivo quando um novo _work-item_ seja adicionado ao projeto, pois o novo item só irá aparecer no board se estiver referenciado na categoria **TaskCategory**.

* `work-item.xml` - esse arquivo define um _work-item_, com seu workflow, campos e restrições. Dificilmente é alterado manualmente, a menos que se deseja criar um novo item com base em um pré-existente. Nesse caso, é muito útil copiar o arquivo existente e apenas alterar o nome do mesmo, importando-o para o TFS por linha de comando.
