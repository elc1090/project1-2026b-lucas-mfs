
# Projeto: Remake de aplicação web simples


## Acesso

https://elc1090.github.io/project1-2026b-lucas-mfs/

## Desenvolvedor(a)

Lucas Medeiros, Sistemas de Informação - UFSM

## App original

### Links

- Repositório: https://github.com/elc1090/demo-challenge-of-the-day

### Descrição

A aplicação original para publicar desafios curtos e registrar respostas de estudantes. O frontend usa HTML, CSS e JavaScript sem frameworks; o backend usa Google Apps Script e Google Sheets.

## Demanda do(a) cliente

### Cliente
Professor(a) / Usuário do sistema

### Demanda
1. "⁠Gostaria de adicionar um cronômetro para cada um dos desafios, com um tempo limite para o estudante enviar a resposta. E o tempo limite poderia ser definido junto com as outras informações do desafio, para que cada um tenha um tempo adequado ao seu nível de dificuldade. Quando o tempo acabar, deve aparecer uma mensagem avisando que não poderá mais enviar a resposta."
2. "Quero que sejam adicionados mais desafios e que cada um seja associado a uma data específica. Também gostaria que os desafios pudessem ser separados por categorias ou assuntos, com uma etiqueta indicando a categoria e que seja exibida junto ao enunciado da questão."
3. "Também gostaria de melhorar a interface da página quando não houver um desafio disponível para o dia. Em vez de deixar apenas a mensagem “Nenhum desafio disponível no momento” na área onde normalmente apareceria o desafio, gostaria que fosse apresentada uma interface específica para essa situação, ~~informando também a data do próximo desafio disponível.~~"
4. ~~"Gostaria que fosse adicionada uma área de desempenho do estudante, mostrando a quantidade de desafios que ele já respondeu, a quantidade de acertos e erros e sua porcentagem de acertos."~~

## Desenvolvimento

### Processo

Iniciei mapeando a comunicação entre o `app.js` e o `Code.gs`. Para a demanda do cronômetro, incluí a propriedade `time_limit_seconds` no JSON do desafio e implementei um `setInterval` no frontend que bloqueia o formulário ao zerar. As etiquetas já possuíam base no código original, exigindo apenas o preenchimento correto da propriedade `topics` na planilha.

O maior desafio foi suportar múltiplos desafios diários. Alterei o backend para retornar um array (`daily_challenges`) e criei um índice global (`currentChallengeIndex`) no frontend. Ao invés de encerrar a sessão após o envio, a interface agora verifica se há mais itens no array e injeta um botão para renderizar o próximo desafio sem recarregar a página.

### Trechos de código

**1. Cronômetro (app.js)**
Bloqueia os inputs automaticamente quando o tempo acaba.
```javascript
function startTimer(limitSeconds) {
  clearInterval(state.timerInterval);
  if (!limitSeconds || limitSeconds <= 0) {
    timerContainerEl.classList.add("hidden");
    return;
  }
  state.timeLeft = limitSeconds;
  timerContainerEl.classList.remove("hidden");
  
  state.timerInterval = setInterval(() => {
    state.timeLeft--;
    if (state.timeLeft <= 0) {
      clearInterval(state.timerInterval);
      submitButton.disabled = true;
      Array.from(responseForm.elements).forEach(el => el.disabled = true);
    }
  }, 1000);
}

```

**2. Transição entre desafios (app.js)**
Avança o índice e re-renderiza o DOM sem reload de página.

```javascript
  if (hasNext) {
    document.querySelector("#next-challenge-button").addEventListener("click", () => {
      state.currentChallengeIndex++;
      renderChallenge();
      renderResponseForm();
      resetResponseState();
      document.querySelector(".app-shell").scrollIntoView({ behavior: "smooth", block: "start" });
    });
  }

```

**3. Atualização do Template (Code.gs)**
Garante que novos desafios criados pelo painel já venham com o tempo limite padrão.

```javascript
function createChallengeTemplate_() {
  return {
    id: 'example-challenge',
    label: 'Example challenge',
    challenge: {
      challenge_id: 'example-001',
      version: 1,
      title: 'Example challenge',
      time_limit_seconds: 300, // Novo campo
      topics: ['example'],
      // ...
    }
  };
}

```

## Tecnologias

### Linguagens e afins

* **HTML5 e CSS3:** Estruturação semântica e estilização (Flexbox, Grid) sem frameworks.
* **JavaScript (ES6+):** Manipulação de DOM, Fetch API, temporizadores e gerenciamento de estado.
* **Google Apps Script (GAS) & Google Sheets:** Criação de API REST e banco de dados.
* **JSON:** Formato de estruturação dos desafios e troca de dados.

### Ambiente de desenvolvimento

* **Visual Studio Code:** Editor de código.
* **Live Server:** Servidor local para desenvolvimento e testes.
* **Editor do Google Apps Script:** Deploy da API na nuvem.
* **GitHub Pages:** Hospedagem da aplicação frontend.

## Referências e créditos

* MDN Web Docs (documentação sobre Fetch API e setInterval).
* [Google Apps Script Reference](https://developers.google.com/apps-script/reference/spreadsheet).
* Código base fornecido pela disciplina.

---

Projeto entregue para a disciplina de [Desenvolvimento de Software para a Web](http://github.com/andreainfufsm/elc1090-2026b) em 2026b

```

```