# Encontro 02 — Apresentação e discussão de todos os diagramas

## Tema

Apresentação, análise comparativa e revisão dos cenários e diagramas de
arquitetura elaborados no Encontro 01.

## Objetivos

- comunicar uma proposta arquitetural com clareza;
- justificar onde a IA agrega valor e onde regras determinísticas permanecem;
- identificar riscos, validações, fallback e responsabilidades no fluxo;
- comparar soluções e incorporar feedback ao diagrama.

## Roteiro obrigatório da apresentação

1. problema, usuário e entrada do sistema;
2. percurso da solicitação pelo diagrama;
3. função atribuída ao modelo;
4. validações antes e depois da inferência;
5. dado que não deve ser enviado ao modelo;
6. principal falha prevista e comportamento de fallback;
7. decisão que depende de confirmação humana.

```mermaid
flowchart LR
    A[Apresentar o problema] --> B[Percorrer o fluxo]
    B --> C[Justificar o uso de IA]
    C --> D[Mostrar controles e fallback]
    D --> E[Receber uma pergunta]
    E --> F[Registrar duas melhorias]
```

## Rubrica de observação

| Critério | Evidência esperada |
|---|---|
| clareza | fluxo explicado na ordem das setas |
| adequação | IA associada a uma tarefa probabilística pertinente |
| controle | validação, regra determinística e fallback visíveis |
| responsabilidade | dados sensíveis e confirmação humana identificados |
| comunicação | clareza e respostas objetivas |

## Produto do encontro

Cada dupla entrega o diagrama revisado ou uma nota de revisão contendo: duas
alterações, justificativa e risco que permaneceu em aberto. Essa versão passa a
ser a referência arquitetural inicial do projeto.

## Síntese do encontro

As apresentações são concluídas integralmente neste encontro. No Encontro 03,
a disciplina avança para o funcionamento conceitual de LLMs, tokens, contexto,
inferência e limitações.
