# CT001 — Manter painéis solares

**Autor:** Alan Borba

## Objetivo

Verificar, em um mesmo fluxo, que o ator consegue cadastrar, consultar, atualizar e excluir um painel solar, conferindo na listagem o resultado de cada operação.

## Requisitos relacionados

- **Feature #24258 — Inclusão de novos painéis solares**
  - US #24495 — Registro Completo de Painel Solar
  - CA #24482 — CA01: campos do cadastro e obrigatoriedade
  - CA #24483 — CA02: impedir gravação com campo obrigatório vazio
  - CA #24488 — CA06: Usina herdada do Inversor conectado
  - CA #24489 — CA07: mensagem de sucesso do cadastro
- **Feature #24260 — Consulta de painéis solares**
  - US #24506 — Consulta Global de Painéis Solares
  - CA #24499 — CA02: colunas da listagem
  - CA #24503 — CA06: filtros da listagem
- **Feature #24261 — Atualização de painéis solares**
  - US #24516 — Atualização Cadastral de Painel Solar
  - CA #24510 — CA01: formulário carregado com os dados atuais
  - CA #24511 — CA02: Número de Série somente para leitura
  - CA #24512 — CA03: alteração do Inversor conectado e do Cooperado
  - CA #24513 — CA04: mensagem de sucesso da atualização
  - CA #35834 — CA06: novos valores na consulta
- **Feature #24262 — Exclusão de painéis solares**
  - US #24526 — Remoção e Inativação de Painéis Solares
  - CA #24519 — CA01: exclusão restrita a Administrador/Gerente
  - CA #24520 — CA02: modal de confirmação com os dados do Cooperado
  - CA #24521 — CA03: quando a exclusão física não é permitida
  - CA #24522 — CA04: exclusão física só para registros "limpos"
  - CA #24523 — CA05: mensagem de sucesso da exclusão

## Ator

Administrador ou Gerente do BVapp. A exclusão é restrita a esses perfis (CA #24519).

## Pré-condições

- O ator está autenticado no BVapp com o perfil Administrador ou Gerente.
- Estão cadastrados e ativos:
  - o fabricante **SolarTech**, com o modelo **ST-550**;
  - o fornecedor **Fornecedor Sol Ltda**;
  - os inversores **INV-001** e **INV-002**, ambos ligados à **Usina Alfa**;
  - a cooperada **Maria Souza**, que já possui outro painel ativo, o **PS-0000**.
- Não existe painel com o Número de Série **PS-0001**.

## Fluxo principal — Cadastrar e consultar

1. O ator acessa a página de inclusão de painel solar.
2. Ele observa que existem os campos [RN1]:
   - Número de Série
   - Marca
   - Modelo
   - Potência de Pico (Wp)
   - Eficiência (%)
   - Data de Fabricação
   - Situação
   - Fornecedor
   - Inversor conectado
   - Cooperado
3. O ator preenche os campos com os dados da coluna "Cadastro" da tabela de dados, incluindo o Cooperado.
4. O ator seleciona "Salvar". [RN1]
5. O sistema grava o painel e exibe a mensagem "Painel solar cadastrado com sucesso e vinculado ao Cooperado Maria Souza". [RN2]
6. O ator acessa a consulta de painéis solares.
7. O ator verifica que o painel PS-0001 está na lista, com Marca, Modelo, Inversor, Cooperado, Usina e Situação, e que a Usina exibida é a Usina Alfa, herdada do inversor INV-001. [RN3]

## Fluxos alternativos

### A1 — Buscar painel

1. O ator acessa a consulta de painéis solares.
2. O ator aplica o filtro por Cooperado e escolhe Maria Souza. [RN3]
3. O sistema exibe os painéis da cooperada, entre eles o PS-0000 e o PS-0001.

### A2 — Editar painel

1. O ator executa [A1].
2. O ator seleciona o painel PS-0001 e a opção de editar.
3. O sistema exibe o formulário de edição preenchido com os dados atuais do painel, incluindo Fornecedor e Cooperado. O campo Número de Série aparece somente para leitura. [RN4]
4. O ator troca o Inversor conectado de INV-001 para INV-002 e seleciona "Salvar".
5. O sistema valida a nova associação, grava a alteração e exibe a mensagem "Dados do painel solar atualizados com sucesso". [RN5]
6. O ator volta à consulta de painéis solares.
7. O ator verifica que o painel PS-0001 aparece com o Inversor INV-002. [RN5]

### A3 — Excluir painel

1. O ator executa [A1].
2. O ator seleciona o painel PS-0001 e a opção "Excluir".
3. O sistema exibe um modal de confirmação com os dados da cooperada vinculada, Maria Souza. [RN6]
4. O ator confirma a exclusão.
5. O sistema exibe uma mensagem de sucesso. [RN6]
6. O ator volta à consulta de painéis solares e verifica o resultado da exclusão na listagem (ver Dúvida 1).

## Regras de negócio

- **RN1 — Campos obrigatórios (CA #24482 e CA #24483):** Número de Série, Marca, Modelo, Potência de Pico (Wp), Eficiência (%), Data de Fabricação, Situação, Fornecedor e Inversor conectado são obrigatórios. O Cooperado deve ser disponibilizado e é opcional no cadastro, segundo o CA #24482. A US #24495 trata o Cooperado como obrigatório. Essa divergência é tratada no CT002. Aqui o Cooperado é informado, então o cadastro é válido nas duas leituras.
- **RN2 — Mensagem do cadastro (CA #24489):** após gravar, o sistema exibe "Painel solar cadastrado com sucesso". Se um cooperado foi informado, acrescenta "e vinculado ao Cooperado [Nome]".
- **RN3 — Listagem (CA #24499, CA #24503 e CA #24488):** a lista exibe Número de Série, Marca, Modelo, Inversor, Cooperado, Usina e Situação. A Usina é herdada do Inversor conectado. A lista pode ser filtrada por Marca, Modelo, Cooperado, Usina, Fornecedor e Inversor.
- **RN4 — Formulário de edição (CA #24510, CA #24511 e CA #24512):** o formulário vem com os dados atuais, incluindo Fornecedor e Cooperado. O Número de Série é somente para leitura. O Inversor conectado e o Cooperado podem ser alterados, e o sistema valida a nova associação.
- **RN5 — Resultado da edição (CA #24513 e CA #35834):** o sistema exibe "Dados do painel solar atualizados com sucesso", e os novos valores aparecem na consulta do mesmo painel.
- **RN6 — Exclusão (CA #24519 a CA #24523):** só Administrador ou Gerente podem excluir. O modal de confirmação mostra os dados do Cooperado vinculado. A exclusão física não é permitida se o painel tiver histórico ou for o único vinculado a um Cooperado ativo, e só é permitida para registros "limpos". Ao concluir, o sistema exibe uma mensagem de sucesso.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Campo | Cadastro (fluxo principal) | Edição (A2) |
|---|---|---|
| Número de Série | PS-0001 | sem alteração (somente leitura) |
| Marca | SolarTech | sem alteração |
| Modelo | ST-550 | sem alteração |
| Potência de Pico (Wp) | 550 | sem alteração |
| Eficiência (%) | 21,3 | sem alteração |
| Data de Fabricação | 10/03/2026 | sem alteração |
| Situação | Ativo | sem alteração |
| Fornecedor | Fornecedor Sol Ltda | sem alteração |
| Inversor conectado | INV-001 (Usina Alfa) | INV-002 (Usina Alfa) |
| Cooperado | Maria Souza | sem alteração |

| Etapa | Resultado esperado |
|---|---|
| Cadastro | Mensagem "Painel solar cadastrado com sucesso e vinculado ao Cooperado Maria Souza". |
| Consulta após o cadastro | PS-0001 na lista, com Inversor INV-001, Cooperado Maria Souza, Usina Alfa e Situação Ativo. |
| A1 — Buscar | Filtro por Maria Souza mostra PS-0000 e PS-0001. |
| A2 — Editar | Formulário preenchido e Número de Série somente para leitura. Mensagem "Dados do painel solar atualizados com sucesso". PS-0001 aparece com o Inversor INV-002 e a Usina Alfa. |
| A3 — Excluir | O modal mostra os dados de Maria Souza. Após a confirmação, o sistema exibe uma mensagem de sucesso. O que a listagem mostra depende da Dúvida 1. |

## Dúvida para esclarecimento

1. A US #24526 diz que, ao excluir um painel vinculado a um cooperado, o sistema faz a **inativação lógica**. Os CAs #24521 e #24522, por outro lado, permitem a **exclusão física** de registros "limpos". O PS-0001 está vinculado a uma cooperada, não tem histórico e não é o único painel dela. O resultado deve ser a remoção do cadastro ou a inativação? A resposta define se ele some da listagem ou continua nela como Inativo.
