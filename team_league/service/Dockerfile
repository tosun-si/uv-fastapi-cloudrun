FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim AS builder

ENV UV_COMPILE_BYTECODE=1 \
    UV_LINK_MODE=copy \
    WORKDIR=/usr/local/src/app

WORKDIR $WORKDIR

COPY pyproject.toml uv.lock ./

RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-install-project

COPY team_league $WORKDIR/team_league

FROM python:3.12-slim

ENV WORKDIR=/usr/local/src/app
WORKDIR $WORKDIR

COPY --from=builder $WORKDIR $WORKDIR

ENV PATH="$WORKDIR/.venv/bin:$PATH"

RUN adduser --disabled-password --gecos '' --home /home/teamleagueuser teamleagueuser && \
    chown -R teamleagueuser $WORKDIR

USER teamleagueuser

ENTRYPOINT ["uvicorn"]
CMD ["team_league.service.main:app", "--host", "0.0.0.0", "--port", "8080"]

