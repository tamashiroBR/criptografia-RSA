# Criptografia RSA em Java 🔐

Implementação didática do algoritmo de criptografia assimétrica **RSA (Rivest–Shamir–Adleman)** em Java. O projeto demonstra o ciclo completo de criptografia de mensagens: geração do par de chaves pública/privada, criptografia de um texto com a chave pública e descriptografia com a chave privada, utilizando as APIs nativas de segurança da plataforma Java (`java.security` e `javax.crypto`).

---

## Sobre o Algoritmo RSA

O RSA é um dos algoritmos de criptografia assimétrica mais utilizados no mundo. Diferentemente da criptografia simétrica, onde a mesma chave é usada para cifrar e decifrar, o RSA utiliza um **par de chaves matematicamente relacionadas**:

- **Chave Pública:** compartilhada livremente, utilizada para **criptografar** a mensagem.
- **Chave Privada:** mantida em sigilo pelo destinatário, utilizada para **descriptografar** a mensagem.

Qualquer pessoa pode cifrar uma mensagem usando a chave pública, mas apenas o detentor da chave privada correspondente consegue decifrá-la. Esse princípio garante a confidencialidade da comunicação sem a necessidade de compartilhar segredos entre as partes.

---

## Funcionalidades

- **Geração de par de chaves RSA** de 1024 bits e armazenamento em arquivos no sistema de arquivos local
- **Verificação de existência** das chaves antes de gerar um novo par, evitando sobrescrita acidental
- **Criptografia de mensagens** em texto puro utilizando a chave pública
- **Descriptografia de mensagens** cifradas utilizando a chave privada
- **Demonstração completa** do fluxo de criptografia e descriptografia via método `main`

---

## Tecnologias e Dependências

O projeto utiliza exclusivamente as bibliotecas nativas do **Java SE**, sem dependências externas:

| Biblioteca | Finalidade |
|---|---|
| `java.security.KeyPairGenerator` | Geração do par de chaves RSA |
| `java.security.KeyPair` | Encapsulamento do par de chaves |
| `java.security.PublicKey` / `PrivateKey` | Representação das chaves pública e privada |
| `javax.crypto.Cipher` | Operações de criptografia e descriptografia |
| `java.io.ObjectOutputStream` / `ObjectInputStream` | Serialização e leitura das chaves em arquivo |

---

## Pré-requisitos

- **Java Development Kit (JDK)** versão 8 ou superior instalado
  - [Download JDK - Oracle](https://www.oracle.com/java/technologies/downloads/)
  - [Download JDK - OpenJDK](https://openjdk.org/)
- Permissão de escrita no diretório `C:/keys/` (Windows) para armazenamento das chaves

Para verificar se o Java está instalado corretamente, execute no terminal:

```bash
java -version
javac -version
```

---

## Instalação e Configuração

### 1. Clone o repositório

```bash
git clone https://github.com/tamashiroBR/criptografia-algoritmo.git
cd criptografia-algoritmo
```

### 2. Crie o diretório para armazenamento das chaves

O programa armazena as chaves geradas em `C:/keys/`. Crie o diretório antes de executar:

**Windows (PowerShell ou CMD):**
```bash
mkdir C:\keys
```

**Linux / macOS:**

Caso queira adaptar o projeto para outros sistemas operacionais, altere as constantes `PATH_CHAVE_PRIVADA` e `PATH_CHAVE_PUBLICA` no arquivo `criptdecriptRSA.java` para um caminho válido no seu sistema:

```java
public static final String PATH_CHAVE_PRIVADA = "/home/usuario/.keys/private.key";
public static final String PATH_CHAVE_PUBLICA  = "/home/usuario/.keys/public.key";
```

### 3. Compile o código-fonte

O arquivo está dentro do pacote `algoritmo`. A partir da raiz do projeto, compile com:

```bash
javac algoritmo/criptdecriptRSA.java
```

> Caso o arquivo esteja na raiz sem estrutura de pacote, utilize apenas `javac criptdecriptRSA.java`.

---

## Como Executar

Após a compilação, execute a classe principal com o comando:

```bash
java algoritmo.criptdecriptRSA
```

### Saída esperada no terminal

Na primeira execução, o programa irá gerar o par de chaves e salvá-las em `C:/keys/`. Em seguida, realizará a criptografia e descriptografia da mensagem de demonstração:

```
Mensagem Original:        Algoritmo RSA de chave assimétrica
Mensagem Criptografada:   [B@6d06d69c
Mensagem Decriptografada: Algoritmo RSA de chave assimétrica
```

> **Nota:** A representação da mensagem criptografada exibida no terminal (`[B@...`) é a referência de memória do array de bytes em Java. Os dados estão corretamente cifrados; para uma exibição legível, o array deveria ser convertido para Base64 ou Hexadecimal.

Nas execuções seguintes, o programa detectará que as chaves já existem em `C:/keys/` e reutilizará o par gerado anteriormente, sem sobrescrever os arquivos.

---

## Estrutura do Projeto

```
criptografia-algoritmo/
├── algoritmo/
│   └── criptdecriptRSA.java    # Classe principal com toda a lógica RSA
└── README.md                   # Documentação do projeto
```

### Descrição dos métodos

**`geraChave()`**
Gera um par de chaves RSA de 1024 bits utilizando o `KeyPairGenerator`. Cria os arquivos `private.key` e `public.key` no diretório `C:/keys/` e serializa os objetos de chave nesses arquivos usando `ObjectOutputStream`.

**`verificaSeExisteChavesNoSO()`**
Verifica se os arquivos `private.key` e `public.key` já existem no sistema de arquivos. Retorna `true` caso ambos existam, evitando a geração desnecessária de um novo par.

**`criptografa(String texto, PublicKey chave)`**
Recebe uma mensagem em texto puro e a chave pública, inicializa o `Cipher` em modo de criptografia (`ENCRYPT_MODE`) e retorna o texto cifrado como um array de bytes (`byte[]`).

**`decriptografa(byte[] texto, PrivateKey chave)`**
Recebe o array de bytes cifrado e a chave privada, inicializa o `Cipher` em modo de descriptografia (`DECRYPT_MODE`) e retorna a mensagem original como `String`.

**`main(String[] args)`**
Método de demonstração que orquestra o fluxo completo: verifica/gera as chaves, lê a chave pública do arquivo, criptografa a mensagem de teste, lê a chave privada do arquivo, descriptografa a mensagem e imprime os resultados no console.

---

## Fluxo de Funcionamento

```
┌─────────────────────────────────────────────────────────────────┐
│                        FLUXO RSA                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Geração de Chaves                                           │
│     KeyPairGenerator → Par (Chave Pública + Chave Privada)      │
│     Salva em: C:/keys/public.key e C:/keys/private.key          │
│                                                                 │
│  2. Criptografia                                                │
│     Mensagem Original ──[Chave Pública]──► Texto Cifrado        │
│                                                                 │
│  3. Descriptografia                                             │
│     Texto Cifrado ──[Chave Privada]──► Mensagem Original        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Licença

Este projeto é de uso privado e foi desenvolvido para fins de estudo e aprendizado de criptografia assimétrica com Java.
