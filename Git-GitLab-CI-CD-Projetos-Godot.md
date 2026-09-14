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

Caso já tenha familiaridade com o Git, pode pular as seções iniciais, pois só servem para dar uma noção geral caso seja extremamente iniciante.

### Primeira vez usando o **Git**?

#### Vamos Configurar

No terminal da sua máquina ou até mesmo no terminal da sua IDE:

```bash
git config --global user.name "Seu nome"
git config --global user.email "email@exemplo.com"
```

Isso servirá para criar um identificador de quem está usando o **Git** em sua máquina. Esses dados aparecem em todo commit que você fizer, então o ideal é se use o mesmo nome/email que você usa na sua conta do **GitLab**, para que os commits fiquem vinculados corretamente ao seu perfil

### Criando seu primeiro repositório

No diretório do seu projeto, escreva os comandos abaixo:

```bash
git init
git add .
git commit -m "feat: primeiro commit"
```

`git init`: inicia seu repositório.

`git add .`: prepara o seu repositório para ser commitadas.

`git commit -m...`: serve para registrar um conjunto de alterações no histórico do seu repositório.

### Como vincular repositório local com o remoto

No GitLab, [crie o projeto remoto primeiro](#como-criar-um-projeto) (não é obrigatório ser nessa ordem, mas será a ordem que utilizaremos nesse guia). Depois de criado, o próprio GitLab te mostra a URL do repositório (HTTPS ou SSH), e é ela que você irá utilizar abaixo

Ainda no diretório do seu projeto, para vincular ele com o remoto:

```bash
git remote add origin https://github.com/usuario/meu-projeto.git
```

Esse comando **não envia nada** para o GitLab ainda. Ele apenas informa ao **Git** que existe um repositório remoto nesse endereço e associa esse endereço ao nome `origin`.

`origin` é um nome convencional usado pelo Git para identificar o repositório remoto principal. Ele poderia ter outro nome, mas usar `origin` é uma prática comum e facilita a execução de comandos.

Caso tenha algum arquivo no seu reposiório remoto, traga essas alterações antes de enviar, para não causar conflito. Descubra o nome da sua branch principal com `git branch` e depois rode, troque o `main` abaixo pelo nome que apareceu quando você utilizou o comando anterior.

```bash
git pull origin main
```

Só depois envie as alterações do seu repositorio local para o remoto:

```bash
git push origin main
```

### Boas práticas de commit

Recomendo seguir os padrões de **Conventional Commits**, que vai facilitar o entendimento do histórico do projeto e gerar changelogs automaticamente (quando você rodar `git log --oneline`, aparecerá a lista de mudanças feitas no código de maneira organizada e bem descritiva).

**Exemplos de utilização**
| Tipo | Exemplo |
| --- | --- |
| `feat:` | `feat: adiciona sistema de diálogo` |
| `fix:` | `fix: corrige colisão do jogador com paredes` |
| `docs:` | `docs: adiciona seção sobre criação de branches` |
| `style:` | `style: ajusta formatação dos arquivos` |
| `perf:` | `perf: otimiza carregamento das salas` |
| `test:` | `test: adiciona testes para sistema de inventário` |
| `build:` | `build: atualiza versão do Godot no projeto` |
| `ci:` | `ci: adiciona verificação automática dos arquivos` |
| `chore:` | `chore: atualiza arquivos de configuração` |
| `revert:` | `revert: remove sistema de diálogo` |

### Um pouco mais sobre branch

#### O que é uma branch?

A branch é uma linha de desenvolvimento.

Um mesmo projeto pode ter várias branches para organizar e separar diferentes partes do desenvolvimento. A `main`, geralmente é a branch principal, e podem existir branches para cada funcionalidade ou de acordo com a dinâmica de desenvolvimento definida para o projeto.

#### Como criar uma branch local (caso não tenha

1. Para criar uma nova branch:

```bash
git switch -c <nome da branch>
```

`git`: comando para executar git
`switch`: vamos mudar de branch
`-c`: criação
`<nome da branch>`: aqui você define o nome da sua branch

```bash
git branch
```

Esse último comando confirma a criação da branch.

### Boas práticas ao nomear branch

O git não exige uma convenção universal para nomenclatura, isso parte de como a equipe de desenvolvimento prefere trabalhar.

Mas podemos citar alguns exemplos de nomenclatura que podemos utilizar.

| Tipo       | Exemplo                             |
| ---------- | ----------------------------------- |
| `feature/` | para novas funcionalidades          |
| `bugfix/`  | para correções de bugs              |
| `hotfix/`  | para correções urgentes em produção |
| `release/` | para preparar uma versão específica |
| `docs`     | para arquivos de guia/documentação  |

**Exemplo:**

- `feature/combate`
- `bugfix/dialogo`
- `release/v1.0`

A `/` faz parte do nome da branch, ela não cria uma pasta nem uma hieraquia, `bugfix/dialogo` é um nome só, e essa branch terá suas alterações que serão referentes ao nome dessa branch.

Mas não é por que ela não possui uma nomenclatura universal que não possui boas práticas que uma convenção deve priorizar:

- clareza
- consistência
- identificação
- nome curto e prático

   **Sugestão para o nosso projeto**

| Tipo                             | Exemplo                                                                        |
| -------------------------------- | ------------------------------------------------------------------------------ |
| `main`                           | principal                                                                      |
| `development`                    | para testarmos as funcionalidades antes de fazer merge (junção com o main)     |
| `feature/nome-da-funcionalidade` | para dividirmos funções e evitar mexer todos na mesma coisa e causar conflitos |

#### Branch local e remota

Lembre-se: nao é porque uma branch existe localmente, que ela existe remotamente

`feature/login` e `origin/feture/login` não são a mesma coisa, a primeira existe localmente e a segunda remotamente, o termo para se referênciar a branch rem
