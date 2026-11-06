# CT007 — Filtros, ordenação e paginação da consulta de painéis solares

**Autor:** Alan Borba

## Objetivo

Verificar se a listagem de painéis solares apresenta as colunas previstas, a ordenação padrão e por coluna, os filtros, a paginação e as indicações de carregamento, de ausência de resultados e de falha.

## Requisitos relacionados

- **Feature #24260 — Consulta de painéis solares**
  - US #24506 — Consulta Global de Painéis Solares
  - US #24507 — Filtros e Exportação de Painéis Solares (apenas a parte dos filtros)
  - CA #24499 — CA02: colunas da listagem e Usina calculada
  - CA #24500 — CA03: ordem padrão decrescente da data de inclusão/fabricação
  - CA #24501 — CA04: ordenação dinâmica por colunas
  - CA #24502 — CA05: paginação de 10, 50, 100 ou Todos
  - CA #24503 — CA06: filtros por Marca, Modelo, Cooperado, Usina, Fornecedor e Inversor
  - CA #35828 — CA08: informações da paginação e manutenção dos filtros ao trocar de página
  - CA #35829 — CA09: carregamento, ausência de resultados e falha

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp.
- Estão cadastrados 15 painéis, de **PS-0101** a **PS-0115**, distribuídos assim:
  - de PS-0101 a PS-0112: marca **SolarTech**, inversor **INV-001** (Usina Alfa), cooperada **Maria Souza**;
  - de PS-0113 a PS-0115: marca **Alfa Solar**, inversor **INV-003** (Usina Beta), cooperado **João Lima**.
- O inversor **INV-005** (Usina Gama) existe e não tem painéis conectados.

## Fluxo principal — Consultar e paginar

1. O ator acessa a consulta de painéis solares.
2. Enquanto a lista é carregada, o sistema indica que o carregamento está em andamento. [RN6]
3. O sistema exibe a lista com as colunas Número de Série, Marca, Modelo, Inversor, Cooperado, Usina e Situação. A Usina de cada painel é a do Inversor conectado. [RN1]
4. O ator verifica que a lista vem, por padrão, em ordem decrescente de data (ver Dúvida 1). [RN2]
5. O ator escolhe 10 registros por página. [RN4]
6. O sistema exibe 10 painéis e informa a página 1 de 2, o total de 15 registros e a quantidade de 10 por página. [RN5]
7. O ator vai para a página 2.
8. O sistema exibe os 5 painéis restantes. [RN5]

## Fluxos alternativos

### A1 — Ordenar por coluna

1. O ator seleciona a coluna Número de Série para ordenar.
2. O sistema reordena a lista pelo Número de Série. [RN3]

### A2 — Filtrar por Usina e trocar de página

1. O ator aplica o filtro Usina = Usina Beta. [RN4]
2. O sistema exibe somente os painéis conectados aos inversores da Usina Beta: de PS-0113 a PS-0115.
3. O ator troca o filtro para Usina = Usina Alfa, com 10 por página.
4. O sistema informa a página 1 de 2 e o total de 12 registros.
5. O ator vai para a página 2.
6. O sistema exibe 2 painéis, ambos da Usina Alfa, e mantém o filtro aplicado. [RN5]

### A3 — Combinar filtros

1. O ator aplica Marca = SolarTech e Cooperado = João Lima.
2. O resultado esperado depende da Dúvida 2. Se os filtros se somarem, nenhum painel atende aos dois.

### A4 — Mostrar todos

1. O ator escolhe "Todos" na paginação.
2. O sistema exibe os 15 painéis em uma única página. [RN4]

### A5 — Sem resultados

1. O ator aplica o filtro Inversor = INV-005.
2. O sistema indica que não há resultados. [RN6]

### A6 — Falha ao obter a lista

1. O ator acessa a consulta enquanto o serviço que fornece a lista está indisponível, em ambiente preparado para essa situação.
2. O sistema indica a falha ao obter a lista e não a apresenta como lista vazia. [RN6]

## Regras de negócio

- **RN1 — Colunas (CA #24499):** a lista exibe Número de Série, Marca, Modelo, Inversor, Cooperado, Usina e Situação. A Usina é inferida pelo Inversor conectado ao painel.
- **RN2 — Ordem padrão (CA #24500):** a lista vem, por padrão, em ordem decrescente da data de inclusão/fabricação.
- **RN3 — Ordenação (CA #24501):** a listagem permite ordenar dinamicamente por colunas.
- **RN4 — Paginação e filtros (CA #24502 e CA #24503):** a paginação oferece 10, 50, 100 ou Todos. Os filtros disponíveis são Marca, Modelo, Cooperado, Usina, Fornecedor e Inversor. O filtro por Usina retorna os painéis conectados aos inversores dessa Usina.
- **RN5 — Informações da paginação (CA #35828):** a paginação informa a página atual, o total de páginas, o total de registros e a quantidade por página, e mantém os filtros ao trocar de página.
- **RN6 — Estados da consulta (CA #35829):** a consulta indica o carregamento em andamento, a ausência de resultados e a falha ao obter a lista.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Ação | Resultado esperado |
|---|---|
| Abrir a consulta | Indicação de carregamento e, em seguida, lista com as 7 colunas. |
| 10 por página | Página 1 de 2, 15 registros, 10 por página; a página 2 traz 5 painéis. |
| Ordenar por Número de Série (A1) | Lista reordenada pelo Número de Série. |
| Usina = Usina Beta (A2) | PS-0113 a PS-0115. |
| Usina = Usina Alfa com 10 por página (A2) | 12 registros em 2 páginas; na página 2, 2 painéis da Usina Alfa, com o filtro mantido. |
| Marca = SolarTech e Cooperado = João Lima (A3) | Depende da Dúvida 2. |
| Todos (A4) | 15 painéis em uma página. |
| Inversor = INV-005 (A5) | Indicação de ausência de resultados. |
| Lista indisponível (A6) | Indicação de falha, diferente da lista vazia. |

## Dúvidas para esclarecimento

1. O CA #24500 fala em "data de inclusão/fabricação". Qual das duas datas define a ordem padrão? E qual é o desempate quando dois painéis têm a mesma data?
2. O CA #24503 lista os filtros, mas não diz como eles atuam juntos. Quando dois filtros são aplicados, a lista deve mostrar só os painéis que atendem aos dois?
