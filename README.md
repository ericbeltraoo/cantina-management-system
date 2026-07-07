<h1 align="center">🍔 Cantina Management System</h1>

<p align="center">
  <strong>Sistema web full-stack para gestão de uma cantina escolar</strong><br/>
  Pedidos online, carteira digital, controle de estoque e painel administrativo — do aluno ao gestor.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java 21"/>
  <img src="https://img.shields.io/badge/Spring_Boot-3.2-6DB33F?style=for-the-badge&logo=springboot&logoColor=white" alt="Spring Boot"/>
  <img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black" alt="React"/>
  <img src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
  <img src="https://img.shields.io/badge/Vite-6-646CFF?style=for-the-badge&logo=vite&logoColor=white" alt="Vite"/>
  <img src="https://img.shields.io/badge/MySQL-8-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL"/>
  <img src="https://img.shields.io/badge/Tailwind-4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" alt="Tailwind"/>
</p>

---

## 📌 Sobre o projeto

O **Cantina Management System** é uma aplicação web completa que digitaliza a operação de uma cantina escolar. Os alunos fazem pedidos, pagam com saldo em carteira e retiram o pedido com um **código de retirada**, sem filas. Os administradores gerenciam produtos, estoque, pedidos e acompanham as vendas por um painel com gráficos.

O projeto foi desenvolvido como trabalho final na **Firjan SESI/SENAI**, com arquitetura **cliente-servidor** desacoplada: uma **API REST em Spring Boot** consumida por um **SPA em React**.

> 💡 **Destaques técnicos:** arquitetura em camadas (Controller → Service → Repository), DTOs para isolar entidades da API, relacionamentos JPA (1:N e N:N), controle de acesso por papel (RBAC), e um front-end moderno com design system baseado em componentes acessíveis.

---

## 📑 Índice

- [Screenshots](#-screenshots)
- [Funcionalidades](#-funcionalidades)
- [Arquitetura](#-arquitetura)
- [Site map / Fluxo de navegação](#-site-map--fluxo-de-navegação)
- [Stack de tecnologias](#-stack-de-tecnologias)
- [Modelo de dados](#-modelo-de-dados)
- [Documentação da API](#-documentação-da-api)
- [Estrutura de pastas](#-estrutura-de-pastas)
- [Como executar](#-como-executar)
- [Roadmap](#-roadmap)
- [Autores](#-autores)

---

## 📸 Screenshots

> 📷 **Adicione aqui os prints da aplicação.** Coloque as imagens na pasta `docs/screenshots/`
> e ajuste os caminhos abaixo. Sugestão de telas que causam boa impressão:

<table>
  <tr>
    <td width="50%" align="center">
      <strong>🔐 Login</strong><br/>
      <img src="docs/screenshots/login.png" alt="Tela de login" width="100%"/>
    </td>
    <td width="50%" align="center">
      <strong>🎓 Painel do Aluno</strong><br/>
      <img src="docs/screenshots/student-dashboard.png" alt="Painel do aluno" width="100%"/>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <strong>🛒 Catálogo & Carrinho</strong><br/>
      <img src="docs/screenshots/catalogo.png" alt="Catálogo de produtos" width="100%"/>
    </td>
    <td width="50%" align="center">
      <strong>📊 Dashboard do Admin</strong><br/>
      <img src="docs/screenshots/admin-dashboard.png" alt="Dashboard administrativo" width="100%"/>
    </td>
  </tr>
</table>

---

## ✨ Funcionalidades

### 👤 Aluno (STUDENT)
- 🔐 Cadastro e login
- 🛒 Catálogo de produtos com busca e filtro por categoria
- ➕ Carrinho de compras com controle de quantidade
- 💳 Pagamento por **saldo em carteira**, **PIX** ou **cartão**
- 💰 Recarga de saldo na carteira digital
- 🎫 **Código de retirada** gerado automaticamente para cada pedido
- ⭐ Favoritar produtos
- 📜 Histórico de pedidos com status em tempo real
- ❌ Cancelamento de pedido com motivo (e estorno de saldo)
- 💬 Canal de suporte / atendimento

### 🛠️ Administrador (ADMIN)
- 📊 **Dashboard de vendas** com gráficos (Recharts)
- 📦 **Gestão de estoque** — cadastro, edição e exclusão de produtos
- 🔄 Atualização de status dos pedidos (pendente → preparando → pronto → concluído)
- 🔎 Busca de pedidos por **código de retirada**
- 👥 Gestão de usuários
- 🎧 Respostas às mensagens de suporte

---

## 🏗️ Arquitetura

```
┌──────────────────────────────┐         HTTP / JSON          ┌──────────────────────────────┐
│        FRONT-END (SPA)        │  ──────────────────────────▶ │        BACK-END (API)         │
│                               │      REST · axios/fetch      │                               │
│  React 18 + TypeScript        │ ◀────────────────────────── │  Spring Boot 3.2 (Java 21)    │
│  Vite · Tailwind · shadcn/ui  │                              │                               │
│                               │                              │  Controller ─▶ Service ─▶ Repo │
│  Aluno UI  │  Admin UI        │                              │        (arquitetura em camadas)│
└──────────────────────────────┘                              └───────────────┬───────────────┘
        localhost:5173                                                         │ Spring Data JPA
                                                                               ▼
                                                                     ┌───────────────────┐
                                                                     │   MySQL 8         │
                                                                     │  sistema_pedidos  │
                                                                     └───────────────────┘
```

**Camadas do back-end:**

| Camada | Responsabilidade |
|--------|------------------|
| `Controller` | Expõe os endpoints REST e trata requisições/respostas HTTP |
| `Service` | Regras de negócio (cálculo de total, estorno, validações) |
| `Repository` | Acesso a dados via Spring Data JPA |
| `Entity` | Mapeamento objeto-relacional (JPA/Hibernate) |
| `DTO` | Objetos de transferência que isolam as entidades da API |
| `Exception` | Tratamento global de erros (`GlobalExceptionHandler`) |

---

## 🗺️ Site map / Fluxo de navegação

```
Cantina Management System
│
├── 🔓 Área Pública
│   ├── /login ............... Autenticação
│   └── /register ............ Cadastro de novo aluno
│
├── 🎓 Área do Aluno (StudentDashboard)
│   ├── Catálogo de Produtos ....... escolha de itens
│   ├── Carrinho ................... revisão e pagamento
│   ├── Favoritos .................. produtos marcados
│   ├── Histórico de Pedidos ....... status + cancelamento
│   ├── Carteira / Saldo ........... recarga de saldo
│   └── Suporte .................... abertura de chamados
│
└── 🛡️ Área do Admin (AdminDashboard)
    ├── Dashboard de Vendas ........ gráficos e métricas
    ├── Gestão de Estoque .......... CRUD de produtos
    ├── Gestão de Pedidos .......... atualização de status
    ├── Busca por Código de Retirada
    └── Central de Suporte ......... responder chamados
```

---

## 🧰 Stack de tecnologias

### Back-end
| Tecnologia | Uso |
|-----------|-----|
| **Java 21** | Linguagem |
| **Spring Boot 3.2** | Framework principal |
| **Spring Web** | API REST |
| **Spring Data JPA / Hibernate** | Persistência e ORM |
| **MySQL 8** | Banco de dados relacional |
| **Bean Validation** | Validação de dados |
| **Lombok** | Redução de boilerplate |
| **Maven** | Build e dependências |

### Front-end
| Tecnologia | Uso |
|-----------|-----|
| **React 18 + TypeScript** | Interface e tipagem |
| **Vite 6** | Build tool e dev server |
| **Tailwind CSS 4** | Estilização utilitária |
| **Radix UI / shadcn/ui** | Componentes acessíveis |
| **Material UI (MUI)** | Componentes complementares |
| **Recharts** | Gráficos do dashboard |
| **React Hook Form** | Formulários |
| **Axios / Fetch** | Consumo da API |
| **Motion** | Animações |

---

## 🗄️ Modelo de dados

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│    Usuario      │         │     Pedido      │         │   ItemPedido    │
├─────────────────┤         ├─────────────────┤         ├─────────────────┤
│ id (PK)         │1       *│ id (PK)         │1       *│ id (PK)         │
│ name            ├────────▶│ usuario_id (FK) ├────────▶│ pedido_id (FK)  │
│ email (unique)  │         │ userName        │         │ produto_id (FK) │
│ password        │         │ total           │         │ quantity        │
│ role (ENUM)     │         │ status (ENUM)   │         │ priceUnit       │
│ balance         │         │ paymentMethod   │         │ subtotal        │
│ className       │         │ pickupCode      │         └────────┬────────┘
│ phone           │         │ cancelReason    │                  │ *
│ registration    │         │ createdAt       │                  │
│ createdAt       │         └─────────────────┘                  │ 1
└────────┬────────┘                                     ┌─────────────────┐
         │ *                                            │    Produto      │
         │      N:N (usuario_favoritos)                 ├─────────────────┤
         └─────────────────────────────────────────────│ id (PK)         │
                                                        │ name            │
   Enums:                                               │ description     │
   • RoleUsuario ...... STUDENT | ADMIN                 │ price           │
   • StatusPedido ..... PENDING | PREPARING |           │ category        │
   •                    READY | COMPLETED | CANCELLED   │ image           │
   • MetodoPagamento .. BALANCE | PIX | CARD            │ available       │
                                                        │ stock           │
                                                        └─────────────────┘
```

**Relacionamentos:**
- `Usuario` **1:N** `Pedido` — um usuário faz vários pedidos
- `Pedido` **1:N** `ItemPedido` — um pedido contém vários itens
- `Produto` **1:N** `ItemPedido` — um produto aparece em vários itens
- `Usuario` **N:N** `Produto` — favoritos (tabela `usuario_favoritos`)

---

## 📡 Documentação da API

Base URL: `http://localhost:8080`

### 🔐 Autenticação — `/api/auth`
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `POST` | `/api/auth/login` | Autentica um usuário |
| `POST` | `/api/auth/register` | Cadastra um novo usuário |

### 🍔 Produtos — `/api/produtos`
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/produtos` | Lista todos os produtos |
| `GET` | `/api/produtos/disponiveis` | Lista produtos disponíveis |
| `GET` | `/api/produtos/categoria/{categoria}` | Filtra por categoria |
| `GET` | `/api/produtos/{id}` | Busca produto por ID |
| `POST` | `/api/produtos` | Cria um produto |
| `PUT` | `/api/produtos/{id}` | Atualiza um produto |
| `PATCH` | `/api/produtos/{id}/estoque` | Atualiza o estoque |
| `DELETE` | `/api/produtos/{id}` | Remove um produto |

### 🧾 Pedidos — `/api/pedidos`
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/pedidos` | Lista todos os pedidos |
| `GET` | `/api/pedidos/usuario/{usuarioId}` | Pedidos de um usuário |
| `GET` | `/api/pedidos/status/{status}` | Pedidos por status |
| `GET` | `/api/pedidos/{id}` | Busca pedido por ID |
| `GET` | `/api/pedidos/codigo/{codigo}` | Busca por código de retirada |
| `POST` | `/api/pedidos` | Cria um pedido |
| `PATCH` | `/api/pedidos/{id}/status` | Atualiza o status |
| `PATCH` | `/api/pedidos/{id}/cancelar` | Cancela o pedido (com motivo) |

### 👥 Usuários — `/api/users`
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/users` | Lista usuários |
| `GET` | `/api/users/{id}` | Busca por ID |
| `GET` | `/api/users/email/{email}` | Busca por e-mail |
| `POST` | `/api/users` | Cria usuário |
| `PUT` | `/api/users/{id}` | Atualiza usuário |
| `DELETE` | `/api/users/{id}` | Remove usuário |
| `POST` | `/api/users/{id}/adicionar-saldo` | Recarrega saldo |
| `POST` | `/api/users/{userId}/favorites/{produtoId}` | Adiciona/remove favorito |

---

## 📁 Estrutura de pastas

```
cantina-management-system/
│
├── back-projetofinal/                 # API REST — Spring Boot
│   ├── src/main/java/com/projetofinal/sistemapedidos/
│   │   ├── config/                    # Configuração de CORS
│   │   ├── controller/                # Endpoints REST
│   │   ├── dto/                       # Objetos de transferência
│   │   ├── entity/                    # Entidades JPA + enums
│   │   ├── exception/                 # Tratamento global de erros
│   │   ├── repository/                # Interfaces Spring Data JPA
│   │   ├── service/                   # Regras de negócio
│   │   └── SistemaPedidosApplication.java
│   ├── src/main/resources/
│   │   └── application.properties     # Configuração do banco/servidor
│   └── pom.xml
│
└── front-projetofinal/                # SPA — React + TypeScript
    ├── src/
    │   ├── app/
    │   │   ├── App.tsx                # Componente raiz e roteamento
    │   │   └── components/            # Telas + design system (ui/)
    │   ├── styles/                    # Tailwind, temas e fontes
    │   └── main.tsx
    ├── index.html
    ├── vite.config.ts
    └── package.json
```

---

## ▶️ Como executar

### Pré-requisitos
- **Java 21+** e **Maven**
- **Node.js 18+**
- **MySQL 8** em execução

### 1️⃣ Back-end (API)

```bash
cd back-projetofinal

# 1. Configure as credenciais do MySQL por variáveis de ambiente.
#    Copie o modelo e preencha com seus dados locais:
cp .env.example .env

# 2. Exporte as variáveis (Linux/macOS). No Windows, defina no sistema
#    ou pela sua IDE. Valores padrão são usados caso não sejam definidas.
export DB_USERNAME=root
export DB_PASSWORD=sua_senha

# 3. O banco 'sistema_pedidos' é criado automaticamente na primeira execução
./mvnw spring-boot:run
```
> A API sobe em **http://localhost:8080**
>
> 🔒 As credenciais do banco são lidas de variáveis de ambiente
> (`DB_USERNAME`, `DB_PASSWORD`, etc.) — nenhuma senha fica versionada no código.

### 2️⃣ Front-end (SPA)

```bash
cd front-projetofinal
npm install
npm run dev
```
> A interface sobe em **http://localhost:5173**

---

## 🚀 Roadmap

Melhorias planejadas / possíveis evoluções:

- [ ] 🔒 Autenticação com **JWT** e criptografia de senhas (BCrypt)
- [ ] 📄 Documentação interativa com **Swagger / OpenAPI**
- [ ] ✅ Testes automatizados (JUnit + Testing Library)
- [ ] 🐳 Containerização com **Docker Compose**
- [ ] ☁️ Deploy (API + banco + front) em nuvem
- [ ] 🔔 Notificações em tempo real (WebSocket) no lugar do polling

---

## 👥 Autores

Projeto final desenvolvido na **Firjan SESI/SENAI**.

| Área | Integrantes |
|------|-------------|
| **Back-end** | Igor, Eric, Emanuel |
| **Front-end** | Sarah, Maria |

---

<p align="center">
  ⭐ Se este projeto te interessou, deixe uma estrela no repositório!
</p>
