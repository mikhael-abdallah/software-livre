# Diário de Bordo - Desenvolvimento de Software Livre

Este é um blog simples feito com [Hugo](https://gohugo.io/) para documentar minhas atividades na disciplina de Desenvolvimento de Software Livre.

## Como usar

### Pré-requisitos

- [Hugo](https://gohugo.io/installation/)
- Git

### Executando localmente

1. Clone este repositório:
   ```bash
   git clone --recurse-submodules https://seu-repositorio/blog.git
   cd blog
   ```

2. Inicie o servidor local:
   ```bash
   hugo server -D
   ```

3. Acesse o blog em `http://localhost:1313`

### Adicionando uma nova entrada de aula

Para adicionar uma nova entrada sobre uma aula:

```bash
hugo new content posts/aula-XX.md -k aula
```

Substitua `XX` pelo número da aula (ex: aula-02, aula-03).

Edite o arquivo criado em `content/posts/aula-XX.md` com suas anotações da aula.

### Gerando o site estático

Para gerar os arquivos estáticos do site:

```bash
hugo
```

Os arquivos serão gerados na pasta `public/`.

### Implantando no GitHub Pages

Este blog está configurado para ser implantado automaticamente no GitHub Pages usando GitHub Actions:

1. Criar um repositório no GitHub (ex: `diario-bordo-livre`)

2. Fazer o commit das mudanças e push para o GitHub:
   ```bash
   git remote add origin https://github.com/seuusername/diario-bordo-livre.git
   git add .
   git commit -m "Configuração inicial do blog"
   git push -u origin main
   ```

3. No GitHub, acesse as configurações do repositório, vá para "Pages" e selecione "GitHub Actions" como fonte.

4. Após o workflow ser executado, seu blog estará disponível em `https://seuusername.github.io/diario-bordo-livre/`

## Estrutura do projeto

- `content/posts/`: Contém os posts do blog (anotações das aulas)
- `content/about/`: Página "Sobre"
- `archetypes/`: Templates para novos conteúdos
- `static/`: Arquivos estáticos (imagens, etc.)
- `themes/`: Tema do blog (Ananke)
- `.github/workflows/`: Configuração do GitHub Actions para implantação automática

## Licença

Este projeto está licenciado sob a licença [MIT](LICENSE). 