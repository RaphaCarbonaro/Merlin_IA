## Passo a Passo da Execução

### 1. Instalar Ollama

```bash
# Baixar em: ollama.com
ollama pull gpt-oss
ollama serve
```

### 2. Instalar Dependências

```bash
pip install streamlit pandas requests
```

### 3. Rodar o Merlin

```bash
python -m streamlit run src/app.py
```

## Código completo

Todo o código-fonte se encontra no `app.py`.

## Estrutura do requirements.txt

```
pip install streamlit pandas requests
 ```

## Estrutura Sugerida

```
src/
├── app.py              # Aplicação principal (Streamlit)
└── requirements.txt    # Dependências
```

## Como Rodar

```bash
# Instalar dependências
pip install -r requirements.txt

# Rodar a aplicação
python -m streamlit run src/app.py
```
