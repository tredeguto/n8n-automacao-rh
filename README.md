---

## 📌 Parte 1: Triagem Inicial e Análise de Candidatos com IA

A primeira etapa foca em processar novas candidaturas, extrair informações de currículos e realizar uma pré-avaliação inteligente.

### Fluxo de Funcionamento
1. **Captura de Dados**: O fluxo é disparado no n8n ao identificar novos registros ou formulários preenchidos.
2. **Análise por IA**:
   * Processa o texto do currículo em PDF/DOCX.
   * Compara os requisitos da vaga com a experiência do candidato.
   * Gera uma **Análise IA** detalhada e um **Score** de aderência à vaga.
3. **Classificação e Registro**:
   * Atualiza a planilha no Google Sheets com o *status* recomendado (`Aprovado`, `Revisar` ou `Reprovado`).
4. **Encaminhamento**:
   * Candidatos com alta aderência são marcados diretamente para avanço de etapa.
   * Candidatos em zona limítrofe recebem o status `Revisar` para avaliação do Gestor de RH.

---

## 📌 Parte 2: Reavaliação de Candidato (Gestor RH)

A segunda parte permite que o gestor de RH revise manualmente os candidatos marcados como `Revisar`, garantindo que perfis promissores não sejam descartados injustamente pela triagem automática.

### Fluxo de Funcionamento
1. **Trigger de Atualização**: O n8n monitora alterações na planilha do Google Sheets (`rowUpdate`).
2. **Filtro de Elegibilidade**: Valida se o status é `revisar` e se há feedback no campo `Análise Humana`.
3. **Definição de Horários (Code Node)**:
   * Calcula automaticamente os horários disponíveis (*slots*) para a entrevista técnica/cultural.
4. **Agendamento no Google Calendar**:
   * Cria o evento de entrevista na agenda com o nome do candidato.
5. **Comunicação por E-mail**:
   * Envia um e-mail em HTML personalizado via Gmail informando a aprovação para a próxima etapa.
6. **Atualização Final**:
   * Grava a data/hora do agendamento e atualiza o status do candidato para `Aprovado` no Google Sheets.

---

## 📋 Estrutura da Planilha (Google Sheets)

Para o funcionamento correto dos fluxos, a planilha deve conter as seguintes colunas:

| Coluna | Descrição |
| :--- | :--- |
| `Candidato` | Nome completo do candidato |
| `e-mail` | Endereço de e-mail de contato |
| `Telefone` | Telefone / WhatsApp do candidato |
| `Curriculo` | Link para o arquivo do currículo |
| `Score` | Nota atribuída pela IA |
| `Análise IA` | Resumo dos pontos fortes e lacunas do candidato |
| `Análise Humana` | Parecer do Gestor de RH |
| `status` | Estado atual (`Pendente`, `Revisar`, `Aprovado`, `Reprovado`) |
| `Agendamento` | Data e hora da entrevista marcada |

---

## ⚙️ Configuração e Instalação

1. **Importação dos Workflows**:
   * Importe os arquivos JSON da **Parte 1** e **Parte 2** para o seu ambiente n8n.
2. **Configuração de Credenciais**:
   * Conecte suas contas do **Google Sheets**, **Google Calendar** e **Gmail** nas credenciais do n8n.
3. **Ajuste de Parâmetros**:
   * Atualize o `documentId` das planilhas nos nós do Google Sheets.
   * Ajuste o e-mail do calendário no nó do Google Calendar.
   * Adapte as datas e horários no nó `Config Agenda` conforme a disponibilidade da sua equipe.
4. **Ativação**:
   * Ative ambos os fluxos no n8n.
