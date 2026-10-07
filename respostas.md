# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome: Arthur de Souza Gois
Matrícula: 26175437
Usuário do GitHub: SGZ777
Usuário do Docker Hub: arthursg7


Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
nginx:1.27-alpine - 73.6 MB
2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
O Nginx procura os arquivos do site na pasta /usr/share/nginx/html. Usei o comando docker exec teste-portal ls /usr/share/nginx/html e confirmei que o index.html está lá dentro.

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
arthursg7/viaserra-portal:1.0-26175437
4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?
docker build -t arthursg7/viaserra-portal:1.0-26175437 ./portal
docker push arthursg7/viaserra-portal:1.0-26175437
## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | COPY pagina/ . | A instrução aponta para uma pasta que não existe no contexto | O docker build falhou com "/pagina": not found | Troquei o COPY pagina/ para COPY site/ |
| 2 | COPY site/ . | Os arquivos foram copiados para /usr/share/nginx, e não para o diretório usado pelo Nginx para servir o site. | O index.html da manutenção ficou em /usr/share/nginx/index.html, enquanto /usr/share/nginx/html continuava com o arquivo padrão. | Alterei para COPY site/ /usr/share/nginx/html/. |
| 3 | CMD ["nginx"] | O Nginx não ficou executando em primeiro plano no container. | O container iniciou e terminou com Exited (0). | Alterei para CMD ["nginx", "-g", "daemon off;"]. |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
A opção -p 7042:80 mapeia a porta 7042 do host para a porta 80 do container. Já -p 80:7042 mapeia a porta 80 do host para a porta 7042 do container. O segundo número é a porta do container.

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.
docker run -d --name portal -p 8037:80 arthursg7/viaserra-portal:1.0-26175437 - docker run -d --name manutencao -p 7037:80 manutencao:26175437
8. Qual comando derruba os dois containers de uma vez?
docker compose down
## Verificador

9. Código de conclusão impresso pelo verificador:

```
VIASERRA-26175437-C88FF524
```
