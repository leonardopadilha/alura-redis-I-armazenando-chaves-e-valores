# Redis I: Armazenamento de chaves e valores

## Etiquetas

[![REDIS](https://img.shields.io/badge/License-redis-red.svg)](https://redis.io/)

## Inicialização

Iniciar servidor do redis

```bash
  redis-server
```

Executar comandos do redis

```bash
  redis-cli
```

## Alguns comandos

| Comando     | Exemplo                       | Descrição                                         |
| :---------- | :---------------------------- | :------------------------------------------------ |
| `SET`       | `SET "total_de_cursos" 105`   | Salva a chave total cursos com o total de 105     |
| `GET`       | `GET "total_de_cursos"`       | Retorna as informações da chave salva             |
| `SET`       | `SET "total_de_cursos" 106`   | O comando SET também pode alterar uma informação  |
| `DEL`       | `DEL "total_de_cursos"`       | Esse comando deleta a chave salva                 |
| `KEYS`      | `KEYS * ou keys *`            | Retorna todas as chaves salvas                    |
| `flushall ` | `flushall`                    | Apaga todas as chaves e valores salvos            |
| `MSET `     | `MSET "chave" "valor"`        | Para salvar várias chaves ao mesmo tempo          |


## É interessante criar um padrão no momento de criar as chaves

```bash
  SET "chave:descrição" "valor"
  SET resultado:17-05-2015:megasena "2, 15, 18, 25, 28, 32"
```

## Outros comandos um pouco mais avançados

| Comando     | Exemplo                       | Descrição                                         |
| :---------- | :---------------------------- | :------------------------------------------------ |
| `MSET `     | `MSET "chave" "valor"`        | Para salvar várias chaves ao mesmo tempo          |

```bash
  MSET "resultado:03-05-2015:megasena" "1, 3, 17, 19, 24, 26" resultado:22-04-2015:megasena "15, 18, 20, 32, 37, 41" resultado:15-04-2015:megasena "10, 15, 18, 22, 35, 43"
```

## Para realizar pesquisas que comecem com "resultado"

```bash
  KEYS "resultado*"
```

## Para realizar pesquisas que comecem a palavra resultado e que sejam do mês de Maio de 2015 da megasena

```bash
  KEYS "resultado:*-05-2015:megasena"
```

## Dessa forma é possível garantir a existência de apenas um caractere utilizando o "?"

```bash
  keys "resultado:1?-04-2015:megasena"
  KEYS "resultado: ??-??-2015*"
  keys "resultado:??-??-????:megasena"
```

## Para realizar uma pesquisa que estejam entre dias (qualquer coisa) 3 e (qualquer coisa) 5

```bash
  keys "resultado:?[35]-??-????:megasena"
  keys "resultado:?[3,5]-??-????:megasena"
```

## Para obter os resultados dos dias 15 ou 17 de qualquer mês, de qualquer ano, da Mega-sena ou da Giga-sena

```bash
  KEYS "resultado:1[57]-??-????:*sena"
  keys "resultado:1[5,7]-??-????:*sena"
```

## Para salvar informações em um tipo de hash

```bash
  HSET "resultado:24-05-2015:megasena" "numeros" "13, 17, 19, 25, 28, 32"
  HSET "resultado:24-05-2015:megasena" "ganhadores" "23"
```

## Para recuperar os dados

```bash
  HGET "resultado:24-05-2015:megasena" "ganhadores"
  HGET "resultado:24-05-2015:megasena" "numeros"
```

## Para remover um campo de um hash, podemos utilizar o comando

```bash
  HDEL "resultado:24-05-2015:megasena" "numeros"
```
## Da mesma forma que podemos atribuir vários conjuntos de chave e valor de um único comando utilizando o MSET, podemos utilizar o comando HMSET para atribuir vários valores quando utilizarmos um hash no valor.

```bash
  HMSET nome_da_chave campo1 "Ola" campo2 "Mundo"
  HMSET "resultado:05-06-2015" "numeros" "5, 19, 23, 28, 46, 49" "ganhadores" "16"
  HMSET "sessao:usuario:1675" "nome" "guilherme" "total_de_produtos" "3" "sobrenome" "silveira"
```
## Para pesquisar as informações

```bash
  HMGET "resultado:05-06-2015" "numeros" "ganhadores" "resultado:05-06-2015" "numeros"
  HMGET "resultado:05-06-2015" "ganhadores"
  HGET "resultado:05-06-2015" "numeros"
  HGET "sessao:usuario:1675" "nome"
```

## Para recuperar todos os valores que existem no hash: HGETALL, passando como parâmetro o nome da chave (e valor) que está associada ao hash

```bash
  HGETALL "resultado:05-06-2015"
```

## Para indicar que queremos que os valores sejam apagados após um certo intervalo de tempo, utilizamos o comando EXPIRE, passando como argumentos o nome da chave e a quantidade de tempo em segundos:

```bash
  EXPIRE "sessao:usuario:1675" 1800
```

## Isso significa que daqui 1800 segundos, ou 30 minutos, os dados do usuário serão apagados, expirados. Para sabermos quanto tempo falta, para que essas informações sejam apagadas usamos o comando Time Left ou "TTL

```bash
  TTL "sessao:usuario:1675"
```
## Para incrementar o contador (um a um)

```bash
  SET pagina:/contato:25-05-2015 1
  INCR pagina:/contato:25-05-2015
```

## Para decrementar o contador (um a um)

```bash
  DECR pagina:/contato:25-05-2015
```

## Para incrementar o valor de 15 em cada compra (incrementa valores diferentes de 1)

```bash
  INCRBY compras:25-05-2015:valor 15
```

## Para decrementar valores diferentes de 1

```bash
  DECRBY compras:25-05-2015:valor 10
```