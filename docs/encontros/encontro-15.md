# Encontro 15 — Apresentação das features desenvolvidas

## Modalidade

Apresentação em duplas das features definidas no Encontro 12. Não haverá novo conteúdo de implementação neste encontro.

## Objetivos

- Demonstrar a feature funcionando sobre o projeto-base.
- Explicar o problema atendido e o contrato adotado.
- Apresentar evidências de validação e testes.
- Relacionar a implementação às análises de experiência e arquitetura.
- Compartilhar limitações, erros encontrados e decisões tomadas.

## Features apresentadas

1. Priorização de chamados.
2. Geração de título e resumo.
3. Sugestão de resposta para o solicitante.
4. Identificação de informações ausentes.
5. Detecção e mascaramento de dados sensíveis.

Se mais de uma dupla tiver recebido a mesma feature, as apresentações deverão destacar diferenças de contrato, critérios, testes e resultados.

## Conteúdo obrigatório da apresentação

Cada dupla deverá apresentar:

1. necessidade atendida;
2. comportamento esperado;
3. contrato de entrada e saída;
4. demonstração funcional;
5. validações realizadas pelo backend;
6. teste sem o modelo real;
7. teste com o Ollama;
8. cenário normal;
9. cenário de fronteira;
10. resposta inválida rejeitada;
11. limitação conhecida;
12. decisão de UX discutida no Encontro 13;
13. decisão arquitetural discutida no Encontro 14;
14. contribuição de cada integrante.

## Roteiro sugerido

### Contexto

Explique o problema em linguagem de produto, sem iniciar pela estrutura interna do código.

### Contrato

Mostre o que a feature recebe, o que devolve e quais valores são rejeitados.

### Demonstração

Execute a aplicação pelo Docker Compose e apresente pelo menos um caso bem-sucedido e um caso de falha controlada.

### Evidências

Apresente testes, resultados observados e critérios de aceite atendidos.

### Decisões

Explique uma escolha relevante, uma alternativa descartada e um compromisso assumido.

### Limitações

Declare o que a feature não resolve e em quais situações exige revisão humana.

## Regras

- os dois integrantes devem participar;
- a demonstração deve utilizar a entrega da própria dupla;
- dados pessoais reais não podem ser utilizados;
- falhas durante a apresentação devem ser analisadas, não escondidas;
- slides não substituem a demonstração;
- a dupla não deve apresentar a saída da IA como verdade garantida;
- dependência de outra feature caracteriza descumprimento do requisito de independência;
- credenciais, segredos e arquivos de ambiente não podem ser exibidos.

## Critérios de avaliação

| Critério | Peso |
|---|---:|
| funcionamento e aderência à feature | 25% |
| demonstração e evidências | 20% |
| validação e tratamento de falhas | 20% |
| justificativa das decisões | 15% |
| análise de limitações e supervisão humana | 10% |
| clareza e participação da dupla | 10% |

## Registro da turma

| Dupla | Feature | Resultado | Ponto forte | Limitação principal |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

## Perguntas para discussão

Após cada apresentação, a turma deverá considerar:

- a feature usa IA em uma parte justificável?
- o contrato permite reconhecer respostas inválidas?
- qual erro teria maior impacto?
- a interface comunica que o resultado foi gerado?
- a revisão humana está posicionada corretamente?
- a solução continuaria útil diante de uma falha do modelo?
- os testes apresentados cobrem situações adversariais?

## Entrega final da dupla

A entrega deverá conter:

- código apresentado;
- README atualizado;
- evidências dos testes;
- resultados dos cenários obrigatórios;
- análise de UX;
- decisão arquitetural;
- limitações conhecidas;
- identificação dos integrantes.

## Resultado esperado

Ao final do encontro, a turma terá comparado cinco formas independentes de ampliar o mesmo sistema com IA, observando diferenças de contrato, risco, experiência do usuário e decisões arquiteturais.
