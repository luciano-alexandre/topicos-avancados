# Projeto Final — Aplicação Web com Inteligência Artificial

## Objetivo

Projetar, implementar, avaliar e apresentar uma aplicação Web que resolva um
problema real com uma funcionalidade de IA justificável. O projeto vale 50% da
nota e será desenvolvido incrementalmente.

## Equipe e processo

- equipes de até 4 integrantes ou trabalho individual;
- repositório Git com contribuições identificáveis;
- proposta aprovada contendo problema, usuários, dados, riscos e critério de sucesso;
- checkpoints nos encontros 24, 30, 35, 37 e 38;
- apresentações e demonstrações nos encontros 39 e 40.

## Requisitos mínimos

- front-end Angular e API NestJS com responsabilidades separadas;
- PostgreSQL e, quando houver recuperação vetorial, pgvector;
- modelo aberto acessado preferencialmente por servidor local;
- ao menos uma técnica central: saída estruturada, tool calling, RAG ou workflow/agente;
- validação das entradas e das saídas do modelo;
- timeout e tratamento explícito de indisponibilidade;
- conjunto de avaliação com casos representativos e baseline;
- logs de latência, modelo, versão e resultado, sem expor conteúdo sensível;
- threat model contemplando prompt injection, permissões e vazamento de dados;
- Docker Compose, `.env.example` e instruções reproduzíveis.

## Entregáveis

1. código-fonte e histórico Git;
2. README com problema, arquitetura, instalação, decisões e limitações;
3. diagrama da solução e fluxo da funcionalidade de IA;
4. dataset/casos de avaliação permitidos e relatório de resultados;
5. documentação da API e dos contratos de ferramentas/saídas;
6. evidências de testes, observabilidade, segurança e revisão humana;
7. demonstração funcional e apresentação curta.

## Restrições

- dados reais sensíveis não devem ser usados;
- ações com efeito externo exigem autorização, validação e menor privilégio;
- o modelo não substitui regras determinísticas críticas;
- modelos, datasets e bibliotecas devem ter licenças verificadas;
- decisões e limitações conhecidas devem ser documentadas.

## Sugestões de temas

- assistente para documentos institucionais com fontes;
- triagem estruturada de solicitações;
- busca semântica em acervo acadêmico;
- assistente de manutenção com ferramentas controladas;
- apoio à revisão de requisitos ou testes de software;
- workflow de análise documental com aprovação humana.
