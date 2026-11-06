# CT006 — Datas e garantias do painel solar

**Autor:** Alan Borba

## Objetivo

Verificar as validações das datas de compra, instalação e entrada em operação e das garantias de compra e de instalação, no cadastro e na edição do painel solar.

## Requisitos relacionados

- **Feature #24258 — Inclusão de novos painéis solares**
  - CA #35821 — CA11: dados complementares de compra, garantias, instalação e entrada em operação
  - CA #35824 — CA14: validação de datas e garantias no cadastro
- **Feature #24261 — Atualização de painéis solares**
  - CA #35839 — CA11: validação de datas e garantias na edição

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp.
- A data atual é **06/11/2026**.
- Estão cadastrados e ativos o fabricante **SolarTech** (modelo **ST-550**), o fornecedor **Fornecedor Sol Ltda**, o inversor **INV-001** (Usina Alfa) e a cooperada **Maria Souza**.
- Não existe painel com o Número de Série **PS-0007**.

## Fluxo principal — Datas e garantias válidas

1. O ator acessa a página de inclusão de painel solar.
2. O ator preenche os campos obrigatórios com os dados da tabela.
3. O ator preenche os dados complementares: Data de Compra 10/04/2026, Garantia de Compra 12, Data de Instalação 20/04/2026, Garantia de Instalação 24 e Data de Entrada em Operação 01/05/2026. [RN1]
4. O ator seleciona "Salvar".
5. O sistema grava o painel e exibe a mensagem de sucesso do cadastro.
6. O ator abre o painel PS-0007 na consulta e verifica que as datas e garantias aparecem como foram informadas.

## Fluxos alternativos

Em cada alternativa, o ator parte do fluxo principal e altera apenas o que está indicado. Se o resultado for "recusa", o painel não é gravado.

### A1 — Data de Compra sem Garantia de Compra

O ator apaga a Garantia de Compra e mantém a Data de Compra. O sistema recusa a gravação. [RN2]

### A2 — Garantia de Compra sem Data de Compra

O ator apaga a Data de Compra e mantém a Garantia de Compra. O sistema recusa a gravação. [RN2]

### A3 — Data de Compra futura

O ator informa a Data de Compra 07/11/2026, posterior à data atual. O sistema recusa a gravação. [RN3]

### A4 — Garantia negativa

O ator informa a Garantia de Compra −1. O sistema recusa a gravação. Em seguida, o ator volta a Garantia de Compra para 12 e informa a Garantia de Instalação −1. O sistema também recusa a gravação. [RN3][RN4]

### A5 — Instalação sem garantia, ou garantia sem instalação

O ator apaga a Garantia de Instalação e mantém a Data de Instalação. O sistema recusa a gravação. O ator faz o inverso e o sistema também recusa. [RN2]

### A6 — Instalação antes da compra

O ator informa a Data de Instalação 05/04/2026, anterior à Data de Compra 10/04/2026. O sistema recusa a gravação. [RN4]

### A7 — Entrada em operação antes da instalação

O ator informa a Data de Entrada em Operação 15/04/2026, anterior à Data de Instalação 20/04/2026. O sistema recusa a gravação. [RN5]

### A8 — Sem dados complementares

O ator deixa vazios todos os dados complementares (as duas datas com suas garantias e a Data de Entrada em Operação). O sistema grava o painel. [RN1][RN2]

### A9 — Mesmas regras na edição

1. Depois do fluxo principal, o ator abre a edição do PS-0007.
2. O ator altera a Data de Compra para 07/11/2026 e seleciona "Salvar".
3. O sistema recusa a alteração. [RN6]
4. O ator consulta o PS-0007 e verifica que a Data de Compra continua 10/04/2026.

## Regras de negócio

- **RN1 — Dados complementares (CA #35821):** o cadastro permite informar dados complementares de compra, garantias, instalação e entrada em operação.
- **RN2 — Pares de data e garantia (CA #35824):** Data de Compra e Garantia de Compra devem ser preenchidas juntas ou ficar ambas vazias. O mesmo vale para Data de Instalação e Garantia de Instalação.
- **RN3 — Compra (CA #35824):** a Data de Compra não pode ser posterior à data atual, e a Garantia de Compra não pode ser negativa.
- **RN4 — Instalação (CA #35824):** a Garantia de Instalação não pode ser negativa, e a Data de Instalação não pode ser anterior à Data de Compra, quando esta estiver preenchida.
- **RN5 — Entrada em operação (CA #35824):** a Data de Entrada em Operação não pode ser anterior à Data de Instalação nem à Data de Compra, quando estiverem preenchidas.
- **RN6 — Edição (CA #35839):** a edição valida datas e garantias com as mesmas regras do cadastro.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Campo | Valor usado |
|---|---|
| Número de Série | PS-0007 |
| Marca / Modelo | SolarTech / ST-550 |
| Potência de Pico (Wp) | 550 |
| Eficiência (%) | 21,3 |
| Data de Fabricação | 10/03/2026 |
| Situação | Ativo |
| Fornecedor / Inversor conectado | Fornecedor Sol Ltda / INV-001 |
| Cooperado | Maria Souza |
| Data de Compra / Garantia de Compra | 10/04/2026 / 12 |
| Data de Instalação / Garantia de Instalação | 20/04/2026 / 24 |
| Data de Entrada em Operação | 01/05/2026 |

| Situação | Resultado esperado |
|---|---|
| Datas e garantias válidas | Painel gravado com os dados informados. |
| A1, A2 e A5 — par incompleto | Recusa. |
| A3 — Data de Compra 07/11/2026 | Recusa. |
| A4 — garantia −1 | Recusa. |
| A6 — instalação em 05/04/2026 | Recusa. |
| A7 — entrada em operação em 15/04/2026 | Recusa. |
| A8 — dados complementares vazios | Painel gravado. |
| A9 — edição com compra futura | Alteração recusada; Data de Compra continua 10/04/2026. |

## Dúvida para esclarecimento

1. Os requisitos não informam a unidade das garantias de compra e de instalação (dias, meses ou anos). O cenário usa os números 12 e 24 sem unidade.
