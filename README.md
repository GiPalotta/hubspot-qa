# HubSpot API Q\&A

Este projeto implementa um sistema de **Perguntas e Respostas sobre a API da HubSpot**, utilizando **RAG (Retrieval Augmented Generation)** e um agente de IA conectado a um **workflow no n8n**.

O objetivo é permitir que desenvolvedores consultem a documentação pública da HubSpot de forma interativa, obtendo respostas claras, úteis e sem alucinações.

---

## 📌 Objetivos Técnicos

* Disponibilizar uma **interface web pública** (via GitHub Pages) para interação com o agente.
* Implementar **RAG com banco vetorial (Qdrant)** contendo conteúdos públicos sobre a API da HubSpot.
* Usar **embeddings da OpenAI** para indexação e busca semântica.
* Gerar respostas usando um **LLM (OpenAI GPT)** baseado nos dados recuperados.
* Retornar uma resposta **completa, concisa e sem inventar informações**, indicando *“Não encontrei isso na documentação da HubSpot”* caso o conteúdo não esteja disponível.

---

## 🚀 Como funciona

1. O desenvolvedor acessa a página web e digita uma pergunta.

2. A pergunta é enviada automaticamente para um **Webhook do n8n**:

   ```
   https://n8n.heavensttwr.com.br/webhook/015bf0b0-754d-42d7-a1ce-a216b4136d48
   ```

3. O workflow do n8n processa a requisição:

   * Recebe a pergunta do usuário.
   * Gera embeddings com `text-embedding-3-small` (OpenAI).
   * Realiza a busca semântica no **Qdrant** para recuperar trechos relevantes da documentação.
   * Monta o contexto e envia ao **AI Agent** (OpenAI Chat Model).
   * O agente gera a resposta final com base nos dados encontrados.

4. O usuário recebe a resposta diretamente na página.

---

## 🖥️ Interface Web

A interface é um HTML simples hospedado via **GitHub Pages**, contendo:

* Um campo de texto para digitar a pergunta.
* Um botão para enviar a questão ao webhook.
* Uma área para exibir a resposta do agente.

O título da página é **“HubSpot API Q\&A”** e a URL do webhook já está fixa no código.

---

## 📂 Estrutura do repositório

* `index.html` → Interface web para interação com o agente.
* `README.md` → Este documento explicativo.

---

## 🌍 Publicação via GitHub Pages

1. Vá até **Settings → Pages** no repositório.
2. Em **Source**, selecione:

   * Branch: `main`
   * Folder: `/ (root)`
3. Clique em **Save**.
4. O GitHub Pages gerará automaticamente uma URL do tipo:

   ```
   https://SEU-USUARIO.github.io/NOME-DO-REPO/
   ```

---

## ⚠️ Notas

* Se houver erro de **CORS** ao chamar o webhook, configure no servidor n8n a variável de ambiente:

  ```bash
  N8N_CORS_ALLOW_ORIGIN=*
  ```

  ou configure o nó **Respond to Webhook** para incluir headers:

  * `Access-Control-Allow-Origin: *`
  * `Access-Control-Allow-Headers: Content-Type`

---

✍️ Desenvolvido como solução para o **case técnico de integração com a documentação da HubSpot API**.
