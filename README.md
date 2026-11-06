# BVapp — Cenários de Teste de Painéis Solares

**Autor:** Alan Borba  
**Disciplina:** Verificação, Validação e Teste de Software — IFPB, Campus João Pessoa  
**Professor:** Maxwell Anderson Ielpo do Amaral

## Apresentação

Este repositório individual organiza os cenários de teste funcionais do épico **Gestão de Painéis Solares (#24189)**, que faz parte do módulo Gestão de Ativos do BVapp. O material foi feito para a atividade "Elaboração de cenários de teste do BVapp".

## Objetivo

Documentar cenários de teste ligados aos requisitos funcionais de painéis solares, de forma que possam ser aproveitados em etapas futuras de implementação e automação. Nesta etapa não há implementação, interface, automação nem execução de testes.

## Fonte dos requisitos

Os requisitos vêm do OpenProject do BVapp, épico #24189, consultado em 06/11/2026, depois da revisão feita no walkthrough da equipe. Cada cenário cita as Features, US e CAs pelo ID do OpenProject.

Os resultados esperados seguem o texto dos requisitos, e não o comportamento atual do sistema. Quando um requisito está ausente ou é contraditório, o cenário registra a dúvida para esclarecimento com os stakeholders, em vez de escolher uma regra.

## Escopo

São dez cenários, e o CT001 reúne as quatro operações do CRUD (cadastrar, consultar, atualizar e excluir) num fluxo completo. Os outros cenários detalham as regras de cada Feature:

- **Inclusão:** campos obrigatórios, Marca e Modelo, Inversor e Usina, Número de Série e Potência, datas e garantias;
- **Consulta:** filtros, ordenação e paginação, e a visualização detalhada;
- **Atualização:** alteração da situação entre Ativo e Inativo;
- **Exclusão:** casos em que a exclusão é recusada.

Ficam fora destes dez cenários a Feature #35845 (Exportação de painéis solares) e os registros de log das operações (CAs #24492, #24514 e #24524).

## Catálogo de cenários

| ID | Cenário | Feature | Requisitos principais | Documento |
|---|---|---|---|---|
| CT001 | Manter painéis solares (CRUD completo) | #24258, #24260, #24261, #24262 | US #24495, #24506, #24516, #24526 | [CT001](functional_tests/crud/CT001-manter-paineis-solares.md) |
| CT002 | Campos obrigatórios no cadastro | #24258 | US #24495; CAs #24482, #24483, #24490 | [CT002](functional_tests/inclusao/CT002-campos-obrigatorios.md) |
| CT003 | Seleção de Marca e Modelo | #24258 | CAs #24484, #24485 | [CT003](functional_tests/inclusao/CT003-marca-e-modelo.md) |
| CT004 | Inversor conectado e Usina herdada | #24258 | CAs #24488, #24491, #35825 | [CT004](functional_tests/inclusao/CT004-inversor-e-usina.md) |
| CT005 | Número de Série e Potência de Pico | #24258 | CAs #35822, #35823 | [CT005](functional_tests/inclusao/CT005-numero-de-serie-e-potencia.md) |
| CT006 | Datas e garantias | #24258, #24261 | CAs #35821, #35824, #35839 | [CT006](functional_tests/inclusao/CT006-datas-e-garantias.md) |
| CT007 | Filtros, ordenação e paginação | #24260 | US #24506, #24507; CAs #24499 a #24503, #35828, #35829 | [CT007](functional_tests/consulta/CT007-filtros-ordenacao-paginacao.md) |
| CT008 | Visualização detalhada | #24260 | US #25894; CAs #35830 a #35833 | [CT008](functional_tests/consulta/CT008-visualizacao-detalhada.md) |
| CT009 | Ativar e inativar painel | #24261 | US #29053; CAs #35835, #35836 | [CT009](functional_tests/atualizacao/CT009-ativar-inativar-painel.md) |
| CT010 | Exclusão recusada | #24262 | US #24526; CAs #24519 a #24522, #35842, #35844 | [CT010](functional_tests/exclusao/CT010-exclusao-recusada.md) |

## Estrutura de diretórios

```text
bvapp-cenarios-paineis/
├── README.md
└── functional_tests/
    ├── README.md
    ├── crud/
    │   └── CT001-manter-paineis-solares.md
    ├── inclusao/
    │   ├── CT002-campos-obrigatorios.md
    │   ├── CT003-marca-e-modelo.md
    │   ├── CT004-inversor-e-usina.md
    │   ├── CT005-numero-de-serie-e-potencia.md
    │   └── CT006-datas-e-garantias.md
    ├── consulta/
    │   ├── CT007-filtros-ordenacao-paginacao.md
    │   └── CT008-visualizacao-detalhada.md
    ├── atualizacao/
    │   └── CT009-ativar-inativar-painel.md
    └── exclusao/
        └── CT010-exclusao-recusada.md
```

A pasta `functional_tests/` corresponde à pasta de testes funcionais da estrutura apresentada na Aula 06. Os cenários estão em Markdown, organizados por Feature. Quando o sistema for implementado e os testes forem automatizados, cada cenário pode dar origem a um arquivo de teste na mesma pasta.

## Como ler um cenário

Cada arquivo segue o formato do exemplo "UC001 — Manter produtos" da Aula 06 e traz:

- identificador e título;
- autor;
- objetivo;
- requisitos relacionados;
- ator e pré-condições;
- fluxo principal;
- fluxos alternativos (A1, A2...);
- regras de negócio (RN1, RN2...), cada uma com o CA de origem;
- dados de entrada e resultados esperados;
- dúvidas para esclarecimento, quando houver.

Os passos indicam entre colchetes a regra ou o fluxo alternativo que se aplica, como `[RN1]` ou `[A1]`. Os dados são fictícios e servem só de exemplo, sem criar limite ou regra nova.

## Autoria

Todos os dez cenários foram elaborados por **Alan Borba**.
