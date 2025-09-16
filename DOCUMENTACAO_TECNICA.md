# Academia Fit - Documentação Técnica

## 🏗️ **Arquitetura do Sistema**

### **Stack Tecnológico**
- **Backend**: Python 3.11 + Flask
- **Frontend**: HTML5 + CSS3 + JavaScript (Vanilla)
- **Banco de Dados**: SQLite
- **Autenticação**: JWT-like tokens + SHA256
- **API**: RESTful com CORS habilitado

### **Estrutura de Diretórios**
```
academia_webapp/
├── backend/
│   ├── src/
│   │   ├── main.py              # Aplicação principal
│   │   └── routes/              # Endpoints da API
│   │       ├── auth.py          # Autenticação
│   │       ├── usuarios.py      # Gestão de usuários
│   │       ├── exercicios.py    # Catálogo de exercícios
│   │       ├── treinos.py       # Gestão de treinos
│   │       ├── atribuicoes.py   # Sistema de atribuições
│   │       ├── series_exercicios.py
│   │       └── historico.py
│   ├── frontend/                # Arquivos estáticos
│   │   ├── index.html           # Dashboard principal
│   │   ├── login.html           # Tela de login
│   │   ├── styles.css           # Estilos principais
│   │   ├── script.js            # JavaScript principal
│   │   └── login-script.js      # JavaScript do login
│   ├── database/
│   │   └── academia.db          # Banco SQLite
│   └── venv/                    # Ambiente virtual Python
├── database/
│   ├── create_tables.sql        # Script de criação
│   └── insert_test_data.sql     # Dados de teste
└── docs/                        # Documentação
```

---

## 🗄️ **Modelo de Dados**

### **Tabela: usuarios**
```sql
CREATE TABLE usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    senha VARCHAR(255) NOT NULL,
    tipo_usuario VARCHAR(20) NOT NULL CHECK(tipo_usuario IN ('admin', 'instrutor', 'aluno')),
    telefone VARCHAR(20),
    data_nascimento DATE,
    data_cadastro DATETIME NOT NULL,
    ativo BOOLEAN DEFAULT 1
);
```

### **Tabela: exercicios**
```sql
CREATE TABLE exercicios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    grupo_muscular VARCHAR(50),
    equipamento VARCHAR(100),
    nivel_dificuldade VARCHAR(20) CHECK(nivel_dificuldade IN ('iniciante', 'intermediario', 'avancado')),
    instrucoes TEXT,
    data_criacao DATETIME NOT NULL,
    ativo BOOLEAN DEFAULT 1
);
```

### **Tabela: treinos**
```sql
CREATE TABLE treinos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    objetivo VARCHAR(100),
    nivel_dificuldade VARCHAR(20),
    duracao_estimada INTEGER,
    data_criacao DATETIME NOT NULL,
    ativo BOOLEAN DEFAULT 1
);
```

### **Tabela: treino_atribuicoes**
```sql
CREATE TABLE treino_atribuicoes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    treino_id INTEGER NOT NULL,
    aluno_id INTEGER NOT NULL,
    instrutor_id INTEGER NOT NULL,
    data_atribuicao DATETIME NOT NULL,
    data_inicio DATE,
    data_fim DATE,
    observacoes TEXT,
    ativo BOOLEAN DEFAULT 1,
    FOREIGN KEY (treino_id) REFERENCES treinos (id),
    FOREIGN KEY (aluno_id) REFERENCES usuarios (id),
    FOREIGN KEY (instrutor_id) REFERENCES usuarios (id)
);
```

### **Tabela: sessoes**
```sql
CREATE TABLE sessoes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    usuario_id INTEGER NOT NULL,
    token VARCHAR(255) UNIQUE NOT NULL,
    created_at DATETIME NOT NULL,
    expires_at DATETIME NOT NULL,
    ativo BOOLEAN DEFAULT 1,
    FOREIGN KEY (usuario_id) REFERENCES usuarios (id)
);
```

---

## 🔐 **Sistema de Autenticação**

### **Fluxo de Login**
1. **POST /api/auth/login** com email/senha
2. Validação de credenciais (SHA256)
3. Geração de token único (32 bytes)
4. Armazenamento na tabela `sessoes`
5. Retorno do token para o cliente

### **Middleware de Autenticação**
```python
@require_auth
def protected_endpoint():
    # Acesso ao request.current_user_id
    # Acesso ao request.current_user_type
    pass
```

### **Níveis de Permissão**
- `@require_auth` - Qualquer usuário logado
- `@require_admin` - Apenas administradores
- `@require_instructor_or_admin` - Instrutores ou admins

---

## 🌐 **API Reference**

### **Autenticação**
```http
POST /api/auth/login
Content-Type: application/json

{
    "email": "user@email.com",
    "password": "senha123"
}

Response:
{
    "message": "Login realizado com sucesso",
    "token": "AWJTFDeoWe51zqmh2ldFjDEvcKdecMb2H2q4ybNKErY",
    "user": {
        "id": 1,
        "nome": "João Silva",
        "email": "joao.silva@email.com",
        "tipo_usuario": "aluno"
    }
}
```

### **Usuários**
```http
GET /api/usuarios/
Authorization: Bearer {token}

POST /api/usuarios/
Authorization: Bearer {token}
Content-Type: application/json

{
    "nome": "Novo Usuário",
    "email": "novo@email.com",
    "senha": "senha123",
    "tipo_usuario": "aluno"
}
```

### **Exercícios**
```http
GET /api/exercicios/
GET /api/exercicios/{id}

POST /api/exercicios/
Authorization: Bearer {token}
Content-Type: application/json

{
    "nome": "Supino Reto",
    "descricao": "Exercício para peitoral",
    "grupo_muscular": "Peito",
    "nivel_dificuldade": "intermediario"
}
```

### **Treinos**
```http
GET /api/treinos/
Authorization: Bearer {token}

POST /api/treinos/
Authorization: Bearer {token}
Content-Type: application/json

{
    "nome": "Treino de Força",
    "descricao": "Treino focado em ganho de força",
    "objetivo": "Hipertrofia",
    "duracao_estimada": 60
}
```

### **Atribuições**
```http
GET /api/atribuicoes/
Authorization: Bearer {token}

POST /api/atribuicoes/
Authorization: Bearer {token}
Content-Type: application/json

{
    "treino_id": 1,
    "aluno_id": 3,
    "data_inicio": "2025-06-01",
    "data_fim": "2025-07-01",
    "observacoes": "Treino inicial"
}
```

---

## 🎨 **Frontend Architecture**

### **Componentes Principais**
- **app.js** - Gerenciador principal da aplicação
- **auth.js** - Controle de autenticação
- **api.js** - Comunicação com backend
- **ui.js** - Manipulação da interface

### **Sistema de Roteamento**
```javascript
const routes = {
    'dashboard': showDashboard,
    'usuarios': showUsuarios,
    'exercicios': showExercicios,
    'treinos': showTreinos,
    'atribuicoes': showAtribuicoes
};
```

### **Controle de Permissões**
```css
.admin-only { display: none; }
.instructor-only { display: none; }

.user-admin .admin-only,
.user-instrutor .admin-only,
.user-instrutor .instructor-only {
    display: inline-flex;
}
```

---

## 🚀 **Deploy e Configuração**

### **Requisitos do Sistema**
- Python 3.11+
- Flask 2.0+
- SQLite 3
- Navegador moderno (Chrome, Firefox, Safari)

### **Instalação**
```bash
# 1. Clonar/extrair projeto
cd academia_webapp/backend

# 2. Criar ambiente virtual
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows

# 3. Instalar dependências
pip install flask flask-cors

# 4. Executar aplicação
python src/main.py
```

### **Configuração de Produção**
```python
# Para produção, alterar em main.py:
app.run(host='0.0.0.0', port=5000, debug=False)

# Usar servidor WSGI como Gunicorn:
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 src.main:app
```

### **Variáveis de Ambiente**
```bash
export FLASK_ENV=production
export SECRET_KEY=sua_chave_secreta_aqui
export DATABASE_URL=sqlite:///academia.db
```

---

## 🔧 **Manutenção e Monitoramento**

### **Logs do Sistema**
- Logs automáticos no console do Flask
- Códigos de status HTTP padronizados
- Tratamento de exceções em todos os endpoints

### **Backup do Banco**
```bash
# Backup manual
cp backend/database/academia.db backup_$(date +%Y%m%d).db

# Restauração
cp backup_20250606.db backend/database/academia.db
```

### **Monitoramento de Performance**
- Endpoint `/api/health` para health checks
- Métricas de tempo de resposta
- Controle de sessões ativas

---

## 📊 **Métricas e Analytics**

### **Dados Coletados**
- Logins por usuário e data
- Exercícios mais utilizados
- Treinos mais atribuídos
- Tempo médio de sessão

### **Relatórios Disponíveis**
- Usuários ativos por período
- Exercícios por grupo muscular
- Efetividade dos treinos
- Estatísticas de uso do sistema

---

## 🛡️ **Segurança**

### **Medidas Implementadas**
- ✅ Hash SHA256 para senhas
- ✅ Tokens de sessão únicos
- ✅ Validação de entrada em todos os endpoints
- ✅ Controle de permissões por rota
- ✅ CORS configurado adequadamente
- ✅ Sanitização de dados SQL

### **Recomendações Adicionais**
- Usar HTTPS em produção
- Implementar rate limiting
- Logs de auditoria detalhados
- Backup automático regular
- Monitoramento de tentativas de login

---

## 📝 **Changelog**

### **v2.0.0 - Junho 2025**
- ✅ Sistema de autenticação completo
- ✅ Controle de permissões por usuário
- ✅ Sistema de atribuição de treinos
- ✅ Interface responsiva moderna
- ✅ API RESTful completa
- ✅ Documentação técnica

### **v1.0.0 - Versão Inicial**
- ✅ CRUD básico para usuários, exercícios e treinos
- ✅ Interface web funcional
- ✅ Banco de dados estruturado

---

**Desenvolvido para Academia Fit**  
**Versão**: 2.0.0  
**Documentação atualizada**: Junho 2025

