# CT005 — Número de Série e Potência de Pico no cadastro de painel solar

**Autor:** Alan Borba

## Objetivo

Verificar as validações do Número de Série (tamanho e espaços) e da Potência de Pico (vazia, zero ou negativa) no cadastro de painel solar.

## Requisitos relacionados

- **Feature #24258 — Inclusão de novos painéis solares**
  - US #24495 — Registro Completo de Painel Solar
  - CA #35822 — CA12: recusar Potência de Pico vazia, zero ou negativa
  - CA #35823 — CA13: Número de Série com até 100 caracteres, sem espaços no início e no fim e não composto apenas por espaços

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp.
- Estão cadastrados e ativos o fabricante **SolarTech** (modelo **ST-550**), o fornecedor **Fornecedor Sol Ltda**, o inversor **INV-001** (Usina Alfa) e a cooperada **Maria Souza**.
- Não existe painel com o Número de Série **PS-0006**.

## Fluxo principal — Número de Série com espaços nas pontas

1. O ator acessa a página de inclusão de painel solar.
2. O ator preenche os campos com os dados da tabela, digitando o Número de Série com espaços antes e depois: `   PS-0006   `.
3. O ator seleciona "Salvar".
4. O sistema grava o painel e exibe a mensagem de sucesso do cadastro.
5. O ator acessa a consulta de painéis solares e verifica que o painel aparece com o Número de Série `PS-0006`, sem os espaços. [RN1]

## Fluxos alternativos

### A1 — Número de Série só com espaços

1. O ator preenche o Número de Série apenas com espaços e seleciona "Salvar".
2. O sistema não grava o painel. [RN1]

### A2 — Tamanho do Número de Série

1. O ator preenche um Número de Série com exatamente 100 caracteres e seleciona "Salvar".
2. O sistema grava o painel. [RN1]
3. O ator repete com um Número de Série de 101 caracteres.
4. O sistema não grava o painel. [RN1]

### A3 — Potência de Pico inválida

1. O ator preenche a Potência de Pico com 0 e seleciona "Salvar".
2. O sistema recusa o cadastro. [RN2]
3. O ator repete com a Potência de Pico −10 e depois com a Potência de Pico vazia.
4. Nas duas tentativas, o sistema recusa o cadastro. [RN2]

## Regras de negócio

- **RN1 — Número de Série (CA #35823):** admite até 100 caracteres, desconsidera os espaços no início e no fim e não pode ser composto apenas por espaços.
- **RN2 — Potência de Pico (CA #35822):** o sistema deve recusar Potência de Pico vazia, zero ou negativa.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Campo | Valor usado |
|---|---|
| Marca / Modelo | SolarTech / ST-550 |
| Potência de Pico (Wp) | 550 (exceto em A3) |
| Eficiência (%) | 21,3 |
| Data de Fabricação | 10/03/2026 |
| Situação | Ativo |
| Fornecedor | Fornecedor Sol Ltda |
| Inversor conectado | INV-001 |
| Cooperado | Maria Souza |

| Valor testado | Resultado esperado |
|---|---|
| Número de Série `   PS-0006   ` | Gravado como `PS-0006`. |
| Número de Série só com espaços (A1) | Não gravado. |
| Número de Série com 100 caracteres (A2) | Gravado. |
| Número de Série com 101 caracteres (A2) | Não gravado. |
| Potência de Pico 0, −10 ou vazia (A3) | Recusado. |

## Dúvidas para esclarecimento

1. A US #24495 manda "aplicar a regra de unicidade", mas nenhum CA diz qual dado deve ser único. O cadastro deve recusar um Número de Série que já existe?
2. Os requisitos não definem valor máximo nem casas decimais para a Potência de Pico e a Eficiência. Também não dizem se a Eficiência aceita 0 % ou valores acima de 100 %.
