# CT009 — Ativar e inativar painel solar

**Autor:** Alan Borba

## Objetivo

Verificar se o ator consegue alterar a situação de um painel solar entre Ativo e Inativo, com confirmação que identifica o painel, e se os demais dados do painel são preservados.

## Requisitos relacionados

- **Feature #24261 — Atualização de painéis solares**
  - US #29053 — Alterar status do Painel Solar
  - CA #35835 — CA07: alterar a situação entre Ativo e Inativo, preservando os demais dados
  - CA #35836 — CA08: confirmação com o Número de Série; cancelar mantém a situação anterior

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp e está na consulta de painéis solares.
- Está cadastrado o painel **PS-0007**, com a Situação **Ativo** e os demais dados do CT008.

## Fluxo principal — Inativar

1. Na listagem, o ator aciona a alteração de situação do PS-0007 para Inativo.
2. O sistema pede confirmação e identifica o painel pelo Número de Série PS-0007. [RN2]
3. O ator confirma a alteração.
4. O sistema aplica a Situação Inativo ao painel. [RN1]
5. O ator verifica que o PS-0007 aparece como Inativo na listagem.
6. O ator abre o PS-0007 e verifica que os demais dados continuam iguais. [RN1]

## Fluxos alternativos

### A1 — Cancelar a alteração

1. No passo 2 do fluxo principal, o ator cancela.
2. O PS-0007 continua Ativo na listagem. [RN2]

### A2 — Reativar

1. Com o PS-0007 Inativo, o ator aciona a alteração de situação para Ativo.
2. O sistema pede confirmação identificando o PS-0007.
3. O ator confirma.
4. O PS-0007 volta a aparecer como Ativo, com os demais dados iguais. [RN1][RN2]

## Regras de negócio

- **RN1 — Situação (CA #35835):** o sistema permite alterar a situação do painel entre Ativo e Inativo, preservando os demais dados.
- **RN2 — Confirmação (CA #35836):** antes de alterar a situação, o sistema pede confirmação identificando o painel pelo Número de Série. Cancelar mantém a situação anterior.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Ação | Resultado esperado |
|---|---|
| Inativar o PS-0007 e confirmar | Confirmação com "PS-0007"; Situação Inativo; demais dados iguais. |
| Inativar e cancelar (A1) | Situação continua Ativo. |
| Reativar e confirmar (A2) | Situação Ativo; demais dados iguais. |

## Dúvidas para esclarecimento

1. Os requisitos não dizem o que acontece se o painel já estiver na situação escolhida. Por exemplo, outro usuário inativou o painel antes de o ator confirmar a mesma alteração.
2. O cadastro tem um campo Situação, mas os requisitos não fecham quais situações existem. Se houver outras além de Ativo e Inativo, a ação desta tela também vale para elas?
