# Pratica-de-RPC

Alunos : Pedro Victor Gomes, Guilherme Hentges

Desafios :

# Sistema RPC em Python com RPyC

Este projeto demonstra a implementação de um **sistema de chamadas de procedimento remoto (RPC)** utilizando Python e a biblioteca **RPyC**.

O sistema é composto por dois programas:

* **servidor.py** → responsável por disponibilizar métodos que podem ser chamados remotamente.
* **cliente.py** → responsável por se conectar ao servidor e executar esses métodos.

Neste documento serão descritas as **instruções de execução** e a **documentação das opções 7 e 8 do cliente**, que utilizam chamadas RPC para manipular dados no servidor.

---

# 1. Requisitos

Antes de executar o projeto, é necessário instalar a biblioteca utilizada para RPC.

Instalação da biblioteca:

```
pip install rpyc
```

Também é necessário ter **Python 3 instalado** no sistema.

---

# 2. Estrutura do Projeto

```
projeto-rpc/
│
├── servidor.py
├── cliente.py
└── README.md
```

---

# 3. Como Executar o Sistema

## 3.1 Iniciar o servidor

Abra um terminal e execute:

```
python servidor.py
```

Se o servidor iniciar corretamente, será exibida a mensagem:

```
Servidor RPC iniciado em localhost:18861
```

O servidor ficará aguardando conexões do cliente.

---

## 3.2 Executar o cliente

Em outro terminal execute:

```
python cliente.py
```

Se a conexão for estabelecida corretamente, aparecerá:

```
Conectado ao servidor localhost:18861
```

Em seguida será exibido um **menu interativo** com as opções disponíveis.

---

# 4. Documentação dos Métodos (Opções 7 e 8)

## 4.1 Opção 7 — Inverter String

### Descrição

Esta funcionalidade permite que o usuário envie um texto ao servidor e receba como resposta **a string invertida**.

A inversão da string ocorre **no servidor**, demonstrando o funcionamento de uma chamada RPC.

---

### Implementação no Servidor

```
def exposed_inverter_string(self, texto):
    return str(texto)[::-1]
```

### Funcionamento

1. O cliente solicita ao usuário um texto.
2. O cliente envia esse texto ao servidor através da chamada remota.
3. O servidor converte o texto para string (caso necessário).
4. O servidor inverte a string utilizando **fatiamento de Python (`[::-1]`)**.
5. O resultado é retornado ao cliente.

---

### Chamada no Cliente

```
texto = input("Digite um texto para inverter: ")
resultado = conn.root.inverter_string(texto)
print(f"Texto invertido (servidor): {resultado}")
```

---

### Exemplo de Execução

Entrada:

```
Digite um texto para inverter: Python
```

Saída:

```
Texto invertido (servidor): nohtyP
```

---

# 4.2 Opção 8 — Deletar Item da Lista Remota

### Descrição

Esta funcionalidade permite **remover um item de uma lista armazenada no servidor**.

A lista é compartilhada entre todas as conexões e mantida no servidor.

---

### Implementação no Servidor

```
def exposed_deletar_item(self, item):
    try:
        self.__class__.lista_compartilhada.remove(item)
    except ValueError:
        return f"Item '{item}' não encontrado na lista remota."
    return self.__class__.lista_compartilhada
```

---

### Funcionamento

1. O cliente solicita a lista atual ao servidor.
2. O usuário escolhe qual item deseja remover.
3. O cliente envia o item ao servidor.
4. O servidor tenta remover o item da lista compartilhada.
5. Caso o item exista, ele é removido.
6. Caso o item não exista, o servidor retorna uma mensagem de erro.
7. O resultado é retornado ao cliente.

---

### Chamada no Cliente

```
print(conn.root.listar_itens())
texto = input("Digite o item a deletar: ")
resultado = conn.root.deletar_item(texto)
print(f"Lista remota após deleção: {resultado}")
```

---

### Exemplo de Execução

Lista atual:

```
['maçã', 'banana', 'uva']
```

Entrada:

```
Digite o item a deletar: banana
```

Saída:

```
Lista remota após deleção: ['maçã', 'uva']
```

---

### Exemplo de Erro

Se o item não existir:

Entrada:

```
Digite o item a deletar: laranja
```

Saída:

```
Item 'laranja' não encontrado na lista remota.
```

---

# 5. Conceitos Demonstrados

Este projeto demonstra alguns conceitos importantes de **computação distribuída**:

* **RPC (Remote Procedure Call)**
* Comunicação cliente-servidor
* Execução remota de métodos
* Compartilhamento de dados no servidor
* Manipulação de listas remotas

---

# 6. Observação

Todos os métodos do servidor que começam com **`exposed_`** podem ser chamados remotamente pelo cliente através de:

```
conn.root.nome_do_metodo()
```

Isso permite que o cliente execute funções no servidor como se fossem locais.

---
