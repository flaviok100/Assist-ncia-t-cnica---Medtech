# Assist-ncia-t-cnica---Medtech
Projeto da disciplina CCD410 - PERFORMANCE E TUNNING DE DADOS, 
Projeto MedTech – Polyglot Persistence
1. Explicação do tema escolhido
O projeto MedTech tem como objetivo desenvolver um sistema para gerenciar equipamentos médicos e suas respectivas manutenções técnicas.

O sistema permite cadastrar equipamentos, registrar manutenções, consultar informações e estabelecer relacionamentos entre equipamentos e manutenções realizadas.

A proposta é utilizar diferentes bancos de dados em uma mesma aplicação, escolhendo cada tecnologia de acordo com o tipo de informação que precisa ser armazenada.

O projeto utiliza o conceito de Polyglot Persistence, que consiste em utilizar diferentes modelos de bancos de dados dentro de uma mesma aplicação, aproveitando as características de cada tecnologia.
Funcionalidades principais
- Cadastro, consulta, alteração e exclusão de equipamentos.
- Cadastro, consulta, alteração e exclusão de manutenções.
- Associação entre equipamentos e manutenções.
- Visualização dos relacionamentos em um banco de dados orientado a grafos.

2. Justificativa dos bancos de dados utilizados
O MedTech utiliza três bancos de dados diferentes: PostgreSQL, MongoDB e Neo4j. Cada um foi escolhido considerando as características das informações armazenadas e a maneira como elas são utilizadas pelo sistema.
2.1 PostgreSQL – Banco de dados relacional
  O PostgreSQL foi escolhido para armazenar os equipamentos porque essas informações possuem uma estrutura bem definida, com campos e tipos de dados previamente estabelecidos.

A tabela equipamentos armazena informações como:
- ID do equipamento.
- Nome.
- Fabricante.
- Modelo.
- Número de série.
- Data de aquisição.
- Status.
Justificativa: o PostgreSQL oferece consistência dos dados, integridade referencial, suporte a transações e consultas SQL. Essas características são importantes para manter organizadas as informações cadastrais dos equipamentos médicos.
2.2 MongoDB – Banco de dados orientado a documentos
  O MongoDB foi escolhido para armazenar as manutenções porque esse tipo de informação pode apresentar diferentes detalhes, como descrição do serviço, técnico responsável, observações e tipo de manutenção.

A coleção manutencoes armazena informações como:
- Identificador da manutenção.
- ID do equipamento relacionado.
- Técnico responsável.
- Descrição do serviço.
- Data da manutenção.
- Tipo de manutenção.
- Status.
- Observações.
Justificativa: o MongoDB permite armazenar documentos com estrutura flexível, facilitando a inclusão de novos campos e informações específicas de cada manutenção sem exigir uma estrutura rígida de tabelas.
2.3 Neo4j – Banco de dados orientado a grafos
  O Neo4j foi escolhido para representar os relacionamentos entre os equipamentos cadastrados no PostgreSQL e as manutenções registradas no MongoDB.

No banco de grafos são utilizados:
- Nó Equipamento: representa um equipamento.
- Nó Manutencao: representa uma manutenção.
- Relacionamento TEVE_MANUTENCAO: conecta o equipamento à manutenção realizada.
Justificativa: o Neo4j é adequado para representar e consultar conexões entre diferentes informações. Ele facilita a identificação do histórico de manutenção de cada equipamento e a visualização das relações existentes.
2.4 Comparação dos bancos utilizados
Banco	Modelo	Informação armazenada
PostgreSQL	Relacional	Equipamentos
MongoDB	Documentos	Manutenções
Neo4j	Grafos	Relacionamentos entre equipamentos e manutenções

A utilização dos três bancos permite que cada tipo de informação seja armazenado no modelo mais adequado, demonstrando na prática o conceito de Polyglot Persistence.

3. Arquitetura e implementação do backend
O backend é responsável por receber as requisições do frontend, executar as operações necessárias nos bancos de dados e devolver as respostas para a interface.

No projeto MedTech, o backend foi desenvolvido utilizando Node.js e Express.
3.1 Arquitetura do projeto
Frontend (HTML, CSS, JavaScript)
              ↕
Backend (Node.js + Express)
       ↙      ↓      ↘
 PostgreSQL MongoDB Neo4j
3.2 Tecnologias utilizadas no backend
- Node.js: ambiente de execução JavaScript no servidor.
- Express: framework utilizado para criar as rotas da API.
- pg: biblioteca responsável pela comunicação com o PostgreSQL.
- mongodb: biblioteca utilizada para conectar e executar operações no MongoDB.
- neo4j-driver: biblioteca responsável pela conexão e execução de consultas no Neo4j.
- cors: permite a comunicação entre frontend e backend quando necessário.
- dotenv: permite carregar configurações e informações de conexão armazenadas no arquivo .env.
3.3 Funcionamento do backend
O backend foi organizado em rotas para permitir operações CRUD (Create, Read, Update e Delete).
Operação	Método HTTP	Finalidade
Criar	POST	Cadastrar novos registros
Consultar	GET	Buscar registros cadastrados
Atualizar	PUT	Modificar registros existentes
Excluir	DELETE	Remover registros

As principais rotas implementadas são:

Equipamentos – PostgreSQL
- /equipamentos
- /equipamentos/:id
Manutenções – MongoDB
- /manutencoes
- /manutencoes/:id
Relacionamentos – Neo4j
- /relacionamentos
- /relacionamentos/:equipamentoId/:manutencaoId
O backend também realiza validações para garantir que uma manutenção esteja associada ao equipamento correto antes de criar ou atualizar um relacionamento no Neo4j.
3.4 Exemplo de funcionamento integrado
Quando o usuário deseja relacionar um equipamento a uma manutenção, ocorre o seguinte processo:
1. O frontend envia os identificadores do equipamento e da manutenção.
2. O backend consulta o equipamento no PostgreSQL.
3. O backend consulta a manutenção no MongoDB.
4. O backend verifica se a manutenção pertence ao equipamento informado.
5. Se os dados forem válidos, o backend cria os nós e o relacionamento no Neo4j.
6. O frontend recebe a resposta e atualiza a tabela de relacionamentos.
Esse processo demonstra a integração entre os três bancos de dados por meio de um único backend.

4. Como executar o projeto
O projeto foi desenvolvido para execução local no Windows 11, utilizando o VS Code e os serviços dos três bancos de dados.
4.1 Programas e serviços necessários
Antes de executar o sistema, é necessário ter os seguintes programas instalados:
Programa/serviço	Finalidade
Node.js e npm	Executar o backend
PostgreSQL	Armazenar equipamentos
pgAdmin 4	Administrar e consultar o PostgreSQL
MongoDB Server	Armazenar manutenções
MongoDB Compass (opcional)	Visualizar documentos do MongoDB
Neo4j	Armazenar os relacionamentos
Neo4j Browser ou Neo4j Desktop	Administrar e consultar o banco de grafos
Visual Studio Code	Editar e executar o projeto

4.2 Estrutura de pastas
O projeto está organizado da seguinte maneira:
MedTech/
│
├── backend/
│   ├── database/
│   │   ├── postgres.js
│   │   ├── mongo.js
│   │   └── neo4j.js
│   │
│   ├── server.js
│   ├── package.json
│   ├── package-lock.json
│   ├── .env
│   └── node_modules/
│
└── frontend/
    ├── index.html
    ├── style.css
    └── script.js


Descrição dos principais arquivos:
- server.js: contém o servidor Express e as rotas da API.
- postgres.js: configura a conexão com o PostgreSQL.
- mongo.js: configura a conexão com o MongoDB.
- neo4j.js: configura a conexão com o Neo4j.
- .env: armazena as configurações de conexão e variáveis de ambiente.
- index.html: estrutura da interface.
- style.css: aparência visual do sistema.
- script.js: comunicação entre o frontend e o backend.
4.3 Passo a passo para iniciar o sistema
Etapa 1 – Iniciar o PostgreSQL
- Verificar se o serviço PostgreSQL está iniciado no Windows.
- Abrir o pgAdmin 4.
- Conectar ao servidor local.
- Confirmar que o banco Medtech está disponível.
Etapa 2 – Iniciar o MongoDB
- Verificar se o serviço MongoDB Server está iniciado.
- Confirmar que o MongoDB está disponível na porta 27017.
- O banco utilizado pelo projeto é MedTech, com a coleção manutencoes.
Para verificar a porta pelo PowerShell:
Test-NetConnection localhost -Port 27017

O resultado esperado é:
TcpTestSucceeded : True

Etapa 3 – Iniciar o Neo4j
- Abrir o Neo4j Desktop, caso esteja utilizando essa ferramenta.
- Iniciar a instância local do banco de dados.
- Verificar se a conexão Bolt está disponível na porta 7687.
- Abrir o Neo4j Browser para consultar os nós e relacionamentos.
Etapa 4 – Iniciar o backend

Abrir o projeto MedTech no Visual Studio Code.

No terminal, executar:
cd "C:\Users\leand\Desktop\FEI 2026\2º semestre\Tunning de Dados\Projeto\MedTech\backend"

Instalar as dependências (necessário na primeira configuração ou quando elas forem alteradas):
npm install

Iniciar o servidor:
npm run dev

O backend ficará disponível no endereço:
http://localhost:3000

O servidor também pode abrir o navegador automaticamente, conforme a configuração existente no server.js.

Etapa 5 – Acessar o frontend

Abrir o navegador e acessar:
Abrir o sistema MedTechA interface permitirá cadastrar equipamentos, registrar manutenções, criar relacionamentos e executar as operações disponíveis.
4.4 Ordem correta de inicialização
1. PostgreSQL
   
   Iniciar o serviço e verificar o banco Medtech.
2. MongoDB
   
   Iniciar o serviço e verificar a porta 27017.
3. Neo4j
   
   Iniciar a instância e verificar a conexão Bolt.
4. Backend Node.js
   
   Executar npm run dev na pasta backend.
5. Frontend
   
   Acessar http://localhost:3000/ pelo navegador.

5. Conclusão
O projeto MedTech demonstra a aplicação prática do conceito de Polyglot Persistence por meio da integração de três diferentes modelos de bancos de dados.

O PostgreSQL é utilizado para armazenar os dados estruturados dos equipamentos, o MongoDB armazena as informações flexíveis das manutenções e o Neo4j representa os relacionamentos entre esses dados.

A utilização do Node.js e do Express permite centralizar a comunicação entre o frontend e os bancos de dados, disponibilizando uma API responsável pelas operações de cadastro, consulta, atualização e exclusão.

Dessa forma, o projeto demonstra que diferentes bancos de dados podem trabalhar de maneira integrada em uma única aplicação, aproveitando as vantagens de cada tecnologia.
