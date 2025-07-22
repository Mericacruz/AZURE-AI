# AZURE-AI - FEITO COM AJUDA DA IA

from azure.core.credentials import AzureKeyCredential
from azure.ai.documentintelligence import DocumentAnalysisClient

# Substitua pelos seus dados
endpoint = "https://<SEU_ENDPOINT>.cognitiveservices.azure.com/"
key = "<SUA_CHAVE_API>"

# Cria o cliente
client = DocumentAnalysisClient(endpoint=endpoint, credential=AzureKeyCredential(key))

# Caminho para o documento (pode ser imagem, PDF, etc.)
file_path = "documento_sensivel.pdf"

with open(file_path, "rb") as f:
    poller = client.begin_analyze_document("prebuilt-document", document=f)
    result = poller.result()

# Exibe os campos analisados
for page in result.pages:
    print(f"\nPágina {page.page_number}")
    for line in page.lines:
        print(line.content)

print("\nCampos extraídos:")
for kv in result.key_value_pairs:
    key = kv.key.content if kv.key else "N/A"
    value = kv.value.content if kv.value else "N/A"
    print(f"  - {key}: {value}")

# Exemplo de verificação simples de padrão (ex: campo obrigatório ausente)
campos_esperados = ["CNPJ", "Assinatura", "Data"]
for campo in campos_esperados:
    if not any(campo.lo
