# Configuração do Firebase

Este guia irá ajudá-lo a configurar o Firebase para o projeto GEM CCB.

## 1. Criar Projeto no Firebase

1. Acesse [Firebase Console](https://console.firebase.google.com/)
2. Clique em "Adicionar projeto"
3. Digite um nome para o projeto (ex: "gem-ccb-controle")
4. Siga os passos de configuração

## 2. Configurar Authentication

1. No console do Firebase, vá para "Authentication"
2. Clique em "Get started"
3. Vá para a aba "Sign-in method"
4. Habilite "Email/Password"
5. Clique em "Save"

## 3. Configurar Firestore Database

1. No console do Firebase, vá para "Firestore Database"
2. Clique em "Create database"
3. Escolha "Start in test mode" (você pode ajustar as regras depois)
4. Escolha a localização mais próxima (ex: us-central1)
5. Clique em "Done"

## 4. Configurar Storage (Opcional)

1. No console do Firebase, vá para "Storage"
2. Clique em "Get started"
3. Escolha "Start in test mode"
4. Escolha a localização mais próxima
5. Clique em "Done"

## 5. Obter Credenciais

1. No console do Firebase, clique na engrenagem (⚙️) ao lado de "Project Overview"
2. Selecione "Project settings"
3. Role para baixo até "Your apps"
4. Clique no ícone da web (</>)
5. Digite um nome para o app (ex: "gem-ccb-web")
6. Clique em "Register app"
7. Copie as credenciais que aparecem

## 6. Configurar o Projeto

1. Abra o arquivo `src/firebase/config.ts`
2. Substitua as configurações mockadas pelas suas credenciais reais:

```typescript
const firebaseConfig = {
  apiKey: "sua-api-key-aqui",
  authDomain: "seu-projeto.firebaseapp.com",
  projectId: "seu-projeto-id",
  storageBucket: "seu-projeto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

## 7. Configurar Regras do Firestore

1. No console do Firebase, vá para "Firestore Database"
2. Clique na aba "Rules"
3. Substitua as regras existentes pelo conteúdo do arquivo `firestore.rules`
4. Clique em "Publish"

## 8. Configurar Índices (Opcional)

1. No console do Firebase, vá para "Firestore Database"
2. Clique na aba "Indexes"
3. Adicione os índices definidos no arquivo `firestore.indexes.json`

## 9. Testar a Configuração

1. Execute `npm start` no projeto
2. Acesse `http://localhost:3000`
3. Tente criar uma conta
4. Verifique se os dados estão sendo salvos no Firestore

## 10. Deploy (Opcional)

Para fazer o deploy no Firebase Hosting:

1. Instale o Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Faça login:
```bash
firebase login
```

3. Inicialize o projeto:
```bash
firebase init
```

4. Build do projeto:
```bash
npm run build
```

5. Deploy:
```bash
firebase deploy
```

## Estrutura do Banco de Dados

O sistema criará automaticamente as seguintes coleções:

- **users**: Dados dos usuários (instrutores e alunos)
- **students**: Dados dos alunos
- **methodProgress**: Progresso nos métodos (MSA e instrumento)
- **msaTests**: Testes do MSA
- **hymnProgress**: Progresso nos hinos

## Segurança

As regras do Firestore garantem que:
- Apenas usuários autenticados podem acessar os dados
- Instrutores só veem seus próprios alunos
- Alunos só veem seu próprio progresso
- Dados sensíveis são protegidos

## Suporte

Se encontrar problemas:
1. Verifique se as credenciais estão corretas
2. Confirme se o Authentication está habilitado
3. Verifique se as regras do Firestore estão publicadas
4. Consulte os logs do console do navegador 