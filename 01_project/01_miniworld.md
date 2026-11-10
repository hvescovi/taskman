Minimundo: Sistema de Gerenciamento de Tarefas Kanban
Contexto

Uma equipe de apoio administrativo e acadêmico de um programa de pesquisa (coordenação de editais, estágios, extensão e prestação de contas) controla hoje suas atividades por meio de uma planilha com duas abas: “coisas pendentes”, com as tarefas em aberto, e “histórico”, com as ações já realizadas. Esse controle manual gera retrabalho (movimentar linhas entre abas), perda de visibilidade sobre o andamento e dificuldade de priorização. O novo sistema deve substituí-lo por um quadro Kanban.

Descrição

O sistema é utilizado por membros da equipe (ex.: Hylson, Samara, Raquel, Gio, Andrey, Sofia, Claudir). Cada membro possui nome e e-mail e pode assumir dois papéis em relação a uma tarefa: responsável (quem deve fazer/fez) e registrante (quem a cadastrou no sistema). Uma tarefa pode ter um ou mais responsáveis (ex.: “Gio e Raquel”, “Hylson, Samara, Claudir”, “todos”) ou, alternativamente, ter responsáveis definidos como alternativas (“Hylson ou Samara”). O sistema deve registrar se a atribuição é conjunta ou se basta que um dos indicados a execute.

Um quadro (board) organiza as tarefas em colunas, que representam as situações do fluxo de trabalho. Pelas planilhas, as situações identificadas são: Para fazer, Em andamento, Em verificação, Concluída e Cancelada. A situação “Criada”, presente no histórico, equivale ao registro inicial da tarefa. Cada coluna possui nome e ordem de exibição, e o sistema deve permitir que a equipe crie, renomeie ou reordene colunas. Opcionalmente, uma coluna pode ter limite de tarefas simultâneas (WIP).

Cada tarefa (cartão) possui título/descrição, data de registro, data de conclusão (preenchida quando concluída ou cancelada), situação atual (coluna), prioridade, natureza da ação, responsáveis, registrante e comentários. A prioridade é numérica: 0 indica que a tarefa deve ser feita na próxima oportunidade, 1 indica importante e 2 em diante indica “em tempo”, com valores maiores para menor urgência. A natureza da ação classifica a tarefa em categorias como Pesquisa, Estágio, EPE (editais), Extensão, entre outras, e pode ficar em branco. As categorias são cadastráveis pela equipe.

A descrição de uma tarefa pode conter links (documentos do Google Docs, processos no SIGAA/SIGA, etc.), e o sistema deve armazená-los como anexos/links vinculados ao cartão, cada um com URL e rótulo. Uma tarefa pode ter vários links.

Os comentários são registros textuais datados feitos por um membro sobre uma tarefa (ex.: “06/04: está liberada”, “Raquel disparou a tarefa, Andrey executou”). Uma tarefa pode ter vários comentários, que formam uma conversa cronológica em vez de um campo único.

Quando uma tarefa muda de coluna, o sistema registra automaticamente a movimentação: tarefa, coluna de origem, coluna de destino, data/hora e membro que a realizou. Esse registro substitui a aba “histórico” da planilha: o histórico passa a ser uma consulta sobre tarefas concluídas e movimentações, e o contador “Ações realizadas” (hoje 78) passa a ser calculado automaticamente a partir das tarefas concluídas em um período.

Algumas tarefas surgem de outras ou dependem delas (ex.: “Receber resoluções de pendências” só é concluída após sanar inadimplências, e então é preciso informar o responsável pela aprovação do relatório). O sistema deve permitir relacionar tarefas entre si (dependência ou tarefa-mãe/subtarefa).

Também há tarefas recorrentes ou associadas a editais/projetos específicos (ex.: edital 15/2025, edital 10/2024, edital integrado 2024). O sistema deve permitir agrupar tarefas por projeto/edital (ou etiqueta), de modo que seja possível filtrar tudo o que se relaciona a um mesmo edital.

O sistema deve permitir filtrar e ordenar o quadro por responsável, natureza, prioridade, projeto/edital e período, além de listar as tarefas de um membro e as tarefas atrasadas, quando houver prazo definido (a ser incluído como atributo opcional, pois a planilha atual não o possui).

Regras de negócio
Toda tarefa pertence a exatamente um quadro e está em exatamente uma coluna por vez.
Toda tarefa possui data de registro e registrante.
A data de conclusão só pode ser preenchida quando a tarefa estiver em Concluída ou Cancelada, e é gerada automaticamente na movimentação para essas colunas.
Tarefas concluídas ou canceladas saem da visão principal do quadro, mas permanecem consultáveis no histórico.
A prioridade é obrigatória, e o sistema ordena os cartões de cada coluna por prioridade e, em seguida, por data de registro.
Uma tarefa pode ter nenhum ou vários responsáveis, e um membro pode ser responsável por várias tarefas.
Uma tarefa possui no máximo uma natureza e um projeto/edital.
Toda movimentação entre colunas fica registrada e não pode ser apagada.
Toda movimentação também pode ser enviada por email para os usuários que desejam acompanhar as tarefas.

Resumo dos principais conceitos (para modelagem)

Entidades candidatas: Membro, Quadro, Coluna, Tarefa, Natureza, Projeto/Edital, Comentário, Link/Anexo, Movimentação e Dependência (autorrelacionamento de Tarefa).

Relacionamentos principais:

Quadro 1:N Coluna
Coluna 1:N Tarefa
Tarefa N:M Membro (responsáveis, com atributo “tipo de atribuição”: conjunta ou alternativa)
Tarefa N:1 Membro (registrante)
Tarefa N:1 Natureza
Tarefa N:1 Projeto/Edital
Tarefa 1:N Comentário (cada comentário N:1 Membro)
Tarefa 1:N Link
Tarefa 1:N Movimentação (cada movimentação N:1 Membro e referencia coluna de origem e destino)
Tarefa N:M Tarefa (dependência)