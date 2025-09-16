# Academia Fit - Manual do Usuário

## 🏋️ **Bem-vindo ao Academia Fit**

Sistema completo de gerenciamento para academias com controle de usuários, exercícios, treinos e atribuições.

---

## 🚀 **Como Iniciar**

### **1. Executar o Sistema**
```bash
cd academia_webapp/backend
source venv/bin/activate
pip install flask flask-cors
python src/main.py
```

### **2. Acessar o Sistema**
- **URL Principal**: http://localhost:5000
- **Tela de Login**: http://localhost:5000/login.html

---

## 👥 **Usuários de Demonstração**

### **🔴 Administrador**
- **Email**: admin@academia.com
- **Senha**: admin123
- **Permissões**: Acesso total ao sistema

### **🟢 Professor/Instrutor**
- **Email**: carlos.oliveira@email.com
- **Senha**: senha789
- **Permissões**: Gerenciar exercícios e treinos, atribuir para alunos

### **🔵 Aluno**
- **Email**: joao.silva@email.com
- **Senha**: senha123
- **Permissões**: Visualizar treinos atribuídos

---

## 📱 **Funcionalidades por Tipo de Usuário**

### **🔴 ADMINISTRADOR**
- ✅ Gerenciar todos os usuários (criar, editar, desativar)
- ✅ Acesso completo a exercícios e treinos
- ✅ Visualizar todas as atribuições
- ✅ Estatísticas gerais do sistema
- ✅ Configurações avançadas

### **🟢 PROFESSOR/INSTRUTOR**
- ✅ Criar e editar exercícios
- ✅ Gerenciar treinos
- ✅ Atribuir treinos específicos para alunos
- ✅ Acompanhar progresso dos alunos
- ✅ Visualizar histórico de treinos

### **🔵 ALUNO**
- ✅ Visualizar treinos atribuídos pelo professor
- ✅ Acessar detalhes dos exercícios
- ✅ Registrar execução de treinos
- ✅ Acompanhar histórico pessoal
- ✅ Interface simplificada e intuitiva

---

## 🎯 **Como Usar - Passo a Passo**

### **Para Professores: Atribuir Treino**
1. Faça login como professor
2. Vá em "Treinos" → "Novo Treino"
3. Preencha nome, descrição e exercícios
4. Vá em "Atribuições" → "Nova Atribuição"
5. Selecione o treino e o aluno
6. Defina período e observações

### **Para Alunos: Visualizar Treinos**
1. Faça login como aluno
2. Na tela inicial, veja treinos atribuídos
3. Clique em "Ver Detalhes" para instruções
4. Execute o treino seguindo as orientações
5. Registre a execução no histórico

### **Para Admins: Gerenciar Sistema**
1. Faça login como admin
2. Acesse "Usuários" para gerenciar contas
3. Monitore "Estatísticas" do sistema
4. Configure exercícios e treinos globais

---

## 🔧 **Recursos Técnicos**

### **API Endpoints Principais**
- `POST /api/auth/login` - Autenticação
- `GET /api/usuarios/` - Listar usuários
- `GET /api/exercicios/` - Listar exercícios
- `GET /api/treinos/` - Listar treinos
- `GET /api/atribuicoes/` - Listar atribuições

### **Banco de Dados**
- **SQLite** com 6 tabelas relacionadas
- **Backup automático** dos dados
- **Integridade referencial** garantida

### **Segurança**
- **Senhas criptografadas** (SHA256)
- **Tokens de sessão** seguros
- **Controle de permissões** por endpoint
- **Validação de dados** em todas as operações

---

## 📞 **Suporte**

### **Problemas Comuns**
- **Erro de login**: Verifique email e senha
- **Página não carrega**: Confirme se servidor está rodando
- **Sem permissão**: Verifique tipo de usuário logado

### **Logs do Sistema**
- Verifique o terminal onde o servidor está rodando
- Logs detalhados de todas as operações
- Códigos de erro específicos para diagnóstico

---

## 🎉 **Pronto para Usar!**

O sistema Academia Fit está completo e pronto para gerenciar sua academia de forma eficiente e organizada.

**Versão**: 2.0.0  
**Última Atualização**: Junho 2025

