# Encontro 15 — Seleção de modelos para aplicações com IA

## Tema

Critérios para selecionar modelos considerando qualidade, custo, latência, privacidade, requisitos operacionais e condições de uso.

O encontro é conceitual e não exige implementação.

## Objetivos

- Evitar escolhas baseadas apenas em popularidade ou tamanho.
- Relacionar capacidade do modelo à tarefa da aplicação.
- Comparar execução local, serviço externo e abordagem híbrida.
- Considerar idioma, contexto, hardware e privacidade.
- Identificar restrições de licença e condições de uso.
- Definir avaliação baseada em casos do projeto.

## A pergunta correta

Não pergunte apenas “qual é o melhor modelo?”. Pergunte qual modelo atende a determinada tarefa, ambiente e conjunto de limites.

Um modelo mais capaz pode ser inviável por custo, latência ou memória. Um modelo menor pode atender classificação e ser inadequado para análise extensa.

## Critérios de seleção

| Critério | Pergunta |
|---|---|
| qualidade | acerta os casos relevantes? |
| idioma | compreende o português utilizado? |
| formato | respeita o contrato esperado? |
| contexto | suporta a entrada necessária? |
| latência | responde no prazo aceitável? |
| custo | cabe no volume previsto? |
| hardware | funciona na infraestrutura? |
| privacidade | os dados podem ir para esse ambiente? |
| licença | o uso pretendido é permitido? |
| operação | a equipe consegue manter e atualizar? |

## Evidências úteis

Considere documentação oficial, model card, licença, requisitos de hardware, limites de contexto, avaliações públicas relevantes, dataset próprio e resultados no ambiente real.

Ranking isolado, marketing ou opinião sem contexto não devem sustentar a decisão.

## Local, externo ou híbrido

### Local

Favorece controle de dados, mas exige hardware, atualização e operação.

### Externo

Pode oferecer maior capacidade, mas envolve rede, custo variável, retenção de dados e dependência do fornecedor.

### Híbrido

Pode separar tarefas por sensibilidade ou dificuldade, mas aumenta complexidade e necessidade de comparação.

## Licenciamento

Verifique permissão de uso, redistribuição, atribuição, modelos derivados, políticas aceitáveis e origem da informação. Disponibilidade para download não significa liberdade para qualquer uso.

## Matriz de decisão

Cada dupla deverá comparar três alternativas para sua feature:

| Critério | Peso | Alternativa A | Alternativa B | Alternativa C |
|---|---:|---:|---:|---:|
| qualidade na tarefa |  |  |  |  |
| português |  |  |  |  |
| formato |  |  |  |  |
| latência |  |  |  |  |
| custo |  |  |  |  |
| privacidade |  |  |  |  |
| hardware |  |  |  |  |
| licença |  |  |  |  |
| operação |  |  |  |  |

Os pesos devem refletir a feature. Mascaramento pode priorizar privacidade; rascunho pode exigir qualidade linguística; priorização pode exigir consistência e baixa latência.

## Atividade conceitual

Produza uma recomendação contendo:

- requisitos prioritários;
- três alternativas;
- evidências consultadas;
- matriz preenchida;
- alternativa recomendada;
- riscos e limitações;
- condição para revisar a decisão.

Não é necessário instalar, baixar ou integrar novos modelos.

## Erros comuns

- escolher pelo maior número de parâmetros;
- confiar em benchmark sem relação com a tarefa;
- ignorar idioma e entradas reais;
- comparar com prompts diferentes;
- esquecer custo do modelo local;
- desconsiderar licença e retenção;
- tratar uma avaliação como garantia permanente;
- acoplar o contrato público ao fornecedor.

## Checklist

- [ ] tarefa definida;
- [ ] critérios e pesos justificados;
- [ ] alternativas locais e externas consideradas;
- [ ] privacidade e licença avaliadas;
- [ ] hardware e latência considerados;
- [ ] evidências vão além de popularidade;
- [ ] recomendação declara riscos;
- [ ] existe condição para reavaliar.

## Resultado esperado

Matriz e recomendação fundamentada para a feature da dupla, sem implementação.

## Síntese

Seleção de modelo é uma decisão de engenharia. A escolha conecta capacidade, risco, custo e operação aos requisitos concretos da aplicação.
