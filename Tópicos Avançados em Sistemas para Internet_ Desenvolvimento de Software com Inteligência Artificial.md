# Tópicos Avançados em Sistemas para Internet: Desenvolvimento de Software com Inteligência Artificial

## Ementa

Aplicação de técnicas de Inteligência Artificial no desenvolvimento de sistemas para Internet utilizando tecnologias open source. Integração de modelos de linguagem a aplicações Web desenvolvidas com TypeScript, Angular, NestJS e PostgreSQL. Execução local de modelos, engenharia de prompts, geração estruturada de dados, embeddings, busca semântica, bancos vetoriais, Retrieval-Augmented Generation (RAG), Tool Calling e agentes inteligentes. Aplicação de IA no ciclo de desenvolvimento de software. Avaliação, observabilidade, segurança, privacidade, desempenho, ética e uso responsável de sistemas baseados em Inteligência Artificial. Desenvolvimento de projeto final envolvendo a aplicação dos conteúdos estudados.

## Objetivo geral

Capacitar o estudante a projetar, implementar e avaliar funcionalidades de Inteligência Artificial integradas a sistemas Web, utilizando tecnologias open source e modelos de linguagem executados preferencialmente em infraestrutura própria.

A disciplina considera que o estudante já possui conhecimentos prévios de desenvolvimento Web, incluindo TypeScript, Angular, NestJS, APIs REST, bancos de dados relacionais, PostgreSQL, Git e arquitetura básica de aplicações.

O foco estará na utilização desses conhecimentos para construção de sistemas que incorporem recursos de Inteligência Artificial de maneira segura, eficiente e arquiteturalmente adequada.

## Tecnologias de referência

A disciplina adotará preferencialmente tecnologias open source, utilizando como stack de referência:

- **Linguagem:** TypeScript;
- **Front-end:** Angular;
- **Back-end:** NestJS;
- **Banco de dados:** PostgreSQL;
- **Armazenamento vetorial:** PostgreSQL com pgvector;
- **Execução local de modelos:** Ollama ou solução equivalente;
- **Containers:** Docker e Docker Compose;
- **Versionamento:** Git;
- **Integração:** APIs REST;
- **Modelos de IA:** modelos de linguagem abertos.

Angular, NestJS, PostgreSQL e demais tecnologias de desenvolvimento serão utilizados como infraestrutura para os experimentos e aplicações, não sendo objeto de ensino introdutório durante a disciplina.

# Conteúdo programático

## Unidade 1 – Inteligência Artificial Generativa aplicada a sistemas Web

- Inteligência Artificial Generativa;
- Modelos de linguagem de grande escala – LLMs;
- Tokens;
- Janelas de contexto;
- Inferência;
- Modelos multimodais;
- Modelos proprietários e modelos open source;
- Modelos fundacionais;
- Limitações dos modelos generativos;
- Alucinações;
- Determinismo e variabilidade das respostas;
- Aplicações de modelos de linguagem em sistemas Web;
- IA como componente da arquitetura de software.

O objetivo desta unidade é fornecer os conceitos necessários para que o estudante compreenda o funcionamento e as limitações dos modelos que serão integrados às aplicações.

## Unidade 2 – Modelos de linguagem open source

- Ecossistema de modelos de linguagem abertos;
- Famílias de modelos;
- Modelos especializados e modelos de propósito geral;
- Modelos pequenos e grandes;
- Requisitos computacionais;
- Quantização;
- Formatos de distribuição de modelos;
- Execução local;
- Ollama;
- Gerenciamento de modelos;
- Exposição de modelos por API;
- Seleção de modelos conforme o problema;
- Comparação entre modelos;
- Privacidade proporcionada pela execução local;
- Vantagens e limitações da utilização de modelos locais.

Serão priorizados modelos e ferramentas disponíveis como software aberto e que possam ser executados localmente sempre que a infraestrutura disponível permitir.

## Unidade 3 – Integração de modelos de IA com aplicações Web

- Integração entre aplicações e modelos de linguagem;
- Comunicação entre back-end e servidores de inferência;
- APIs de inferência;
- Requisições síncronas e assíncronas;
- Streaming de respostas;
- Controle de contexto;
- Histórico de conversação;
- Gerenciamento de sessões;
- Tratamento de falhas;
- Timeout;
- Retry;
- Fallback;
- Cache de respostas;
- Separação entre regras de negócio e decisões realizadas por modelos de IA.

Nesta unidade, NestJS e Angular serão utilizados exclusivamente para implementar e consumir funcionalidades de IA.

## Unidade 4 – Engenharia de prompts

- Estrutura de prompts;
- System Prompt;
- User Prompt;
- Contexto;
- Instruções;
- Restrições;
- Zero-shot prompting;
- Few-shot prompting;
- Exemplos;
- Templates;
- Contextualização dinâmica;
- Delimitadores;
- Controle do formato da resposta;
- Prompts parametrizados;
- Versionamento de prompts;
- Estratégias para redução de alucinações;
- Avaliação e comparação de prompts;
- Boas práticas para prompts utilizados em sistemas de produção.

Será enfatizada a diferença entre a utilização informal de prompts e a construção de prompts que fazem parte efetivamente da lógica de uma aplicação.

## Unidade 5 – Geração estruturada de dados

- Structured Output;
- Geração de JSON;
- Definição de schemas;
- Restrições de formato;
- Conversão de respostas para objetos TypeScript;
- Validação de respostas;
- Tratamento de respostas inválidas;
- Extração de entidades;
- Classificação de informações;
- Extração estruturada de dados a partir de textos;
- Integração dos resultados produzidos pela IA com regras de negócio;
- Persistência de informações extraídas.

Aplicações práticas poderão incluir:

- classificação automática de solicitações;
- extração de dados de documentos;
- identificação de categorias;
- geração de metadados;
- análise de textos;
- transformação de linguagem natural em estruturas utilizadas pelo sistema.

## Unidade 6 – Tool Calling e Function Calling

- Conceito de ferramentas utilizadas por modelos;
- Function Calling;
- Tool Calling;
- Definição de ferramentas;
- Descrição de parâmetros;
- Seleção de ferramentas pelo modelo;
- Execução controlada pelo back-end;
- Ferramentas para consulta a banco de dados;
- Ferramentas para acesso a APIs;
- Ferramentas para execução de regras de negócio;
- Encadeamento de ferramentas;
- Controle de permissões;
- Validação dos parâmetros produzidos pelo modelo;
- Tratamento de erros;
- Segurança na utilização de ferramentas.

Fluxo conceitual:

Usuário  
↓  
Aplicação  
↓  
Modelo de linguagem  
↓  
Seleção da ferramenta  
↓  
Serviço da aplicação  
↓  
Dados ou serviço externo  
↓  
Resultado  
↓  
Modelo  
↓  
Resposta final

Um aspecto central será compreender que o modelo pode decidir **qual ferramenta utilizar**, mas a execução efetiva deve permanecer sob controle da aplicação.

## Unidade 7 – Embeddings

- Conceito de embeddings;
- Representação vetorial de informações;
- Vetores semânticos;
- Dimensionalidade;
- Similaridade;
- Similaridade de cosseno;
- Distância entre vetores;
- Modelos de embeddings;
- Geração de embeddings localmente;
- Embeddings de textos;
- Embeddings de documentos;
- Embeddings de consultas;
- Casos de utilização.

Os estudantes deverão compreender como informações textuais podem ser transformadas em representações matemáticas utilizadas para comparação semântica.

## Unidade 8 – Busca semântica e armazenamento vetorial

- Busca por palavras-chave versus busca semântica;
- Bancos vetoriais;
- Armazenamento de embeddings;
- PostgreSQL com pgvector;
- Tipos vetoriais;
- Consultas por similaridade;
- Indexação vetorial;
- Estratégias de recuperação;
- Top-K;
- Filtros por metadados;
- Combinação de consultas relacionais e vetoriais;
- Busca híbrida;
- Avaliação da qualidade da recuperação.

O PostgreSQL será utilizado tanto como banco relacional da aplicação quanto como mecanismo de armazenamento e recuperação vetorial por meio da extensão pgvector.

## Unidade 9 – Retrieval-Augmented Generation – RAG

- Conceito de Retrieval-Augmented Generation;
- Limitações do conhecimento interno dos modelos;
- Bases de conhecimento próprias;
- Pipeline de RAG;
- Ingestão de documentos;
- Extração de conteúdo;
- Normalização;
- Chunking;
- Estratégias de divisão de documentos;
- Sobreposição entre segmentos;
- Metadados;
- Geração de embeddings;
- Indexação;
- Recuperação de contexto;
- Construção dinâmica de prompts;
- Geração da resposta;
- Indicação das fontes;
- Controle da quantidade de contexto;
- Reranking;
- Avaliação da qualidade do RAG.

Fluxo básico:

Documentos  
↓  
Extração  
↓  
Chunking  
↓  
Embeddings  
↓  
PostgreSQL + pgvector  
↓  
Busca semântica  
↓  
Seleção de contexto  
↓  
Modelo de linguagem  
↓  
Resposta

## Unidade 10 – RAG avançado e recuperação de informações

- Estratégias avançadas de chunking;
- Busca híbrida;
- Filtragem por metadados;
- Reranking;
- Reescrita de consultas;
- Expansão de consultas;
- Recuperação de múltiplas fontes;
- Contextualização de documentos;
- Controle da janela de contexto;
- RAG para bases extensas;
- RAG aplicado a documentos institucionais;
- RAG aplicado a sistemas corporativos;
- Avaliação de precisão e relevância da recuperação.

Esta unidade permitirá aprofundar problemas encontrados em implementações reais de RAG, evitando que o conteúdo se limite à implementação básica de um pipeline.

## Unidade 11 – Agentes de Inteligência Artificial

- Conceito de agente;
- Diferença entre chatbot, workflow e agente;
- Objetivos;
- Estado;
- Memória;
- Ferramentas;
- Planejamento;
- Tomada de decisão;
- Execução iterativa;
- Agentes orientados a tarefas;
- Agentes com acesso a APIs;
- Agentes com acesso a bancos de dados;
- Agentes especializados;
- Orquestração;
- Sistemas multiagentes;
- Supervisão humana;
- Limites de autonomia;
- Riscos associados a agentes.

Será enfatizada a utilização de agentes como componentes controlados dentro da arquitetura da aplicação, evitando delegar ao modelo decisões críticas que deveriam permanecer nas regras tradicionais do sistema.

## Unidade 12 – Workflows inteligentes

- Workflows tradicionais e workflows baseados em IA;
- Encadeamento de prompts;
- Pipelines de processamento;
- Decisões condicionais;
- Classificação e roteamento;
- Execução de múltiplos modelos;
- Processamento em etapas;
- Human-in-the-loop;
- Processamento assíncrono;
- Filas para processamento de tarefas de IA;
- Integração entre IA e processos de negócio;
- Orquestração de fluxos inteligentes.

O estudante deverá compreender situações em que um workflow determinístico é mais adequado do que um agente autônomo.

## Unidade 13 – IA aplicada ao ciclo de desenvolvimento de software

- Assistentes de programação;
- Geração de código;
- Explicação de código;
- Refatoração;
- Code Review assistido por IA;
- Identificação de problemas;
- Geração de testes;
- Geração de documentação;
- Análise de logs;
- Auxílio em debugging;
- Análise de requisitos;
- Geração de histórias de usuário;
- Geração de dados sintéticos para testes;
- Desenvolvimento orientado por IA;
- Limitações do código produzido por modelos;
- Validação humana;
- Uso responsável de IA no processo de desenvolvimento.

O foco não será ensinar programação utilizando IA, mas analisar como ferramentas baseadas em modelos podem aumentar a produtividade e apoiar diferentes etapas da Engenharia de Software.

## Unidade 14 – Avaliação de sistemas baseados em IA

- Qualidade das respostas;
- Relevância;
- Consistência;
- Correção factual;
- Groundedness;
- Avaliação de sistemas RAG;
- Casos de teste;
- Datasets de avaliação;
- Testes automatizados;
- Avaliação humana;
- LLM as Judge;
- Comparação entre modelos;
- Testes de regressão de prompts;
- Avaliação de alterações em prompts;
- Métricas para sistemas generativos.

O estudante deverá compreender que funcionalidades baseadas em IA exigem estratégias de teste diferentes das utilizadas exclusivamente em software determinístico.

## Unidade 15 – Observabilidade e desempenho

- Logs de interações com modelos;
- Monitoramento;
- Tracing;
- Latência;
- Tokens de entrada e saída;
- Uso de memória;
- Uso de CPU e GPU;
- Tempo de inferência;
- Monitoramento de modelos locais;
- Cache;
- Otimização de contexto;
- Otimização de prompts;
- Desempenho de consultas vetoriais;
- Identificação de gargalos;
- Rastreamento de chamadas a ferramentas;
- Observabilidade de pipelines RAG e agentes.

## Unidade 16 – Segurança em aplicações baseadas em IA

- Novas superfícies de ataque introduzidas pela IA;
- Prompt Injection;
- Indirect Prompt Injection;
- Manipulação de contexto;
- Jailbreak;
- Vazamento de informações;
- Exposição de prompts internos;
- Dados sensíveis;
- Controle de ferramentas;
- Validação de entradas;
- Validação das respostas;
- Sanitização;
- Controle de permissões;
- Princípio do menor privilégio;
- Segurança em RAG;
- Segurança de agentes;
- Proteção de bases de conhecimento;
- Isolamento de modelos executados localmente.

## Unidade 17 – Privacidade, ética e uso responsável

- Privacidade;
- Proteção de dados pessoais;
- Uso de dados para treinamento;
- Execução local e soberania dos dados;
- Viés;
- Transparência;
- Explicabilidade;
- Direitos autorais;
- Responsabilidade sobre conteúdo produzido;
- Supervisão humana;
- Impactos sociais;
- Uso acadêmico responsável;
- Limitações da automação baseada em IA.

# Avaliação

A composição da avaliação será:

- **30% – Atividades práticas;**
- **20% – Seminário e estudo de caso;**
- **50% – Projeto final.**
