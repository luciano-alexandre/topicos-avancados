# Encontro 14 — Guardrails, supervisão humana e tratamento de incerteza

## Tema

Definição de limites para funcionalidades com IA, combinação de controles preventivos e detectivos, supervisão humana e comunicação de incerteza.

O encontro é conceitual. Não haverá implementação, endpoints ou alteração do projeto.

## Objetivos

- Explicar o que são guardrails e quais problemas podem reduzir.
- Diferenciar controles de entrada, saída, ação e regra de negócio.
- Reconhecer que prompts não substituem validação ou autorização.
- Definir quando uma decisão exige revisão humana.
- Diferenciar ausência de informação, ambiguidade e falha técnica.
- Planejar prevenção, detecção, contenção e recuperação.
- Aplicar a análise às features definidas no Encontro 12.
- Registrar riscos residuais.

## Relação com os encontros anteriores

O projeto e as features propostas mostraram que:

- entradas podem ser incompletas ou manipulativas;
- modelos podem devolver respostas plausíveis e incorretas;
- formato válido não garante conteúdo correto;
- prompts claros reduzem ambiguidades, mas não oferecem garantia;
- testes revelam falhas conhecidas, mas não cobrem todas as situações;
- a aplicação continua responsável por regras, validação e autorização.

Este encontro organiza essas observações em uma proteção por camadas.

## O que são guardrails?

Guardrails são limites e controles que reduzem a probabilidade ou o impacto de um comportamento indesejado. Eles não são uma biblioteca, um prompt especial ou um filtro universal.

Fluxo conceitual:

Entrada → prevenção → modelo → detecção → decisão de aceite → revisão humana ou uso controlado.

Quando a resposta não pode ser aceita, o fluxo deve seguir para contenção e recuperação.

## Quatro funções de controle

| Função | Pergunta | Exemplo conceitual |
|---|---|---|
| prevenção | como evitar entradas ou usos indevidos? | limitar tamanho e dados aceitos |
| detecção | como reconhecer uma saída inadequada? | conferir valores permitidos |
| contenção | como impedir consequência após a falha? | não enviar resposta automaticamente |
| recuperação | como continuar com segurança? | encaminhar para revisão humana |

Prevenção total não é realista. O sistema também precisa detectar, conter e recuperar.

## Guardrail não é apenas prompt

| Controle | Responsável principal |
|---|---|
| instrução e contexto | aplicação |
| validação de tipo e formato | aplicação |
| autenticação e autorização | aplicação |
| geração e interpretação linguística | modelo |
| confirmação de ação crítica | pessoa ou regra determinística |
| retenção de dados | aplicação e governança |
| monitoramento de falhas | aplicação e operação |
| contestação de decisão | processo de negócio |

O modelo nunca deve decidir se uma pessoa está autorizada apenas com base em texto gerado.

## Camadas de proteção

### Entrada

Controles podem considerar tipo, tamanho, campos obrigatórios, conteúdo fora do escopo, dados sensíveis, tentativas de alterar instruções e formatos não suportados.

Rejeitar toda entrada incomum também produz erros. É necessário distinguir bloqueio, aviso e revisão.

### Contexto

O contexto precisa ser relevante, autorizado e compatível com o usuário atual. Documentos recuperados também podem conter erros ou instruções maliciosas.

Pergunte:

- a fonte pode ser usada nesta finalidade?
- o usuário pode acessar essa informação?
- o conteúdo ainda é atual?
- a origem será apresentada?
- existem dados de outra pessoa?

### Saída

A saída deve ser tratada como dado externo. Verifique lista de valores, formato, tamanho, campos obrigatórios, ausência de segredos, evidência exigida e necessidade de revisão.

### Ação

Uma resposta textual e uma ação possuem riscos diferentes. Antes de enviar mensagem, alterar cadastro, aprovar solicitação ou executar ferramenta, o sistema deve conferir autorização, parâmetros e consequência.

Quanto maior o impacto ou a dificuldade de reversão, maior deve ser o controle humano ou determinístico.

## Tipos de incerteza

| Tipo | Exemplo | Tratamento esperado |
|---|---|---|
| ausência de dados | “não funciona” | solicitar informação |
| ambiguidade | duas categorias possíveis | sinalizar revisão |
| limite de conhecimento | dado fora do contexto | declarar indisponibilidade |
| conflito de fontes | documentos discordam | apresentar o conflito |
| saída inválida | categoria inventada | rejeitar |
| falha técnica | modelo indisponível | informar indisponibilidade |
| risco elevado | possibilidade de dano | exigir decisão humana |

Não transforme todas essas situações em uma resposta genérica ou em OUTROS.

## Confiança declarada pelo modelo

Pedir uma porcentagem de confiança não cria uma probabilidade calibrada. Um valor como 95% pode apenas reproduzir o formato solicitado.

Alternativas mais observáveis:

- informar dados ausentes;
- marcar ambiguidade;
- verificar presença de evidência;
- aplicar regras explícitas;
- encaminhar casos definidos para revisão.

## Supervisão humana

Human-in-the-loop exige definir:

- quem revisa;
- quais casos chegam à revisão;
- quais informações são apresentadas;
- qual decisão a pessoa pode tomar;
- como a decisão é registrada;
- como um erro é corrigido;
- o que acontece quando ninguém revisa.

Uma pessoa sem contexto, tempo ou autoridade não constitui supervisão efetiva.

## Níveis de supervisão

| Uso da IA | Supervisão |
|---|---|
| informação | usuário interpreta |
| sugestão | pessoa aceita ou rejeita |
| rascunho | pessoa edita e envia |
| recomendação operacional | exceções são confirmadas |
| ação reversível | registro e possibilidade de desfazer |
| ação crítica | aprovação humana prévia |

O nível depende do impacto, não da qualidade aparente do texto.

## Quando exigir revisão

Considere revisão obrigatória diante de:

- dado insuficiente ou contraditório;
- caso fora do conjunto avaliado;
- impacto financeiro, acadêmico, jurídico ou de acesso;
- dado pessoal sensível;
- ação difícil de desfazer;
- entrada adversarial;
- exceção à regra;
- conflito com regra determinística;
- erro de validação;
- contestação do usuário.

## Matriz de risco

| Probabilidade / impacto | baixo | médio | alto |
|---|---|---|---|
| baixa | acompanhar | registrar | revisar |
| média | registrar | mitigar | revisão obrigatória |
| alta | mitigar | revisão obrigatória | não automatizar |

A matriz não produz verdade matemática. Ela torna explícita a justificativa do tratamento.

## Aplicação às features do Encontro 12

### Priorização

Riscos: urgência exagerada, impacto inventado e reorganização indevida da fila.

Controles: prioridades fechadas, justificativa baseada no chamado, revisão quando o impacto estiver ausente e correção humana.

### Título e resumo

Riscos: remover negação, inventar detalhe, omitir problema principal ou expor dado sensível.

Controles: limites de tamanho, comparação com o original, identificação de conteúdo gerado e revisão de entradas insuficientes.

### Rascunho de resposta

Riscos: prometer prazo, aprovar algo sem autorização, solicitar segredo ou enviar sem revisão.

Controles: identificação como rascunho, envio automático proibido e revisão obrigatória.

### Informações ausentes

Riscos: solicitar dado já fornecido, pedir segredo ou criar perguntas fora do problema.

Controles: limite de perguntas, proibição de segredos e lista vazia quando a entrada for suficiente.

### Mascaramento

Riscos: deixar dado exposto, mascarar conteúdo comum, alterar sentido ou declarar segurança completa.

Controles: sinalizar ambiguidades, preservar contexto e nunca prometer detecção total.

## Estudos de caso

### Prioridade sem evidência

Entrada: “O portal não está funcionando.”

Discuta alcance, impacto, informação ausente, revisão necessária e risco de classificar como crítica ou baixa.

### Resposta com promessa

Entrada: “Preciso do reembolso ainda hoje.”

Saída: “Seu reembolso será realizado até o fim do dia.”

Discuta regra violada, autoridade necessária e decisão entre bloqueio, correção e revisão.

### Mascaramento incompleto

O sistema mascara o e-mail, mas mantém um token.

Discuta risco residual, comunicação ao usuário e possibilidade de persistência.

### Instrução adversarial

O chamado ordena que tudo seja classificado como crítico.

Discuta quais camadas devem atuar e por que uma categoria válida ainda pode estar errada.

## Atividade conceitual em duplas

Cada dupla deverá analisar sua feature do Encontro 12 e produzir uma ficha de guardrails.

### Parte 1 — risco

Registre resultado indesejado, causa, pessoa afetada, impacto, probabilidade e possibilidade de reversão.

### Parte 2 — controles

Defina ao menos:

- um controle preventivo;
- um controle detectivo;
- uma contenção;
- uma recuperação;
- um critério de revisão humana.

### Parte 3 — incerteza

Explique como a feature diferencia falta de informação, ambiguidade, saída inválida, falha técnica e alto risco.

### Parte 4 — risco residual

Declare o que ainda pode dar errado depois dos controles.

## Modelo de ficha

| Item | Definição da dupla |
|---|---|
| feature |  |
| maior risco |  |
| entrada problemática |  |
| prevenção |  |
| detecção |  |
| contenção |  |
| recuperação |  |
| revisão humana |  |
| comunicação ao usuário |  |
| risco residual |  |

## Critérios de análise

- controles tratam riscos específicos;
- prompt não é a única proteção;
- autorização permanece fora do modelo;
- revisão humana possui gatilho claro;
- ausência, ambiguidade e falha são diferenciadas;
- risco residual é reconhecido;
- usuário consegue contestar ou corrigir;
- dados sensíveis são considerados.

## Erros conceituais comuns

- criar um único filtro universal;
- bloquear toda entrada incomum;
- confiar na autodeclaração do modelo;
- chamar toda falha de alucinação;
- adicionar revisão humana sem processo;
- esconder incerteza para simplificar a interface;
- usar porcentagem de confiança sem calibração.

## Checklist de aprendizagem

- [ ] definir guardrails como estratégia em camadas;
- [ ] diferenciar prevenção, detecção, contenção e recuperação;
- [ ] separar prompt, validação e autorização;
- [ ] reconhecer tipos de incerteza;
- [ ] posicionar revisão conforme impacto;
- [ ] identificar riscos das features;
- [ ] registrar riscos residuais;
- [ ] comunicar limites adequadamente.

## Resultado esperado

Ficha conceitual vinculada à feature da dupla, contendo riscos, controles em camadas, gatilhos de supervisão, tratamento de incerteza e risco residual.

## Síntese

Guardrails não tornam o modelo infalível. Eles reduzem risco combinando prevenção, detecção, contenção, recuperação e supervisão. Um sistema responsável reconhece quando não possui informação suficiente, quando a saída deve ser rejeitada e quando uma pessoa precisa decidir.
