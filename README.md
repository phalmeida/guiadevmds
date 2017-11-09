
## Guia de desenvolvimento php (API RESTful)

## Sumário
  1. [Introdução](#introdução)
  2. [Requisições](#requisições)
  3. [Respostas](#respostas)
     * [Use namespace em itens sozinhos](#use-namespace-em-itens-sozinhos)
     * [Use namespace para vários itens](#use-namespace-para-vários-itens)
  4. [Fluxo de versionamento](#fluxo-de-versionamento)
     * [Branches principais](#branches-principais)
     * [Branches de suporte](#branches-de-suporte)
        1. [Branches de melhorias](#branches-de-melhorias)
        2. [Criando um  branch de melhoria](#criando-um-branch-de-melhoria)
        3. [Finalizando um branch de melhoria](#finalizando-um-branch-de-melhoria)
        4. [Branches de lançamento](#branches-de-lançamento)
        5. [Criando um branch de lançamento](#criando-um-branch-de-lançamento)
        6. [Finalizando um branch de lançamento](#finalizando-um-branch-de-lançamento)
        7. [Branches de correções](#branches-de-correções)        
  5. [Variáveis](#variáveis)
     * [Use variáveis pronunciaveis e com significado claro](#use-variáveis-pronunciaveis-e-com-significado-claro)
     * [Use o mesmo vocabulário para o mesmo tipo de variável](#use-o-mesmo-vocabulário-para-o-mesmo-tipo-de-variável)

## Introdução

Padrões de codificação são definições de como estruturar o código em qualquer dado projeto. 
Os padrões se aplicam a tudo, de conversões de nomenclatura a espaços ou códigos de etatus, erros 
e mensagens, requisições e respostas dos dados. Usamos padrões como uma maneira de manter o 
código uniforme em todo o projeto, independentemente do número de desenvolvedores que possam trabalhar nele.

## Requisições

...

**[⬆ voltar para o topo](#sumário)**

## Respostas

Colocando a coleção no namespace *“data”*, poderemos facilmente acrescentar outro contexto
relacionado à resposta, que não é parte da lista de recursos. Contagens, links e etc poderão
fazer parte da resposta. Assim também é possível incorporar outras relações aninhadas em 
seu próprio elemento *“data”* e até mesmo incluir metadados para essas relações incorporadas. 

### Use namespace em itens sozinhos

```json
{
    "data": {
        "nome": "Philipe Almeida",
        "id": "511501255"
    }
}
```
**[⬆ voltar para o topo](#sumário)**

### Use namespace para vários itens

```json
{
    "data": [
        {
            "nome": "Júlia Costa",
            "id": "100002"
        },
        {
            "nome": "Aline Foley",
            "id": "100003"
        }
    ]
}
```

**[⬆ voltar para o topo](#sumário)**

## Fluxo de versionamento

Trabalhando em equipe ou sozinho, usar um padrão facilita primeiramente porque você não precisa pensar muito, uma vez 
que o padrão já foi escolhido e documentado. Segundo, usar um padrão reconhecido e principalmente documentado facilita 
pra cada pessoa que entrar, visualizar ou decidir contribuir para o seu projeto vai reconhecer aquele padrão e assim vai 
diminuir a curva de aprendizado e ainda aumentar a produtividade.

### Branches principais

O repositório central possui dois branches principais com vida infinita.

- master
- develop

O branch **master** é o branch principal, a HEAD do projeto, nele há somente versões que estão em produção.

O branch **develop** possui todo código já entregue e as últimas de desenvolvimento para a próxima versão. Quando o código 
do branch **develop** é considerado estável e pronto para ser implantado, todas as alterações devem ser mescladas de volta para 
o branch **master** e criada uma *tag*. Como isso é feito com mais detalhes a seguir.

**[⬆ voltar para o topo](#sumário)**

### Branches de suporte

Junto aos branches  master e develop utilizamos outros branches de suporte, para correção de erros, criação  de melhorias 
e preparação para implantação. Diferente dos branches principais, esses tem uma vida limitada, uma vez que eles 
são removidos eventualmente.

Os diferentes tipos de branches que  usaremos são:

- Branches de melhorias (feature)
- Branches de lançamento (release)
- Branches de correções (hotfix)

Cada tipo de branch tem um propósito específico e segue regras de quais branches devem ser originados e mesclados.

#### Branches de melhorias

Deve ser criado a partir de:
- develop

Deve ser mesclado de volta para:
- develop

Convensão de nome:
- Qualquer nome exceto master, develop, release/*, ou hotfix/*

Branches de melhorias são usados para desenvolver novas funcionalidades para o próximo lançamento. Em excência, um branch 
de melhoria existe apenas enquanto está em desenvolvimento, devendo ser mesclado ao branch develop, assumindo que entrará 
no próximo lançamento, ou descartado caso não seja útil ou seja um experimento.

#### Criando um  branch de melhoria

Ao iniciar o desenvolvimento de uma funcionalidade, crie um branch a partir do branch develop

```sh
$ git checkout -b feature/xpto develop
```

#### Finalizando um branch de melhoria

Uma vez concluído o desenvolvimento no branch, ele deve ser incorporado de volta no branch develop através de um pull request.

Envie seu branch para o servidor:

```sh
$ git push origin feature/xpto
```

Navegue até a interface do projeto no GitLab e clique em **New merge request**, selecione como base seu branch e o branch develop.

Uma vez aceito o merge request pelo administrador do projeto, a melhoria entrará na próxima versão.


#### Branches de lançamento

Deve ser criado a apartir de:
- develop

Deve ser mesclado de volta para:
- develop e master

Convensão de nome:
- release/MAJOR.MINOR.PATCH

Branches de lançamento são usados para preparação do lançamento da próxima versão de produção. Nele são permitidas 
pequenas correções e atualização de versão nos arquivos. Fazendo isso no branch de lançamento, 
o branch develop fica livre para receber novas melhorias para a próxima versão.

Na criação do branch de lançamento é decidido qual versão o projeto terá, até este momento o branch develop reflete as 
alterações da próxima versão, independende de qual for. Esta decisão é feita na criação do branch de lançamento e segue 
as convensões de versionamento do projeto.

#### Criando um branch de lançamento

Branches de lançamentto são criados a partir do branch **develop**. Digamos que o projeto está na versão 1.5.3 e há uma 
implantação em breve. O estado do branch **develop** está pronto para a próxima implantação e decidimos que vai se 
tornar 1.2.0 (ao invés de 1.1.5 ou 2.0.0). Então criamos um branch com o nome da versão que escolhemos.

```sh
$ git checkout -b release/1.2.0 develop
```

Depois de criar o branch, a primeira coisa a fazer é aumentar a versão no arquivo composer.json e comitar.

```sh
$ git commit -a -m "Atualização da aplicação para a versão 1.2.0"
```

Este branch deve existir por enquanto, até que esteja pronto para ser implantado definitivamente em produção. Pequenas 
correções são permitidas nele (ao invés do branch develop). Adicionar funcionalidades novas nele são proibidas, apenas 
correções pontuais desta versão de lançamento.

#### Finalizando um branch de lançamento

Quando o branch de lançamento estiver pronto para ser implantado em produção, algumas ações precisam ser tomadas. 
Primeiro o brach de lançamento é mesclado com o branch **master** (uma vez que cada commit no master é uma nova versão, 
por definição):

```sh
$ git checkout master
```

```sh
$ git merge --no-ff release/1.2.0
```

Em seguida este commit deve ser *tageado* para referência futura para esta versão:

```sh
$ git tag -a 1.2.0
```

Finalmente, as mudanças feitas no branch de lançamento precisam mescladas devolta no branch **develop**, para que as 
versões futuras também possuam as correções feitas neste branch:

```sh
$ git checkout develop
```

Neste momento podem ocorrer alguns conflitos, já que as correções podem mudar a versão dos arquivos, se acontecer, corrija e comite.

Feitos os merges podemos excluir o branch de lançamento, já que não precisamos mais dele:

```sh
$ git branch -d release/1.2.0
```

#### Branches de correções

Deve ser criado a apartir de:

- master

Deve ser mesclado de volta para:

- develop e master

Convensão de nome:

- hotfix/*

Branches de correções são muito parecidos com branches de lançamentos em sua concepção, pois tem o mesmo objetivo de prepara uma versão para produção, embora não planejada. Eles surgem da necessidade de agir imediatamente em uma versão de produção já implantada. Quando um *bug* crítico ocorre em produção um branch de correção precisa ser criado a partir da tag correspondente.
A ideia é que o time que está trabalhando na próxima versão no branch **develop** possa continuar enquanto alguém prepara uma correção.

**[⬆ voltar para o topo](#sumário)**


## Variáveis

### Use variáveis pronunciaveis e com significado claro

**Ruim:**

```php
$dmastr = $data->format('y-m-d');
```

**Bom:**

```php
$dataAtual = $data->format('y-m-d');
```
**[⬆ voltar para o topo](#sumário)**

### Use o mesmo vocabulário para o mesmo tipo de variável

**Ruim:**

```php
getUserInfo();
getUserData();
getUserRecord();
getUserProfile();
```

**Bom:**

```php
getUser();
```
**[⬆ voltar para o topo](#sumário)**

