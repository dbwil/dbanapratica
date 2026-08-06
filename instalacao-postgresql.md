# Instalação do PostgreSQL no Rocky Linux

## Objetivo

Este documento descreve o procedimento utilizado para instalar e colocar o PostgreSQL em funcionamento no Rocky Linux utilizando o repositório oficial PGDG.

---

## Como instalar o PostgreSQL

Primeiramente foi adicionado o repositório oficial do PostgreSQL (PGDG). Em seguida, os módulos antigos do PostgreSQL foram desabilitados para evitar conflitos de versões. Após isso foi realizada a instalação da versão mais recente disponível utilizando o DNF.

---

## Como verificar se a instalação foi concluída com sucesso

A instalação foi confirmada verificando os pacotes instalados e consultando a versão do PostgreSQL através do comando:

```
/usr/pgsql-17/bin/psql --version
```

Resultado:

```
psql (PostgreSQL) 17.10
```

---

## Como iniciar o serviço

```
systemctl start postgresql-17
```

Esse comando inicia o servidor PostgreSQL.

---

## Como parar o serviço

```
systemctl stop postgresql-17
```

Interrompe o funcionamento do banco de dados.

---

## Como reiniciar o serviço

```
systemctl restart postgresql-17
```

Utilizado quando alguma configuração é alterada ou quando é necessário reiniciar o servidor.

---

## Como verificar o status

```
systemctl status postgresql-17
```

Esse comando informa se o serviço está em execução e mostra informações sobre seu funcionamento.

---

## Como acessar o PostgreSQL

Primeiro acessar o usuário postgres:

```
su - postgres
```

Depois abrir o terminal do PostgreSQL:

```
psql
```

---

## Como listar os bancos existentes

Dentro do psql:

```
\l
```

Esse comando mostra todos os bancos de dados existentes.

---

## Como criar um banco de dados

```
CREATE DATABASE laboratorio;
```

Após criar, utilize novamente:

```
\l
```

para confirmar sua criação.

---

## Localização dos arquivos

### Binários

```
/usr/pgsql-17/bin
```

---

### Arquivos de configuração

```
/var/lib/pgsql/17/data/postgresql.conf
```

```
/var/lib/pgsql/17/data/pg_hba.conf
```

---

### Diretório de dados

```
/var/lib/pgsql/17/data
```

---

### Bancos de dados

```
/var/lib/pgsql/17/data/base
```

---

### Logs

Os logs podem ser consultados utilizando:

```
journalctl -u postgresql-17
```

---

## Serviço criado

```
postgresql-17.service
```

---

## Usuário responsável

```
postgres
```
