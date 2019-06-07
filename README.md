# speedtest-telegram
Teste a velocidade de internet e receba no Telegram. 

Desenvolvido para uso em Zabbix Server.

Alpine Linux (alpine-4.2.2).

    # apk add --update --no-cache tg curl py-pip speedtest-cli && pip install --upgrade pip && rm -rf /usr/share/man /tmp/* /var/cache/apk/*

    # cd /usr/lib/zabbix/alertscripts/

    # vi speedtest-telegram.sh

    #!/bin/bash
    #
    # José Rodrigues Filho
    #
    # Insere a data.
    date '+Data do teste: %m/%d/%y|%H:%M:%S' > /etc/zabbix/scripts/speedtest.txt 
    # Faz o teste.
    speedtest-cli --share >> /etc/zabbix/scripts/speedtest.txt
    # Captura o link da imagem.
    var="$(cat /etc/zabbix/scripts/speedtest.txt | grep "Share" | cut -c 15-)"
    # Salva a imagem.
    wget $var
    # Renomeia a imagem.
    png="$(cat /etc/zabbix/scripts/speedtest.txt | grep "Share" | cut -c 48-)"
    # Telegram.
    BOT_TOKEN=""
    USER=""
    # Envia a foto
    curl -k -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendPhoto" -F chat_id="${USER}" -F photo="@${png}" > /dev/null
    # deleta a foto.
    rm $png
    #
    exit 0

Salve o arquivo e digite os comandos abaixo.

    # chmod +x speedtest-telegram.sh
    # chown zabbix:zabbix speedtest-telegram.sh

 Execute:

    # sh speedtest-telegram.sh
