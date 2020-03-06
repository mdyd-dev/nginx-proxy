In development it allows accessing services in docker-compose with port 80 and 443
Based on https://medium.com/@francoisromain/set-a-local-web-development-environment-with-custom-urls-and-https-3fbe91d2eaf0

Start service

    docker network create nginx-proxy
    dc up -d
    
Add to service environments

    VIRTUAL_HOST: "*.platform.pos"
    VIRTUAL_PORT: 3000
    

Add networks

    networks:
      default:
        external:
          name: nginx-proxy


Generate certs
    
    mkdir certs
    cd certs/
    openssl req -new > platform.ssl.csr
    openssl rsa -in privkey.pem -out platform.cert.key
    openssl x509 -in platform.ssl.csr -out platform.cert.cert -req -signkey platform.cert.key
    cp platform.cert.cert platform.pos.crt
    cp platform.cert.key platform.pos.key
    
Trust certs

    sudo trust anchor --store platform.pos.crt
    sudo trust extract-compat

