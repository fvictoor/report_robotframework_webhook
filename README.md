# 📉 Relatório de Testes para Webhook
Este documento detalha o funcionamento e o uso do script report_webhook.py, projetado para processar os resultados de testes do Robot Framework, gerar um relatório consolidado e enviá-lo para um endpoint de webhook.

## 🌐 Visão Geral
O script report_webhook.py automatiza o processo de notificação dos resultados de testes automatizados. Ele realiza as seguintes ações:

- Processa dois arquivos de resultado do Robot Framework (output.xml): um da execução principal e outro de uma re-execução (rerun).

- Coleta estatísticas detalhadas, como número total de testes, aprovados, reprovados e a lista de testes que falharam na re-execução.

- Agrega resultados por tags específicas, permitindo uma análise segmentada da suíte de testes.

- Formata um relatório de texto claro e informativo com todas as métricas coletadas.

- Envia o relatório para um URL de webhook configurado via variáveis de ambiente.

## 📋 Pré-requisitos
Antes de executar o script, é necessário configurar um arquivo .env na raiz do projeto para armazenar as variáveis de ambiente.

### 🧪 Variáveis de Ambiente
Crie um arquivo chamado .env e adicione as seguintes variáveis:

 
```bash
WEBHOOK_URL="INFORME AQUI SEU LINK DE WEBHOOK"
TAGS="INFORME PELO MENOS UMA TAG"
```

- O URL completo do webhook para onde o relatório será enviado.

https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX

- TAGS

Uma lista de tags (separadas por vírgula) que devem ser analisadas individualmente no relatório.


## 🚀 Uso
O script é executado através da linha de comando e requer a especificação dos caminhos para os arquivos de resultado.

Argumentos da Linha de Comando

- Caminho do XML de Execução
- Caminho do XML de Reexecução
- Titulo do Teste

Exemplo de Execução
```Bash

python report_webhook.py ./logs/output.xml ./logs/rerun_output.xml --titulo "🚀 Relatório de Testes - Projeto Phoenix"
```

## 🏷️ Estrutura do Body para o Webhook
O script envia uma requisição POST para o WEBHOOK_URL configurado. O corpo (body) da requisição é um JSON simples contendo uma única chave: ***content***

```json
{
  "content": "..."
}
```
content (string): Contém o relatório completo formatado como texto puro, com quebras de linha (\n) para estruturação.

Exemplo de Conteúdo do Body
O conteúdo da chave content será uma string formatada similar ao exemplo abaixo:

>📋 Relatório de Testes - Projeto Phoenix*
>
>🚀 Qtd. Total de Testes: 150
>
>✅ Qtd. Testes Aprovados (final): 145
>
>❌ Qtd. Testes Reprovados (inicial): 10
>
>🔁 Qtd. Testes que persistiram no erro (Rerun): 5
>
>⚠️ Testes que falharam no Rerun:*
>- Cenário de Login com Credenciais Inválidas
>- Validar API de Criação de Usuário
>- Teste de Conexão com Banco de Dados
>- Submeter Formulário sem Preencher Campos Obrigatórios
>- Verificar Geração de Relatório em PDF
>
>🚩 Resultados por TAG:
>
>🔖 Smoke
>  - Total: 71
>  - Passaram: 71
>  - Falharam: 0
>  - Tempo: 5m12s
>
>🔖 Regression
>  - Total: 58
>  - Passaram: 56
>  - Falharam: 2
>  - Tempo: 45m30s
>
>📊 Tempos de Execução:
>
>🕐 Tempo Execução Principal: 29m15s
>
>🔁 Tempo Execução Rerun: 1m16s
>
>⌛ Tempo Total: 30m31s