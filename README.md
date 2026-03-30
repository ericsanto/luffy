
<div align="center">
  <!-- Certifique-se de salvar a logo da pasta docs/logo.png -->
  <img src="docs/logo.png" alt="Luffy Framework Logo" width="350"/>

  # Luffy
  
  **Um framework web leve e ultrarrápido sendo escrito em C puro.**
  
  [![C](https://img.shields.io/badge/Linguagem-C-A8B9CC?style=for-the-badge&logo=c&logoColor=white)](https://pt.wikipedia.org/wiki/C_(linguagem_de_programa%C3%A7%C3%A3o))
  [![Docker](https://img.shields.io/badge/Docker-Pronto-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
  [![Status: Em Desenvolvimento](https://img.shields.io/badge/Status-Em_Desenvolvimento-yellow?style=for-the-badge)]()
  [![License: MIT](https://img.shields.io/badge/Licen%C3%A7a-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
  

</div>

---

## 📖 Sobre o Projeto

O **Luffy** é um framework web acadêmico e experimental **em desenvolvimento**, projetado para construir APIs REST com extrema eficiência. **Sendo construído** do zero em C, sem dependências pesadas, o projeto remove as abstrações modernas dos frameworks atuais para expor o poder bruto dos sockets TCP e do protocolo HTTP.

Este projeto **está sendo desenvolvido** como trabalho de conclusão de curso (TCC) para demonstrar, na prática, a engenharia de sistemas de baixo nível. O objetivo foca em cenários onde **desempenho, mínimo consumo de memória e latência previsível** são críticos: dispositivos IoT, Edge Computing e sistemas embarcados.

## ✨ Funcionalidades (Escopo Atual)

- 🚀 **Concorrência TCP Bruta:** Servidor multithreadado utilizando POSIX threads (`pthreads`).
- 📨 **Parser HTTP Artesanal:** Analisador integrado para processar requisições HTTP/1.1 de forma eficiente.
- 🛣️ **Roteamento Inteligente:** Mapeamento rápido de endpoints utilizando Tabelas Hash.
- ⛓️ **Cadeia de Middlewares:** Implementação de lógicas transversais (Autenticação, Logs) utilizando o padrão *Chain of Responsibility* através de ponteiros para funções.
- 🧩 **JSON Nativo:** Integração planejada com a biblioteca `cJSON` para manipulação de corpos de requisição e resposta.
- 🐳 **Micro-Dockerização:** CLI nativa para gerar `Dockerfiles` otimizados em múltiplos estágios, visando imagens finais de apenas alguns megabytes (`FROM scratch`).

---

## 🚀 Início Rápido (Objetivo da API)

*O código abaixo representa o design final de uso que o framework está implementando.*

### Pré-requisitos
- GCC (Linux/macOS) ou MinGW (Windows)
- Make
- Biblioteca `cJSON` (a ser linkada na pasta `lib/`)

### Exemplo de Uso Futuro
Crie um arquivo chamado `main.c`:

```c
#include "luffy.h"

// Define o seu handler (controlador)
void hello_handler(Request *req, Response *res) {
    cJSON *json = cJSON_CreateObject();
    cJSON_AddStringToObject(json, "mensagem", "Olá do Luffy!");
    cJSON_AddStringToObject(json, "versao", "1.0");
    
    res_set_status(res, 200);
    res_set_json(res, json);
    
    cJSON_Delete(json);
}

int main() {
    App *app = luffy_create();

    // Registra a rota
    luffy_get(app, "/hello", hello_handler);

    // Inicia o servidor na porta 8080
    luffy_listen(app, 8080);

    luffy_destroy(app);
    return 0;
}
```

### Compilação Almejada
```bash
gcc main.c -o minha_api -I./include -L./lib -lluffy -lpthread
./minha_api
```

---

## 🐳 A Mágica do Docker

O Luffy **irá fornecer** uma ferramenta de linha de comando (CLI) para auxiliar no scaffold do projeto. O comando `create dockerfile` gerará um ambiente otimizado para aplicações C:

```bash
luffy create dockerfile
docker build -t minha-api-luffy .
docker run -p 8080:8080 minha-api-luffy
```

**O objetivo dessa abordagem:** 
O Dockerfile gerado usará uma build em múltiplos estágios. Ele compila o código em um ambiente Alpine e copia **apenas o binário estático** para uma imagem `FROM scratch`. O tamanho final do container deve ficar em torno de **2MB a 5MB**, contra centenas de megabytes em containers Node.js ou Python.

---

## 🏗️ Arquitetura (Foco Acadêmico)

O Luffy **está sendo arquitetado** com rigor acadêmico para demonstrar conceitos de engenharia de sistemas de baixo nível:
- **Sem Coletor de Lixo (Garbage Collection):** Controle total sobre a alocação de memória (com validação contínua e rigorosa via `Valgrind` durante o desenvolvimento).
- **Padrões de Projeto em C:** Implementação do *Chain of Responsibility* (Middlewares) e *Front Controller* (Router) utilizando ponteiros para funções, substituindo a necessidade de Orientação a Objetos.
- **Camadas de Rede:** Interação direta com a Camada de Transporte (TCP/IP) para compreender os protocolos da Camada de Aplicação (HTTP).

---

## 📜 Licença

Este projeto está licenciado sob a licença Apache 2.0 - veja o arquivo [LICENSE](LICENSE) para mais detalhes.
