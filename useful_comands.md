# comandos úteis do meu dia a dia

### docker

- docker exec -it <b>nome do container</b> /bin/bash
<h6>entrar dentro de algum container</h6>

- docker compose up -d <b>nome do serviço</b>
<h6>builda/roda um serviço apenas do docker compose (detached)</h6>

### youtube audio downloader

- yt-dlp -x --audio-format opus --audio-quality 0 <b>url do vídeo</b> -o "audio_teste.ogg"
<h6>baixa algum áudio de vídeo no youtube</h6>

### find

- find /path/to/directory -type f -exec du -h {} \; | sort -rh | head -n 10
<h6>encontra os maiores arquivos dentro de um diretório</h6>

### swap

- sudo swapoff -a && sudo swapon -a
<h6>limpa swap e joga de volta pra memória principal</h6>
