# 🔍 Azure AI Search: Indexação e Consulta Inteligente de Dados

Projeto com foco em **indexação semântica e consulta inteligente** de dados utilizando o poder do **Azure AI Search**. A aplicação explora os recursos avançados de **cognição**, **IA generativa**, **busca vetorial** e **integração com LLMs**, transformando dados brutos em respostas úteis, rápidas e contextuais.

---

## 🚀 Visão Geral

Este repositório demonstra como configurar e utilizar o **Azure AI Search** para:

- Indexar dados estruturados e não estruturados (como PDFs, JSON, CSV, bancos de dados SQL etc).
- Habilitar **busca semântica** e/ou **vetorial** (com embeddings).
- Integrar com **Azure OpenAI** ou **OpenAI API** para enriquecer as respostas com IA generativa.
- Criar pipelines de enriquecimento com **Skillsets** personalizados.

---

## 🛠️ Tecnologias que Podem ser Utilizadas

- [Azure AI Search](https://learn.microsoft.com/en-us/azure/search/)
- [Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/)
- Python (SDKs do Azure)
- Azure Blob Storage (para dados)
- Embeddings com `text-embedding-ada-002` ou similar
- Opcional: LangChain / Semantic Kernel / Streamlit (para interface e RAG)

---

## ⚙️ Passo a Passo: Configuração da Pesquisa

### 1. Criar e configurar o serviço do Azure AI Search

1. Acesse o [Portal do Azure](https://portal.azure.com)
2. Crie um novo recurso: `Azure AI Search`
3. Escolha o nível de pricing (S1+ permite recursos como busca semântica e vetorial)
4. Copie a **chave de administração** e a **URL do serviço**

---

### 2. Preparar os Dados

Você pode usar:

- Documentos PDF, Word, CSV
- Dados estruturados (SQL, Cosmos DB, etc)
- Dados não estruturados em Blob Storage

> 💡 Dica: Use o Azure Blob Storage para armazenar os arquivos a serem indexados.

---

### 3. Criar o Índice

Você pode criar o índice via portal, CLI ou programaticamente (recomendado):

```python
from azure.search.documents.indexes import SearchIndexClient
from azure.core.credentials import AzureKeyCredential
from azure.search.documents.indexes.models import SearchIndex, SimpleField, edm

credential = AzureKeyCredential("<SUA-CHAVE>")
client = SearchIndexClient(endpoint="<URL-DO-SEU-SERVIÇO>", credential=credential)

index = SearchIndex(
    name="meu-indice",
    fields=[
        SimpleField(name="id", type=edm.String, key=True),
        SimpleField(name="titulo", type=edm.String, searchable=True),
        SimpleField(name="conteudo", type=edm.String, searchable=True),
    ]
)

client.create_index(index)
```

---

### 4. Ingerir os Dados

Você pode utilizar:

- Push API (`upload_documents`)
- Azure Data Factory
- Skillsets automáticos com indexadores

```python
from azure.search.documents import SearchClient

search_client = SearchClient(endpoint="<URL>", index_name="meu-indice", credential=credential)

docs = [
    {"id": "1", "titulo": "Exemplo", "conteudo": "Texto indexado sobre IA e Azure Search"}
]

result = search_client.upload_documents(documents=docs)
```

---

### 5. Consultar os Dados

```python
results = search_client.search("IA no Azure", query_type="semantic")
for result in results:
    print(result)
```

Você pode configurar:

- **QueryType**: `simple`, `semantic`, `vector`
- **Filtros** por metadata
- **Boosting** por campo
- **Respostas geradas por IA** (se usar Azure OpenAI)

---

## 🤖 Integração com LLMs (RAG)

Combine com modelos da OpenAI para implementar RAG (Retrieval-Augmented Generation):

```python
# Exemplos com LangChain ou Semantic Kernel para:
# 1. Buscar documentos relevantes no Azure Search
# 2. Gerar respostas contextualizadas com GPT
```

---

## 🧠 Insights e Possibilidades

O Azure AI Search vai muito além de uma simples busca textual. Aqui estão algumas aplicações práticas:

| Aplicação | Benefícios |
|----------|------------|
| **Chatbots corporativos** | Busca precisa em documentos internos com respostas generativas |
| **Pesquisa jurídica** | Indexação de jurisprudências e leis para consultas semânticas |
| **E-commerce** | Busca por similaridade e recomendação de produtos |
| **Suporte técnico** | Consulta em base de conhecimento e manuais PDF |
| **Análise de documentos legais ou contratos** | Busca vetorial e sumarização com IA |

---

## 🛠️ Ferramentas que se beneficiam dessa integração

- **Power BI**: Para dashboards com dados indexados e enriquecidos
- **Microsoft Teams + Bots**: Chatbots corporativos integrados com AI Search
- **Aplicações Web** com Python, Node.js, React
- **LangChain / Semantic Kernel**: Para workflows de RAG com IA generativa
- **Elastic Stack (via conector)**: Integração com ferramentas já usadas em empresas

---

## 📚 Referências

- [Documentação oficial do Azure AI Search](https://learn.microsoft.com/azure/search/)
- [Azure OpenAI + Search Pattern](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/ai/semantic-search-openai)
- [GitHub Microsoft - Azure Search Samples](https://github.com/Azure-Samples/azure-search-openai-demo)

---

## 🤝 Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou PRs com melhorias, exemplos ou dúvidas.

---

## 📄 Licença

Distribuído sob a licença MIT. Veja `LICENSE` para mais detalhes.

