# Docker Stream

Como streamar para mais de uma plataforma ao mesmo tempo usando Docker e Nginx

Tutorial:
https://www.youtube.com/watch?v=EzmA8uksOG4

https://obsproject.com/forum/resources/obs-studio-stream-to-multiple-platforms-or-channels-at-once.932/

Lista de servidores:
https://github.com/obsproject/obs-studio/blob/master/plugins/rtmp-services/data/services.json

Servidor Twitch:
São Paulo: rtmp://live-sao.twitch.tv/app
Rio de Janeiro: rtmp://live-rio.twitch.tv/app

Servidor Youtube:
rtmp://a.rtmp.youtube.com/live2

## Como configurar 

1. Instale o docker;

2. Copie o arquivo `nginx.conf.example` para `nginx.conf` e ajuste as URLs colocando as suas chaves;

3. Crie o container com o comando:

```console
docker run -d -p 1935:1935 --name stream --mount type=bind,source=$(pwd)/nginx.conf,target=/etc/nginx/nginx.conf tiangolo/nginx-rtmp
```

(se já estava criado, só subir com `docker start stream`)

4. Atualize os pacotes: 

```console
docker exec stream apt update && docker exec stream apt upgrade -y
```

5. Na Twitch, se vc quiser testar a conexão antes entrar live, pode usar uma query string depois da key: `?bandwidthtest=true`
Link para testar a Twitch: https://inspector.twitch.tv/#/fdaciuk/

6. Salve o arquivo e saia. Digite `docker exec stream nginx -t` pra testar as configurações.

7. Reinicie o container com `docker restart stream`

8. Entre no OBS, escolha Custom Stream e coloque o endereço como `rtmp://localhost/live`

9. Se estiver usando outro PC para criar o servidor, use o IP do PC ao invés do localhost

10. Agora pode testar sua stream e boa! Lembre de tirar o `?bandwidthtest=true` da Twitch para que a stream passe ao vivo :)

## Pós Instalação

Se já tiver feito os passos acima pelo menos uma vez:

- Verificar os containers em pé:
`docker ps`

- Verificar se o container _stream_ está na lista de containers:
`docker ps -a`

- Subir o container (se já não estiver em pé):
`docker start stream`

- Derrubar o container (quando não estiver utilizando):
`docker stop stream`

- Verificar se as chaves da Twitch e YouTube estão corretas (para editar, só alterar o arquivo `nginx.conf` e resetar o container):
`docker exec stream cat /etc/nginx/nginx.conf`
