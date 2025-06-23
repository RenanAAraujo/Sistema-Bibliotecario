# Sistema Bibliotecário

Este projeto é um sistema web de gerenciamento de biblioteca, desenvolvido em **PHP** (sem framework) com arquitetura MVC rudimentar, utilizando **PDO** para acesso a **MySQL/MariaDB**. A interface do usuário é construída com **Bootstrap 5** e **jQuery**, fornecendo funcionalidades de CRUD para livros, usuários e empréstimos.

---

## Índice

1. [Pré-requisitos](#pré-requisitos)
2. [Instalação](#instalação)
3. [Importação do Banco de Dados](#importação-do-banco-de-dados)
4. [Configuração](#configuração)
5. [Estrutura de Pastas](#estrutura-de-pastas)
6. [Funcionalidades](#funcionalidades)
7. [Autenticação & Permissões](#autenticação--permissões)
8. [Melhorias Sugeridas](#melhorias-sugeridas)
9. [Testes & CI/CD](#testes--cicd)
10. [Contribuição](#contribuição)
11. [Licença](#licença)

---

## Pré-requisitos

- PHP 7.4+ com extensões **PDO** e **mysqli** habilitadas
- MySQL 5.7+ ou MariaDB 10+
- Servidor web (Apache com **mod\_rewrite** ou Nginx)
- Composer (opcional, para gerenciamento de dependências futuras)

---

## Instalação

1. Clone o repositório:
   ```bash
   git clone https://seu-repositorio.com/Sistema-Bibliotecario-main.git
   cd Sistema-Bibliotecario-main
   ```
2. Configure o seu servidor web (DocumentRoot) apontando para a raiz do projeto.
3. Certifique-se de que o `.htaccess` está ativo (Apache) ou ajuste as regras de rota no Nginx.
4. (Opcional) Instale dependências via Composer:
   ```bash
   composer install
   ```

---

## Importação do Banco de Dados

1. Crie um banco de dados vazio, por exemplo `biblioteca`.
2. Importe o dump SQL com esquema e dados de exemplo:
   ```bash
   mysql -u usuario -p biblioteca < database/bibliotecario.sql
   ```
3. Verifique as tabelas criadas: `books`, `users`, `loans`, entre outras.

---

## Configuração

1. Renomeie o arquivo de configuração de exemplo (se existir) ou crie `config/database.php` com suas credenciais:
   ```php
   <?php
   define('DB_HOST', 'localhost');
   define('DB_NAME', 'biblioteca');
   define('DB_USER', 'root');
   define('DB_PASS', 'senha');

   try {
       $pdo = new PDO("mysql:host=".DB_HOST.";dbname=".DB_NAME.";charset=utf8", DB_USER, DB_PASS);
       $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
   } catch (PDOException $e) {
       die('Erro na conexão: ' . $e->getMessage());
   }
   ```
2. Ajuste o fuso horário e outras configurações em `src/helpers.php`, se necessário.

---

## Estrutura de Pastas

```bash
Sistema-Bibliotecario-main/
├─ .gitignore
├─ index.php                  # Página inicial/landing
├─ login.php                  # Formulário de login
├─ logout.php                 # Logout e destruição de sessão
│
├─ config/                    # Arquivos de configuração
│   └─ database.php           # Conexão PDO com o banco
│
├─ public/                    # Recursos públicos (CSS, JS, imagens)
│   ├─ css/
│   │   ├─ bootstrap.min.css
│   │   └─ styles.css
│   ├─ js/
│   │   ├─ bootstrap.bundle.js
│   │   └─ main.js            # Lógica de UI (alertas, validações)
│   └─ img/
│       └─ logo.png
│
├─ src/                       # Código-fonte MVC
│   ├─ controllers/           # Lógica de controle e roteamento
│   │   ├─ BookController.php
│   │   ├─ UserController.php
│   │   └─ LoanController.php
│   ├─ models/                # Classes de mapeamento de dados
│   │   ├─ Book.php
│   │   ├─ User.php
│   │   └─ Loan.php
│   ├─ views/                 # Templates de saída HTML
│   │   ├─ header.php
│   │   ├─ footer.php
│   │   ├─ list_books.php
│   │   ├─ form_book.php
│   │   ├─ list_users.php
│   │   ├─ form_user.php
│   │   ├─ list_loans.php
│   │   └─ form_loan.php
│   └─ helpers.php            # Funções utilitárias (sanitização, validações)
│
└─ database/                  # Scripts SQL
    └─ bibliotecario.sql     # Dump com esquema e dados de exemplo
```

---

## Funcionalidades

- **Gestão de Livros**: criação, edição, listagem e exclusão.
- **Gestão de Usuários**: CRUD completo de perfis (leitores e bibliotecários).
- **Empréstimos**: registrador saída e devolução de livros com controle de cópias.
- **Autenticação**: login/logout com sessões PHP.
- **Interface Responsiva**: construção com Bootstrap 5.

---

## Autenticação & Permissões

- **Login de Usuário**: pagina `login.php` valida credenciais via `Controlador de usuário`.
- **Sessões**: ao autenticar, é criada sessão com dados do usuário.
- **Proteção de Rotas**: controllers verificam sessão ativa antes de permitir acesso.

---

## Melorias Sugeridas

1. **Segurança**:

   - Implementar `senha_hash` e `senha_verificar` para senhas.
   - Usar **Tokens CSRF** em formulários.
   - Valide e sanitize todos os insumos usando `filter_input()`.

2. **Arquitetura**:

   - Migre para um micro-framework (Slim, Lumen) ou full MVC (Laravel) para melhor organização.
   - Separe rotas e lógica de visualização com um roteador dedicado.

3. **Front-End Moderno**:

   - Adote bundler (Vite/Webpack) para pré-processamento de CSS (Sass) e módulos JS.
   - Implemente chamadas AJAX para CRUD sem recarga de pagina.

4. **Funcionalidades Avançadas**:

   - Relatórios em PDF/CSV de empreendimentos em atraso.
   - Notificações por e-mail para devoluções pendentes.
   - API REST pública para integração com front-ends externos.

5. **Testes e CI/CD**:

   - Escreva testículos unitários com PHPUnit.
   - Configurar pipeline de CI (GitHub Actions) para lint, testículos e implantar automático.

---

## Contribuição

Contribuições são bem-vindas:

1. Faça um fork deste repositório
2. Filial Crie Uma: `git checkout - b feature/nome-da-feature`
3. Comprometer-se: `git commit - m "Descrição da feature"`
4. Empurrar: `recurso de origem git push/nome-da-feature`
5. Abra um Pull Solicitação

---

## Licença

Este projeto é distribuído sob uma licença **MIT**. . Consulte o arquivo `LICENÇA` para detalhes.

