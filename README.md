# GEM CCB - Controle de Progresso Musical

Sistema web completo para gerenciar o progresso de estudo musical dos alunos da Congregação Cristã no Brasil (CCB).

## 🎵 Funcionalidades

### Para Instrutores:
- **Dashboard** com visão geral dos alunos e progresso
- **Gestão de Alunos** - cadastro, edição e visualização
- **Acompanhamento de Progresso** nos métodos (MSA e instrumento)
- **Controle de Hinos** - registro de progresso nos 480 hinos
- **Testes do MSA** - registro de avaliações

### Para Alunos:
- **Visualização do Progresso** pessoal
- **Acompanhamento** dos métodos estudados
- **Progresso nos Hinos** com indicadores visuais

## 🚀 Tecnologias Utilizadas

- **React 18** com TypeScript
- **Firebase** (Autenticação, Firestore, Storage)
- **Tailwind CSS** para estilização
- **React Router** para navegação
- **React Query** para gerenciamento de estado
- **React Hook Form** com validação Zod
- **Lucide React** para ícones
- **Date-fns** para manipulação de datas

## 📋 Pré-requisitos

- Node.js 16+ 
- npm ou yarn
- Conta no Firebase

## 🔧 Instalação

1. **Clone o repositório:**
```bash
git clone <url-do-repositorio>
cd gem-ccb-controle
```

2. **Instale as dependências:**
```bash
npm install
```

3. **Configure o Firebase:**
   - Crie um projeto no [Firebase Console](https://console.firebase.google.com/)
   - Ative Authentication com Email/Password
   - Crie um banco Firestore
   - Configure Storage (opcional, para fotos dos alunos)
   - Copie as credenciais do projeto

4. **Configure as variáveis do Firebase:**
   - Edite o arquivo `src/firebase/config.ts`
   - Substitua as configurações mockadas pelas suas credenciais reais:

```typescript
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";

// Configuração do Firebase
const firebaseConfig = {
  apiKey: "AIzaSyBBsUmWqkO3iXtgGtEb6W8sFl-g9UazjPg",
  authDomain: "gem-ccb-70241.firebaseapp.com",
  projectId: "gem-ccb-70241",
  storageBucket: "gem-ccb-70241.appspot.com", // Corrigido aqui!
  messagingSenderId: "213291465514",
  appId: "1:213291465514:web:8e7749610e9ad87473bc53",
  measurementId: "G-86434HMP1G"
};

// Inicializa o Firebase
const app = initializeApp(firebaseConfig);

// Inicializa o Analytics (opcional, só funciona em ambiente web)
const analytics = getAnalytics(app);

export { app, analytics };
```

Se for usar Firestore, Auth, etc., adicione:

```typescript
import { getFirestore } from "firebase/firestore";
import { getAuth } from "firebase/auth";

export const db = getFirestore(app);
export const auth = getAuth(app);
```

### 3. Como usar nos outros arquivos

Quando precisar acessar o Firestore, Auth ou Analytics, basta importar:

```typescript
import { db, auth, analytics } from '../firebase/config';
```

---

Se precisar de exemplos de uso do Firestore, Auth ou outro serviço, é só pedir!  
Se já fez isso, seu Firebase está pronto para uso no seu projeto.

5. **Configure as regras do Firestore:**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Usuários podem ler/escrever seus próprios dados
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Instrutores podem gerenciar seus alunos
    match /students/{studentId} {
      allow read, write: if request.auth != null && 
        (resource.data.instructorId == request.auth.uid || 
         request.auth.token.role == 'instructor');
    }
    
    // Progresso dos métodos
    match /methodProgress/{progressId} {
      allow read, write: if request.auth != null;
    }
    
    // Testes do MSA
    match /msaTests/{testId} {
      allow read, write: if request.auth != null;
    }
    
    // Progresso dos hinos
    match /hymnProgress/{progressId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

6. **Execute o projeto:**
```bash
npm start
```

O aplicativo estará disponível em `http://localhost:3000`

## 📁 Estrutura do Projeto

```
src/
├── components/          # Componentes reutilizáveis
│   └── Header.tsx      # Cabeçalho com navegação
├── contexts/           # Contextos React
│   └── AuthContext.tsx # Contexto de autenticação
├── data/              # Dados estáticos
│   └── hymns.ts       # Lista dos 480 hinos
├── firebase/          # Configuração do Firebase
│   └── config.ts      # Configuração e exportações
├── pages/             # Páginas da aplicação
│   ├── Dashboard.tsx  # Dashboard do instrutor
│   ├── Login.tsx      # Página de login
│   ├── MyProgress.tsx # Progresso do aluno
│   ├── Students.tsx   # Lista de alunos
│   └── StudentProgress.tsx # Progresso individual
├── types/             # Definições TypeScript
│   └── index.ts       # Interfaces e tipos
├── App.tsx            # Componente principal
├── index.tsx          # Ponto de entrada
└── index.css          # Estilos globais
```

## 🎯 Como Usar

### Primeiro Acesso:
1. Acesse o sistema
2. Clique em "Cadastre-se"
3. Crie uma conta como instrutor
4. Faça login

### Para Instrutores:
1. **Cadastrar Alunos:**
   - Acesse "Alunos" no menu
   - Clique em "Novo Aluno"
   - Preencha os dados do aluno

2. **Registrar Progresso:**
   - Clique em "Ver Progresso" no aluno
   - Use as abas "Progresso nos Métodos" e "Progresso nos Hinos"
   - Adicione registros de progresso

3. **Acompanhar Desenvolvimento:**
   - Use o Dashboard para visão geral
   - Visualize progresso individual de cada aluno

### Para Alunos:
1. Faça login com sua conta
2. Acesse "Meu Progresso"
3. Visualize seu desenvolvimento nos métodos e hinos

## 🎨 Personalização

### Cores e Tema:
- Edite `tailwind.config.js` para personalizar cores
- As cores da CCB estão definidas em `ccb.blue` e `ccb.gold`

### Logo:
- Substitua `/public/img/Logo_oficial_CCB.png` pelo logo da sua congregação

### Hinos:
- Edite `src/data/hymns.ts` para incluir títulos reais dos hinos
- Atualmente usa "Hino X" como placeholder

## 🔒 Segurança

- Autenticação obrigatória para todas as rotas
- Controle de acesso baseado em roles (instrutor/aluno)
- Instrutores só veem seus próprios alunos
- Alunos só veem seu próprio progresso

## 📱 Responsividade

O aplicativo é totalmente responsivo e funciona em:
- Desktop
- Tablet
- Smartphone

## 🚀 Deploy

### Firebase Hosting:
```bash
npm run build
firebase deploy
```

### Outras plataformas:
- Vercel
- Netlify
- AWS S3 + CloudFront

## 🤝 Contribuição

1. Faça um fork do projeto
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## 📄 Licença

Este projeto é desenvolvido para uso interno da Congregação Cristã no Brasil.

## 📞 Suporte

Para dúvidas ou problemas:
- Abra uma issue no repositório
- Entre em contato com a equipe de desenvolvimento

---

**Desenvolvido com ❤️ para a Congregação Cristã no Brasil** 