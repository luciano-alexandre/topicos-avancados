# Encontro 04 — Contexto, geração, limitações e seleção de modelos

## Tema

Janela de contexto, parâmetros de geração, variabilidade e limitações dos LLMs,
articulados à seleção responsável de modelos, licenças e model cards.

## Objetivos

- Explicar o que ocupa a janela de contexto e como administrar seu orçamento.
- Relacionar temperatura, top-k e top-p à variabilidade das respostas.
- Identificar alucinação, vieses e outras limitações relevantes.
- Diferenciar pesos abertos, código aberto, API pública e modelo proprietário.
- Reconhecer famílias, variantes e modelos especializados.
- Ler model cards e identificar evidências relevantes.
- Analisar licenças, limitações e adequação ao idioma e à tarefa.
- Produzir uma comparação técnica sem depender de rankings isolados.

## Visão geral

O resultado da tokenização estudada no Encontro 03 é apenas uma parte do
comportamento operacional. O contexto disponível, os parâmetros de geração e as
limitações do modelo também influenciam qualidade, latência e segurança.

Escolher um modelo porque ele é popular ou ocupa boa posição em um ranking não
é suficiente. A seleção precisa considerar tarefa, idioma, licença, memória,
latência, formato de distribuição, qualidade, riscos e manutenção.

## Janela de contexto

A janela de contexto é a quantidade máxima de tokens que o modelo consegue
considerar em uma inferência. Ela pode incluir:

- instrução de sistema;
- mensagem atual do usuário;
- histórico da conversa;
- exemplos few-shot;
- documentos recuperados;
- descrições e resultados de ferramentas;
- tokens da resposta, conforme a API e o modelo.

A **instrução de sistema**, frequentemente chamada de `system prompt`, define o
papel e as regras gerais que devem orientar o modelo. **Exemplos few-shot** são
pares de entrada e saída incluídos no contexto para demonstrar o comportamento
esperado antes da solicitação real.

```mermaid
flowchart LR
    S[System prompt] --> J[Janela de contexto]
    H[Histórico] --> J
    E[Exemplos] --> J
    D[Documentos] --> J
    U[Pedido atual] --> J
    J --> O[Resposta]
```

### Janela maior resolve tudo?

Não. Um contexto maior pode elevar consumo de memória e latência e inserir
conteúdo irrelevante ou conflitante. A aplicação deve selecionar o que realmente
ajuda a tarefa.

### Exemplo de orçamento

Suponha que uma aplicação estabeleça um orçamento conceitual de contexto:

| Parte | Reserva |
|---|---:|
| instruções e contrato | 15% |
| pergunta e histórico recente | 20% |
| documentos recuperados | 50% |
| margem para resposta | 15% |

Os percentuais não são universais. Eles tornam explícita a necessidade de
controlar o contexto em vez de enviar todo o conteúdo disponível.

```mermaid
pie showData
    title Exemplo de orçamento da janela de contexto
    "Instruções e contrato" : 15
    "Pergunta e histórico" : 20
    "Documentos recuperados" : 50
    "Margem para resposta" : 15
```

## Como o próximo token é escolhido

O modelo produz probabilidades para possíveis continuações. A estratégia de
decodificação decide como selecionar o próximo token.

### Temperatura

Controla, de modo geral, a dispersão da distribuição:

- valores menores tendem a favorecer saídas mais previsíveis;
- valores maiores tendem a aumentar diversidade e risco de variação;
- temperatura baixa não garante verdade nem determinismo absoluto.

### Top-k e top-p

- `top-k` restringe a seleção a um número de candidatos mais prováveis;
- `top-p` restringe a um conjunto cuja probabilidade acumulada atinge um limite;
- a disponibilidade e a interpretação exata dependem do servidor e do modelo.

```mermaid
flowchart LR
    L[Logits] --> P[Probabilidades]
    P --> K[Filtro top-k]
    P --> N[Filtro top-p]
    K --> T[Temperatura e amostragem]
    N --> T
    T --> O[Próximo token]
```

## Determinismo e variabilidade

Software tradicional costuma produzir a mesma saída para a mesma entrada e o
mesmo estado. Sistemas generativos podem variar.

```ts
function calcularMedia(a: number, b: number): number {
  return (a + b) / 2;
}
```

Para a mesma entrada, a função possui resultado definido. Já o pedido “explique
este conceito de modo simples” admite muitas respostas aceitáveis.

Isso afeta testes: nem sempre se deve comparar a resposta inteira com uma string
fixa. Pode ser melhor validar schema, presença de fatos, fontes, critérios de uma
rubrica ou métricas de qualidade.

Um **schema** é uma descrição formal da estrutura esperada para os dados. Em uma
resposta JSON, pode definir campos obrigatórios, tipos e valores permitidos,
possibilitando que o backend rejeite uma saída incompatível.

## Alucinação

Alucinação é a produção de conteúdo incorreto, não sustentado ou inventado, que
pode ser apresentado com fluência e confiança aparentes.

### Por que acontece?

O objetivo básico do modelo é gerar uma continuação plausível, não consultar
automaticamente a verdade. Contexto insuficiente, pergunta ambígua e padrões
aprendidos podem resultar em uma resposta plausível, porém falsa.

### Estratégias de redução

- fornecer contexto relevante e delimitado;
- exigir indicação das fontes utilizadas;
- permitir que o modelo declare insuficiência de informação;
- usar saída estruturada e validação;
- consultar ferramentas ou base de conhecimento;
- verificar fatos críticos com fonte independente;
- manter revisão humana em decisões importantes.

Nenhuma estratégia elimina completamente o problema.

```mermaid
flowchart LR
    Q[Pergunta] --> C{Contexto suficiente?}
    C -->|não| N[Declarar insuficiência]
    C -->|sim| G[Gerar resposta]
    G --> V{Schema e evidências válidos?}
    V -->|não| R[Rejeitar ou revisar]
    V -->|sim| H{Decisão crítica?}
    H -->|sim| U[Revisão humana]
    H -->|não| S[Entregar resposta]
    U --> S
```

## Outras limitações

### Conhecimento desatualizado ou ausente

O modelo pode não conhecer eventos recentes, dados privados ou regras locais.

### Sensibilidade à formulação

Pequenas alterações na instrução ou ordem do contexto podem mudar a resposta.

### Viés

Dados e escolhas do desenvolvimento podem reproduzir ou ampliar vieses.

### Falta de causalidade e compreensão garantida

Uma explicação coerente não prova que o modelo raciocinou como uma pessoa ou
compreendeu o domínio da maneira esperada.

### Prompt injection

Conteúdo recebido como dado pode tentar alterar instruções ou induzir o sistema
a revelar informação e usar ferramentas indevidamente.

## Modelos multimodais

Modelos multimodais recebem ou produzem mais de um tipo de informação, como
texto, imagem e áudio. A integração exige considerar:

- formatos e tamanho dos arquivos;
- custo de processamento;
- acessibilidade;
- dados pessoais presentes em imagem ou voz;
- validação específica para cada modalidade.

## Experimento guiado: variabilidade, instrução e contexto

Este experimento será realizado diretamente no chatbot gratuito disponível em
[gemini.google.com](https://gemini.google.com/). Não é necessário instalar um
modelo, criar projeto no Google Cloud, gerar chave de API ou ativar faturamento.

Se a interface oferecer um modelo ou recurso marcado como **Pro**, **Advanced**,
**Upgrade** ou equivalente, não o selecione. Permaneça no modelo padrão gratuito.

### Objetivo da atividade

Observar que a saída de um LLM pode variar quando o mesmo pedido é repetido,
verificar a sensibilidade à formulação da instrução e comparar uma resposta sem
evidências com outra apoiada em contexto delimitado. Ao final, o estudante
deverá justificar por que aplicações precisam validar conteúdo e estrutura em
vez de comparar a resposta inteira com uma string fixa.

### Limites do uso de um chatbot

A interface gratuita pode não informar o identificador técnico ou a versão do
modelo e não permite controlar temperatura, top-p, top-k ou seed. Esses valores
não serão estimados nem inventados. A ausência de tais informações deve ser
registrada como uma limitação do experimento.

O experimento não pretende comparar modelos nem medir o efeito da temperatura.
Ele controla apenas elementos disponíveis a qualquer estudante: histórico,
texto da instrução e contexto fornecido.

### Antes de começar

1. Acesse [gemini.google.com](https://gemini.google.com/) em um navegador.
2. Entre com uma conta Google pessoal, se a interface solicitar. Algumas funções
   básicas podem estar disponíveis sem autenticação.
3. Não aceite oferta de teste pago nem selecione **Upgrade**.
4. Use o modelo padrão gratuito apresentado pela interface.
5. Desative busca, pesquisa aprofundada, Canvas e outras ferramentas, quando a
   interface permitir.
6. Inicie uma conversa nova e confirme que ela não possui mensagens anteriores.
7. Registre data, horário, nome comercial exibido e navegador. Se a versão não
   aparecer, escreva **modelo não informado pela interface**.

Não envie dados pessoais, documentos institucionais ou informações sigilosas.

### Tabela de registro

Preencha uma linha imediatamente após cada execução.

| Execução | Parte | Instrução | Contexto | Comportamento observado | Limitação |
|---:|:---:|---|---|---|---|
| 1 | A | idêntica | nenhum | | |
| 2 | A | idêntica | nenhum | | |
| 3 | A | idêntica | nenhum | | |
| 4 | B | aberta | nenhum | | |
| 5 | B | estruturada | nenhum | | |
| 6 | C | pergunta | nenhum | | |
| 7 | C | pergunta | Política Zeta | | |

Em **Comportamento observado**, registre tamanho, argumentos, organização,
atendimento às restrições, eventual recusa e informação aparentemente
inventada. Em **Limitação**, registre versão desconhecida, ferramenta que não
pôde ser desativada ou qualquer condição que prejudique a comparação.

### Parte A — repetição em conversas independentes

O objetivo é observar variabilidade sem permitir que uma resposta anterior
influencie a seguinte.

1. Inicie uma conversa nova.
2. Envie exatamente:

```text
Explique em até quatro frases por que uma aplicação deve validar a saída de um
modelo de linguagem.
```

3. Registre a execução 1.
4. Inicie outra conversa nova. Não use **regenerar resposta**, pois isso pode
   preservar estado ou metadados da conversa.
5. Envie o mesmo texto, sem alterar palavras, espaços ou pontuação.
6. Registre a execução 2.
7. Repita todo o procedimento em uma terceira conversa e registre a execução 3.

Compare:

- argumentos e exemplos escolhidos;
- quantidade de frases;
- atendimento ao limite solicitado;
- vocabulário e ordem das ideias;
- afirmações que exigiriam verificação.

Uma resposta diferente não é necessariamente pior, e três respostas parecidas
não demonstram que o conteúdo esteja correto.

### Parte B — mudança controlada da instrução

Esta parte substitui o teste de temperatura, que não pode ser controlado de
forma confiável no chatbot gratuito.

#### Execução com instrução aberta

1. Inicie uma conversa nova.
2. Envie:

```text
Explique por que uma aplicação deve validar a saída de um modelo de linguagem.
```

3. Registre a execução 4, observando formato, extensão e aspectos escolhidos
   livremente pelo modelo.

#### Execução com instrução estruturada

1. Inicie outra conversa nova.
2. Envie:

```text
Explique por que uma aplicação deve validar a saída de um modelo de linguagem.
Responda em exatamente três itens numerados. Cada item deve conter uma frase e
tratar, respectivamente, de estrutura, fatos e segurança.
```

3. Registre a execução 5.
4. Compare as execuções 4 e 5 sem avaliar apenas qual texto parece mais bonito.
5. Verifique se a segunda resposta respeitou número, ordem, tamanho e temas.

A mudança observada não deve ser atribuída à temperatura: a variável alterada
foi a formulação da instrução.

### Parte C — ausência e fornecimento de contexto

#### Execução sem contexto

1. Inicie uma conversa nova.
2. Envie:

```text
Segundo a Política Acadêmica Zeta, em quantos dias um estudante pode solicitar
a revisão de uma atividade e qual formulário deve utilizar?
```

3. Registre a execução 6 e classifique o comportamento:

   - declarou não possuir a política;
   - pediu que o documento fosse fornecido;
   - respondeu apenas de forma genérica;
   - inventou prazo, formulário ou fonte.

#### Execução com contexto delimitado

1. Inicie outra conversa nova.
2. Envie exatamente:

```text
Responda somente com base no trecho delimitado. Se a resposta não estiver no
trecho, diga "informação não disponível no trecho".

<politica_zeta>
Art. 12. O estudante poderá solicitar revisão de atividade em até cinco dias
úteis após a publicação do resultado. A solicitação deverá ser registrada pelo
Formulário Z-2 no sistema acadêmico.
</politica_zeta>

Pergunta: em quantos dias o estudante pode solicitar a revisão e qual formulário
deve utilizar?
```

3. Registre a execução 7.
4. Marque quais palavras da resposta estão diretamente apoiadas pelo trecho.
5. Verifique se o chatbot acrescentou regras que não foram fornecidas.

### Análise individual

Responda por escrito:

1. O que permaneceu igual e o que variou nas três repetições?
2. Alguma resposta desrespeitou o limite de quatro frases?
3. Qual foi o efeito de tornar a instrução mais estruturada?
4. A resposta estruturada cumpriu todos os critérios verificáveis?
5. Como o chatbot respondeu quando a Política Zeta não foi fornecida?
6. Quais afirmações da execução 7 estão diretamente sustentadas pelo trecho?
7. Que validação poderia ser implementada pelo backend em cada parte?
8. Quais limitações impedem reproduzir exatamente o experimento no futuro?

### Produto da atividade

Entregue a tabela preenchida e as respostas às oito questões. Capturas de tela
são opcionais; quando usadas, devem ocultar conta, e-mail e outros dados
pessoais.

### Se o Gemini gratuito não estiver disponível

Utilize outro chatbot gratuito acessível e siga o mesmo roteiro. Registre o nome
do produto e escreva **modelo não informado** quando necessário. Não assine um
plano pago para cumprir a atividade.

Fontes de apoio: [acesso ao aplicativo Gemini](https://support.google.com/gemini/answer/13278668),
[limites dos níveis da API](https://ai.google.dev/gemini-api/docs/rate-limits)
e [faturamento do Gemini](https://ai.google.dev/gemini-api/docs/billing).

## Da compreensão do comportamento à seleção

Os conceitos anteriores oferecem critérios técnicos para comparar candidatos.
A segunda parte do encontro amplia a análise para disponibilidade dos artefatos,
licença, finalidade, evidências de avaliação e requisitos operacionais.

## Pergunta central

O que significa dizer que um modelo é “aberto” e quais evidências permitem
decidir se ele pode ser usado em uma aplicação real?

## Termos que não devem ser confundidos

```mermaid
flowchart TB
    M[Artefato de IA] --> C{O que está disponível?}
    C --> API[Apenas acesso por API]
    C --> P[Pesos do modelo]
    C --> COD[Código]
    C --> D[Dados e processo de treinamento]
    API --> L1[Termos do serviço]
    P --> L2[Licença dos pesos]
    COD --> L3[Licença do código]
    D --> L4[Origem e permissões]
```

A abertura não é binária: cada artefato possui disponibilidade, licença e
obrigações próprias.

### Modelo proprietário acessado por API

O usuário envia entradas a um serviço e recebe saídas, mas normalmente não
obtém os pesos nem executa o modelo em infraestrutura própria.

### Pesos abertos

Os arquivos com parâmetros treinados podem ser obtidos, sujeitos à licença. A
disponibilidade dos pesos não garante que dados, código de treinamento e todo o
processo sejam abertos.

Os **pesos** são os valores numéricos dos parâmetros aprendidos pelo modelo.
Quando um modelo é carregado para inferência, esses valores são colocados na
memória e usados nos cálculos que produzem a saída.

### Código aberto

O código é distribuído sob licença que permite uso, estudo, modificação e
redistribuição conforme suas condições. É necessário verificar separadamente a
licença do código, dos pesos e dos dados.

### Open source e open weights

Na prática, muitos modelos chamados informalmente de “open source” oferecem
pesos sob licenças próprias com restrições. A equipe deve registrar a licença
exata em vez de assumir liberdade irrestrita.

| Elemento | Pergunta de verificação |
|---|---|
| código | está disponível? sob qual licença? |
| pesos | podem ser baixados, modificados e redistribuídos? |
| dados | origem, permissão e composição são documentadas? |
| uso | há restrição comercial, de escala ou de finalidade? |
| derivados | podem ser distribuídos? quais avisos são obrigatórios? |

## Modelos fundacionais e especializados

### Modelo fundacional

É treinado de forma ampla e pode servir de base para diferentes tarefas. Pode
ser adaptado por prompting, fine-tuning ou outras técnicas.

**Fine-tuning** é um treinamento adicional realizado sobre um modelo já
treinado para adaptar seu comportamento a uma tarefa, domínio ou conjunto de
exemplos. Ele altera os parâmetros do modelo, diferentemente do prompting, que
apenas fornece instruções e contexto durante a inferência.

### Modelo de propósito geral

Busca atender tarefas variadas: conversa, resumo, extração, explicação e código.
Essa amplitude não garante que seja o melhor em um domínio específico.

### Modelo especializado

É otimizado ou ajustado para um domínio ou tarefa, como código, embeddings,
saúde ou um idioma. A especialização deve ser validada para o caso real.

### Modelo base e modelo instruction-tuned

- modelo base prevê continuações, sem necessariamente seguir pedidos de conversa;
- modelo ajustado a instruções foi adaptado para responder a comandos;
- uma aplicação de chat geralmente prefere uma variante preparada para instruções.

### Variações multimodais nas famílias

Processam mais de uma modalidade. É necessário confirmar exatamente quais
entradas e saídas a variante suporta; o nome de uma família não garante que
todas as versões sejam multimodais.

## Famílias e variantes

Uma família pode conter versões diferentes em:

- quantidade de parâmetros;
- arquitetura ou geração;
- tamanho da janela de contexto;
- variante base ou ajustada;
- idioma ou domínio;
- precisão numérica e quantização;
- suporte a ferramentas ou saída estruturada.

**Quantização** é a representação dos pesos com menor precisão numérica para
reduzir requisitos de memória e, em alguns ambientes, acelerar a execução. Ela
pode afetar a qualidade e será estudada de forma aplicada no Encontro 05.

O nome completo da variante precisa ser registrado. Comparar apenas o nome da
família pode levar a conclusões erradas.

```mermaid
flowchart TD
    F[Família] --> B[Modelo base]
    F --> I[Modelo de instruções]
    F --> C[Variante para código]
    I --> Q1[Quantização A]
    I --> Q2[Quantização B]
    I --> S[Servidor de inferência]
```

### Mapa para nomear uma variante sem ambiguidade

```mermaid
flowchart LR
    F[Família] --> G[Geração ou versão]
    G --> Z[Tamanho]
    Z --> A[Base ou instrução]
    A --> E[Especialização]
    E --> Q[Precisão ou quantização]
    Q --> X[Artefato exato a testar]
```

## O que é um model card?

Model card é um documento que descreve características, usos, avaliação e
limitações de um modelo. A qualidade varia entre projetos, portanto a ausência
de uma informação também é um dado relevante.

```mermaid
mindmap
  root((Model card))
    Identidade
      nome e versão
      autoria
    Permissão
      licença
      usos restritos
    Capacidade
      idiomas
      contexto
      modalidade
    Operação
      formato
      hardware
      template
    Evidências
      datasets
      benchmarks
      limitações
```

### Informações essenciais

1. nome, versão e autoria;
2. licença dos pesos;
3. arquitetura e tamanho;
4. contexto suportado;
5. idiomas e domínios;
6. formato e requisitos de execução;
7. usos pretendidos e usos desaconselhados;
8. datasets ou processo de treinamento, quando informados;
9. avaliações e benchmarks;
10. riscos, vieses e limitações;
11. template de prompt esperado;
12. histórico de atualização.

## Como ler benchmarks criticamente

Um número isolado não responde se o modelo funcionará no projeto.

```mermaid
flowchart LR
    B[Resultado publicado] --> T{Mesma tarefa?}
    T -->|não| X[Evidência fraca]
    T -->|sim| I{Mesmo idioma e domínio?}
    I -->|não| X
    I -->|sim| R{Configuração reproduzível?}
    R -->|não| X
    R -->|sim| E[Teste com dados locais]
    E --> D[Decisão contextualizada]
```

Pergunte:

- a tarefa medida é parecida com a aplicação?
- o idioma e o domínio são os mesmos?
- a configuração de inferência foi equivalente?
- houve contaminação entre treinamento e teste?
- o resultado foi reproduzido independentemente?
- a métrica mede correção, preferência ou apenas estilo?
- qual foi o custo de memória e latência?

### Benchmark não substitui avaliação local

O modelo escolhido deve ser testado com exemplos representativos do sistema.
Uma central de chamados em português possui vocabulário, categorias e erros
diferentes de um benchmark geral em inglês.

## Licenças: perguntas práticas

Antes de incorporar um modelo, registre:

- o uso educacional é permitido?
- o uso comercial é permitido?
- existe limite de usuários ou porte da organização?
- redistribuição dos pesos ou derivados é permitida?
- atribuição ou aviso é obrigatório?
- existem restrições de uso aceitável?
- a licença é compatível com o modo de distribuição do projeto?

Esta verificação não substitui análise jurídica. Ela evita que a equipe trate a
licença como detalhe posterior.

## Privacidade e execução local

Executar localmente pode evitar o envio do prompt a um provedor externo e
oferecer controle sobre versão e disponibilidade. Porém, ainda é preciso:

- proteger o endpoint do servidor de inferência;
- limitar logs e retenção;
- controlar quem acessa os dados;
- atualizar imagens e dependências;
- verificar a licença;
- avaliar respostas, vieses e ataques.

## Processo de seleção em camadas

```mermaid
flowchart TD
    A[Definir tarefa e critério de sucesso] --> B[Aplicar restrições eliminatórias]
    B --> C[Licença, idioma, modalidade e hardware]
    C --> D[Selecionar poucos candidatos]
    D --> E[Testar dataset local]
    E --> F[Medir qualidade, latência e memória]
    F --> G[Registrar decisão e fallback]
```

### Matriz visual de decisão

```mermaid
quadrantChart
    title Adequação à tarefa versus viabilidade operacional
    x-axis Baixa adequação --> Alta adequação
    y-axis Baixa viabilidade --> Alta viabilidade
    quadrant-1 Testar primeiro
    quadrant-2 Operável, mas pouco adequado
    quadrant-3 Descartar
    quadrant-4 Promissor, porém caro
    "Modelo A": [0.78, 0.72]
    "Modelo B": [0.42, 0.84]
    "Modelo C": [0.82, 0.30]
```

As posições são ilustrativas. A turma deve substituí-las por evidências e
critérios explícitos, nunca por preferência pessoal.

### Restrições eliminatórias

Um modelo pode ser descartado antes de benchmarks se:

- a licença impedir o uso pretendido;
- não executar no hardware disponível;
- não suportar o idioma ou a modalidade necessária;
- não aceitar o contexto mínimo;
- exigir envio de dados incompatível com a política de privacidade.

## Oficina: leitura comparativa de model cards

Em grupos de 3 ou 4 integrantes, analise duas variantes de modelos disponíveis
em catálogo público ou no ambiente local.

### Ficha de análise

| Critério | Modelo A | Modelo B | Evidência/fonte |
|---|---|---|---|
| nome e versão | | | |
| variante base/instrução | | | |
| licença | | | |
| parâmetros/tamanho | | | |
| contexto | | | |
| idiomas | | | |
| requisitos | | | |
| usos recomendados | | | |
| limitações | | | |
| avaliação relevante | | | |
| informação ausente | | | |

### Cenário de decisão

Considere uma aplicação acadêmica em português, executada localmente, que
classifica solicitações em dez categorias e deve responder em JSON. Com base
apenas em evidências documentadas:

1. qual modelo seria testado primeiro?
2. qual requisito eliminou ou enfraqueceu o outro candidato?
3. que informação ainda precisa ser confirmada por experimento?
4. qual risco precisa ser registrado?

### Produto

Um registro de decisão curto:

```text
Contexto:
Critérios obrigatórios:
Candidatos:
Evidências:
Decisão provisória:
Riscos e lacunas:
Próximo experimento:
```

## Erros comuns

### Escolher somente pelo número de parâmetros

Mais parâmetros podem aumentar requisitos sem garantir melhor resultado na
tarefa específica.

### Confiar apenas no nome da família

Variantes possuem finalidade, licença, contexto e quantização diferentes.

### Ignorar o template de conversa

Um formato incompatível pode degradar respostas mesmo quando o modelo é bom.

### Tratar benchmark como verdade universal

Resultados dependem de dataset, métrica, configuração e domínio.

### Confundir disponibilidade com permissão

Ser possível baixar um arquivo não significa que qualquer uso ou redistribuição
esteja autorizado.

## Questões para revisão

1. O que disputa espaço na janela de contexto?
2. Por que temperatura menor não garante uma resposta verdadeira?
3. Como alucinação e variabilidade afetam a validação da aplicação?
4. Qual a diferença entre pesos abertos e código aberto?
5. O que diferencia um modelo base de um modelo ajustado a instruções?
6. Por que a avaliação local continua necessária mesmo com bons benchmarks?
7. Quais evidências mínimas devem sustentar a seleção de um modelo?

## Checklist de aprendizagem

- [ ] explicar o orçamento de uma janela de contexto;
- [ ] relacionar parâmetros de geração à variabilidade;
- [ ] identificar alucinação e outras limitações;
- [ ] usar terminologia precisa ao falar de abertura;
- [ ] localizar licença e versão de uma variante;
- [ ] extrair informações essenciais de um model card;
- [ ] avaliar criticamente um benchmark;
- [ ] aplicar restrições antes de comparar qualidade;
- [ ] produzir uma decisão provisória baseada em evidências.

## Síntese do encontro

Compreender contexto, geração e limitações permite transformar características
do modelo em critérios de seleção. Model cards, licenças e avaliações oferecem
evidências, mas não dispensam experimentos com o domínio real. No próximo
encontro, essa análise será conectada aos requisitos de hardware, quantização e
desempenho.

## Preparação para o próximo encontro

Registrar CPU, memória RAM e GPU — quando disponível — de um equipamento que
poderia executar o ambiente da disciplina. Não publicar identificadores,
credenciais ou outras informações sensíveis.
