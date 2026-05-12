# MarkItDown Web Converter

Aplicação web simples para converter arquivos em Markdown usando uma interface de upload/download.

> **Importante:** a conversão é feita **exclusivamente** pela biblioteca **MarkItDown**. A aplicação Flask apenas recebe o arquivo, chama o MarkItDown e devolve o resultado para download.

## Visão geral do produto

Este projeto oferece uma camada web mínima sobre o MarkItDown para facilitar o fluxo:

1. enviar arquivo;
2. converter para Markdown;
3. baixar o `.md` gerado.

A proposta é reduzir atrito para uso local e testes rápidos de conversão sem interação direta com scripts CLI.

## Pré-requisitos

- Python 3.10+ (recomendado 3.11 ou 3.12)
- `pip`
- Ambiente virtual (fortemente recomendado)

## Instalação

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows PowerShell

pip install -r requirements.txt
```

### Dependências mínimas

O arquivo `requirements.txt` foi mantido propositalmente enxuto:

- `Flask`: servidor web e rotas da interface.
- `markitdown`: mecanismo de conversão de arquivos para Markdown.

Nenhuma dependência opcional extra foi adicionada neste momento.

## Execução local

Exemplo padrão com Flask:

```bash
export FLASK_APP=app.py
export FLASK_ENV=development
flask run
```

Depois, abra no navegador:

- `http://127.0.0.1:5000`

> Se o projeto usar outro ponto de entrada (por exemplo `main.py`), ajuste o `FLASK_APP` conforme o arquivo real.

## Uso passo a passo

1. **Upload**
   - Abra a página da aplicação.
   - Clique para selecionar um arquivo suportado.

2. **Converter**
   - Acione o botão de conversão.
   - O backend envia o arquivo para o `MarkItDown`.

3. **Baixar**
   - Ao concluir, faça o download do arquivo `.md` gerado.

## Formatos suportados (MarkItDown no ambiente)

Com base na instalação validada no ambiente (`markitdown==0.1.5`), os conversores embutidos aceitam, entre outros:

- Documentos: `.pdf`, `.docx`, `.pptx`, `.xlsx`, `.xls`, `.epub`, `.msg`
- Dados/texto: `.txt`, `.text`, `.md`, `.markdown`, `.json`, `.jsonl`, `.csv`, `.ipynb`
- Imagens/áudio: `.jpg`, `.jpeg`, `.png`, `.wav`, `.mp3`, `.m4a`, `.mp4`
- Web/feed/containers: `.html`, `.htm`, `.rss`, `.atom`, `.xml`, `.zip`

> A disponibilidade prática de alguns tipos pode depender de bibliotecas auxiliares e qualidade/estrutura do arquivo de entrada.

## Limitações conhecidas e mensagens de erro esperadas

### Limitações

- Arquivos fora dos tipos suportados não serão convertidos.
- Arquivos corrompidos, protegidos por senha ou muito fora do padrão podem falhar.
- Conversões de imagem/áudio dependem de extração textual (podendo retornar pouco conteúdo).
- O resultado em Markdown pode exigir ajustes manuais para casos complexos de layout.

### Mensagens de erro esperadas

Erros comuns esperados ao usar o mecanismo de conversão:

- Formato não suportado:
  - `Could not convert stream to Markdown. No converter attempted a conversion, suggesting that the filetype is simply not supported.`
- URI inválida/esquema não suportado (quando aplicável):
  - `Unsupported URI scheme ...`
  - `Unsupported file URI ...`

Na camada web, também são esperados erros de validação como:

- nenhum arquivo enviado;
- nome de arquivo vazio;
- falha interna durante processamento.

## Como validar funcionamento

Checklist rápido de validação local:

1. Suba a aplicação Flask.
2. Envie um arquivo simples (`.txt` ou `.md`) e confirme download do `.md`.
3. Envie um arquivo suportado adicional (ex.: `.pdf` ou `.docx`) e confirme conversão sem erro.
4. Envie um tipo não suportado (ex.: `.exe`) e confirme mensagem de erro amigável.
5. Verifique logs para garantir que exceções foram tratadas sem derrubar o servidor.

## Estrutura do projeto

Estrutura atual do repositório:

```text
.
├── README.md           # documentação de uso, limitações e validação
└── requirements.txt    # dependências mínimas para rodar o projeto
```

Quando novos arquivos forem adicionados (ex.: `app.py`, `templates/`, `static/`), recomenda-se atualizar esta seção para facilitar manutenção.
