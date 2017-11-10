![alt text](http://imageshack.com/a/img922/5924/DMrAzb.png)

## Repositório de Templates TFS

Esse repositório tem o objetivo de reunir os diferentes templates, configurações e customizações dos diferentes projetos TFS, bem como rastrear as alterações feitas ao longo do tempo em cada projeto / template.

## Como Utilizar esse Repositório

### Estratégia de Branching

A branch `master` deve conter um projeto de exemplo e o `README.md` atualizado.

Para cada projeto TFS versionado aqui, deverá ser criado uma nova branch com o nome do projeto, seguindo o padrão `nome-da-collection_nome-do-projeto`.

### Fotografia do Projeto

Ao inciar o versionamento de um projeto TFS aqui, deve-se exportar todos os arquivos do projeto e salvar aqui na estrutura proposta em `Estrutura de Arquivos` commitando todos os arquivos de um vez, representando o `initial commit`. 

Quando alguma alteração for feita no projeto TFS, exporte os arquivos impactados pela alteração e substitua os que estão aqui no repositório, commitando com uma mensagem clara que indique o que está sendo alterado. Exemplo `adicionando campo Reviewer em Bugs e Tasks` ou `Adicionando novo work-item Impedimento`.

Como não é garantido que ninguém mais faz alterações no projeto TFS desde seu último commit; antes de fazer qualquer alteração, exporte os arquivos que serão impactados pela alteração e substitua pelos que estão na pasta `src`. Se indicar alguma aleração, significa que alguma alteração não foi devidamente registrada aqui no repositório, logo, antes de fazer a alteração desejada, faça um commit com os arquivos alterados indicando na mensagem de commit que houve `alterações não registradas`.

## Glossário

* `template` - Modelo de projeto utilizado na criação de um novo projeto TFS. _Templates_ são vinculados a uma _collection_ e passam ser apresentados como uma opção no momento de criar um novo projeto dentro dessa _collection_. Uma vez criado um projeto com um template, ele **nunca** mais poderá ser alterado, permitindo apenas customizações.

* `collection` - É uma coleção de projetos TFS. Na CWI, cada cliente possui uma _collection_ (LojasRenner) com vários projetos dentro (Portal Realize). Para adicionar ou remover templates e projetos é necessário ter a permissão de `collection administrator`.

* `project` - Cada projeto TFS. Dentro de um projeto podem ser definidas _areas_ e _teams_, de forma que seja possível organizar frentes de trabalho dentro de cada projeto. Para gerenciar _teams_, _areas_ e _iterations_, conceder permissão de acesso, customizar, adicionar e remover work-items, é necessário ter a permissão `project administrator`.

* `team` - É um sub-projeto. Todo projeto nasce com um _team_ \<Nome do Projeto>Team e esse será usado quando a raiz do projeto for acessada. Mas é possível definir outros sub-projetos. É muito fácil confundir _team_ com _area_. **O time é um sub-projeto e nada mais**: ele pode possuir uma configuração específica de usuários, grupos e permissões de acesso e é o time que irá aparecer na interface quando estiver procurando por projetos. Quando se está usando times, a url, então, fica `url-tfs/collection/project/team`.

* `area` - Uma _area_ é um conjunto de trabalho a ser feito. **Enquanto times estão ligados a projeto e usuários, as áreas estão unicamente ligadas a trabalho e aos _work-items_**. Os times podem ser associados a mais de uma área, o que indicaria que um determinado time poderia atuar sobre mais de um grupo de trabalho. Então podemos ter uma area X com suas _epics_, _features, __user stories_, _tasks_, enfim, e uma area Y com outros _work-items_ desses tipos. Associando um time à área A, permitiria que esse time enxergasse todos _work-items_ dessa área em seu backlog, mas não os definidos na area Y (que não foi associada ao time). Geralmente se usa uma única _area_ principal associada a um time, mas é possível criar _areas_ filhas para agrupar seções de trabalho (não lembro de alguém já ter usado dessa forma). 

* `iteration` - A _iteration_ é a **sprint**. A _iteration_ define um intervalo de trabalho onde _work-items_ serão associados para que sejam concluídos. Assim como as _areas_, _iterations_ podem ser associadas a _teams_ e diferentes _teams_ podem ter uma mesma _iteration_. Possui apenas um nome (obrigatório), data de início e fim (opcionais). Geralmente se usa uma _iteration_ principal associada a um time e outras _iterations_ filhas com o nome desejado das sprints.

> Nota I: perceba que não existe uma ligação direta entre _team_, _area_ e _iteration_. Assim sendo, é possível que existam _areas_ e _iterations_ não associadas a _team_ algum - essas ficarão no limbo. A associação é feita sempre a partir do time.

> Nota II: é altamente recomendável nomear a raiz da _area_ e a raiz da _iteration_ com nomes adequados ao _team_ (geralmente a relação é um para um). Isso evita casos como no passado em que o time era "Análise da Proposta Team", a área era "Análise da Proposta" e a iteration raiz era "Team Análise da Proposta". Padrão, por favor.

* `work-item` - Um _work-item_ é um ítem de trabalho ( ¯\\\_(ツ)\_/¯ ). Cada elemento passível de ser entregue é um _work-item_: _tasks_, _user stories_, _features_, enfim. Os _work-items_ são visualizados no _backlog_ e no _board_ (desde que estejam em uma _iteration_) do projeto.

## Estrutura de Arquivos

A estrutura é bem simples e consiste em apenas uma pasta `src` contendo todos os arquivos `xml` do projeto. Isso inclui o `process-config.xml` e `categories.xml` bem como uma subpasta contento todos os _work-items_. 

* `process-config.xml` - é o principal arquivo do projeto. Nele estão contidas as informações básicas do projeto, fluxos de trabalho, os possíveis estados de cada item, as definições de campos, enfim. Editar esse arquivo pode causar inúmeros problemas e é comum deixar o projeto todo inacessível se houver algum erro no arquivo. Esse arquivo será editado quando for adicionado ou removido algum estado do workflow de algum item ou quando for necessário a cor de algum item no board.

* `categories.xml` -  esse arquivo serve para definir as categorias de _work-items_. Na prática, só vamos editar esse arquivo quando um novo _work-item_ seja adicionado ao projeto, pois o novo item só irá aparecer no board se estiver referenciado na categoria **TaskCategory**.

* `work-itemm.xml` - esse arquivo define um _work-item_, com seu workflow, campos e restrições. Dificilmente é alterado manualmente, a menos que se deseja criar um novo item com base em um pré-existente. Nesse caso, é muito útil copiar o arquivo existente e apenas alterar o nome do mesmo, importando-o para o TFS por linha de comando.

## Custommização de Projetos

### Pré-requisitos para Customização

Para customizar um projeto do TFS é necessário:

* Windows - né? :)
* Visual Studio - versão **maior ou igual** à versão do TFS
* Productivity Power Tools - versão **igual** a do Visual Studio

### Customização via Linha de Comando

A primeira coisa a ter em mente é que os comandos não funcionam em qualquer terminal. Precisa usar o terminal instalado junto com a instalação do Visual Studio. Para encontrá-lo, digite `prompt` na busca do Windows e escolha a opção `Developer Command Prompt for {version}`. 

Esse processo é comumente utilizados quando queremos copiar uma customização de um projeto para outro. Nesse caso, exportamos os arquivos _process-config_  e _categories_ , e todos os _work-items_ de um projeto e importamos em outro. Outro caso em que essa abordagem é utilizada, é quando queremos criar um novo _work-item_ baseado em um pré-existente. Para isso, exportamos o item existente, alteramos o nome e importamos novamente, criando uma cópia com outro nome. 

Os principais comandos utilizados são:

* Exportar o `process-config` atual do projeto:

```
witadmin exportprocessconfig /collection: "https://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Importar um arquivo de `process-config` e substituir o do projeto:

```
witadmin importprocessconfig /collection:"http://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Exportar o `categories` atual do projeto:

```
witadmin exportcategories /collection: "https://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Importar um arquivo de `categories` e substituir o do projeto:

```
witadmin importcategories /collection:"http://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo"
```

* Exportar algum `work-item` existente no projeto:

```
witadmin exportwitd /collection: "https://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo" /n:"NomeDoWorkItem"
```

* Importar um novo `work-item` ou substituir um existente no projeto:

```
witadmin importwitd /collection:"http://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /f:"CaminhoDoArquivo" /n:"NomeDoWorkItem"
```
 
* Listar todos os `work-items` existentes no projeto:

```
witadmin listwitd /collection:"http://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto"
```
 
* Remover do projeto um `work-item` existente:

```
witadmin importwitd /collection:"http://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /n:"NomeDoWorkItem"
```

* Renomear um `work-item` existente no projeto:

```
witadmin renamewitd /collection:"http://tfs.cwi.com.br/tfs/NomeDaCollection" /p:"NomeDoProjeto" /n:"NomeDoWorkItem" /new:NovoNome
```

### Customização via Visual Studio

Vamos recorrer ao Visual Studio quando for necessário alterar campos de um item, alterar as regras de preenchimento dos campos ou ainda o workflow do mesmo. Tudo isso pode ser feito apenas editando e importando arquivos `xml`, mas é muito mais fácil pelo Visual Studio nesses casos.

É possível alterar tanto ítens direto do projeto, quanto arquivos xml em disco. Também é possível exportar e importar _work-items_ pela interface do Visual Studio, mas não creio que seja possível exportar o _process-config_ e o _categories_.

Para alterar um _work-item_ existente no servidor, clique em `Tools > Process Editor > Open WITD from Server`. Se for a primeira vez, será necessário informar um usuário e senha válidos - lembrando que é necessário ter permissão de `project administrator` para ter acesso às custmizações. Após logado, selecione o projeto desejado e, então, o item desejado.

1. **Campos** - Primeira aba. Permite adicionar novas propriedades aos ítens, além de definir regras de preenchimento para os campos, para que seja possíve estipular quais informações são default, obrigatórias, opcionais, disponíveis em uma lista, enfim. Também é possível criar campos novos customizados para atender particulidades do projeto.

> Nota: Uma vez criado um novo campo, é **impossível** alterar seu _reference name_ ou _data type_. Ou seja, uma vez criado um campo como `String`, ele será para sempre `String`.

2. **Workflow** - Segunda aba. Permite definir os status e transições entre status de um item. É possível, também, definir regras de preenchimento de campos específicos para cada status do item, como definir que para o status `closed` é necessário ter apropriado horas no campo `completed work`. Nas transições é possíve definir que campos terão valores alterados automaticamente após o item passar para determinado status.

> Nota: Se for criado um status novo, diferente de todos que existam em outros ítens, será necessário alterar o _process-config_ para adicionar esse status.

3. **Form** - Terceira aba. Permite customizar a interface gráfica de criação e edição do _work-item_. É possível adicionar, na interface, campos customizados criados na primeira aba, alterar os labels dos campos e estrutura de layout da modal de edição do ítem. 

