# 🧪 Atividade de Ciências — Revisão em Dupla

> Aplicação web interativa para revisão de Ciências do 6º ao 9º ano, com questões de múltipla escolha, imagens reais e envio de respostas para Google Sheets.

---

## 📋 Sobre o Projeto

Esta atividade foi desenvolvida para a **E.E. Profª Wanda Mascagni de Sá** e permite que os alunos:

- Selecionem sua turma e dupla (apoio + mentor)
- Respondam 10 questões de revisão por turma
- Visualizem imagens reais ilustrativas em cada questão
- Enviem as respostas automaticamente para uma planilha Google

---

## 🚀 Como Usar

### Opção 1: Acessar Online (GitHub Pages)

1. Acesse o link publicado: `https://SEU-USUARIO.github.io/NOME-DO-REPO/`
2. Selecione a turma e a dupla
3. Responda as questões clicando nas alternativas
4. Clique em **"Enviar respostas"**

### Opção 2: Rodar Localmente

1. Baixe ou clone este repositório
2. Abra o arquivo `index.html` em qualquer navegador moderno (Chrome, Firefox, Edge)
3. **Não requer servidor** — funciona 100% offline

---

## 📁 Estrutura do Projeto

```
📦 atividade-ciencias/
├── 📄 index.html              # Página principal (HTML + CSS + JS)
├── 📁 imagens/                # Imagens reais ilustrativas
│   ├── eclipse_solar.png
│   ├── vulcao_erupcao.png
│   ├── coracao.png
│   ├── dna.png
│   └── ... (37 imagens no total)
└── 📄 README.md               # Este arquivo
```

---

## 🔧 Configuração do Envio (Google Sheets)

Para que os alunos possam enviar as respostas, é necessário configurar o **Google Apps Script**:

### 1. Criar a Planilha

1. Acesse [Google Sheets](https://sheets.new) e crie uma planilha nova
2. Renomeie a primeira aba para `"Respostas"`
3. Na célula **A1**, cole este cabeçalho:

```
Data|Turma|Dupla|Apoio|Mentor|Q1|Q2|Q3|Q4|Q5|Q6|Q7|Q8|Q9|Q10
```

### 2. Criar o Apps Script

1. No menu da planilha: `Extensões` → `Apps Script`
2. Apague o código padrão e cole o script abaixo:

```javascript
function doPost(e) {
  const planilha = SpreadsheetApp.getActiveSpreadsheet();
  const aba = planilha.getSheetByName("Respostas");

  const dados = JSON.parse(e.postData.contents);
  const linha = [
    new Date(),
    dados.turma,
    dados.duplaIndex,
    dados.apoioNome,
    dados.mentorNome,
    ...dados.respostas
  ];

  aba.appendRow(linha);

  return ContentService.createTextOutput(JSON.stringify({status: "ok"}))
    .setMimeType(ContentService.MimeType.JSON);
}

function doGet(e) {
  return ContentService.createTextOutput("OK");
}
```

3. Salve o projeto (Ctrl+S) e dê um nome (ex: `AtividadeCiencias`)

### 3. Publicar como Aplicativo Web

1. No Apps Script, clique em `Implantar` → `Novo implantação`
2. Tipo: **Aplicativo da Web**
3. Execute como: **Eu**
4. Acesso: **Qualquer pessoa**
5. Clique em `Implantar` e copie a URL (termina em `/exec`)

### 4. Inserir a URL no Código

1. Abra o arquivo `index.html`
2. Localize a linha:
   ```javascript
   const WEBAPP_URL = 'https://script.google.com/macros/s/.../exec';
   ```
3. Substitua pela URL que você copiou
4. Salve e envie as alterações para o GitHub

---

## 🎨 Personalização

### Alterar Questões

As questões estão no bloco `<script id="dados-turmas" type="application/json">` dentro do `index.html`. Cada turma possui:

- `nomeTurma`: nome exibido (ex: "6º Ano A")
- `duplas`: lista de pares apoio/mentor
- `questoes`: array com pergunta, alternativas e imagem

### Alterar Imagens

1. Substitua o arquivo PNG desejado na pasta `imagens/`
2. Mantenha o mesmo nome do arquivo
3. Commit e push

### Alterar Estilo Visual

O CSS está embutido no `<style>` do `index.html`. Principais variáveis:

```css
:root {
  --azul-900: #0f2a4d;   /* Azul escuro */
  --azul-700: #1a3c6e;   /* Azul médio */
  --azul-500: #2f5aa8;   /* Azul claro */
  --cinza-900: #20242c;  /* Texto principal */
  --cinza-300: #d7dbe2;  /* Bordas */
  --cinza-100: #f4f6f9;  /* Fundo */
}
```

---

## 🖨️ Imprimir a Atividade

O CSS inclui uma media query `@media print` que:

- Remove o cabeçalho azul
- Tira sombras e cores de fundo
- Deixa as alternativas em preto e branco
- Ajusta margens para A4

> Basta pressionar **Ctrl+P** (ou Cmd+P no Mac) no navegador.

---

## ⚠️ Limitações Conhecidas

| Limitação | Explicação |
|-----------|------------|
| **Modo `no-cors`** | O envio usa `mode: 'no-cors'`, então não é possível confirmar sucesso/falha diretamente no JS. A planilha recebe os dados normalmente. |
| **Offline** | As imagens precisam ser carregadas uma vez. Após isso, o navegador as cacheia. |
| **Navegadores antigos** | Requer navegador com suporte a ES6 (Chrome 60+, Firefox 55+, Edge 79+). |

---

## 📝 Licença

Este projeto é de uso educacional livre para professores da rede pública.

Desenvolvido para a **E.E. Profª Wanda Mascagni de Sá**.

---

## 💡 Dicas

- **Backup:** Exporte a planilha do Google Sheets periodicamente (CSV ou Excel)
- **Teste:** Sempre teste o envio com uma dupla de teste antes de aplicar com os alunos
- **Mobile:** A atividade funciona perfeitamente em celulares e tablets

---

> 💬 Dúvidas ou sugestões? Abra uma *issue* neste repositório.
