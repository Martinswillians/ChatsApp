# 💬 ChatsApp Família

App de mensagens e videochamada familiar no estilo WhatsApp, com Firebase como backend.

---

## 🚀 Como configurar o Firebase (gratuito)

### Passo 1 — Criar projeto Firebase
1. Acesse [console.firebase.google.com](https://console.firebase.google.com)
2. Clique em **"Adicionar projeto"**
3. Nomeie como `chatapp-familia` e clique em **Criar**

### Passo 2 — Ativar Authentication
1. No menu lateral → **Authentication** → **Começar**
2. Clique em **E-mail/senha** → ative a primeira opção → **Salvar**

### Passo 3 — Criar o Realtime Database
1. No menu lateral → **Realtime Database** → **Criar banco de dados**
2. Escolha a localização mais próxima (ex: us-central1)
3. Comece no **modo de teste** (pode ajustar as regras depois)

### Passo 4 — Pegar as credenciais
1. Clique no ícone de engrenagem ⚙️ → **Configurações do projeto**
2. Role até **"Seus aplicativos"** → clique no ícone **`</>`** (Web)
3. Registre o app com um nome
4. Copie o objeto `firebaseConfig` que aparecer

### Passo 5 — Colar no index.html
No arquivo `index.html`, localize este trecho e substitua:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDEMO_SUBSTITUA_COM_SUA_API_KEY",
  authDomain: "chatapp-familia.firebaseapp.com",
  databaseURL: "https://chatapp-familia-default-rtdb.firebaseio.com",
  projectId: "chatapp-familia",
  storageBucket: "chatapp-familia.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

Cole as suas credenciais reais do Firebase.

### Passo 6 — Regras do Database (OBRIGATÓRIO para adicionar contatos)
No console Firebase → Realtime Database → **Regras**, cole exatamente isso e clique em **Publicar**:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null",
        ".write": "auth != null && auth.uid == $uid"
      }
    },
    "email_index": {
      ".read": "auth != null",
      ".write": "auth != null"
    },
    "contact_requests": {
      "$uid": {
        ".read": "auth != null && auth.uid == $uid",
        ".write": "auth != null"
      }
    },
    "messages": {
      "$chatId": {
        ".read": "auth != null",
        ".write": "auth != null"
      }
    }
  }
}
```

> ⚠️ **Sem essas regras o erro "Permission denied" aparece ao adicionar familiar.**

### ⚠️ Usuários já cadastrados antes desta correção
Se você já criou contas, precisará recriar ou rodar este passo manual no **Console Firebase → Realtime Database → Dados**:
Adicione manualmente em `/email_index` a chave `seuemail,com` (troque `.` por `,`) com o valor do UID do usuário.

---

## ✨ Funcionalidades

| Feature | Status |
|---------|--------|
| Cadastro e login por e-mail | ✅ |
| Escolha de avatar emoji | ✅ |
| Lista de contatos familiares | ✅ |
| Adicionar familiar por e-mail | ✅ |
| Mensagens em tempo real | ✅ |
| Emojis no chat | ✅ |
| Indicador online/offline | ✅ |
| Videochamada (WebRTC) | ✅ |
| Modo demo (sem Firebase) | ✅ |
| Notificações de mensagem | ✅ |

---

## 📱 Como usar

1. Abra o `index.html` em qualquer navegador
2. Em modo demo, use qualquer e-mail + senha com 6+ caracteres
3. Com Firebase configurado, todos os familiares usam o mesmo app

## 🔧 Hospedar gratuitamente

Use o **Firebase Hosting**:
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

Ou simplesmente faça upload do `index.html` no **Netlify** arrastando o arquivo para [netlify.com/drop](https://netlify.com/drop).
