# Encontro 13 — Experiência do usuário em produtos com IA

## Tema

Análise de como sistemas com IA devem comunicar capacidades, limites, incerteza e controle ao usuário, sem atividade de implementação.

## Objetivos

- Diferenciar uma interface tradicional de uma interação mediada por IA.
- Reconhecer expectativas inadequadas criadas pela linguagem da interface.
- Projetar experiências que permitam revisão, correção e desistência.
- Discutir transparência, confiança, acessibilidade e supervisão humana.
- Avaliar situações em que a IA não deveria tomar a decisão final.

## Por que UX para IA é diferente?

Em uma interface tradicional, a mesma entrada costuma produzir um comportamento previamente programado. Em sistemas com IA, a saída pode variar, conter erros plausíveis ou não atender exatamente ao pedido.

A experiência precisa preparar o usuário para essa característica. Não basta colocar um campo de texto e um botão “Gerar”.

## Princípios de interação

### Comunicar a função real

A interface deve explicar o que a IA faz naquele contexto. Expressões como “assistente inteligente” são vagas. É melhor declarar que o sistema sugere uma categoria, resume um chamado ou prepara um rascunho sujeito a revisão.

### Não criar aparência de certeza

Tom confiante não significa resultado correto. A interface deve distinguir resultado gerado, dado confirmado e decisão humana.

### Manter o usuário no controle

O usuário deve conseguir revisar, editar, rejeitar, solicitar nova tentativa e continuar sem utilizar a sugestão.

### Explicar limites relevantes

A aplicação deve informar, em linguagem adequada ao público, quando a resposta pode estar incompleta, quando depende apenas do texto informado e quando exige confirmação.

### Planejar a recuperação de falhas

Uma boa experiência prevê indisponibilidade, resposta inválida, demora, cancelamento e falta de informação. Mensagens genéricas como “algo deu errado” não ajudam o usuário a decidir o próximo passo.

## Níveis de participação da IA

| Nível | Papel da IA | Papel humano |
|---|---|---|
| apoio | organiza ou resume | decide integralmente |
| recomendação | sugere uma opção | revisa e confirma |
| execução supervisionada | prepara uma ação | autoriza antes da execução |
| automação limitada | executa casos previstos | acompanha exceções |
| decisão autônoma | decide e executa | audita posteriormente |

Quanto maior o impacto, maior deve ser a exigência de supervisão, explicação e possibilidade de contestação.

## Padrões úteis

- apresentar sugestões como sugestões;
- mostrar a entrada usada para produzir o resultado;
- permitir edição antes de salvar ou enviar;
- solicitar confirmação para ações com consequência;
- indicar quais informações estão ausentes;
- preservar o trabalho do usuário quando ocorrer erro;
- oferecer cancelamento em operações demoradas;
- diferenciar conteúdo original de conteúdo gerado;
- fornecer um caminho para revisão humana.

## Padrões problemáticos

- esconder que uma resposta foi gerada por IA;
- usar porcentagens de confiança sem significado validado;
- apresentar rascunho como decisão definitiva;
- induzir o usuário a aceitar a sugestão mais rapidamente;
- responsabilizar o usuário por erros que ele não pode detectar;
- impedir correção ou contestação;
- pedir dados desnecessários para a tarefa;
- utilizar linguagem humana para sugerir capacidades inexistentes.

## Discussão aplicada ao projeto

Para cada feature do Encontro 12, analise:

1. O resultado é sugestão, dado ou decisão?
2. Quem deve revisar?
3. O que acontece quando faltam informações?
4. Qual erro pode causar maior dano?
5. Como o usuário corrige o resultado?
6. A aplicação deixa claro o que veio do chamado e o que foi gerado?
7. É possível continuar sem aceitar a sugestão?
8. Quais dados não deveriam aparecer na interface?

## Estudos de situação

### Priorização

Uma prioridade alta pode reorganizar uma fila. A interface deve permitir revisão e não pode apresentar a classificação como fato absoluto.

### Rascunho de resposta

O texto pode parecer pronto, embora contenha promessa indevida. Ele deve permanecer identificado como rascunho até aprovação humana.

### Mascaramento

A ausência de detecção não prova ausência de dado sensível. A interface não deve declarar que o texto está “completamente seguro”.

### Informação ausente

Perguntas sugeridas precisam ser pertinentes e não podem solicitar segredos.

## Atividade de análise

Cada dupla deverá selecionar a feature recebida no Encontro 12 e produzir:

- descrição do usuário principal;
- objetivo do usuário;
- risco mais relevante;
- ponto em que a revisão humana é necessária;
- mensagem apresentada quando faltar informação;
- mensagem apresentada quando a IA falhar;
- forma de corrigir ou rejeitar o resultado;
- indicação visual de que o conteúdo foi gerado;
- um exemplo de padrão problemático a evitar.

A atividade é de análise e documentação. Não haverá implementação.

## Resultado esperado

Documento curto de experiência do usuário contendo fluxo principal, situações de falha, decisões de supervisão e justificativa das mensagens apresentadas.

## Checklist

- [ ] a função da IA está clara;
- [ ] sugestão e decisão não são confundidas;
- [ ] há possibilidade de revisão e correção;
- [ ] falta de informação foi considerada;
- [ ] falhas possuem mensagens úteis;
- [ ] ações relevantes exigem confirmação;
- [ ] dados sensíveis foram considerados;
- [ ] limitações não foram escondidas;
- [ ] acessibilidade e linguagem foram observadas.

## Síntese

Em produtos com IA, a interface também é uma camada de segurança. Ela molda expectativas, preserva controle e impede que uma saída provável seja confundida com uma decisão confirmada.
