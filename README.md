Acessar:
https://bresodev.github.io/webCarteira

# 📄 Documentação da Página Web: Carteira WEB V3

## Índice

- [📌 Descrição Geral](#-descrição-geral)
- [🚀 Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [📁 Estrutura de Arquivos](#-estrutura-de-arquivos)
- [🧩 Funcionalidades](#-funcionalidades)
- [🔐 Criptografia Utilizada](#-criptografia-utilizada)
- [📦 IndexedDB (Armazenamento)](#-indexeddb-armazenamento)
- [📲 Service Worker e Manifest](#-service-worker-e-manifest)
- [💻 Interface de Usuário](#-interface-de-usuário)
- [🧪 Recursos Especiais para Desenvolvedor](#-recursos-especiais-para-desenvolvedor)
- [🔧 Como Rodar e Testar](#-como-rodar-e-testar)
- [📌 Considerações de Segurança](#-considerações-de-segurança)
- [📎 Licença](#-licença)

---

## 📌 Descrição Geral

A **Carteira WEB V3** é uma aplicação web PWA (Progressive Web App) que permite armazenar documentos pessoais criptografados localmente no navegador utilizando imagens (em Base64) e IndexedDB. A descriptografia depende de uma chave inserida pelo usuário, garantindo privacidade e controle.

---

## 🚀 Tecnologias Utilizadas

- **HTML5/CSS3/JavaScript**
- **IndexedDB** – Armazenamento local no navegador
- **Base64 + XOR** – Para criptografia simples de imagens
- **Service Worker** – Funcionalidade PWA offline
- **Manifest Web App** – Instalação como app mobile/desktop
- **EventListeners e DOM Manipulation** – Para interatividade

---

## 📁 Estrutura de Arquivos

```text
/
├── index.html              # Página principal
├── manifest.json           # Configuração PWA
├── service-worker.js       # Arquivo do Service Worker
├── 181534.png              # Ícone da aplicação
```

---

## 🧩 Funcionalidades

### ➕ Adicionar Documento
- Botão: **"+ Adicionar novo documento"**
- Inputs:
  - Chave de segurança
  - Nome do documento (ex: CPF, RG)
  - Upload da imagem
- A imagem é convertida para Base64, criptografada via XOR e salva no `IndexedDB`.

### 🔍 Buscar Documento
- Campo de busca filtra documentos pelo nome em tempo real.

### 🧾 Listar Documentos Salvos
- Cada item mostra:
  - Nome (em um `<details>`)
  - Campo para inserir senha
  - Botão para descriptografar
  - Botão ❌ para excluir

### 🖼️ Visualizar Documento
- Se a senha for correta, o documento descriptografado aparece na tela como imagem.

### 🗑️ Excluir Documento
- Confirmação de exclusão antes de apagar do IndexedDB.

---

## 🔐 Criptografia Utilizada

- O algoritmo utilizado é **XOR Simples**, com a chave fornecida pelo usuário.
- O conteúdo da imagem é convertido para **Base64**, criptografado com XOR e convertido novamente com `btoa`.

```js
function xorEncryptDecrypt(data, key) {
  let r = '';
  for (let i = 0; i < data.length; i++)
    r += String.fromCharCode(data.charCodeAt(i) ^ key.charCodeAt(i % key.length));
  return r;
}
```

⚠️ **Atenção:** Embora funcional, esse tipo de criptografia é fraca e **não segura para uso em produção com dados sensíveis**.

---

## 📦 IndexedDB (Armazenamento)

- Nome do banco: `DocsDB`
- Object Store: `docs`
- Campos salvos:
  - `title`: Nome do documento
  - `data`: Imagem criptografada (string Base64)

---

## 📲 Service Worker e Manifest

- O app registra um **Service Worker** que permite funcionamento **offline**.
- O arquivo `manifest.json` permite que o app seja instalado em dispositivos móveis ou desktops como um **aplicativo nativo**.

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('service-worker.js')
    .then(() => console.log('✅ Service Worker registrado!'))
    .catch((err) => console.error('Erro ao registrar o SW:', err));
}
```

---

## 💻 Interface de Usuário

### Estilo:
- Tema escuro (`background-color: #121212`)
- Estilo responsivo para dispositivos móveis
- Inputs, botões e modais customizados com CSS puro

### Elementos principais:
- `#addBtn`: Botão de adicionar documento
- `#searchInput`: Campo de busca
- `#savedItems`: Lista dos documentos
- `#outputImage`: Exibição da imagem descriptografada
- Modal `#modalAdd`: Interface de adição de novo documento

---

## 🧪 Recursos Especiais para Desenvolvedor

- Um modo "DEV oculto" é ativado ao clicar 5 vezes rapidamente no ícone da logo.

```js
let ct = 0;
document.getElementById('abrirDEV').addEventListener('click', () => {
  ct++;
  if (ct >= 5) document.getElementById('abrirDEV').style.filter = 'none';
});
```

---

## 🔧 Como Rodar e Testar

1. **Abra o `index.html`** em qualquer navegador moderno.
2. **Permita armazenamento local (IndexedDB)**.
3. Clique em `+ Adicionar novo documento`.
4. Preencha os campos e salve.
5. Faça buscas ou visualize seus documentos criptografados.

> ⚠️ Arquivos são armazenados apenas **localmente** (não há backend).

---

## 📌 Considerações de Segurança

- A criptografia XOR é **apenas para fins educacionais ou protótipos**.
- Para um sistema de produção, use:
  - AES ou outra cifra segura (via Web Crypto API)
  - Armazenamento seguro com autenticação
  - Validação de arquivos
- Atualmente, **qualquer um com acesso ao navegador e à senha pode ver os documentos**.

---

## 📎 Licença

Este projeto é fornecido sem garantia de segurança ou suporte. Livre para uso pessoal e educacional.

  

