# Команды для разработки и проверки качества кода ЛР1.
# Запускаются из папки lab_1: make <цель>, список целей: make help.
# Работает и на Windows (GNU make из winget), и на Linux (GitHub Actions).

ifeq ($(OS),Windows_NT)
PYTHON ?= py -3.12
VENV_PY := .venv/Scripts/python.exe
else
PYTHON ?= python3
VENV_PY := .venv/bin/python
endif

SETTINGS ?= settings.json
PRE_COMMIT_CONFIG := .pre-commit-config.yaml

.DEFAULT_GOAL := help
.PHONY: help install gen enc dec lint lint-fix format format-check typecheck \
	fullcheck precommit precommit-run clean

help:
	$(info make install        create .venv, install dependencies and dev tools)
	$(info make gen|enc|dec    generate keys / encrypt / decrypt, SETTINGS=$(SETTINGS))
	$(info make lint           ruff check)
	$(info make lint-fix       ruff check --fix)
	$(info make format         ruff format)
	$(info make format-check   ruff format --check)
	$(info make typecheck      pyright, strict mode)
	$(info make fullcheck      lint + format-check + typecheck)
	$(info make precommit      install pre-commit and pre-push git hooks)
	$(info make precommit-run  run all hooks on all files)
	$(info make clean          remove tool caches)
	@exit 0

$(VENV_PY):
	$(PYTHON) -m venv .venv

install: $(VENV_PY)
	$(VENV_PY) -m pip install --upgrade pip
	$(VENV_PY) -m pip install -r requirements-dev.txt

gen:
	$(VENV_PY) main.py --gen $(SETTINGS)

enc:
	$(VENV_PY) main.py --enc $(SETTINGS)

dec:
	$(VENV_PY) main.py --dec $(SETTINGS)

lint:
	$(VENV_PY) -m ruff check .

lint-fix:
	$(VENV_PY) -m ruff check --fix .

format:
	$(VENV_PY) -m ruff format .

format-check:
	$(VENV_PY) -m ruff format --check --diff .

typecheck:
	$(VENV_PY) -m pyright

fullcheck: lint format-check typecheck

precommit:
	$(VENV_PY) -m pre_commit install --config $(PRE_COMMIT_CONFIG) --allow-missing-config

precommit-run:
	$(VENV_PY) -m pre_commit run --config $(PRE_COMMIT_CONFIG) --all-files

clean:
	$(VENV_PY) -c "import shutil; shutil.rmtree('.ruff_cache', ignore_errors=True); shutil.rmtree('__pycache__', ignore_errors=True)"
