FROM python:3.9-alpine as build
ENV POETRY_VIRTUALENVS_CREATE=false

WORKDIR /srv
RUN apk add --update gcc musl-dev libffi-dev openssl-dev make
RUN pip install --upgrade pip setuptools poetry

COPY pyproject.toml poetry.lock ./
RUN poetry install --no-dev
RUN poetry export -f requirements.txt -o requirements.txt --without-hashes
RUN pip wheel -w wheelhouse -r requirements.txt

EXPOSE 8000
COPY . .
RUN poetry build
FROM build as dev

WORKDIR /srv
RUN poetry install
CMD [ "uvicorn", "stkapi.server:app", "--host", "0.0.0.0", "--reload" ]

FROM python:3.9-alpine as prod
WORKDIR /srv
COPY --from=build /srv/wheelhouse/ ./wheelhouse
COPY --from=build /srv/dist/ ./dist
RUN pip install dist/*.whl --find-links ./wheelhouse

CMD [ "gunicorn", "--worker-class", "uvicorn.workers.UvicornWorker", "stkapi.server:app", "--bind", "0.0.0.0" ]

