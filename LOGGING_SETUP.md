# 📊 Sistema de Logging de Acesso - Login

## Descrição

Sistema implementado para registrar e monitorar todos os acessos à página de login do Admin Renascer. O sistema utiliza **Firebase Firestore** para armazenar os logs de forma segura e escalável.

## ✨ Funcionalidades

### 1. **Registro Automático de Acessos**
- Cada vez que um usuário acessa a página de login (`/login/index.html`), um log é registrado automaticamente
- Informações capturadas:
  - Tipo de acesso (acesso_pagina)
  - Data e hora do acesso
  - User Agent (navegador/dispositivo)
  - URL
  - Idioma do navegador

### 2. **Registro de Tentativas de Login**
- Todas as tentativas de login (bem-sucedidas e falhadas) são registradas
- Informações capturadas:
  - Email utilizado
  - Status (sucesso/falha)
  - Código de erro (em caso de falha)
  - Data e hora
  - Dispositivo/navegador

### 3. **Dashboard de Logs**
- Página dedicada para visualizar todos os logs: `/login/logs.html`
- Requer autenticação (apenas admins autenticados podem acessar)
- Funcionalidades:
  - Visualização em tabela com paginação
  - Filtros por tipo, status e email
  - Estatísticas em tempo real
  - Modal com detalhes completos de cada log

## 📂 Arquivos Modificados/Criados

### Modificados:
- **`/login/index.html`**
  - Adicionado import do Firestore
  - Adicionada função `registrarAcesso()`
  - Registro automático de acesso à página
  - Registro de tentativas de login

### Criados:
- **`/login/logs.html`**
  - Dashboard completo para visualização de logs
  - Interface responsiva com Tailwind CSS
  - Estatísticas em cards
  - Tabela com filtros e paginação
  - Modal com detalhes dos logs

## 🚀 Como Usar

### Acessar o Dashboard de Logs
1. Faça login normalmente em `/login/index.html`
2. Navegue para `/login/logs.html`
3. (Ou adicione um link na sidebar do admin-dashboard.html apontando para os logs)

### Visualizar Logs
- Todos os logs são exibidos em ordem cronológica (mais recentes primeiro)
- Use os filtros para buscar logs específicos
- Clique em "Ver mais" para ver detalhes completos de cada log

### Dados Registrados

Cada log contém:
```json
{
  "tipo": "acesso_pagina | tentativa_login",
  "email": "usuario@email.com (null para acesso_pagina)",
  "sucesso": "true | false | null",
  "erroMensagem": "código do erro (quando falha)",
  "userAgent": "navegador/dispositivo",
  "url": "url da página",
  "idioma": "idioma do navegador",
  "timestamp": "data/hora do servidor Firebase"
}
```

## 🔐 Segurança

### Permissões no Firestore (Regras de Segurança)

Você deve configurar as regras de segurança do Firestore para:
1. **Permitir leitura apenas para usuários autenticados**
2. **Permitir escrita apenas de código do servidor (funções Firebase)**

Exemplo de regra (ajuste conforme necessário):
```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /login_logs/{document=**} {
      // Leitura: apenas admins autenticados
      allow read: if request.auth != null;
      // Escrita: apenas funções do Firebase (via cliente com permissão específica)
      allow write: if request.auth != null;
    }
  }
}
```

## 📊 Estatísticas

O dashboard exibe:
- **Total de Acessos**: Quantidade de acessos à página de login
- **Logins Bem-sucedidos**: Quantidade de logins com sucesso
- **Logins Falhados**: Quantidade de tentativas de login falhadas
- **Hoje**: Total de acessos no dia atual

## 🔄 Próximas Melhorias Sugeridas

1. **Análise de IPs**: Capturar IP do usuário (requer backend)
2. **Alertas de Segurança**: Notificar administradores sobre múltiplas falhas
3. **Exportação de Logs**: Exportar logs em CSV/PDF
4. **Gráficos**: Visualizar tendências ao longo do tempo
5. **Limpeza Automática**: Limpar logs antigos (> 90 dias)
6. **Rate Limiting**: Limitar tentativas de login por IP

## 🧪 Testando

1. **Acesse a página de login**: Um log tipo "acesso_pagina" será criado
2. **Tente fazer login com credenciais erradas**: Um log tipo "tentativa_login" com falha será criado
3. **Faça login com sucesso**: Um log tipo "tentativa_login" com sucesso será criado
4. **Navegue para `/login/logs.html`**: Você verá todos os logs registrados

## 📞 Suporte

Para dúvidas ou melhorias, consulte a documentação do Firebase:
- [Firebase Firestore Documentation](https://firebase.google.com/docs/firestore)
- [Firebase Authentication](https://firebase.google.com/docs/auth)
