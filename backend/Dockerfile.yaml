FROM python:3.10-slim

#install curl and other dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# install poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Add Poetry to PATH
ENV PATH="${PATH}:/root/.local/bin"

# set working directory
WORKDIR /app

# copy only the pyproject.toml and poetry.lock files
COPY pyproject.toml poetry.lock* ./

# install dependencies using poetry
RUN poetry install --no-root

# copy the rest of the application to the container
COPY . .

#Make the prestart script executable
RUN chmod +x ./prestart.sh

#Expose port 8000
EXPOSE  8000

CMD ["poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
