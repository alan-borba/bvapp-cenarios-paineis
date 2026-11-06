# CT003 — Seleção de Marca e Modelo no cadastro de painel solar

**Autor:** Alan Borba

## Objetivo

Verificar que o campo Marca lista apenas fabricantes de painéis ativos, em ordem alfabética, e que o campo Modelo mostra apenas os modelos da Marca selecionada.

## Requisitos relacionados

- **Feature #24258 — Inclusão de novos painéis solares**
  - CA #24484 — CA03: Marca lista apenas fabricantes de painéis ativos, em ordem alfabética
  - CA #24485 — CA04: Modelo filtrado pela Marca selecionada

## Ator

Usuário do BVapp com acesso à gestão de painéis solares.

## Pré-condições

- O ator está autenticado no BVapp.
- Estão cadastrados os fabricantes de painéis:
  - **SolarTech** (ativo), com os modelos **ST-550** e **ST-600**;
  - **Alfa Solar** (ativo), com o modelo **AS-400**;
  - **Brisa Painéis** (ativo), com o modelo **BP-300**;
  - **Velha Energia** (inativo), com o modelo **VE-100**.

## Fluxo principal

1. O ator acessa a página de inclusão de painel solar.
2. O ator abre a lista do campo Marca.
3. O sistema apresenta Alfa Solar, Brisa Painéis e SolarTech, nessa ordem. A Velha Energia não aparece. [RN1]
4. O ator seleciona SolarTech.
5. O ator abre a lista do campo Modelo.
6. O sistema apresenta apenas ST-550 e ST-600. [RN2]
7. O ator seleciona ST-550.

## Fluxos alternativos

### A1 — Trocar a Marca

1. Depois do passo 7, o ator troca a Marca para Alfa Solar.
2. O ator abre a lista do campo Modelo.
3. O sistema apresenta apenas AS-400. [RN2]
4. O ator verifica o que aconteceu com o Modelo ST-550 escolhido antes (ver Dúvida 2).

## Regras de negócio

- **RN1 — Lista de Marcas (CA #24484):** o campo Marca lista apenas fabricantes de painéis ativos no sistema, em ordem alfabética.
- **RN2 — Lista de Modelos (CA #24485):** o campo Modelo é filtrado com base na Marca selecionada.

## Dados de entrada e resultados esperados

Os valores são exemplos fictícios. Eles não criam limite nem regra nova.

| Ação | Resultado esperado |
|---|---|
| Abrir a lista de Marca | Alfa Solar, Brisa Painéis, SolarTech, nessa ordem; Velha Energia ausente. |
| Selecionar SolarTech e abrir Modelo | ST-550 e ST-600. |
| Trocar para Alfa Solar e abrir Modelo (A1) | AS-400. |

## Dúvidas para esclarecimento

1. O CA #24484 fala em "fabricantes de painéis ativos", mas não diz o que torna um fabricante "de painéis". É o fabricante que tem modelos de painel cadastrados ou existe outra marcação? Também precisa ficar claro que "ativo" se refere à situação do fabricante.
2. O CA #24485 não diz o que acontece com o Modelo já escolhido quando a Marca é trocada. Ele deve ser limpo?
