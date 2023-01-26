
<h1 align="center">task01-grupo07</h1>


<p align="center">Repositório da tarefa 01 do programa Vem Ser contendo a documentação dos comandos git rebase, cherry-pick, revert e squash.</p>

<h1 align="center">Colaboradores</h1>

<p align="center">[Carolina Bastos](https://github.com/bastoscarolina) - Trilha QA
[Daniel Vieira](https://github.com/dhermys) - Trilha QA
[Danilo Torquato](https://github.com/DaniloTorquatoUbaldine) - Trilha QA</p>

<h2 align="center">Cherry-pick</h2>
Funcionamento: dado um ou mais commits existentes, aplique a alteração que cada um introduz, registrando um novo commit para cada um. Isso requer que a sua árvore de trabalho esteja limpa (nenhuma alteração a partir do commit HEAD). 
Quando não é óbvio como aplicar uma alteração, acontece o seguinte: 

1. O ramo atual e o ponteiro HEAD permanecem no último commit realizado com sucesso.

2. A referência CHERRY_PICK_HEAD é configurada para apontar para o commit que introduziu a mudança que é difícil de aplicar.

3. Caminhos nos quais a mudança aplicada corretamente são atualizados no arquivo de índice e na sua árvore de trabalho.

4. Para os caminhos conflitantes, o arquivo do índice registra até três versões, conforme descrito na seção "MESCLAGEM REAL" do git-merge[1]. Os arquivos da árvore de trabalho incluirão uma descrição do conflito entre os marcadores de conflito habituais <<<<<<< e >>>>>>>.

5. Nenhuma outra modificação é feita.

Sintaxe:  
`git cherry-pick [--edit] [-n] [-m parent-number] [-s] [-x] [--ff]
		  [-S[<keyid>]] <commit>…​`

`git cherry-pick (--continue | --skip | --abort | --quit)`

Aplicação:  

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



### Referências: As informações referentes ao CHerry-pick foram retiradas da documentação da ferramenta: [link name](https://git-scm.com/docs/git-cherry-pick/pt_BR)


