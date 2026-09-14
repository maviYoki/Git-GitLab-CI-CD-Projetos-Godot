# Como utilizar o Git e GitLab no seu projeto Godot

Essa ideia de guia veio primeiramente para auxiliar e padronizar o meu projeto de **TCC** em grupo da faculdade, de **Engenharia de Software**, mas nosso **TCC** se retratará de um jogo educativo.

Este guia se baseará em conceitos de Git Flow, mas com adaptações para o nosso caso de uso, e nas documentações oficiais do Git, GitLab e Godot.

## Índice

- **Git**
   - [Primeira vez usando o Git?](#primeira-vez-usando-o-git)
   - [Criando seu primeiro repositório](#criando-seu-primeiro-repositório)
   - [Como vincular repositório local com o remoto](#como-vincular-repositório-local-com-o-remoto)
   - [Boas práticas de commit](#boas-práticas-de-commit)
   - [Um pouco mais sobre branch](#um-pouco-mais-sobre-branch)
   - [Utilizando o Git em grupo](#utilizando-o-git-em-grupo)

- **GitLab**
   - [Primeira vez utilizando o GitLab](#primeira-vez-utilizando-o-gitlab)
   - [Como criar um projeto](#como-criar-um-projeto)
   - [Como adicionar Grupo](#como-adicionar-grupo)
   - [Como clonar repositório](#como-clonar-repositório)

- **Desenvolvimento em Grupo**
   - [Resolvendo conflitos de merge](#resolvendo-conflitos-de-merge)

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
git remote add origin https://gitlab.com/usuario/meu-projeto.git
```

Esse comando **não envia nada** para o GitLab ainda. Ele apenas informa ao **Git** que existe um repositório remoto nesse endereço e associa esse endereço ao nome `origin`.

`origin` é um nome convencional usado pelo Git para identificar o repositório remoto principal. Ele poderia ter outro nome, mas usar `origin` é uma prática comum e facilita a execução de comandos.

Caso tenha algum arquivo no seu reposiório remoto, traga essas alterações antes de enviar, para não causar conflito. Descubra o nome da sua branch principal com `git branch` e depois rode, troque o `main` abaixo pelo nome que apareceu quando você utilizou o comando anterior.

```bash
git pull origin main
```

Só depois envie as alterações do seu repositório local para o remoto:

```bash
git push origin main
```

Resumindo a ordem lógica: `remote add` → `pull` (se já existir algo no remoto) → `push`.

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

### Um pouco mais sobre branches

#### O que é uma branch?

A branch é uma linha de desenvolvimento independente que pode ou não ser permanente (saiba mais em [Branches são temporárias](#branches-são-temporárias)).

Um mesmo projeto pode ter várias branches para organizar e separar diferentes partes do desenvolvimento. A `main`, geralmente é a branch principal, e podem existir branches para cada funcionalidade, de acordo com a dinâmica de desenvolvimento definida para o projeto.

#### Como criar uma branch local

1. Para criar e já mudar para uma nova branch:

```bash
git switch -c <nome da branch>
```

`git`: comando para executar git
`switch`: vamos mudar de branch
`-c`: criação
`<nome da branch>`: aqui você define o nome da sua branch

2. Para conferir em qual branch você está e listar as branches locais:

```bash
git branch
```

#### Branches são temporárias

Um ponto importante para já ter em mente desde a criação é que branches de feature (`feature/nome-da-funcionalidade`) **não são para durar para sempre**. A ideia é, você cria a branch, desenvolve aquela funcionalidade específica, e quando ela é aceita de volta na `development` (via [Merge Request](#merge-request)), a branch já cumpriu seu papel e pode ser apagada sem perda nenhuma. Isso porque tudo que estava nela já foi copiado para a `development` (baseado nos conceitos de GitFlow).

Isso muda um pouco como pensamos a nomeação: já vale criar a branch pensando em uma tarefa específica e "fechada" (ex.: `feature/menu-inicial`), em vez de uma branch genérica e duradoura (ex.: `feature/telas`, que nunca teria um fim claro). E claro que seguindo as boas práticas de commits, não iremos nos perder com a criação, desenvolvimento e fim dessas branches.

#### Boas práticas ao nomear branches

O Git não exige uma convenção universal para nomenclatura, isso parte de como a equipe de desenvolvimento prefere trabalhar. Mas podemos citar alguns exemplos de nomenclatura comuns.

| Tipo       | Exemplo                             |
| ---------- | ----------------------------------- |
| `feature/` | para novas funcionalidades          |
| `bugfix/`  | para correções de bugs              |
| `hotfix/`  | para correções urgentes em produção |
| `release/` | para preparar uma versão específica |
| `docs`     | para arquivos de guia/documentação  |

**Exemplos:**

- `feature/combate`
- `bugfix/dialogo`
- `release/v1.0`

A `/` faz parte do nome da branch, ela **não** cria uma pasta nem uma hieraquia. `bugfix/dialogo` é um nome só, e essa branch conterá suas alterações referentes a esse nome.

Mesmo sem uma convenção universal, uma boa nomenclatura deve priorizar:

- **clareza**
- **consistência**
- **identificação**
- **nome curto e prático**

---

**Sugestão para o nosso projeto**

| Branch                           | Finalidade                                                                  |
| -------------------------------- | --------------------------------------------------------------------------- |
| `main`                           | versão estável/principal                                                    |
| `development`                    | para testar as funcionalidades antes de fazer merge com a `main`            |
| `feature/nome-da-funcionalidade` | para dividir tarefas e evitar que todos mexam na mesma coisa ao mesmo tempo |

---

Lembre-se: não é porque uma branch existe localmente, que ela existe remotamente, e vice-versa.

`feature/login` (local) e `origin/feture/login` (remota) não são a mesma coisa: a primeira existe apenas na sua máquina, e a segunda remotamente (GitLab). O termo para se referir à branch remota é sempre `<nome-do-remoto>/<nome-da-branch>` — no nosso caso, `origin/feature/login`.

Tirando as branches `main` e `development`, o restante será criada de acordo com o fluxo de desenvolvimento da equipe.

Para publicar uma branch local no remoto pela primeira vez, usamos a flag `-u` (abreviação de `--set-upstream`), que associa a branch local à remota e depois disso, `git push` e `git pull` sozinhos já sabem para onde ir:

```bash
git push -u origin feature/login
```

O nome igual entre as branches **local** e **remota**, por si só, não cria essa associação. O **upstream** é que informa ao **Git** qual branch remota deve ser usada como referência.

Nas próximas vezes pode utilizar (se estiver na branch correta):

```bash
git push
```

### Utilizando o Git em grupo

Trabalhar sozinho e trabalhar em equipe com Git exigem hábitos diferentes. Sozinho, você raramente tem conflito. Já em grupo pode acontecer de um colega sobrescrever seu código ou quebrar a `main` por acidente.

#### Nunca trabalhe direto na `main`

A `main` (ou `development` no caso do nosso projeto de **TCC**) deve representar sempre a versão estável. Trabalhar direto nela significa que qualquer erro seu já afeta todo mundo que for trabalhar nela diretamente. Por isso: toda **tarefa nova** começa criando uma branch ([Como criar uma branch local](#como-criar-uma-branch-local)), e só volta para a `main` (ou `development`) através de um [Merge Request](#merge-request), depois de revisado.

#### Atualize sua branch com frequência

Enquanto você trabalha na sua branch, outro colega pode estar enviando alterações para as deles e algumas já podem ter sido mescladas nas versões estáveis. Caso você demore muito, as chances de acontecer um conflito aumentam.

Antes de começar a trabalhar no dia, e antes de abrir um Merge Request, é uma boa prática buscar as novidades:

```bash
git fetch origin
git pull origin <development>
```

`git fetch` só baixa as informações do remoto (sem alterar seus arquivos), serve para você ver o que mudou antes de decidir trazer. `git pull` já baixa **e** mescla na sua branch atual.

Pode trocar `development` pela branch estável do seu projeto

#### Confira o estado do repositório antes de commitar

Antes de um `git add`, rode:

```bash
git status
```

Isso mostra quais arquivos foram alterados, quais já estão preparados para commit e em qual branch você está

#### Fluxo resumido de trabalho em equipe

Caso esteja se baseando no nosso guia e nos conceitos de fluxo do GitFlow.

1. Atualize sua branch local (`git pull origin development`)(troque pelo nome da sua branch estável).
2. Crie uma branch de feature a partir dela (ou uma branch que case com o que você irá fazer).
3. Trabalhe, commitando em pequenos passos com mensagens no padrão [Conventional Commits](#boas-práticas-de-commit).
4. Publique a branch (`git push -u origin feature/nome`) (lembre-se que o -u é apenas quando é primeiro push para a nova branch).
5. Abra um [Merge Request](#merge-request) para `development`.
6. Peça revisão de um colega.
7. Depois de aprovado, faça o merge e delete a branch de feature.

## GitLab

> O GitLab é uma plataforma completa de DevSecOps que hospeda repositórios Git na nuvem (ou on-premise), e adiciona recursos como controle de acesso, issues, Merge Requests, CI/CD e wikis.
>
> — [Documentação oficial do GitLab](https://docs.gitlab.com/)

### Primeira vez utilizando o GitLab

Após criar sua conta, um passo interessante é configurar sua chave **SSH**, mas antes vamos falar para que serve.

#### Por que configurar SSH no GitLab?

Quando o Git precisa conversar com o GitLab, ele precisa se autenticar, ou seja, provar que você tem permissão para acessar aquele repositório.

O GitLab oferece diferentes formas de autenticação, como:

- **SSH**: usa um par de chaves SSH para autenticar o computador.
- **HTTPS + Personal Access Token (PAT)**: usa uma URL HTTPS e um token como credencial.

_Nota_: Como citado, isso é apenas para o desenvolvedor se identificar e autenticar para poder ter acesso ao projeto, então use o que for mais confortável.

#### SSH

Cria um par de chaves, uma **pública** (essa será utilizada na sua conta do **GitLab**) e uma **privada** (essa fica apenas na sua máquina e não deve ser compartilhada).

A principal vantagem é que, depois de configurar a chave, você não precisa fornecer seu usuário e senha do **GitLab** a cada `push` ou `pull`.

#### Configurando o SSH

##### 1. Verifique se você já possui as chaves:

```bash
ls -la ~/.ssh
```

Procure por arquivos como:

```bash
id_ed25519 ← chave privada
id_ed25519.pub ← chave pública
```

##### 2. Como gerar uma chave SSH

Caso não tenha uma chave, execute:

```bash
ssh-keygen -t ed25519 -C "seu-email@example.com"
```

O GitLab recomenda o tipo ED25519 para novas chaves.

O terminal perguntará onde deseja salvar a chave:

```bash
Enter file in which to save the key (/home/seuusuario/.ssh/id_ed25519):
```

Se quiser utilizar o local padrão, pressione Enter. Depois será solicitada uma passphrase. Ela funciona como uma senha para proteger sua chave privada.

Você pode digitar uma senha e pressionar Enter (ou deixar vazio e apenas pressionar Enter).

Depois digite a mesma senha novamente quando for solicitado (caso tenha colocado uma senha). Ao terminar, serão criados dois arquivos:

```bash
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

##### 3. Adicione a chave pública ao GitLab

Agora coloque somente a chave pública na sua conta do GitLab. Para visualizar a chave:

```bash
cat ~/.ssh/id_ed25519.pub
```

Será mostrada uma linha parecida com:

```bash
ssh-ed25519 AAAAC3... seu-email@example.com
```

Copie a linha inteira.

##### 4. No GitLab:

4.1. Clique na sua foto de perfil.

4.2. Entre em Edit profile.

4.3. No menu lateral, acesse Access → SSH keys.

4.4 Clique em Add new key.

4.5. Cole a chave no campo Key.

4.6. Dê um nome para identificar o computador, por exemplo: `Meu computador`.

4.7. Adicione a chave.

Ao fim desse processo se tudo deu certo, o GitLab irá associar sua conta à sua chave pública.

##### 5. Vamos testar a conexão

No seu terminal, rode:

```bash
ssh -T git@gitlab.com
```

`ssh` → programa SSH do seu computador

`-T` → pede um teste de autenticação, sem abrir um terminal no servidor

`git@gitlab.com` → usuário "git" no servidor GitLab

Após rodar, ele perguntará se você deseja confiar nesse servidor, aí é só digitar: `y` (yes).

Depois de confirmar a conexão, deverá retornar uma mensagem de boas-vindas do GitLab:

```bash
Welcome to GitLab, @nome-usuario!
```

Com uma chave SSH configurada, o GitLab consegue reconhecer seu computador por meio da sua chave.

##### Caso não saiba sua passphrase SSH

Crie outra chave ([Como gerar uma chave SSH](#2-como-gerar-uma-chave-ssh)), mas com um passo diferente, quando perguntar aonde você quer salvar, você deve dar um nome diferente, para não sobrescrever a chave antiga, por exemplo:

```bash
/home/seuusuario/.ssh/id_ed25519_gitlab
```

Eai você pode salvar seu arquivo assim:

```bash
~/.ssh/id_ed25519_gitlab.pub
```

Após isso, continue com os passos normalmente, mas se atente que agora a chave que será utilizada não será mais a padrão `ssh/id_ed25519.pub` e sim `ssh/id_ed25519_gitlab.pub`.

#### HTTPS

Outra opção para o **Git** se conectar, é usar uma URL **HTTPS** + **PAT** :

```bash
https://gitlab.com/usuario/projeto.git
```

##### Como utilizar o metódo HTTPS

Você precisará do **HTTPS** + **PAT** (Personal Access Token), que será utilizado como meio de autenticação.

Esse **PAT** é um código gerado pelo próprio GitLab

##### Como criar o PAT

**avatar → Edit profile → Access** → **Personal access tokens**

Depois escolha **Generate token** → **Legacy token**

Você verá campos como:

**Token name** → dê um nome para identificar o token.
**Expiration date** → data em que ele deixará de funcionar.
**Scopes** → quais permissões o token terá.

Para usar Git por HTTPS, o token precisa ter permissão para as operações que você pretende realizar.

Se você quer fazer:

```bash
git clone
git pull
git push
```

precisa de permissão de leitura e escrita.

Depois disso é só clicar em **Generate token**

**_Nota: Guarde esse token em um lugar seguro, pois não será possível acessa-lo novamente_**

##### Como testar o PAT

Vá no terminal do seu projeto e tente fazer um `git pull`.

Digite seu nome de usuário do GitLab e na senha, você coloca o Token gerado no passo anterior, e pronto

## Desenvolvimento em Grupo

### Resolvendo conflitos de merge

1. Veja quais arquivos estão em conflito:

```bash
git status
```

2. Abra o arquivo em conflito

Abra o arquivo indicado pelo `git status` e procure as marcações que estarão mais ou menos assim:

```bash
<<<<<<< HEAD
código da sua branch atual
=======
código da branch que está sendo mesclada
>>>>>>> feature/combate
```

3. Decida qual trecho irá manter (ou uma combinação deles).
4. Apague as marcações `<<<<<<<`, `=======`, `>>>>>>>`.
5. Salve o arquivo, adicione e finalize o merge:

```bash
git add nome-do-arquivo
git commit -m "fix: resolve conflito de merge em nome-do-arquivo"
```

A maioria das IDEs (VS Code, por exemplo) e o próprio GitLab (na tela do Merge Request) oferecem uma interface visual para resolver conflitos, o que costuma ser mais fácil do que editar o arquivo manualmente.
