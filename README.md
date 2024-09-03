## Consumindo APIs Paginadas de Forma Distribuída com Python e Spark
#### Obs. No final do código tem o exemplo do código rodando... não trouxe todo o log para evitar ficar grande, mas ela vai salvando os dados em uma landing zone no storage, recomendo já ir lendo o dado em paralelo conforme neste link: https://www.linkedin.com/posts/wellikiandre_ambientereal-dataengineer-activity-7236798969064919040-Srmx?utm_source=share&utm_medium=member_desktop


😉 Consumindo APIs Paginadas de Forma Distribuída com Python e Spark
Quando trabalhamos com a coleta de grandes volumes de dados de APIs, o consumo eficiente e o controle das requisições tornam-se cruciais. Um cenário comum é a necessidade de realizar requisições paginadas para coletar todos os dados históricos disponíveis. Para alcançar essa proeza de forma mais rápida, a estratégia é clara: distribuir para conquistar.

No exemplo de código que desenvolvi, utilizei concurrent.futures para realizar esse consumo de forma distribuída, com controle detalhado das páginas consumidas e não consumidas. Os parâmetros devem ser testados previamente para evitar sobrecarga na API e garantir uma execução suave. Afinal, ninguém quer ver a API "chorar" por excesso de requisições, certo? 😅

🚀 Estrutura do Código
O código implementa uma função chamada requisicoes_paginadas_e_controle, que recebe os seguintes parâmetros:

url: Base da URL para as requisições GET.
headers: Cabeçalhos da requisição.
initial_params: Parâmetros iniciais para a requisição.
pg_init e pg_fim: Página inicial e final para as requisições paginadas.
max_workers [Dentro da Função]: Especifica o número de threads que o executor deve usar para executar tarefas simultaneamente. Esse número deve ser ajustado com base nos requisitos específicos da aplicação, no tipo de tarefa (I/O-bound ou CPU-bound), e nas limitações do sistema e da API.
📋 Funcionamento
Setup Inicial e Sessão HTTP: Utilizamos requests.Session() para manter uma sessão HTTP persistente, reduzindo o tempo de conexão para requisições subsequentes e aumentando a eficiência do processo.

Execução Distribuída das Requisições: Com concurrent.futures.ThreadPoolExecutor, realizamos requisições em paralelo, cada uma sendo executada em uma thread separada. Isso acelera significativamente a coleta dos dados, aproveitando ao máximo os recursos disponíveis.

Controle de Páginas e Erros: O código mantém um controle detalhado de cada página consumida, registrando o sucesso ou falha de cada requisição. Em caso de falha, ele armazena a página específica e os detalhes do erro, facilitando a análise posterior.

Tratamento de Erros: Em vez de um tratamento imediato para os erros, o código permite primeiro entender o motivo da falha. A sugestão no final do script é que você faça uma análise cuidadosa dos dados de controle antes de tomar qualquer ação corretiva.

💡 Observações
Há espaço para melhorias, como a substituição dos print por um sistema de logging mais robusto e uma melhor gestão das tarefas I/O-bound. Entretanto, o esqueleto do código já está funcional e foi testado em produção, com um tempo de desenvolvimento de aproximadamente 2 horas.
