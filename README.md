# Como utilizar o Git e GitLab no seu projeto Godot

Essa ideia de guia veio primeiramente para auxiliar e padronizar o meu projeto de **TCC** em grupo da faculdade, de **Engenharia de Software**, mas nosso **TCC** se retratará de um jogo educativo.

Este guia se baseará em conceitos de Git Flow, mas com adaptações para o nosso caso de uso, e nas documentações oficiais do Git, GitLab e Godot.

## Índice

- **Git**
   - [Primeira vez usando o Git?](#primeira-vez-usando-o-git)
   - [Criando seu primeiro repositório](#criando-seu-primeiro-repositório)
   - [Como vincular repositório local com o remoto](#como-vincular-repositório-local-com-o-remoto)
   - [Boas práticas de commit](#boas-práticas-de-commit)
   - [Um pouco mais sobre branches](#um-pouco-mais-sobre-branches)
   - [Utilizando o Git em grupo](#utilizando-o-git-em-grupo)

- **GitLab**
   - [Primeira vez utilizando o GitLab](#primeira-vez-utilizando-o-gitlab)
   - [Como criar um projeto](#como-criar-um-projeto)
   - [Como adicionar Grupo](#como-adicionar-grupo)
   - [Como clonar repositório](#como-clonar-repositório)
   - [Merge Request](#merge-request)

- **Godot**
   - [Deixe o Godot criar o `.gitignore` para você](#deixe-o-godot-criar-o-gitignore-para-você)
   - [Um pouco mais sobre o `.gitignore` no Godot](#um-pouco-mais-sobre-o-gitignore-no-godot)
   - [Git LFS plugin](#git-lfs-plugin)

- **Desenvolvimento em Grupo**
   - [Fluxo do grupo](#fluxo-do-grupo)
   - [Algumas regras do grupo](#algumas-regras-do-grupo)
   - [Ajuda com erros e alguns problemas comuns](#ajuda-com-erros-e-alguns-problemas-comuns)

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
| `refactor:` | `refactor: extrai lógica de vida do jogador para um script próprio` |

---

Existe uma regra que utiliza `!` para mudanças que quebram compatibilidade:
`feat!: muda o formato do save`.

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

### Como criar um projeto

1. No canto superior, clique em Create new (ícone de +) e depois em New project/repository.
2. Selecione Create blank project.
3. Preencha os detalhes do projeto:
   - **Project name**: nome do projeto (ex.: jogo-educativo-tcc).
   - **Project slug**: caminho usado na URL do projeto (preenchido automaticamente a partir do nome, mas pode ser editado).
   - **Visibility Level**: **Private** (só quem for convidado acessa), **Internal** ou **Public**.
   - **Project description** (opcional): uma descrição curta do projeto.
4. Clique em Create project.

**Dica para evitar um problema comum de `refusing to merge unrelated histories` :** crie o projeto no GitLab **vazio**, sem README, mas caso tenha dado esse erro, rode:

```bash
git pull origin main --allow-unrelated-histories
```

### Como adicionar Grupo

1. Na barra lateral esquerda, clique em **Create new** → **New group**.
2. Defina o **Group name** e o **Visibility level**.
3. Depois de criado, entre no grupo e vá em **Manage** → **Members**.
4. Clique em **Invite members**. Se o outro membro já tiver conta no GitLab, digite o nome de usuário dele, se não tiver, digite o e-mail.
5. Escolha o papel (Role) de cada um:
   - **Developer**: pode enviar código, criar branches e **Merge Requests** (papel recomendado para os integrantes do projeto).
   - **Maintainer**: além disso, pode alterar configurações do projeto/grupo e aprovar merges na `main`.
6. Clique em **Invite**. Ao criar um novo projeto, escolha esse grupo como namespace — assim, todos os membros do grupo automaticamente têm acesso ao projeto.

### Como clonar repositório

1. No projeto no GitLab, clique no botão **Clone**.
2. Copie a URL em **Clone with HTTPS** ou **Clone with SSH** (se já tiver configurado a chave SSH, prefira essa opção, para evita digitar usuário/senha toda hora).
3. No terminal do diretório/pasta onde você quer salvar o projeto:

Caso esteja usando autenticação **HTTPS**:

```bash
git clone https://gitlab.com/usuario-ou-grupo/meu-projeto.git
```

Se estiver usando **SSH**:

```bash
git clone git@gitlab.com:usuario-ou-grupo/meu-projeto.git
```

Isso cria uma pasta com o nome do projeto, já com o remoto `origin` configurado automaticamente (basicamente, não precisa rodar `git remote add origin` de novo).

### Merge Request

Depois de terminar uma alteração em uma branch, você pode criar um **Merge Request (MR)** para propor que essas alterações sejam incorporadas a outra branch.

```bash
git push -u origin feature/combate
```

Depois do `push`, o **GitLab** pode mostrar uma opção para **Create merge request**. Também é possível criar um **Merge Request** pela página **Code** → **Merge requests** → **merge request**.

Por exemplo:

```
Source branch: development
Target branch: main
```

**_Dica:_** O único MR que tem a `main` como destino é o de `development → main`,
quando o grupo fecha uma versão.

#### Preenchendo o Merge Request

Na página de criação do **Merge Request**:

1. Confira a **Source** branch (que contém as alterações.) e a **Target** branch (receberá essas alterações).
2. Preencha o **Title** com um título que identifique claramente o objetivo do **Merge Request**: "Adição da lógica de dano do personagem X".
3. Preencha a Description explicando o que foi alterado ou fornecendo informações importantes para quem fará a revisão (coloque como testar essa funcionalidade).
4. **Reviewer**: se o projeto tiver revisão de código, adicione a pessoa que responsável.
5. **Assignee**: indique a pessoa responsável pelo andamento do Merge Request, caso tenha alguém que irá monitorar como está indo.
6. Depois de preencher os campos necessários, selecione **Create merge request**.

#### Apagando a branch depois do merge

Depois que um **Merge Request** é mesclado, a branch de origem pode ser excluída caso não seja mais necessária.

Na criação do **Merge Request**, o GitLab oferece a opção **Delete source branch when merge request is accepted**, se selecionada, a branch de origem será excluída automaticamente após o merge.

Isso exclui a branch no repositório remoto. A cópia local pode ser excluída separadamente:

```bash
git branch -d feature/combate
```

**_Dica:_** `git branch -d` só apaga a branch se ela já tiver sido mesclada. Se não foi, o **Git** recusa. Com D (maiúsculo) força a exclusão. **Não use `-D` sem ter certeza absoluta.**

## Godot

> Godot busca ser amigável a sistemas de controle de versão e gerar arquivos majoritariamente legíveis e mescláveis.
>
> — [Documentação oficial do Godot](https://docs.godotengine.org/en/stable/tutorials/best_practices/version_control_systems.html)

### Deixe o Godot criar o `.gitignore` para você

Não utilize um `.gitignore` aleatorio da internet, pois aqui o Godot ajuda de mais.

**Em um projeto novo**: no Gerenciador de Projetos, na janela **New Project** → **Git** em _Version Control Metadata_. O Godot cria os arquivos `.gitignore` e `.gitattributes` na raiz do projeto.

**Em um projeto que já existe**: no editor, acesse o menu **Project** → **Version Control** → **Generate Version Control Metadata**. O resultado é o mesmo.

### Um pouco mais sobre o `.gitignore` no Godot

O Godot acaba criando alguns arquivos sozinhos:

| Item            | O que é                                                              |
| --------------- | -------------------------------------------------------------------- |
| `.godot/`       | pasta que guarda dados de cache do projeto                           |
| `*.translation` | traduções binárias, geradas automaticamente a partir de arquivos CSV |

---

### Git LFS plugin

Essa extensão é utilizada para lidar com arquivos grande. Basicamente mantém no repositório um arquivo pequeno chamado ponteiro (pointer), que indica qual arquivo grande deve ser obtido. O conteúdo do arquivo grande é armazenado no servidor de armazenamento do **Git LFS**, separado dos objetos Git normais.

#### Configure o LFS ANTES de commitar os assets

o **LFS** precisa estar configurado antes de você commitar os arquivos. Se eles já estiverem no repositório, será preciso removê-los e readicioná-los depois de configurar o LFS.

Para configurar:

```bash
git lfs install
git lfs track "*.png"
git lfs track "*.ogg"
```

Cada `git lfs track` escreve uma linha no `.gitattributes`. Commite esse arquivo junto com o resto.

Um detalhe importante: **quem clonar o repositório precisa ter o Git LFS instalado** na máquina para conseguir baixar os arquivos reais.

## Desenvolvimento em Grupo

### Fluxo do grupo

#### Setup: você faz isso uma única vez

- Instalar o [Git](https://git-scm.com/downloads)
- Instalar o [Git LFS](https://git-lfs.com/) (**obrigatório**, senão as imagens e sons não baixam)
- Instalar o Godot na versão combinada pelo grupo
- Configurar sua identidade, usando o mesmo e-mail da sua conta do GitLab:
- Criar conta no GitLab e aceitar o convite do grupo
- Configurar a [chave SSH](#configurando-o-ssh) (ou o [PAT via HTTPS](#como-criar-o-pat))
- Clonar o projeto

> **Para evitar erros, revisite as seções referentes a cada etapa**

#### A rotina de todo dia

**1. Antes de começar**, atualize a branch estável e crie a sua branch de tarefa:

```bash
git switch development
git pull origin development
git switch -c feature/nome-da-sua-tarefa
```

**2. Enquanto trabalha**, repita quantas vezes forem necessárias:

```bash
git status
git add .
git commit -m "feat: descreve o que você fez"
```

**3. Quando terminar a tarefa**, publique a branch:

```bash
git push -u origin feature/nome-da-sua-tarefa
```

O `-u` é apenas no **primeiro** push desta branch. Depois disso, `git push` sozinho já basta.

**4. Abra o [Merge Request](#merge-request) no GitLab:**

#### Revisão de arte é visual

Diferente de código, a pessoa que revisa **não consegue ver a mudança só lendo o MR no GitLab**. Ela precisa:

1. Fazer um `git fetch` para trazer a branch remota.
2. Rodar `git switch art/nome-do-asset` para mudar para aquela branch.
3. Abrir o projeto no Godot.
4. Navegar até a cena onde os assets são usados.
5. Olhar e conferir se ficou como esperado (cores certas, animação fluida, áudio sincronizado, etc.).
6. Voltar para a description do MR no GitLab e clicar em **Approve**

### Algumas regras do grupo

1. **Nunca commite direto na `main` ou na `development`.** Sempre branch + Merge Request.
2. **Uma tarefa = uma branch = um Merge Request.**
3. **Avise no grupo antes de mexer numa cena.** "Peguei a `fase_01.tscn`". Duas pessoas na mesma cena ao mesmo tempo aumenta chances de conflitos.
4. **Dê `git pull origin development` antes de começar o dia.**
5. **Commite sempre e com frequência.** lembre-se, quanto mais demorar pra commitar, mas confuso será para entender o que você mudou e aumenta chances de conflito.
6. **Se der erro, converse com a equipe.** Pois tentar solucionar sozinho, pode resultar em partes apagadas sem a intenção, entre outros problemas maiores.

### Ajuda com erros e alguns problemas comuns

#### Resolvendo conflitos de merge

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

#### Tabela de socorro rápida

| O que apareceu na tela                            | O que aconteceu                              | O que fazer                                                                  |
| ------------------------------------------------- | -------------------------------------------- | ---------------------------------------------------------------------------- |
| `fatal: not a git repository`                     | Você não está na pasta do projeto            | Use `cd` até a pasta certa                                                   |
| `Updates were rejected`                           | Alguém enviou algo antes de você             | `git pull origin <sua-branch>`, resolva se houver conflito, e envie de novo  |
| `refusing to merge unrelated histories`           | Os dois repositórios nasceram separados      | `git pull origin main --allow-unrelated-histories`                           |
| `CONFLICT (content): ...`                         | Duas pessoas mexeram na mesma linha          | Siga a seção [Resolvendo conflitos de merge](#resolvendo-conflitos-de-merge) |
| Conflito em um arquivo `.tscn`                    | Duas pessoas mexeram na mesma cena           | **Pare.** Chame a outra pessoa. Não resolva na mão                           |
| `Permission denied (publickey)`                   | Sua chave SSH não está configurada no GitLab | Refaça a seção [Configurando o SSH](#configurando-o-ssh)                     |
| Muitos arquivos "modificados" sem você ter mexido | Fim de linha (CRLF/LF) no Windows            | Veja Windows e o fim de linha                                                |
| Imagens e sons vieram como texto estranho         | O Git LFS não está instalado                 | Instale o Git LFS e rode `git lfs pull`                                      |
