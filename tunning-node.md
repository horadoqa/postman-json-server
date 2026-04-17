# NODE

Os três pilares:

✔️ JavaScript (base bem feita)
✔️ JSON (estrutura de dados)
✔️ HTTP (comunicação)

🟡 JavaScript (base bem feita)

Não só o básico, mas principalmente manipulação de dados. No backend, você vai lidar com objetos e arrays o tempo todo. Se operações como map, filter ou desestruturação ainda não são naturais pra você, isso começa a impactar diretamente na forma como o código é escrito e mantido.

E aqui entra um ponto importante: assincronismo.

No Node.js, praticamente tudo envolve async/await. Entender como isso funciona evita desde bugs simples até problemas de desempenho mais difíceis de identificar depois.
—
🔵 JSON (estrutura de dados)

É o formato padrão de comunicação das APIs. Mais do que saber o que é, o importante é entender como os dados estão sendo estruturados e manipulados. Muitos erros comuns no backend vêm justamente de inconsistências nesse processo.
—
🟢 HTTP (comunicação)

O backend, na prática, é sobre responder requisições. Então entender status codes e headers faz diferença. Não só saber que 201 é criação ou 403 é permissão, mas entender o que cada resposta está comunicando. Headers também entram nisso, principalmente em coisas como autenticação e definição do tipo de conteúdo. Sem esse entendimento, fica difícil estruturar bem uma API.
—

## Melhorias de performance

1. Connection pool no banco

A API abria uma conexão nova no PostgreSQL pra cada request. Em picos de tráfego, o banco recusava conexões e a API caía. Coloquei pg-pool com 20 conexões reutilizáveis. Latência caiu 60% só com isso.

2. Removi await sequencial onde dava pra paralelizar

Encontrei dezenas de trechos assim: const user = await getUser(id); const orders = await getOrders(id); const address = await getAddress(id);

Três queries independentes rodando em série. Troquei por Promise.all e o tempo de resposta caiu de 900ms pra 280ms na rota.

3. Cache em memória para dados quentes

Configurações, permissões, feature flags. Tudo isso era buscado do banco a cada request. Coloquei um LRU cache em memória com TTL de 60 segundos. O banco passou a receber 1/100 das queries que recebia antes.

4. Streaming ao invés de carregar tudo na memória

Endpoint que retornava 50.000 registros estava fazendo o servidor consumir 2GB de RAM por request. Troquei por stream do PostgreSQL e a memória voltou pra 50MB. O processo parou de morrer com out of memory.

5. Cluster mode do Node

Node é single-threaded. Numa máquina com 8 cores, você está usando 1/8 do potencial se não rodar em cluster. Configurei PM2 em modo cluster e a capacidade de processamento simplesmente multiplicou por 8.

6. JSON.stringify customizado para respostas grandes

Sim, JSON.stringify pode ser gargalo. Pra respostas muito grandes, usei a lib fast-json-stringify com schema pré-compilado. 4x mais rápido que o nativo.

7. Compressão na saída

gzip nos responses grandes. Reduziu o tempo de transferência em 70% e o uso de banda do servidor caiu junto.

## Resultado final

A mesma máquina que trava com 100 req/s pode vir a servir 50.000 req/s com essas implementações.
