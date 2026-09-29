# Encontro 14 — Decisões arquiteturais em sistemas com IA

## Tema

Análise de decisões, limites e trade-offs arquiteturais em aplicações que incorporam modelos de IA, sem atividade de implementação.

## Objetivos

- Identificar quando uma regra determinística é suficiente.
- Comparar modelo local, serviço externo e abordagem híbrida.
- Relacionar qualidade, custo, latência, privacidade e operação.
- Reconhecer fronteiras de confiança em uma arquitetura com IA.
- Avaliar o papel de validação, supervisão e observabilidade.
- Justificar decisões técnicas sem depender de modismos.

## A primeira decisão: a tarefa precisa de IA?

IA pode ser adequada quando a entrada é não estruturada, há variação linguística ou o resultado esperado envolve síntese, classificação contextual ou geração.

Uma regra tradicional tende a ser melhor quando:

- o comportamento precisa ser exato e previsível;
- todas as condições podem ser expressas claramente;
- erros possuem consequência elevada;
- auditoria exige uma explicação determinística;
- custo e latência de inferência não se justificam;
- a tarefa pode ser resolvida com consulta ou cálculo convencional.

Usar IA onde uma enumeração, expressão regular ou consulta resolve o problema aumenta complexidade sem benefício necessário.

## Dimensões de decisão

| Dimensão | Pergunta |
|---|---|
| qualidade | o resultado atende aos casos relevantes? |
| latência | quanto o usuário pode esperar? |
| custo | qual é o custo por uso e por operação? |
| privacidade | quais dados deixam a aplicação? |
| disponibilidade | o sistema funciona quando o modelo falha? |
| controle | a saída pode ser validada? |
| atualização | quais informações precisam ser atuais? |
| operação | a equipe consegue manter a solução? |
| escala | hardware e serviço suportam a demanda? |
| governança | quem responde pela decisão? |

Não existe escolha melhor em todas as dimensões. Arquitetura é uma composição explícita de compromissos.

## Modelo local, serviço externo e abordagem híbrida

### Modelo local

Pode favorecer controle de dados e independência de um serviço externo, mas exige capacidade computacional, atualização, monitoramento e conhecimento operacional.

### Serviço externo

Pode oferecer modelos mais capazes e menor esforço de infraestrutura, mas introduz dependência de rede, custo variável, limites de uso e análise cuidadosa de privacidade.

### Abordagem híbrida

Pode encaminhar tarefas conforme sensibilidade ou complexidade, mas aumenta regras de roteamento, testes, observabilidade e risco de comportamento diferente entre modelos.

## Fronteiras de confiança

Considere como não confiáveis:

- entrada do usuário;
- documentos recuperados;
- conteúdo gerado pelo modelo;
- argumentos sugeridos para ferramentas;
- dados vindos de integrações externas.

Validação de formato não confirma veracidade. Autorização não deve ser delegada ao modelo. Dados sensíveis não devem ser enviados apenas porque cabem no contexto.

## Onde cada responsabilidade deve ficar?

| Responsabilidade | Dono principal |
|---|---|
| regra de negócio | aplicação |
| autenticação e autorização | aplicação |
| validação de entrada e saída | aplicação |
| geração ou interpretação linguística | modelo |
| persistência | camada de dados |
| seleção de fontes autorizadas | aplicação |
| confirmação de ação crítica | pessoa ou regra explícita |
| registro de métricas | infraestrutura e aplicação |

O modelo participa do fluxo, mas não substitui as demais camadas.

## Acoplamento ao fornecedor

Uma aplicação fica fortemente acoplada quando regras de negócio dependem de nomes, campos e comportamentos exclusivos de um provedor.

Questões para análise:

- o contrato interno descreve a necessidade da aplicação?
- a troca de modelo exige alterar controllers e regras de negócio?
- parâmetros externos aparecem no contrato público?
- testes conseguem substituir o modelo?
- a aplicação conhece a diferença entre indisponibilidade e resposta inválida?

Abstração em excesso também tem custo. Ela deve proteger uma variação real, não apenas uma possibilidade imaginada.

## Custo total

O preço de inferência ou o custo do hardware é apenas uma parte. Considere:

- desenvolvimento e manutenção;
- criação e revisão de datasets;
- avaliação contínua;
- observabilidade;
- armazenamento de entradas e resultados;
- revisão humana;
- incidentes e correções;
- atualização de modelos;
- adequação jurídica e de privacidade.

Uma solução aparentemente barata pode transferir custo para revisão manual ou operação.

## Decisões reversíveis e irreversíveis

Decisões reversíveis podem ser experimentadas com menor risco, como alterar um prompt ou comparar modelos em ambiente controlado.

Decisões de difícil reversão incluem armazenar grandes volumes de dados pessoais, tornar um fornecedor parte do contrato público ou automatizar uma decisão crítica sem mecanismo de contestação.

Quanto menos reversível a decisão, maior deve ser a evidência exigida.

## Análise do projeto da disciplina

Cada dupla deverá analisar sua feature do Encontro 12 respondendo:

1. A tarefa realmente necessita de IA?
2. Qual parte poderia ser determinística?
3. Qual é o maior risco de uma resposta incorreta?
4. Quais dados entram e quais saem?
5. Onde a validação deve ocorrer?
6. Quem confirma o resultado?
7. O sistema ainda oferece valor se o modelo estiver indisponível?
8. Qual dimensão é prioritária: qualidade, custo, latência ou privacidade?
9. O que precisaria ser medido em produção?
10. Qual decisão seria mais difícil de reverter?

## Matriz de decisão

A dupla deverá preencher uma matriz qualitativa:

| Alternativa | Qualidade | Latência | Privacidade | Custo | Operação | Risco |
|---|---|---|---|---|---|---|
| regra determinística |  |  |  |  |  |  |
| modelo local |  |  |  |  |  |  |
| serviço externo |  |  |  |  |  |  |
| solução híbrida |  |  |  |  |  |  |

As classificações devem ser justificadas. Não existe obrigação de escolher IA como alternativa final.

## Atividade de análise

Produza uma decisão arquitetural curta contendo:

- contexto e problema;
- alternativas consideradas;
- critérios utilizados;
- alternativa escolhida;
- vantagens aceitas;
- custos e riscos aceitos;
- consequências esperadas;
- condição que justificaria revisar a decisão.

A atividade é conceitual. Não haverá implementação ou alteração do projeto.

## Checklist

- [ ] alternativas sem IA foram consideradas;
- [ ] critérios estão explícitos;
- [ ] dados e fronteiras de confiança foram identificados;
- [ ] autorização permanece fora do modelo;
- [ ] custo total foi considerado;
- [ ] supervisão humana foi posicionada;
- [ ] indisponibilidade foi discutida;
- [ ] decisão e trade-offs foram justificados;
- [ ] condições de revisão foram registradas.

## Síntese

Uma arquitetura com IA não é definida apenas pelo modelo escolhido. Ela resulta da distribuição de responsabilidades, das fronteiras de confiança e dos compromissos assumidos entre qualidade, custo, latência, privacidade e operação.
