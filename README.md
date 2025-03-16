# Template de Assistente de Desenvolvimento Node.js com Fastify e Zod para Claude

Este repositório contém um template de instruções para transformar o Claude em um assistente especializado em desenvolvimento backend com Node.js, Fastify e Zod, focando em TypeScript. Com esse template, o Claude se torna uma ferramenta poderosa para ajudar desenvolvedores a escrever código limpo, bem tipado e seguindo as melhores práticas para aplicações backend de alta performance.

## Sobre

Este template instrui o Claude a:

- Utilizar TypeScript como linguagem padrão para todos os exemplos
- Focar em Node.js, Fastify, Zod e outras tecnologias backend modernas
- Fornecer código bem escrito, com tipagem estrita e sem uso de `any`, seguindo boas práticas
- Explicar conceitos de arquitetura e decisões de design de forma clara
- Organizar respostas de maneira estruturada e educativa
- Priorizar a segurança, escalabilidade e alta performance em aplicações backend
- Explorar os benefícios do Fastify em relação a outros frameworks

## Como Usar

1. **Copie o template**: Abra o arquivo `template.md` e copie todo o conteúdo
2. **Cole no início da sua conversa com Claude**: Comece uma nova conversa com o Claude e cole o template como primeira mensagem
3. **Envie o template**: Depois que o Claude receber e processar o template, você pode começar a fazer suas perguntas relacionadas a desenvolvimento backend

### Dicas de Uso

- O template funciona melhor com o Claude 3 Opus ou Claude 3.5 Sonnet ou versões superiores
- O efeito do template persiste durante toda a conversa - você não precisa reenviá-lo para cada pergunta
- Para conversas longas, você pode ocasionalmente lembrar o Claude para "continuar seguindo o template de desenvolvimento Node.js com Fastify e Zod"
- Se quiser modificar algum aspecto específico da resposta do Claude, você pode fazer solicitações diretas como "responda com mais exemplos de código" ou "explique em mais detalhes"

## Exemplos de Uso

Aqui estão alguns exemplos de perguntas que você pode fazer ao Claude após aplicar o template:

### Exemplo 1: Solicitar implementação de um recurso específico

```
Crie uma API RESTful para gerenciar tarefas (todo list) usando Fastify, 
TypeScript e Zod. Inclua validação de entrada, tratamento de erros, e 
documentação com Swagger.
```

### Exemplo 2: Resolver um problema específico

```
Estou tendo problemas com validação de dados em minha API Fastify. Como posso 
integrar o Zod com o sistema de validação nativo do Fastify? Quero aproveitar as 
vantagens de performance do Fastify mas com a flexibilidade do Zod.
```

### Exemplo 3: Aprender sobre um conceito

```
Explique como implementar autenticação JWT em uma API Fastify com TypeScript. 
Como posso criar hooks de pré-validação para proteger rotas e implementar 
diferentes níveis de autorização para usuários?
```

### Exemplo 4: Solicitar refatoração

```
Refatore este código Express para usar Fastify, TypeScript e validação com Zod:

const express = require('express');
const app = express();
app.use(express.json());

app.post('/users', (req, res) => {
  const { name, email, age } = req.body;
  
  if (!name || !email || !age) {
    return res.status(400).json({ error: 'Missing required fields' });
  }
  
  if (typeof name !== 'string' || name.length < 3) {
    return res.status(400).json({ error: 'Name must be at least 3 characters' });
  }
  
  if (typeof email !== 'string' || !email.includes('@')) {
    return res.status(400).json({ error: 'Invalid email format' });
  }
  
  if (typeof age !== 'number' || age < 18) {
    return res.status(400).json({ error: 'User must be at least 18 years old' });
  }
  
  // Save user to database...
  
  res.status(201).json({ message: 'User created successfully' });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

## Personalização

Você pode personalizar o template para se adequar melhor às suas necessidades:

1. Abra o arquivo `template.md`
2. Modifique as seções relevantes para enfatizar tecnologias ou padrões específicos
3. Adicione exemplos personalizados ou remova partes que não são relevantes para seu caso de uso
4. Salve as alterações e use o template modificado nas suas conversas com Claude

## Por que Fastify?

Fastify é um framework web para Node.js que oferece:

- **Alta Performance**: Um dos frameworks mais rápidos do ecossistema Node.js
- **Baixa Sobrecarga**: Footprint mínimo e uso eficiente de recursos
- **Plugins**: Sistema robusto e modular de plugins
- **Validação Integrada**: Sistema de validação e serialização eficiente
- **Esquemas JSON**: Validação baseada em esquemas para melhor performance
- **Tipagem**: Excelente suporte para TypeScript
- **Logging**: Sistema de logging assíncrono de alto desempenho

## Contribuições

Contribuições são bem-vindas! Se você tiver sugestões para melhorar o template:

1. Abra uma issue descrevendo sua sugestão
2. Envie um pull request com suas alterações
3. Compartilhe exemplos de uso bem-sucedidos

## Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo LICENSE para mais detalhes.
