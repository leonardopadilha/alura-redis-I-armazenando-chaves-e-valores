# Redis I: Armazenamento de chaves e valores

## Etiquetas

[![REDIS](https://img.shields.io/badge/License-redis-red.svg)](https://redis.io/)

## Inicialização

Iniciar o redis após instalação

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