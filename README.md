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



#### Criando um primeiro Workflow

Criar um repositorio no Guthub fullcycle-ci-go e inici a um repositorio git

```bash
git init
```

Vamos adionar utilizando o conventional commits plugin do vscode.

