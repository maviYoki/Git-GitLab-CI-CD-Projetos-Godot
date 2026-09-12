# Como utilizar o Git e GitLab no seu projeto Godot

Essa ideia de guia veio primeiramente para auxiliar e padronizar o meu projeto de **TCC** em grupo da faculdade, de **Engenharia de Software**, mas nosso **TCC** se retratará de um jogo educativo.


## Índice

- **Git**
  - [Primeira vez usando o Git?](#primeira-vez-usando-o-git)
  - [Criando seu primeiro repositório](#criando-seu-primeiro-repositório)
  - [Como vincular repositório local com o remoto](#como-vincular-repositório-local-com-o-remoto)
  - [Boas práticas de commit](#boas-práticas-de-commit)
  - [Um pouco mais sobre branch](#um-pouco-mais-sobre-branch)

- **GitLab**
  - [Primeira vez utilizando](#primeira-vez-utilizando)
  - [Como criar um projeto](#como-criar-um-projeto)
  - [Como adicionar Grupo](#como-adicionar-grupo)
  - [Como clonar repositório](#como-clonar-repositório)

## Git

> **Git** é um sistema de controle de versões distribuído, usado principalmente no desenvolvimento de software, mas pode ser usado para registrar o histórico de edições de qualquer tipo de arquivo.
>
> — [Git](https://git-scm.com/)

Caso já tenha familiaridade com o Git, pode pular as seções .... pois só servem para dar uma noção geral caso seja extremamente iniciante.


### [Primeira vez usando o **Git**?](#primeira-vez-utilizando)
#### Vamos Configurar
No terminal da sua máquina ou até mesmo no terminal da sua IDE:
```bash
git config --global user.name "Seu nome"
git config --global user.email "[email@exemplo.com](mailto:email@exemplo.com)"
```
Isso servirá para criar um identificador de quem está usando o **Git** em sua máquina.

### [Criando seu primeiro repositório](#criando-seu-primeiro-repositório)

No diretório do seu projeto, escreva os comandos abaixo:
```bash 
git init 
git add . 
git commit -m "feat: primeiro commit" 
```
`git init`: inicia seu repositório.
`git add .`: prepara o seu repositório para ser commitado.
`git commit -m...`: serve para registrar as um conjunto de alterações.

### [Como vincular repositório local com o remoto](#como-vincular-repositório-local-com-o-remoto)

Ainda no diretório do seu projeto, para vincular ele com o remoto ([para criar um projeto remoto no GitLab](#como-criar-um-projeto):
```bash
git remote add origin https://github.com/usuario/meu-projeto.git
```
<!-- arrumar esse trecho e vincular a secao de branchs -->
Isso ainda não manda as alterações apenas vincula os dois diretórios, mas sem sincronizar. Caso tenha algo no seu remoto, o indicado é você trazer os arquivos antes de enviar, para não causar conflito (só coloque main, caso esse seja o nome da sua branch, aqui mostra como descobrir isso):

 
```bash
git pull origin main
```
Depois você manda as alterações feitas do seu repositorio local para o remoto:
```bash
git pull origin main
```

### [Boas práticas de commit](#boas-práticas-de-commit)
### Boas práticas ao nomear branch
 **prefixos padronizados**
  -   `feature/` → para novas funcionalidades
  -   `bugfix/` → para correções de bugs
  -   `hotfix/` → para correções urgentes em produção
  -   `release/` → para preparar uma versão específica  
**Exemplo:**
  -   `feature/combate`
  -   `bugfix/dialogo`
  -   `hotfix/inimigos-crash`
  -   `release/v1.0`  
**Sugestão para o nosso projeto**
-   main (principal)
-   development (para testarmos as funcionalidades antes de fazer a merge (junção com o main))
-   feature/nome-da-funcionalidade para dividirmos funções e evitar mexer todos na mesma coisa e causar conflitos

### [Um pouco mais sobre branch](#um-pouco-mais-sobre-branch)
#### O que é uma branch?
A branch é uma linha de desenvolvimento. Um mesmo projeto pode ter várias branches para organizar e separar diferentes partes do desenvolvimento.  A `main` é geralmente a branch principal, e podem existir branches para cada funcionalidade ou de acordo com a dinâmica de desenvolvimento definida para o projeto.
#### Como criar uma branch local (caso n'ao
1.  Para criar uma nova branch:
```bash
git switch -c <nome da branch>
```
`git`:  comando para executar git 
`switch`:  vamos mudar de branch
 `-c`: criação
`<nome da branch>`:  aqui você define o nome da sua branch
```bash
git branch
```
Esse último comando confirma a criação da branch


