# StayCore

Quarto projeto de estudo do Lucas, depois de ShopFlowAPI → DigitalBankAPI → EntregGo. Domínio: **sistema de reservas de hotel** (usuários do sistema, hóspedes, quartos, reservas). Roadmap em 6 sprints: `docs/ROADMAP.MD` — **sempre consultar primeiro** ao retomar.

## Método de trabalho

Igual aos projetos anteriores: **Lucas digita o código**, a IA explica o conceito, passa o roteiro e revisa linha a linha explicando o porquê de cada erro. Não escrever o código por ele.

## O que é novo aqui (objetivos de aprendizado)

Comparado aos projetos anteriores, o StayCore pratica:

1. **Alembic desde a configuração inicial do banco** (decisão de 2026-09-10): nada de `Base.metadata.create_all()`. Toda tabela nasce de uma migration versionada.
2. **Camada `service`** separada do `crud`: o `crud` só conversa com o banco, e o `service` guarda as regras de negócio (conflito de reserva, cálculo de valor, transições de status). Nos projetos anteriores o padrão era model→schema→crud→router.
3. **Autenticação de verdade**: hash de senha, login, JWT e roles (Sprint 2).
4. **Docker / docker-compose** para subir a API + PostgreSQL localmente.
5. **Regras de negócio com datas**: impedir reserva conflitante por sobreposição de períodos e calcular as diárias.
6. **`config.py` com pydantic-settings** em vez de ler `os.getenv` espalhado pelo código.

## Domínios (`backend/app/<domínio>/`)

Cada domínio tem `model.py`, `schema.py`, `crud.py`, `service.py` e `router.py`.

- **users**: quem **opera** o sistema (recepcionista, admin). Faz login e tem role. Não confundir com guest.
- **guests**: o **hóspede**. Não faz login, é só um cadastro.
- **rooms**: quarto, com número (único), tipo, capacidade, preço da diária e status.
- **reservations**: liga user (quem criou), guest e room, com `check_in`, `check_out`, status e valor total.

## Convenções herdadas (ShopFlowAPI/DigitalBank/EntregGo)

- 4 espaços de indentação; `__tablename__` em inglês e no plural.
- Funções no singular/plural conforme retornam um ou vários itens.
- Update sempre parcial (`exclude_unset=True`).
- 404 tratado em toda busca por id.
- `.env` nunca commitado.

## Fluxo de trabalho (decidido em 2026-10-06)

- Trabalho organizado **por sprints com datas**, gerenciado no **Jira**: uma Epic por sprint e uma task/story por item do roadmap.
- **Nunca commitar direto na `main`.** Cada task tem sua própria branch, que entra na `main` por Pull Request no GitHub.
- Nome da branch: `tipo/CHAVE-JIRA-descricao-curta` (ex.: `feature/SC-3-health-check`).
- Commits pequenos, no padrão Conventional Commits e com a chave do Jira: `feat(rooms): SC-12 cria model Room`. Tipos: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`.
- Ao fim de cada sprint: atualizar os checkboxes do `ROADMAP.MD` e criar uma tag (`v0.1.0` na Sprint 1, `v0.2.0` na Sprint 2, …).

## Status

- **2026-10-06**: só scaffolding. A estrutura de pastas e os arquivos existem, mas estão vazios, exceto `ROADMAP.MD` e `requirements.txt`. Ainda não há commits. Próximo passo: Sprint 1 (Fundação).
