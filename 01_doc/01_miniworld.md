# Minimundo: Sistema de Gerenciamento de Tarefas Kanban

## Contexto

Uma equipe de apoio administrativo e acadêmico de um programa de pesquisa (coordenação de editais, estágios, extensão e prestação de contas) controla hoje suas atividades por meio de uma planilha com duas abas: "coisas pendentes", com as tarefas em aberto, e "histórico", com as ações já realizadas. Esse controle manual gera retrabalho (movimentar linhas entre abas), perda de visibilidade sobre o andamento e dificuldade de priorização. O novo sistema deve substituí-lo por um quadro Kanban.

## Descrição

**Membros.** O sistema é utilizado por membros da equipe (ex.: Hylson, Samara, Raquel, Gio, Andrey, Sofia, Claudir). Cada membro possui nome, e-mail e uma opção indicando se deseja ou não receber notificações por e-mail sobre mudanças de situação das tarefas. Em relação a uma tarefa, o membro pode assumir dois papéis: **responsável** (quem deve fazer ou fez) e **registrante** (quem a cadastrou no sistema).

**Responsáveis.** Uma tarefa pode ter um ou mais responsáveis (ex.: "Gio e Raquel", "Hylson, Samara, Claudir", "todos"). Os responsáveis podem ser atribuídos de forma **conjunta** (todos executam) ou **alternativa** (basta um dos indicados executar, ex.: "Hylson ou Samara"). O sistema registra o tipo de atribuição.

**Quadro e colunas.** Um quadro organiza as tarefas em colunas, que representam as situações do fluxo de trabalho. Pelas planilhas, as situações identificadas são: *Para fazer*, *Em andamento*, *Em verificação*, *Concluída* e *Cancelada*. A situação "Criada", presente no histórico, equivale ao registro inicial da tarefa. Cada coluna possui nome e ordem de exibição, e a equipe pode criar, renomear e reordenar colunas. Opcionalmente, uma coluna pode ter limite de tarefas simultâneas (WIP).

**Tarefas.** Cada tarefa (cartão) possui título/descrição, data de registro, data de conclusão (preenchida quando concluída ou cancelada), prazo (opcional, pois a planilha atual não o possui), situação atual (coluna), prioridade, natureza da ação, projeto/edital, responsáveis, registrante e comentários.

- A **prioridade** é numérica: 0 indica que a tarefa deve ser feita na próxima oportunidade, 1 indica importante e 2 em diante indica "em tempo", com valores maiores para menor urgência.
- A **natureza da ação** classifica a tarefa em categorias como *Pesquisa*, *Estágio*, *EPE* (editais), *Extensão*, entre outras, e pode ficar em branco. As categorias são cadastráveis pela equipe.

**Links.** A descrição de uma tarefa pode conter links (documentos do Google Docs, processos no SIGAA/SIGA etc.). O sistema os armazena como anexos vinculados ao cartão, cada um com URL e rótulo. Uma tarefa pode ter vários links.

**Comentários.** Os comentários são registros textuais datados, feitos por um membro sobre uma tarefa (ex.: "06/04: está liberada", "Raquel disparou a tarefa, Andrey executou"). Uma tarefa pode ter vários comentários, que formam uma conversa cronológica em vez de um campo único.

**Movimentações e histórico.** Quando uma tarefa muda de coluna, o sistema registra automaticamente a movimentação: tarefa, coluna de origem, coluna de destino, data/hora e membro que a realizou. Esse registro substitui a aba "histórico" da planilha: o histórico passa a ser uma consulta sobre tarefas concluídas e movimentações, e o contador "Ações realizadas" (hoje 78) passa a ser calculado automaticamente a partir das tarefas concluídas em um período.

**Notificações.** A cada movimentação, o sistema envia um e-mail de notificação aos membros que optaram por receber notificações de mudança de situação das tarefas. Cada membro pode ativar ou desativar essa opção a qualquer momento.

**Dependências.** Algumas tarefas surgem de outras ou dependem delas (ex.: "Receber resoluções de pendências" só é concluída após sanar inadimplências, e então é preciso informar o responsável pela aprovação do relatório). O sistema permite relacionar tarefas entre si (dependência ou tarefa-mãe/subtarefa).

**Projetos/editais.** Há tarefas recorrentes ou associadas a editais/projetos específicos (ex.: edital 15/2025, edital 10/2024, edital integrado 2024). O sistema permite agrupar tarefas por projeto/edital, de modo que seja possível filtrar tudo o que se relaciona a um mesmo edital.

**Consultas.** O sistema permite filtrar e ordenar o quadro por responsável, natureza, prioridade, projeto/edital e período, além de listar as tarefas de um membro e as tarefas atrasadas (quando houver prazo definido).

## Regras de negócio

- **RN1:** Toda tarefa pertence a exatamente um quadro e está em exatamente uma coluna por vez.
- **RN2:** Toda tarefa possui data de registro e registrante.
- **RN3:** A data de conclusão só pode ser preenchida quando a tarefa estiver em *Concluída* ou *Cancelada*, e é gerada automaticamente na movimentação para essas colunas.
- **RN4:** Tarefas concluídas ou canceladas saem da visão principal do quadro, mas permanecem consultáveis no histórico.
- **RN5:** A prioridade é obrigatória, e o sistema ordena os cartões de cada coluna por prioridade e, em seguida, por data de registro.
- **RN6:** Uma tarefa pode ter nenhum ou vários responsáveis, e um membro pode ser responsável por várias tarefas.
- **RN7:** Uma tarefa possui no máximo uma natureza e um projeto/edital.
- **RN8:** Toda movimentação entre colunas é registrada e não pode ser apagada.
- **RN9:** Toda movimentação é notificada por e-mail aos membros que desejam acompanhar as tarefas.
- **RN10:** Cada membro define se deseja ou não receber e-mails de notificação de mudança de situação das tarefas.

## Resumo dos principais conceitos (para modelagem)

**Entidades candidatas:** Membro, Quadro, Coluna, Tarefa, Natureza, Projeto/Edital, Comentário, Link, Movimentação e Dependência (autorrelacionamento de Tarefa). A Notificação por e-mail pode ser modelada como entidade, caso se queira registrar os envios, ou tratada apenas como processo.


**Relacionamentos principais:**
- Quadro 1:N Coluna
- Coluna 1:N Tarefa
- Tarefa N:M Membro (responsáveis, com atributo "tipo de atribuição": conjunta ou alternativa)
- Tarefa N:1 Membro (registrante)
- Tarefa N:1 Natureza
- Tarefa N:1 Projeto/Edital
- Tarefa 1:N Comentário (cada comentário N:1 Membro)
- Tarefa 1:N Link
- Tarefa 1:N Movimentação (cada movimentação N:1 Membro e referencia coluna de origem e destino)
- Tarefa N:M Tarefa (dependência)
- Movimentação N:M Membro (notificação, apenas membros com *recebe_notificacoes* ativo; opcional, se os envios forem registrados)
