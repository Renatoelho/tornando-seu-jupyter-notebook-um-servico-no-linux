# Tornando seu Jupyter Notebook um serviço no Linux

Que tal você ter seu servidor ***Jupyter Notebook*** sempre em execução? Neste tutorial vou ensinar como você transforma um servidor Jupyter notebook em um serviço, que sempre estará em execução em seu computador *Linux* *(Ubuntu 20.04 ou qualquer outra distribuição Linux que contenha o Systemd)* e com isso ter mais tempo para desenvolver seus códigos em ***Python***. 

## Requisitos:

Linux Ubuntu 20.04 ou outra distribuição que contenha o Systemd

Python 3.8 ou superior

## Detalhamento da implementação:

### 1º Passo configurar um ambiente virtual para Jupyter Notebook

- Verifique se o Python3 está instalado:

```bash
python3 -V
```

- Se sim, não será necessário sua instalação, caso contrário instale com o seguinte comando:

```bash
sudo apt install python3.8 python3.8-venv -y
```

- Crie um diretório exclusivo para executar o Jupyter Notebook.

```bash
cd
mkdir -p python/jupyter
```

- Crie um ambiente virtual exclusivo para o Jupyter

```bash
cd python/jupyter
python3 -m venv .venv
source .venv/bin/activate
```

- Atualize o PIP e instale o Jupyter

```bash
pip install -U PIP
pip install jupyter
```

- Subindo o servidor Jupyter

```bash
jupyter notebook
```

- Comando de execução do Jupyter Notebook

```bash
cd /home/<Seu usuário>/python/jupyter/ && source .venv/bin/activate && jupyter notebook > logs-servidor-jupyter.log 2>&1
```

>>***Observação:***<br>*Se tudo estiver OK com o servidor Jupyter vamos para criação do serviço no Linux.*

## Criando o serviço do Jupyter no systemd

- Crie o arquivo: jupyter.service

***Com o seguinte conteúdo:***

```bash
[Unit]
Description=Servidor Jupyter Notebook
After=multi-user.target

[Service]
Type=simple
Restart=always
WorkingDirectory=/home/<Seu usuário>/python/jupyter/
User=<Seu usuário>
ExecStart=/bin/bash -c 'cd /home/<Seu usuário>/python/jupyter/ && source .venv/bin/activate && jupyter notebook > logs-servidor-jupyter.log 2>&1'

[Install]
WantedBy=multi-user.target
```

>>***Observação:***<br>*Antes de salvar e fechar o arquivo verifique as aspas do comando ‘ExecStart’, se estão no padrão do Linux.*

- Salve em: /etc/systemd/system/

### Carregando o Daemon do Systemctl

```bash
sudo systemctl daemon-reload
```

### Ativando o serviço no Systemctl

```bash
sudo systemctl enable jupyter.service
```

### Iniciando o serviço no Systemctl

```bash
sudo systemctl start jupyter.service
sudo systemctl start jupyter
```

### Verificando status do serviço

```bash
systemctl status jupyter
```

***Exemplo:***

![Status Serviço](https://drive.google.com/uc?export=view&id=14S9Qj7I3zoTh5jE_dmkJYOwY4l5nGKB_)

### Parando o serviço no Systemctl

```bash
sudo systemctl stop jupyter
```

### Reiniciando o serviço no Systemctl

```bash
sudo systemctl restart jupyter
```

***Pronto! Agora você vai ter seu servidor Jupyter Notebook sempre em execução.***

#### Acesse em: http://localhost:8888 

>> ***Referências:***<br>[https://docs.jupyter.org/en/latest/](https://docs.jupyter.org/en/latest/)<br>[https://www.freedesktop.org/software/systemd/man/systemd.service.html](https://www.freedesktop.org/software/systemd/man/systemd.service.html)<br>



