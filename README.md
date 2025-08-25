# trabalho-Documentar-o-projeto-do-link

comentários do projeto:
O main() é chamado.


A função carregar() é executada:


O código verifica se existe o arquivo data_tasks.csv.


Se existir, ele lê cada linha e popula os arrays ids, titulos, descrs, status e criados.


O número de tarefas existentes n é atualizado.


O servidor HTTP é criado na porta 8080.


O servidor define dois contextos:


/ → RootHandler (a página HTML)


/api/tasks → ApiTasksHandler (a API REST)


O servidor começa a rodar e imprime:

 Servindo em http://localhost:8080



Quando acesso o navegador na raiz /
O RootHandler é chamado.


O código verifica se o método é GET → verdadeiro.


O HTML contido em INDEX_HTML é convertido em bytes e enviado como resposta.


O navegador recebe o HTML, renderiza o Kanban:


Formulário para criar tarefas


Três colunas: To-Do, Doing, Done


Botões para mover tarefas e deletar tarefas



Quando o JS do navegador roda listar()
O JS faz um fetch("/api/tasks") → requisição GET para a API.


O ApiTasksHandler é chamado:


Verifica que é GET e que o caminho é /api/tasks.


Chama listarJSON() → percorre todos os arrays de tarefas e transforma em JSON.


Envia JSON de volta para o navegador.


O navegador recebe a lista de tarefas e a função render() cria os cards de cada tarefa em suas colunas correspondentes.



Quando o usuário cria uma nova tarefa
O usuário preenche título e descrição e clica em “Adicionar”.


O JS envia um POST para /api/tasks com JSON { "titulo": "...", "descricao": "..." }.


ApiTasksHandler é chamado:


Lê o corpo do request.


Extrai titulo e descricao com jsonGet().


Valida se o título não é vazio.


Chama criar(titulo, descricao) → adiciona a tarefa no próximo índice de arrays, define status = 0 e timestamp.


Atualiza n.


Salva tudo no CSV com salvar().


Retorna JSON da tarefa criada.


O JS recebe o JSON e chama listar() novamente → a nova tarefa aparece na coluna To-Do.



Quando o usuário move uma tarefa de coluna
Usuário clica no botão ◀ ou ▶.


JS chama fetch("/api/tasks/{id}/status", { method: "PATCH", body: { status: novoStatus } }).


ApiTasksHandler processa PATCH:


Extrai id da URL.


Lê status do corpo JSON.


Procura índice da tarefa em ids[] usando findIdxById().


Atualiza status[i] = novoStatus.


Salva no CSV.


Retorna JSON atualizado.


JS chama listar() → move a tarefa para a coluna correta na interface.



Quando o usuário deleta uma tarefa
Usuário clica “Excluir”.


JS faz DELETE /api/tasks/{id}.


ApiTasksHandler processa DELETE:


Procura índice da tarefa.


Remove o elemento dos arrays e decrementa n.


Salva no CSV.


Retorna 204 (sem conteúdo).


JS chama listar() → remove o card da interface.



