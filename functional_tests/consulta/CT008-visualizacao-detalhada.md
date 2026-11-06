# CT008 — Visualização detalhada de painel solar

**Autor:** Alan Borba

## Objetivo

Verificar se a visualização detalhada de um painel solar apresenta os dados previstos, oferece o histórico de ordens de serviço, trata painel inexistente ou excluído e não altera o painel ao ser aberta e fechada.

## Requisitos relacionados

- **Feature #24260 — Consulta de painéis solares**
  - US #25894 — Visualização de Painel Solar
  - CA #35830 — CA10: dados apresentados na visualização detalhada
  - CA #35831 — CA11: painel inexistente ou excluído
  - CA #35832 — CA12: aba Histórico de Ordens de Serviço
  - CA #35833 — CA13: abrir e fechar a visualização não altera o painel

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp e tem autorização para consultar painéis solares (US #25894).
- O ator está na consulta de painéis solares.
- Está cadastrado o painel **PS-0007**, com os dados da tabela e com a ordem de serviço **OS-0010** registrada.
- Está cadastrado o painel **PS-0008**, que será excluído por outro usuário durante o fluxo A1.

## Fluxo principal

1. O ator seleciona o painel PS-0007 na listagem.
2. O sistema apresenta a visualização detalhada com: Número de Série, Potência, Modelo, Fabricante, Fornecedor, Usina, Situação, vínculo (Cooperado ou Cooperativa), Data de Compra, Data de Instalação, Data de Entrada em Operação e garantias. [RN1]
3. O ator abre a aba Histórico de Ordens de Serviço.
4. O sistema apresenta a OS-0010 do PS-0007. [RN2]
5. O ator fecha a visualização detalhada.
6. O ator abre novamente o PS-0007 e verifica que todos os dados continuam iguais. [RN3]

## Fluxos alternativos

### A1 — Painel excluído depois de a lista ser carregada

1. Com a lista já aberta, outro usuário exclui o painel PS-0008.
2. O ator seleciona o PS-0008 na lista ainda aberta.
3. O sistema informa o erro e não apresenta dados de outro painel. [RN4]

## Regras de negócio

- **RN1 — Dados do detalhe (CA #35830):** a visualização detalhada apresenta Número de Série, Potência, Modelo, Fabricante, Fornecedor, Usina, Situação, vínculo (Cooperado ou Cooperativa), datas de compra, instalação e entrada em operação e garantias.
- **RN2 — Histórico de OS (CA #35832):** a visualização detalhada oferece a aba Histórico de Ordens de Serviço do painel selecionado.
- **RN3 — Sem alteração (CA #35833):** abrir e fechar a visualização detalhada não altera os dados do painel.
- **RN4 — Painel indisponível (CA #35831):** se o painel selecionado não existir ou estiver excluído, o sistema informa o erro, sem apresentar dados de outro registro.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Campo do PS-0007 | Valor esperado no detalhe |
|---|---|
| Número de Série | PS-0007 |
| Potência | 550 Wp |
| Modelo / Fabricante | ST-550 / SolarTech |
| Fornecedor | Fornecedor Sol Ltda |
| Usina | Usina Alfa (do inversor INV-001) |
| Situação | Ativo |
| Vínculo | Cooperada Maria Souza |
| Data de Compra / Garantia de Compra | 10/04/2026 / 12 |
| Data de Instalação / Garantia de Instalação | 20/04/2026 / 24 |
| Data de Entrada em Operação | 01/05/2026 |
| Histórico de Ordens de Serviço | OS-0010 |

| Situação | Resultado esperado |
|---|---|
| Abrir o PS-0007 | Todos os dados da tabela acima. |
| Abrir, fechar e abrir de novo | Os mesmos dados, sem alteração. |
| Abrir o PS-0008 já excluído (A1) | Mensagem de erro, sem dados de outro painel. |

## Dúvida para esclarecimento

1. Os requisitos não dizem o que a visualização detalhada mostra enquanto os dados carregam, nem como o usuário tenta de novo se o carregamento falhar.
