# CT010 — Exclusão de painel solar recusada

**Autor:** Alan Borba

## Objetivo

Verificar se o sistema impede a exclusão de painéis solares nas situações previstas nos requisitos, preserva o cadastro e informa o motivo, sem indicar que a exclusão foi concluída.

## Requisitos relacionados

- **Feature #24262 — Exclusão de painéis solares**
  - US #24526 — Remoção e Inativação de Painéis Solares
  - CA #24519 — CA01: exclusão restrita ao perfil Administrador/Gerente
  - CA #24520 — CA02: modal de confirmação com os dados do Cooperado vinculado
  - CA #24521 — CA03: sem exclusão física se houver histórico ou se o painel for o único de um Cooperado ativo
  - CA #24522 — CA04: exclusão física só para registros "limpos"
  - CA #35842 — CA07: recusar a exclusão de painel associado a um serviço
  - CA #35844 — CA09: exibir o motivo se a exclusão for recusada ou falhar

## Ator

Administrador ou Gerente do BVapp. A exclusão é restrita a esses perfis (CA #24519).

## Pré-condições

- O ator está autenticado no BVapp com o perfil Administrador ou Gerente e está na consulta de painéis solares.
- Estão cadastrados:
  - **PS-0020**, associado ao serviço **Limpeza mensal**, da cooperada **Maria Souza**, que tem outros painéis;
  - **PS-0021**, único painel do cooperado ativo **João Lima**, sem histórico e sem serviço;
  - **PS-0022**, com histórico registrado (ver Dúvida 1), sem serviço, da cooperada Maria Souza.
- Existe o usuário **Ana Costa**, autenticada com um perfil que não é Administrador nem Gerente.

## Fluxo principal — Painel associado a um serviço

1. O ator seleciona o painel PS-0020 e a opção "Excluir".
2. O sistema exibe o modal de confirmação com os dados da cooperada Maria Souza. [RN2]
3. O ator confirma a exclusão.
4. O sistema recusa a exclusão e informa o motivo, sem indicar que a exclusão foi concluída. [RN4][RN5]
5. O ator verifica que o PS-0020 continua na listagem, com os mesmos dados.

## Fluxos alternativos

### A1 — Único painel de um cooperado ativo

1. O ator seleciona o PS-0021, a opção "Excluir", e confirma no modal, que mostra os dados de João Lima.
2. O sistema não faz a exclusão física do PS-0021. [RN3]
3. O PS-0021 continua no cadastro. Se ele fica como está ou é inativado depende da Dúvida 2.

### A2 — Painel com histórico

1. O ator seleciona o PS-0022, a opção "Excluir", e confirma no modal.
2. O sistema não faz a exclusão física do PS-0022. [RN3]
3. O PS-0022 continua no cadastro. Se ele fica como está ou é inativado depende da Dúvida 2.

### A3 — Usuário sem o perfil exigido

1. Ana Costa acessa a consulta de painéis solares e tenta excluir o PS-0021.
2. O sistema não permite a exclusão. [RN1]
3. O PS-0021 continua no cadastro.

### A4 — Falha durante a exclusão

1. O ator tenta excluir um painel que poderia ser excluído, em ambiente preparado para falhar no momento da exclusão.
2. O sistema exibe o motivo da falha e não indica que a exclusão foi concluída. [RN5]

## Regras de negócio

- **RN1 — Perfil (CA #24519):** a exclusão é restrita ao perfil Administrador/Gerente.
- **RN2 — Confirmação (CA #24520):** a exclusão exibe um modal de confirmação com os dados do Cooperado vinculado.
- **RN3 — Exclusão física (CA #24521 e CA #24522):** o sistema não permite a exclusão física se houver histórico ou se o painel for o único vinculado a um Cooperado ativo. A exclusão física só é permitida para registros "limpos".
- **RN4 — Serviço (CA #35842):** o sistema recusa a exclusão de painel associado a um serviço, preservando o cadastro e informando o motivo.
- **RN5 — Recusa ou falha (CA #35844):** se a exclusão for recusada ou falhar, o sistema exibe o motivo, sem indicar que foi concluída.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Painel e situação | Resultado esperado |
|---|---|
| PS-0020, associado ao serviço Limpeza mensal | Recusa com motivo; painel preservado na listagem. |
| PS-0021, único painel de João Lima (A1) | Sem exclusão física; painel continua no cadastro (Dúvida 2). |
| PS-0022, com histórico (A2) | Sem exclusão física; painel continua no cadastro (Dúvida 2). |
| Ana Costa, sem o perfil exigido (A3) | Exclusão não permitida; PS-0021 preservado. |
| Falha durante a exclusão (A4) | Motivo da falha; nenhuma indicação de exclusão concluída. |

## Dúvidas para esclarecimento

1. Os CAs #24521 e #24522 usam "histórico" e registro "limpo" sem dizer quais registros contam. Ordens de serviço, alterações de cadastro e logs entram nessa conta?
2. A US #24526 diz que a exclusão de um painel vinculado a cooperado faz a **inativação lógica**. Os CAs #24521 e #24522 falam só em impedir a **exclusão física**. Quando a exclusão física não é permitida, o sistema deve recusar a operação ou inativar o painel?
3. O CA #24520 não diz quais dados do Cooperado aparecem no modal, nem como mostrar um painel vinculado a mais de um cooperado.
