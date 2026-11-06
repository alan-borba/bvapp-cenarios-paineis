# CT004 — Inversor conectado e Usina herdada no cadastro de painel solar

**Autor:** Alan Borba

## Objetivo

Verificar que a seleção do Inversor apresenta apenas inversores ativos, que o painel herda a Usina do Inversor conectado e que o sistema recusa Inversor ou Fornecedor inativos.

## Requisitos relacionados

- **Feature #24258 — Inclusão de novos painéis solares**
  - CA #24488 — CA06: painel conectado a um Inversor ativo; Usina herdada desse Inversor
  - CA #24491 — CA09: a seleção do Inversor apresenta a listagem geral de inversores ativos
  - CA #35825 — CA15: recusar Inversor ou Fornecedor inativos

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp.
- Estão cadastrados os inversores:
  - **INV-001**, ativo, da **Usina Alfa**;
  - **INV-003**, ativo, da **Usina Beta**;
  - **INV-009**, inativo, da **Usina Alfa**.
- Estão cadastrados o fabricante **SolarTech** (modelo **ST-550**), o fornecedor ativo **Fornecedor Sol Ltda** e a cooperada **Maria Souza**.
- Não existem painéis com os Números de Série **PS-0003**, **PS-0004** e **PS-0005**.

## Fluxo principal

1. O ator acessa a página de inclusão de painel solar.
2. O ator abre a lista do campo Inversor conectado.
3. O sistema apresenta INV-001 e INV-003. O INV-009, que está inativo, não aparece. [RN2]
4. O ator seleciona INV-001 e preenche os demais campos com os dados da tabela, usando o Número de Série PS-0003.
5. O ator seleciona "Salvar".
6. O sistema grava o painel e exibe a mensagem de sucesso do cadastro.
7. O ator acessa a consulta de painéis solares e verifica que o PS-0003 aparece com a Usina Alfa, herdada do INV-001. [RN1]

## Fluxos alternativos

### A1 — Inversor de outra Usina

1. O ator repete o fluxo principal com o Número de Série PS-0004 e o inversor INV-003.
2. Na consulta, o PS-0004 aparece com a Usina Beta. [RN1]

### A2 — Inversor inativado durante o preenchimento

1. O ator inicia o cadastro do PS-0005 e seleciona o INV-001.
2. Antes de o ator salvar, outro usuário inativa o INV-001.
3. O ator seleciona "Salvar".
4. O sistema recusa o cadastro, e o PS-0005 não é gravado. [RN3]

### A3 — Fornecedor inativado durante o preenchimento

1. O ator inicia o cadastro do PS-0005 e seleciona o Fornecedor Sol Ltda.
2. Antes de o ator salvar, outro usuário inativa o Fornecedor Sol Ltda.
3. O ator seleciona "Salvar".
4. O sistema recusa o cadastro, e o PS-0005 não é gravado. [RN3]

## Regras de negócio

- **RN1 — Usina herdada (CA #24488):** o painel deve ser conectado a um Inversor ativo, e a Usina do painel é herdada automaticamente desse Inversor.
- **RN2 — Lista de Inversores (CA #24491):** a seleção do Inversor apresenta a listagem geral de inversores ativos.
- **RN3 — Vínculos inativos (CA #35825):** o sistema deve recusar Inversor ou Fornecedor inativos.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Campo | Valor usado |
|---|---|
| Número de Série | PS-0003 (PS-0004 em A1; PS-0005 em A2 e A3) |
| Marca / Modelo | SolarTech / ST-550 |
| Potência de Pico (Wp) | 550 |
| Eficiência (%) | 21,3 |
| Data de Fabricação | 10/03/2026 |
| Situação | Ativo |
| Fornecedor | Fornecedor Sol Ltda |
| Inversor conectado | INV-001 (INV-003 em A1) |
| Cooperado | Maria Souza |

| Situação | Resultado esperado |
|---|---|
| Abrir a lista de Inversor conectado | INV-001 e INV-003; INV-009 ausente. |
| Cadastro com INV-001 | PS-0003 gravado, com a Usina Alfa na consulta. |
| Cadastro com INV-003 (A1) | PS-0004 gravado, com a Usina Beta na consulta. |
| Inversor inativado antes de salvar (A2) | Cadastro recusado; PS-0005 não gravado. |
| Fornecedor inativado antes de salvar (A3) | Cadastro recusado; PS-0005 não gravado. |
