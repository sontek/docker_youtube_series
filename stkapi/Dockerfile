FROM python:3.9

WORKDIR /srv
RUN pip install --upgrade pip poetry
COPY pyproject.toml poetry.lock .
RUN poetry install
COPY . . 

EXPOSE 8000

CMD ["poetry", "run", "uvicorn", "stkapi.server:app", "--host", "0.0.0.0", "--reload"]

