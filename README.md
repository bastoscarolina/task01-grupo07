
<h1 align="center">task01-grupo07</h1>


<p align="center">Repositório da tarefa 01 do programa Vem Ser contendo a documentação dos comandos git rebase, cherry-pick, revert e squash.</p>

<h1 align="center">Colaboradores</h1>


- [Carolina Bastos](https://github.com/bastoscarolina) - Trilha QA
- [Diego Vieira](https://github.com/dhermys) - Trilha QA
- [Danilo Torquato](https://github.com/DaniloTorquatoUbaldine) - Trilha QA

---

## Cherry-pick

### Funcionamento

Dado um ou mais commits existentes, aplica a alteração que cada um introduz, registrando um novo commit para cada um. Isso requer que a sua árvore de trabalho esteja limpa (nenhuma alteração a partir do commit HEAD). 
Quando não é óbvio como aplicar uma alteração, acontece o seguinte: 

1. O ramo atual e o ponteiro HEAD permanecem no último commit realizado com sucesso.

2. A referência CHERRY_PICK_HEAD é configurada para apontar para o commit que introduziu a mudança que é difícil de aplicar.

3. Caminhos nos quais a mudança aplicada corretamente são atualizados no arquivo de índice e na sua árvore de trabalho.

4. Para os caminhos conflitantes, o arquivo do índice registra até três versões, conforme descrito na seção "MESCLAGEM REAL" do git-merge[1]. Os arquivos da árvore de trabalho incluirão uma descrição do conflito entre os marcadores de conflito habituais <<<<<<< e >>>>>>>.

5. Nenhuma outra modificação é feita.

### Sintaxe
`git cherry-pick [--edit] [-n] [-m parent-number] [-s] [-x] [--ff]
		  [-S[<keyid>]] <commit>…​`

`git cherry-pick (--continue | --skip | --abort | --quit)`

### Aplicação

Aplique a mudança introduzida pelo commit na ponta do branch master e crie um novo commit com esta mudança:

`git cherry-pick ..master`

`git cherry-pick ^HEAD master`

Aplique as alterações introduzidas por todos os commits que são ancestrais do master, mas não do HEAD para produzir novos commits:

`git cherry-pick maint próximo ^master`


`git cherry-pick maint master..próximo`

Aplique as alterações introduzidas por todos os commits que sejam ancestrais do "maint" ou "next", porém não do "master" ou dos seus ancestrais. Note que o último não significa que maint e tudo entre master e next; especificamente, maint não será usado caso esteja incluso no master:

`git cherry-pick master~4 master~2`

Aplique as alterações introduzidas pelo quinto e terceiro últimos commits apontados pelo master e crie 2 novos commits com essas mudanças:

`git cherry-pick -n master~1 next`

Aplique as alterações na árvore de trabalho e no índice que foram introduzidos pelo segundo último commit apontada pelo "master" e pelo último commit apontada pelo próximo, porém não crie nenhum commit com estas alterações:

`git cherry-pick --ff ..next`

Caso o histórico seja linear e HEAD seja um ancestral do próximo, atualize a árvore de trabalho e avance o ponteiro HEAD para que coincida com o próximo. Caso contrário, aplique as alterações introduzidas pelos commits que estão na próxima, mas não o `HEAD`para o ramo atual, criando um novo commit para cada nova alteração.

`git rev-list --reverse master -- README | git cherry-pick -n --stdin`

Aplique as alterações introduzidas por todas as confirmações no ramo principal que tocaram no README para a árvore de trabalho e o índice, para que o resultado possa ser inspecionado e transformado em uma única nova confirmação, se adequado.




---
## Rebase

### Funcionamento

- O git rebase serve tanto para atualizar quanto para corrigir a origem de uma branch

### Sintáxe

- Caso seja usado para corrigir a origem de uma branch, a síntexe deve ser:
  -  `git rebase --onto \<origem certa> \<origem errada> \<branch que está tendo a origem modificada>`

- Caso seja usado para atualizar a origem de uma branch:
  - `git rebase \<branch mais atual> \<branch a ter origem atualizada>`

### Aplicação


Suponha que o seguinte histórico exista e que o ramo atual seja "topic":

          A---B---C topic
         /
    D---E---F---G master
A partir deste ponto, o resultado de um dos seguintes comandos:

`git rebase master` <br>
`git rebase master topic` <br>
seria:

                  A'--B'--C' topic
                 /
    D---E---F---G master

---
Aqui está como você transplantaria um ramo do tópico com base em um ramo para outro, para fingir que você bifurcou o ramo do tópico deste último ramo, utilizando rebase --onto.

Primeiro, vamos supor que o seu tópico seja baseado no ramo next. Por exemplo, um recurso desenvolvido em topic depende de algumas funcionalidades encontradas em next.

    o---o---o---o---o  master
         \
          o---o---o---o---o  next
                           \
                            o---o---o  topic


Queremos criar um tópico bifurcado no ramo master; porque a funcionalidade da qual o tópico depende foi mesclado na ramificação master mais estável. Queremos que a nossa árvore fique assim:

    o---o---o---o---o  master
        |            \
        |             o'--o'--o'  topic
         \
          o---o---o---o---o  next
Podemos conseguir isso utilizando o seguinte comando:

`git rebase --onto master next topic`

---
Um outro exemplo da opção --onto é reconstruir a fundação da parte de uma ramificação. Se tivermos a seguinte situação:

                            H---I---J topicB
                           /
                  E---F---G  topicA
                 /
    A---B---C---D  master
então o comando

`git rebase --onto master topicA topicB` <br>
resultaria em:

                 H'--I'--J'  topicB
                /
                | E---F---G  topicA
                |/
    A---B---C---D  master
É útil quando o topicB não depender do topicA.

---
Um intervalo dos commits também pode ser removido com a reconstrução rebase. Se tivermos a seguinte situação:

    E---F---G---H---I---J  topicA
então o comando

git rebase --onto topicA~5 topicA~3 topicA <br>
resultaria na remoção dos commits F e G:

    E---H'---I'---J'  topicA
---

## Revert

### Funcionamento

- Reverte alguns commits existentes

### Sintáxe

- Reverta as alterações informadas pelo quarto último commit no HEAD e crie um novo commit com as alterações revertidas.
`git revert HEAD~3`

- Reverta as alterações feitas pelos commits do quinto último commit no master (incluso) para o terceiro último commit no master (incluso), porém não crie nenhum commit com as alterações revertidas. A reversão altera apenas a árvore de trabalho e o índice.
`git revert -n master~5..master~2`

### Aplicação

- Dado um ou mais commits já existentes, reverta as alterações introduzidas pelos patches relacionados e registre alguns novos commits que registram neles. Isso requer que a sua árvore de trabalho esteja limpa (nenhuma alteração a partir do commit HEAD).

Nota: git revert é utilizado para registrar alguns commits novos para reverter o efeito de alguns commits anteriores (geralmente apenas um com problema). Caso queira descartar todos os commits das alterações que não foram aplicados no seu diretório de trabalho, você deve consultar git-reset[1], em particular a opção --hard. Caso queira extrair arquivos específicos da maneira que eles estavam em um outro commit, você deve consutar git-restore[1], principalmente a opção --source. Tenha cuidado com ambas alternativas pois elas descartam qualquer modificação não aplicada no seu diretório de trabalho.

## Squash

### Funcionamento

- Squash é o ato de transformar vários commits sequenciais em um só. Assim, podemos ter um único commit que contempla toda a funcionalidade em desenvolvimento e, se necessário, modificarmos a mensagem para uma mais condizente ao que foi implementado. Para realizarmos o squash, primeiro precisamos identificar quais os commits que queremos unificar e então pegar o hash do commit anterior ao primeiro commit que será unificado.

### Sintáxe

- Não há um comando `git squash`, para obter o esmagamento de vários commits em um só com o squash precisamos inicialmente utilizar `git rebase -i` como ferramenta interativa para esmagar commits.

### Aplicação

- Considerando o exemplo a seguir, iremos realizar o squash dos 3 últimos commits com a ferramenta interativa `git rebase`, então precisamos pegar o hash do 4º commit conforme imagem abaixo:

![Exemplo squash](https://miro.medium.com/max/640/1*k3hy1YQOmWGlEEQUE_wnsA.webp)

Com o hash em mãos, iremos utilizar o rebase, mas dessa vez usando o modo interativo. Esse modo permite várias alterações interessantes na maneira que o comando será realizado. Iremos utilizar como parâmetro de destino o commit do qual copiamos o hash. Dessa forma o Git irá voltar o histórico para o ponto deste commit e iremos escolher como os commits que sobraram serão aplicados.

![Exemplo squash](https://miro.medium.com/max/640/1*3reDdcvOH6lP_VJCq2DMfQ.webp)

![Exemplo squash](https://miro.medium.com/max/640/1*VHqRAoW7R_iFyT6s7CgcUA.webp)

O resultado é uma tela com a lista de commits que serão aplicados e as opções do que podemos fazer para cada commit. Podemos alterar a ordem que os commits serão aplicados simplesmente trocando as linhas de lugar (a ordem de aplicação é de cima para baixo). No nosso caso, iremos utilizar a opção "s" ou "squash". O squash joga o conteúdo do commit atual junto com o conteúdo do commit acima dele. Isso pode ser feito em sequência, como vou mostrar aqui, porém é necessário manter pelo menos um commit como pick no topo para que o conteúdo dos outros commits sejam jogados nele.

![Exemplo squash](https://miro.medium.com/max/640/1*Zc2UqBXzWgD09tjUv73R1Q.webp)

Após selecionar as opções, salvar e fechar, os commits são aplicados e aparece outra tela onde podemos alterar a mensagem do commit gerado a partir do squash.

![Exemplo squash](https://miro.medium.com/max/640/1*bsP4Z3lqtGkNdBQi8eKyCw.webp)

Após alterada a mensagem, o rebase é finalizado e temos como resultado um único commit no histórico.

![Exemplo squash](https://miro.medium.com/max/640/1*jCeGuwFfTTVhubUJNpYUHA.webp)

Referências:
- [Corrigindo a origem de um branch com git rebase](https://jtemporal.com/corrigindo-a-origem-de-um-branch-com-git-rebase/?utm_source=gitfichas)
- [Atualizando um branch com git rebase](https://jtemporal.com/atualizando-um-branch-com-git-rebase/?utm_source=gitfichas)
- [Git Rebase Documentation](https://git-scm.com/docs/git-rebase/pt_BR)
- [Git Revert Documentation](https://git-scm.com/docs/git-revert/pt_BR)
- [Utilizando rebase e squash para melhorar o histórico do Git](https://medium.com/cwi-software/utilizando-rebase-e-squash-para-melhorar-o-hist%C3%B3rico-do-git-fdb2d952c09c)
- [Git Cherry-pick Documentation](https://git-scm.com/docs/git-cherry-pick/pt_BR)