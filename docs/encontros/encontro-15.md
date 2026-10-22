# Encontro 15 — Treinamento do Llama 3.2

## Tema

Ajuste do Llama 3.2 com exemplos de chamados, execução do treinamento em Docker e importação do modelo resultante no Ollama.

## Objetivos

- preparar exemplos de entrada e resposta;
- ajustar o Llama 3.2 para uma tarefa específica;
- executar o treinamento em um contêiner;
- importar o resultado no Ollama;
- comparar o modelo original com o modelo ajustado;
- utilizar o novo modelo no backend.

## O que será construído?

O modelo original continuará disponível:

```text
llama3.2:1b
```

Um segundo modelo será criado:

```text
llama3.2-suporte:latest
```

O novo modelo será ajustado para classificar chamados em:

- `ACESSO`;
- `FINANCEIRO`;
- `DOCUMENTOS`;
- `SISTEMA`.

```mermaid
flowchart LR
    D[Exemplos de chamados] --> T[Treinamento em Docker]
    L[Llama 3.2] --> T
    T --> N[Novo modelo]
    N --> O[Ollama]
    O --> C[Comparação com o original]
```

## Conceitos 

| Termo | Significado neste tutorial |
|---|---|
| modelo-base | Llama 3.2 original usado como ponto de partida |
| dataset | arquivo com exemplos de chamados e respostas esperadas |
| ajuste fino | continuação do treinamento para ensinar uma tarefa específica |
| LoRA | técnica que mantém o modelo-base congelado e treina apenas um pequeno adaptador |
| adaptador | arquivo pequeno que guarda o que foi aprendido no ajuste |
| modelo mesclado | modelo-base combinado com o adaptador, pronto para ser importado |

O modelo instalado pelo Ollama é preparado para responder prompts. Ele será usado na comparação, mas não será treinado diretamente. O contêiner de treinamento baixará o checkpoint equivalente, criará um adaptador e produzirá um modelo completo para o Ollama.

## Visão geral

```mermaid
flowchart TD
    A[1. Testar modelo original] --> B[2. Criar dataset]
    B --> C[3. Treinar adaptador]
    C --> D[4. Mesclar com Llama 3.2]
    D --> E[5. Importar no Ollama]
    E --> F[6. Repetir os testes]
    F --> G[7. Comparar resultados]
```

## Pré-requisitos

- Docker e Docker Compose;
- Ollama executado por Docker;
- `llama3.2:1b` instalado;
- processador de 64 bits com múltiplos núcleos;
- pelo menos 16 GB de RAM recomendados;
- espaço para o download e para o modelo resultante;
- conta no Hugging Face com acesso ao Llama 3.2.

### Treinamento em CPU


```mermaid
flowchart LR
    C[CPU do computador] --> D[Docker]
    D --> P[PyTorch para CPU]
    P --> L[Llama 3.2 1B + LoRA]
    L --> A[Adaptador treinado]
```

Treinar em CPU é mais lento que treinar em GPU. O resultado depende do processador, da RAM, do tamanho do dataset e do tamanho das sequências. O objetivo é demonstrar o fluxo completo com poucos exemplos, não realizar treinamento em larga escala.

O tutorial utiliza:

```text
meta-llama/Llama-3.2-1B-Instruct
```

No Ollama, utilize a variante correspondente:

```bash
docker compose exec ollama ollama pull llama3.2:1b
```

O checkpoint 1B não deve ser treinado e comparado com a variante 3B. Modelo-base, adaptador e baseline precisam pertencer à mesma variante.

## Estrutura dos arquivos

Na raiz do projeto, crie:

```text
treinamento/
├── Dockerfile
├── requirements.txt
├── train.py
├── merge.py
├── Modelfile
├── dataset/
│   ├── treino.jsonl
│   └── validacao.jsonl
└── output/
    ├── adapter/
    └── merged/
```

Adicione ao `.gitignore`:

```gitignore
.env
treinamento/output/
treinamento/.cache/
```

Não envie tokens, cache ou pesos do modelo ao GitHub.

## Passo 1 — registrar o comportamento do modelo original

Confirme o modelo:

```bash
docker compose exec ollama ollama list
```

No Thunder Client, crie:

```http
POST http://localhost:11434/api/chat
Content-Type: application/json
```

```json
{
  "model": "llama3.2:1b",
  "messages": [
    {
      "role": "user",
      "content": "Classifique o chamado usando ACESSO, FINANCEIRO, DOCUMENTOS ou SISTEMA: Minha conta foi bloqueada."
    }
  ],
  "stream": false,
  "options": {
    "temperature": 0.2,
    "seed": 42
  }
}
```

Salve a resposta. Repita com exemplos de todas as categorias. Esses casos serão usados novamente depois do treinamento e não devem ser copiados para o dataset.

## Passo 2 — obter acesso ao checkpoint

O modelo para treinamento será baixado do Hugging Face:

1. entre em sua conta;
2. abra a página `meta-llama/Llama-3.2-1B-Instruct`;
3. aceite os termos;
4. crie um token pessoal somente para leitura;
5. salve-o em `.env`.

```dotenv
HF_TOKEN=hf_substitua_pelo_token_pessoal
```

O token não deve aparecer no Dockerfile, no Compose, em prints ou commits.

## Passo 3 — criar os exemplos

Crie `treinamento/dataset/treino.jsonl`. Cada linha deve conter uma conversa completa:

```json
{"messages":[{"role":"system","content":"Classifique chamados. Responda com CATEGORIA e JUSTIFICATIVA curta."},{"role":"user","content":"Meu usuário foi bloqueado após várias tentativas."},{"role":"assistant","content":"CATEGORIA: ACESSO\nJUSTIFICATIVA: bloqueio de credenciais."}]}
{"messages":[{"role":"system","content":"Classifique chamados. Responda com CATEGORIA e JUSTIFICATIVA curta."},{"role":"user","content":"O boleto deste mês veio com valor incorreto."},{"role":"assistant","content":"CATEGORIA: FINANCEIRO\nJUSTIFICATIVA: divergência em cobrança."}]}
{"messages":[{"role":"system","content":"Classifique chamados. Responda com CATEGORIA e JUSTIFICATIVA curta."},{"role":"user","content":"Preciso de uma declaração de matrícula."},{"role":"assistant","content":"CATEGORIA: DOCUMENTOS\nJUSTIFICATIVA: solicitação de documento acadêmico."}]}
{"messages":[{"role":"system","content":"Classifique chamados. Responda com CATEGORIA e JUSTIFICATIVA curta."},{"role":"user","content":"O portal fecha quando envio o formulário."},{"role":"assistant","content":"CATEGORIA: SISTEMA\nJUSTIFICATIVA: falha durante operação do portal."}]}
```

Os quatro registros mostram o formato, mas são insuficientes para avaliar melhoria. Acrescente ao menos cinco exemplos diferentes por categoria.

Crie `treinamento/dataset/validacao.jsonl` com outros exemplos:

```json
{"messages":[{"role":"system","content":"Classifique chamados. Responda com CATEGORIA e JUSTIFICATIVA curta."},{"role":"user","content":"A recuperação de senha não envia o código."},{"role":"assistant","content":"CATEGORIA: ACESSO\nJUSTIFICATIVA: problema na recuperação de credenciais."}]}
{"messages":[{"role":"system","content":"Classifique chamados. Responda com CATEGORIA e JUSTIFICATIVA curta."},{"role":"user","content":"Não consigo abrir a página de notas."},{"role":"assistant","content":"CATEGORIA: SISTEMA\nJUSTIFICATIVA: indisponibilidade de funcionalidade do portal."}]}
```

Regras simples:

- não use dados pessoais reais;
- não coloque o mesmo exemplo nos dois arquivos;
- mantenha quantidade semelhante por categoria;
- revise categorias e justificativas.

## Passo 4 — preparar o contêiner

Crie `treinamento/requirements.txt`:

```text
transformers>=4.45,<5
datasets>=3,<5
peft>=0.13,<1
trl>=0.15,<1
accelerate>=1,<2
safetensors>=0.4,<1
sentencepiece>=0.2,<1
```

Crie `treinamento/Dockerfile`:

```dockerfile
FROM python:3.11-slim

WORKDIR /workspace

COPY requirements.txt .
RUN pip install --no-cache-dir torch --index-url https://download.pytorch.org/whl/cpu
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "train.py"]
```

Adicione o serviço `trainer` ao `compose.yaml`:

```yaml
services:
  ollama:
    volumes:
      - ollama-data:/root/.ollama
      - ./treinamento/output/merged:/models/llama32-suporte:ro
      - ./treinamento/Modelfile:/models/Modelfile:ro

  trainer:
    profiles: ["training"]
    build:
      context: ./treinamento
    environment:
      HF_TOKEN: ${HF_TOKEN}
      BASE_MODEL: meta-llama/Llama-3.2-1B-Instruct
    volumes:
      - ./treinamento:/workspace
      - huggingface-cache:/root/.cache/huggingface

volumes:
  ollama-data:
  huggingface-cache:
```

Preserve as demais configurações já existentes no serviço `ollama`. O serviço `trainer` não declara GPU e utiliza a imagem do PyTorch compilada para CPU.

Construa o contêiner e confirme que o PyTorch utilizará CPU:

```bash
docker compose --profile training build trainer
docker compose --profile training run --rm trainer \
  python -c "import torch; print(torch.__version__); print(torch.cuda.is_available())"
```

O segundo valor deve ser `False`. Isso confirma que o experimento não depende de CUDA.

## Passo 5 — criar o script de treinamento

Crie `treinamento/train.py`:

```python
import os
import torch
from datasets import load_dataset
from peft import LoraConfig
from transformers import AutoModelForCausalLM, AutoTokenizer
from trl import SFTConfig, SFTTrainer

MODEL_ID = os.getenv(
    "BASE_MODEL",
    "meta-llama/Llama-3.2-1B-Instruct",
)
TOKEN = os.environ["HF_TOKEN"]

dataset = load_dataset(
    "json",
    data_files={
        "train": "/workspace/dataset/treino.jsonl",
        "validation": "/workspace/dataset/validacao.jsonl",
    },
)

tokenizer = AutoTokenizer.from_pretrained(MODEL_ID, token=TOKEN)
if tokenizer.pad_token is None:
    tokenizer.pad_token = tokenizer.eos_token

model = AutoModelForCausalLM.from_pretrained(
    MODEL_ID,
    token=TOKEN,
    torch_dtype=torch.float32,
    device_map="cpu",
    low_cpu_mem_usage=True,
)
model.config.use_cache = False

lora = LoraConfig(
    r=16,
    lora_alpha=32,
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM",
    target_modules=["q_proj", "v_proj"],
)

config = SFTConfig(
    output_dir="/workspace/output/checkpoints",
    num_train_epochs=3,
    per_device_train_batch_size=1,
    per_device_eval_batch_size=1,
    gradient_accumulation_steps=4,
    learning_rate=2e-4,
    eval_strategy="epoch",
    save_strategy="epoch",
    logging_steps=1,
    max_length=256,
    use_cpu=True,
    bf16=False,
    fp16=False,
    dataloader_pin_memory=False,
    report_to="none",
    seed=42,
)

trainer = SFTTrainer(
    model=model,
    args=config,
    train_dataset=dataset["train"],
    eval_dataset=dataset["validation"],
    processing_class=tokenizer,
    peft_config=lora,
)

trainer.train()
trainer.model.save_pretrained("/workspace/output/adapter")
tokenizer.save_pretrained("/workspace/output/adapter")
```

O script:

1. abre o dataset;
2. carrega o Llama 3.2 1B na CPU;
3. treina o adaptador;
4. salva o resultado em `output/adapter`.

Não altere vários parâmetros ao mesmo tempo. Primeiro execute o exemplo e registre o resultado.

## Passo 6 — executar o treinamento

```bash
docker compose --profile training run --rm trainer python train.py
```

Ao terminar, confirme:

```bash
ls treinamento/output/adapter
```

Devem existir arquivos como:

- `adapter_config.json`;
- `adapter_model.safetensors`;
- arquivos do tokenizer.

Se a perda de treino diminuir enquanto a perda de validação piora, o modelo pode estar memorizando os exemplos.

## Passo 7 — gerar o modelo completo

Crie `treinamento/merge.py`:

```python
import os
import torch
from peft import PeftModel
from transformers import AutoModelForCausalLM, AutoTokenizer

MODEL_ID = os.getenv(
    "BASE_MODEL",
    "meta-llama/Llama-3.2-1B-Instruct",
)
TOKEN = os.environ["HF_TOKEN"]

base = AutoModelForCausalLM.from_pretrained(
    MODEL_ID,
    token=TOKEN,
    torch_dtype=torch.float32,
    device_map="cpu",
    low_cpu_mem_usage=True,
)

model = PeftModel.from_pretrained(
    base,
    "/workspace/output/adapter",
)
model = model.merge_and_unload()

model.save_pretrained(
    "/workspace/output/merged",
    safe_serialization=True,
    max_shard_size="2GB",
)

tokenizer = AutoTokenizer.from_pretrained(MODEL_ID, token=TOKEN)
tokenizer.save_pretrained("/workspace/output/merged")
```

Execute:

```bash
docker compose --profile training run --rm trainer python merge.py
```

O diretório `output/merged` conterá o modelo completo. Essa etapa pode utilizar bastante RAM.

## Passo 8 — importar no Ollama

Crie `treinamento/Modelfile`:

```text
FROM /models/llama32-suporte

PARAMETER temperature 0.2
PARAMETER num_ctx 4096

SYSTEM """
Você classifica chamados de suporte.
Use somente: ACESSO, FINANCEIRO, DOCUMENTOS ou SISTEMA.
Responda com CATEGORIA e JUSTIFICATIVA curta.
"""
```

Recrie o contêiner para aplicar os volumes e importe:

```bash
docker compose up -d --force-recreate ollama
docker compose exec ollama ollama create llama3.2-suporte -f /models/Modelfile
docker compose exec ollama ollama list
```

O novo nome deve aparecer sem remover o modelo original.

## Passo 9 — comparar os modelos

No Thunder Client, repita os testes do Passo 1 alterando somente `model`:

```json
{
  "model": "llama3.2-suporte",
  "messages": [
    {
      "role": "user",
      "content": "Classifique: Minha conta foi bloqueada."
    }
  ],
  "stream": false,
  "options": {
    "temperature": 0.2,
    "seed": 42
  }
}
```

Preencha:

| Caso | Esperado | Modelo original | Modelo ajustado |
|---|---|---|---|
| problema de senha | ACESSO |  |  |
| cobrança duplicada | FINANCEIRO |  |  |
| solicitação de histórico | DOCUMENTOS |  |  |
| erro no portal | SISTEMA |  |  |
| texto ambíguo | reconhecer dúvida |  |  |
| assunto fora do domínio | não inventar categoria |  |  |

Uma resposta conhecida do dataset não comprova melhoria. Utilize textos novos e variações de vocabulário.

## Usar no backend

Altere somente a configuração:

```yaml
environment:
  OLLAMA_MODEL: llama3.2-suporte
```

Recrie o backend:

```bash
docker compose up -d --force-recreate backend
```

O código NestJS continua igual porque o nome do modelo é fornecido pelo ambiente.


## Fontes oficiais

- [Llama 3.2 1B da Meta](https://huggingface.co/meta-llama/Llama-3.2-1B-Instruct)
- [Treinamento em CPU com Transformers](https://huggingface.co/docs/transformers/cpu)
- [Treinamento com TRL](https://huggingface.co/docs/trl/sft_trainer)
- [PEFT e LoRA](https://huggingface.co/docs/peft/quicktour)
- [Importação no Ollama](https://docs.ollama.com/import)
- [Modelfile do Ollama](https://docs.ollama.com/modelfile)
