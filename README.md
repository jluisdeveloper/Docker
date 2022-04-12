# Configuraciones de Docker para distintos frameworks
## Instalar Docker

Si se tiene alguna version de docker abandonada la removemos.
```bash
  sudo apt-get remove docker docker-engine docker.io containerd runc
```

Instalamos los paquetes necesario.
```bash
  sudo apt-get update
  sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

Agregamos el repositorio oficial
```bash
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Actualizamos los repositorios
```bash
sudo apt  update
```

Instalamos los paquetes
```bash
sudo apt install docker-ce docker-ce-cli containerd.io
```
## Instalar Docker-compose

Descargamos el archivo binario
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Se le da permisos a la descarga
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
