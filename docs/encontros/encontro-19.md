# Encontro 19 — Consolidação prática e revisão arquitetural da Unidade 1

## Organização

- Unidade pedagógica: 1;
- tema curricular: integração e revisão dos conceitos da primeira unidade.

## Objetivos

- consolidar o fluxo Angular, NestJS, PostgreSQL, Ollama e Docker;
- revisar contratos de entrada, prompts, saída estruturada e persistência;
- identificar acoplamentos e responsabilidades posicionadas na camada errada;
- corrigir falhas encontradas durante a integração;
- preparar uma explicação técnica curta da arquitetura implementada.

## Desenvolvimento sugerido

1. retomada do fluxo completo e definição dos critérios;
2. revisão individual orientada por checklist;
3. correções e testes do fluxo integrado;
4. demonstração por amostragem e síntese da unidade.

## Checklist de consolidação

- [ ] todos os serviços sobem com Docker Compose;
- [ ] o navegador não acessa diretamente o Ollama nem o PostgreSQL;
- [ ] DTOs validam as entradas no NestJS;
- [ ] prompts ficam centralizados no backend;
- [ ] a saída da IA é validada antes da persistência;
- [ ] erros externos não expõem detalhes internos;
- [ ] dados persistidos podem ser consultados pelo endpoint definido;
- [ ] o README permite reproduzir a execução em outro computador.

## Resultado esperado

Aplicação integrada, executável por Docker Compose, acompanhada de diagrama da
arquitetura, evidências dos testes e registro das limitações ainda existentes.

## Continuidade

O encontro encerra a sequência técnica da Unidade 1 depois das atividades com features e da prática integrada.
