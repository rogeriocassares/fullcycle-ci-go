# Iniciando com CI

É o proesso de integrar modificaçoes do codebase de forma continua e automatizada, evitando assun erros humanos de verificação, garantindo mais agilidade e segurança no  processo de desenvolvimento de um sw;



Processos automatixados para validar mdificações. 

Alemde fazer a PR basica, temos que rodar os testes, verificar se nao tem nenhum bug de seguran~ca, o codelinter e executar diversas verificações. 

Se tivermos que fazer isso para cada PR ia ser algo insano pq a chance de o revisor erraré muito grande. 

Entao o CI vai fazer essas verificações de forma interiramente automatizada.

A todo o momento ele vai quere integrar o codigo novo com o codigo base e impedir que atrapalhe a nossa integração.

Osprincipais processos que normalmente sao importantes com CI:

Testar, verificar, testar integrações. Pegar o codigo que subiu, fazer os testes, e aprovar ou nao,

No caso de CI, eh muito comum nos realizarmos os testes da aplicação, lint, espaçamrnto, etc. Verificação de qualidade de coidigo.Muitos codigos de blocos repetdos que nao gostariamos de acumular para nao gerar uma grandequantidade de debitos tecncos na nossa alicação. Alem disso,  fazer algumas verificações de segurança. Imagina alguem ter colocado ali um token direto sem ter colocado em uma variavel de ambiente ou tem alguma coisa muito erradaque poderiamos fazer essa verificação e poderia passar aos olhos humanos. Entao podemos realizar processos dessa forma para nos ajudar.



Além disso, muitas vezes, queremos pegar esse processo de CI, gerar obuild e gerar os artefatos que vamos utilizar para colcoar issoemproduçao.

Entao vamos imaginar que vamosfazer o CI, testes,linter, segurançae etc e entao baseado nesse resultado vamos gerar um zip, subirem um servidor e quando aluem quiser colcar no ar, a gente pega esse zip e faz o processo de deploy.

As vezes na eh zip, mas o CI vai gerar uma imagem Docker eentaosubmims essaimagemno nosso COntainer Register. Entao quando subimos o nosso processo de CI temos a possibilidade de gerar artefatos para quenos ajude futuramente em um processo de deploy.

Outra coisa beminteressante e muito utilizada: a parte de bump version. Se estuvermos trabalhando com SemVer, e tb ocoventional commits, podemospegar um prgrama especifico para analisar tudo o que foi gerado nos commits dees pull request e baseado nisso ele vai gerar automaticamente uma tag, um release na hora em que subirmos. Fazendo o controle de vesao da aplicalçao de forma automatica simplesmente analisando os processos dos nossos commits e aplicando CCI

CI tem muita liberdade.

A criatividade e a necessidade 

Relizar testes de integração, end0to0end, coleçoes que testou em uma ai no Postman e etc.

#### Status Check

É a garantia de que uma PR nao poderá ser mergeada ao repositorio sem antes ter passadi oelo processo de CI ou mesmo no processo de Code Review.

Podemos falar ao status check se o processo de CI passou. Se ele nao passou, bloquemoas o processo de mergear a pR;

Assim, mesmo que nao estejamos trabalhando com code review, impedimos que outras pessoas posssam fazeer um merge da sua aplicação.

A mais popular é o Jenkins. Gratuito e muito bom! Quando tivermos mutos PRs e commits, vamos ter que clusterizar esse processo para que ele de conta de rodar os jobs em paralelo.

Aqui vamos ver a Github Actions. O diferencial é que ele se integra no github inteiro baseado em eventos. Ele nao eh apenas CI, mas trabalha num workflow de acordo com o que precisamos. 

Se quisermos rodar uma automação quando alguem criar uma nova issue, porexemplo, ou modificar issiu, ou fechar, o push, ou PR. Sao açoes que podemos capturar e ações que podemos executar e podemos pegar ações feitas de outros desenvolvedores e empretamos para podermos utilizar.



Independente da ferramenta que estamos utilizando, os conceitossao os mesmos mas apenas os arquivos ou padroes sao um pouco diferentes.



O GA é um automatizador de workflow de desenvolvimento de sw.

Ele utiliza os principais eventos gerados pelo Github pata==ra que ossamos executar tarefas dos mais variados tipos, incluidno processos de Ci.

DINAMICA:

WORKFLOW -> 

Sao conjuntos de processis definidos por nós. Ex fazer build + rodar os testes da aplicação

É possivel ter mas do que um workflow por repositorio

Definidos em arquivos yaml em .github/workflows.

Possui um ou mais jobs.

É iniciado em eventos do github o atraves de agendamentos.



EVENTO (on push) -> FILTROS (branches master) ->AMBIENTE (ubuntu)-> AÇOES (steps= uses: actions/run-compose; run: npm run prod)

No steps, temos uas opçoes. O uses é quando pegamos uma action do github (um codigo desenvolvido poralguem que podemoscolocarali dentro e elevai executar o codigo no padrao do gh actions. Iclusive temos um marketplace do gha que podemos olhar todas as actions que ja existem. Uma aplicação go, por exemplo, já possui um setup-go que organiza o ambiente ubuntu que para rodaro teste certinho para o que queremos fazer e issso que deica o gha muito poderoso, q afinal podemos compartilhar as action para facilitarmos as coisas.).Joa o comando run executa um comando na maquina ubuntu.



ACTON -> A acion é a ação que de fato será executada em um dos steps de um Job em um workflow . Ela pode ser criada do zero ou ser reutilizada de actions pre-existentes.

A acrion pode ser desenvolvida em:

Javascript

Docker Image

Em um repositorio publico, podemos usar essas actionsavontade.Porem pararepositoriosprivados existem planos 





#### cRIANDO sOFTWARE eXEMPLO

Programa - Test - Repo - Actions

O primeiro passo é criar um progranma de fazer uma conta de somar em go, por exemplo. E depois fazer um test e testar.

```bash
❯ go mod init rogeriocassares/github-actions
go: creating new go.mod: module rogeriocassares/github-actions
```



main.go

```go
package main

import "fmt"

func main() {
	fmt.Println(Soma(10, 10))
}

func Soma(a int, b int) int {
	return a + b
}

```



math_test.go

```bash
package main

import "testing"

func TestSoma(t *testing.T) {

	total := Soma(15, 15)

	if total != 30 {
		t.Errorf("Resultado da soma é invalido: Resultado %d, Esperado: %d", total, 30)
	}
}

```



#### Criando primeiro Workflow

Criar um repositorio no Guthub fullcycle-ci-go e inici a um repositorio git

```bash
git init
```

Vamos adionar ao stage utilizando o conventional commits plugin do vscode.

As vezes quando estamos trabalhando com mommits assinados pode demorar um pouquinho. 

Podemos resolver com o gpg-agent

```bash
gpgconf --launc gpg-agent
```



Agora vamos dar um push e configurar pela primeira vez:

```bash
rogerio in 3.CI on  master [!] 
❯ git branch -M main
git remote add origin https://github.com/rogeriocassares/fullcycle-ci-go.git
rogerio in 3.CI on  main [!] 
❯ git push -u origin main
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 4.18 KiB | 2.09 MiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/rogeriocassares/fullcycle-ci-go.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.

```



Agora vamos criar o 1o workflow utilizando o GHA!



Criar arquivo .github/workflows/ci.yaml

```ỳaml
name: ci-golang-workflow
on: [push]
jobs: 
  check-application:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 
      # Pega os dados que acabamos de subir e vai fazer download na maquina ubuntu.
      - uses: actions/setup-go@v2
      # Faz o setup do go
      # actions/setup é um repositório do github!
        with: 
          go-version: 1.15
      - run: go test
      - run: go run math.go
```



Agora vamos no controle de versao do github, adiconar o ci.yaml , conventional commits plugin ... etc. No proprio source control, podemos dar um push.

Se formos no github -> Action e podemos ver o que ele executou, rodou os testes o programa e depois completou o job!

Nesse ponto, as nossas actions funcionaram e está tudo ok!



#### Fazendo Github actions nao passar

Imaginemos u edesejamos fazer modificação na aplicação. Mudar de Soma para soma e fazer com que o teste nao passe pq o test está utilizando Soma.

Entao vamos dar um commit nesse arquivo.

Source Control - CObventional COmmit nova feature ... 



Vamos em Actions para verificar e olha só. Run go test nao passou!

O PROCESSO DE CI nao passsou pq nao achou a função soma!

Inclusive quando vamos ao codebase, vemos que o último commit nao passou!

Agora vamos fazer este teste passar atribuindo soma minuscula ao nome da função.

Source control - convential commit - push

Agora vimos que passou o go test e conseguiu rodar a nossa função!

SE O CODEIGO NAO PASSAR NO TESTES O GITHUB ACTION NAO PERMITE QUE SEJA DADO O PR PARA O REPOITORIO



#### Ativando status check

Vamos voltar nos padroes de criar um branch develop e PRs

No terminal:

```bash
rogerio in 3.CI on  main [!] 
❯ git checkout -b develop
Switched to a new branch 'develop'

```



E vamos subir um branch igualzinho ao main no github

```bash

rogerio in 3.CI on  develop [!] 
❯ git push origin develop
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'develop' on GitHub by visiting:
remote:      https://github.com/rogeriocassares/fullcycle-ci-go/pull/new/develop
remote: 
To https://github.com/rogeriocassares/fullcycle-ci-go.git
 * [new branch]      develop -> develop
 
```



No github:

Settings, branches

Branch princiap para o develop

E entao vamos criar uma proteção para o branch develop

Add rules e exigir que tenhamos um require status check antes de dar um merge.

Vamos escolher o Job chamado check-application, que foi o nome do Job que criamos no arquivo yaml. Esse teste entao tem que paassar para pdermos dar um merge no nosso github!

E vamios obrigar que os branches estejam na ultima versao para que façam o status check. 

NInguem tb poode commitar diretamente, incluindo administradores ... 

Fazer essa mesma rule para o main

Agora, a regra é que tudo o que estiver em produção é o que etsá no branch main, nesse processo, só vai ser mergeado no develop. O develop é quem faz o merge para o main.

O processo de CI do develo e do main sao diferentes.

No develop, o CI vai apenas verificar se está tudo passando. 

No main, além de passar, ele vai fazer o deploy. E ninsso vai entrar o CD - COntinuos Deployment/Delivery.

Nesse caso, queremos que aconteça somente no branch develop pq as regras do branc em produção sao diferentes,



Entao vamos alterar lgumas coisas no ci.yaml. AO invés on push, on pull-request.

Outra parte para entender, é qual branch queremos filtrar.

Github docs tem diversos templates para os jobs

```yaml
name: ci-golang-workflow
on: 
  pull_request:
    branches:
      - develop
      # Todas as vezes que subirmos para o branch develop, queremos que algo aconteça
jobs: 
  check-application:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 
      - uses: actions/setup-go@v2
        with: 
          go-version: 1.15
      - run: go test
      - run: go run math.go
```



Adicionar , comnvential commits ... push

Entao, todas as v ezes que colocarmos no nosso repositorio e dermos um PR, o status check vai acontecer! E ai vai opassar e podermos dar o merge.



Deu erro ao fazer o push diretamente para a branch develop pq devemos fazer uma PR!!!

Vamos fazer um PR criando uma nova branch!

```bash
rogerio in 3.CI on  develop [!] 
❯ git checkout -b feature/ci
Switched to a new branch 'feature/ci'
rogerio in 3.CI on  feature/ci [!] 
❯ git push origin feature/ci
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (8/8), 4.38 KiB | 4.38 MiB/s, done.
Total 8 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas:   0% (0/3remote: Resolving deltas:  33% (1/3remote: Resolving deltas:  66% (2/3remote: Resolving deltas: 100% (3/3remote: Resolving deltas: 100% (3/3), completed with 2 local objects.
remote: 
remote: Create a pull request for 'feature/ci' on GitHub by visiting:
remote:      https://github.com/rogeriocassares/fullcycle-ci-go/pull/new/feature/ci
remote: 
To https://github.com/rogeriocassares/fullcycle-ci-go.git
 * [new branch]      feature/ci -> feature/ci
```



Vamos no github e create PR com essa nova branch!

Agora quando criamos a PR ele vai começar a rodar os testes e nao consegumos nem mesmo fazer o merge sem que passe no test do workflow.

Funcionou!



Agora deletamos a branch do repositorio remoto, e no nosso terminal:

```bash
git checkout develop
git pull origin develop
git branch -d feature/ci
```



Pronto!



#### Trabalhando com Strategy Matrix

Vale muito apena o processo de realizar os testes em diversos ambientes ou mesmo com diversa versoes do mesmo ambiente/ mesma linguagem. Entao podemos criar uma estratégia e uma matriz de como queremos testar.

```yaml
name: ci-golang-workflow
on: 
  pull_request:
    branches:
      - develop
strategy:
  matrix:
    go: ['1.14','1.15']
jobs: 
  check-application:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 
      - uses: actions/setup-go@v2
        with: 
          go-version: ${{matrix.go}}
      - run: go test
      - run: go run math.go
```





E entao vamos criar uma











