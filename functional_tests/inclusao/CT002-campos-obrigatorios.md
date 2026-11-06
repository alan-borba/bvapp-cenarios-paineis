# CT002 — Campos obrigatórios no cadastro de painel solar

**Autor:** Alan Borba

## Objetivo

Verificar que o sistema não grava o painel solar quando falta um campo obrigatório e que exibe um alerta específico do campo que ficou vazio.

## Requisitos relacionados

- **Feature #24258 — Inclusão de novos painéis solares**
  - US #24495 — Registro Completo de Painel Solar
  - CA #24482 — CA01: campos do cadastro e obrigatoriedade
  - CA #24483 — CA02: impedir a gravação com campo obrigatório vazio, com alerta específico
  - CA #24490 — CA08: Fornecedor obrigatório

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp.
- Estão cadastrados e ativos o fabricante **SolarTech** (modelo **ST-550**), o fornecedor **Fornecedor Sol Ltda**, o inversor **INV-001** (Usina Alfa) e a cooperada **Maria Souza**.
- Não existe painel com o Número de Série **PS-0002**.

## Fluxo principal — Fornecedor vazio

1. O ator acessa a página de inclusão de painel solar.
2. O ator preenche todos os campos com os dados da tabela, mas deixa o **Fornecedor** vazio. [RN1][RN3]
3. O ator seleciona "Salvar".
4. O sistema não grava o painel e exibe um alerta específico informando que o Fornecedor é obrigatório. [RN2]
5. O ator acessa a consulta de painéis solares e verifica que o PS-0002 não está na lista.

## Fluxos alternativos

### A1 — Outro campo obrigatório vazio

1. O ator repete o fluxo principal com o Fornecedor preenchido, mas deixando vazio, um de cada vez, cada um dos campos: Número de Série, Marca, Modelo, Potência de Pico (Wp), Eficiência (%), Data de Fabricação, Situação e Inversor conectado. [RN1]
2. Em cada tentativa, o sistema não grava o painel e exibe um alerta específico do campo que ficou vazio. [RN2]

### A2 — Corrigir e salvar

1. Depois do passo 4 do fluxo principal, o ator seleciona o Fornecedor Sol Ltda e seleciona "Salvar".
2. O sistema grava o painel e exibe a mensagem "Painel solar cadastrado com sucesso e vinculado ao Cooperado Maria Souza".
3. O ator verifica que o PS-0002 está na consulta de painéis solares.

### A3 — Cadastro sem Cooperado

1. O ator preenche todos os campos obrigatórios e deixa o Cooperado vazio.
2. O ator seleciona "Salvar".
3. O resultado esperado depende da Dúvida 1. Se o Cooperado for opcional, o painel é gravado com a mensagem "Painel solar cadastrado com sucesso". Se for obrigatório, o sistema não grava e exibe o alerta do campo.

## Regras de negócio

- **RN1 — Campos obrigatórios (CA #24482):** Número de Série, Marca, Modelo, Potência de Pico (Wp), Eficiência (%), Data de Fabricação, Situação, Fornecedor e Inversor conectado são obrigatórios. O Cooperado deve ser disponibilizado, mas é opcional no cadastro.
- **RN2 — Bloqueio da gravação (CA #24483):** com algum campo obrigatório vazio, o sistema impede a gravação e exibe um alerta específico. O requisito não define o texto do alerta, então o cenário não exige um texto exato.
- **RN3 — Fornecedor (CA #24490):** é obrigatório selecionar um Fornecedor.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Campo | Valor usado |
|---|---|
| Número de Série | PS-0002 |
| Marca / Modelo | SolarTech / ST-550 |
| Potência de Pico (Wp) | 550 |
| Eficiência (%) | 21,3 |
| Data de Fabricação | 10/03/2026 |
| Situação | Ativo |
| Fornecedor | vazio no fluxo principal; Fornecedor Sol Ltda em A1 e A2 |
| Inversor conectado | INV-001 |
| Cooperado | Maria Souza (vazio em A3) |

| Tentativa | Resultado esperado |
|---|---|
| Fornecedor vazio | Painel não gravado; alerta específico do Fornecedor; PS-0002 fora da lista. |
| Cada um dos outros campos obrigatórios vazio (A1) | Painel não gravado; alerta específico do campo. |
| Todos os campos preenchidos (A2) | Painel gravado; mensagem de sucesso com o nome da cooperada; PS-0002 na lista. |
| Cooperado vazio (A3) | Depende da Dúvida 1. |

## Dúvida para esclarecimento

1. O CA #24482 diz que o Cooperado é **opcional** no cadastro. A US #24495 diz que o sistema deve validar a obrigatoriedade de todos os vínculos, **incluindo o Cooperado**. Afinal, é possível cadastrar um painel sem Cooperado?
