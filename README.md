Claro! Aqui está uma documentação completa e organizada para o seu workflow GitHub Actions com integração ao ServiceNow DevOps:

---

## 📄 Documentação: `ChangeDevOps Simulation`

### 📌 Objetivo

Esse workflow simula um pipeline de CI/CD integrado ao **ServiceNow DevOps**, utilizando um fluxo de mudanças automatizadas. A automação inclui etapas de build, desenvolvimento, homologação, criação de change no ServiceNow e implantação em produção.

---

### ⚙️ Disparo Manual

```yaml
on:
  workflow_dispatch:
```

Este pipeline é **executado manualmente** por meio do GitHub Actions UI.

---

### 🧱 Estrutura do Workflow

| Job                      | Descrição                                                    | Dependência              |
| ------------------------ | ------------------------------------------------------------ | ------------------------ |
| `build`                  | Compila o aplicativo (simulado com echo)                     | -                        |
| `desenvolvimento`        | Realiza o deploy em DEV (simulado)                           | `build`                  |
| `homologacao`            | Realiza o deploy em HOMOLOGAÇÃO (simulado)                   | `desenvolvimento`        |
| `ServiceNowDevOpsChange` | Cria automaticamente uma requisição de mudança no ServiceNow | `homologacao`            |
| `producao`               | Realiza o deploy final em PRODUÇÃO (simulado)                | `ServiceNowDevOpsChange` |

---

### 🔄 Integração com ServiceNow

A integração é feita com o action oficial:

```yaml
uses: ServiceNow/servicenow-devops-change@v6.0.0
```

#### 🔑 Entradas usadas:

* `devops-integration-token`: Token armazenado no **Secrets**.
* `instance-url`, `tool-id`: Definidos como **Variables**.
* `context-github`: Contexto JSON da execução.
* `change-request`: Payload com os dados da requisição de mudança, incluindo:

  * `requested_by`: Nome do usuário solicitante
  * `short_description`, `description`
  * Planos: implementação, rollback e testes
  * Comentários e work notes

#### 📤 Saídas:

* `change-request-number`
* `change-request-sys-id`

Estes valores são enviados como output para rastreio posterior.

---

### 🧪 Exemplo de `change-request` payload

```json
{
  "setCloseCode": "true",
  "attributes": {
    "requested_by": { "name": "DevOps Integration User" },
    "priority": "2",
    "short_description": "DevOps Git ServiceNowDummyGitDeploy",
    "description": "Aguardando aprovação no ServiceNow",
    "implementation_plan": "Procedimento CICD DevOps",
    "backout_plan": "Restaurar versão anterior",
    "test_plan": "Testado em ambiente de homologação",
    "comments": "This is a sample pipeline script to be added in your change step",
    "work_notes": "Update this to work_notes"
  }
}
```

---

### 🧯 Segurança

* ⚠️ **Nunca exponha tokens** no código.
* Use `secrets` e `vars` para valores sensíveis.
* Habilite permissões mínimas no repositório.

---

### ✅ Requisitos

* Acesso à instância ServiceNow com DevOps Change instalado.
* Token de integração configurado no ServiceNow.
* Conector e Tool ID criados e ativos.

---

Se quiser, posso montar essa doc como arquivo `.md` ou já criar um template pronto pra README. Só chamar! 💪📘



