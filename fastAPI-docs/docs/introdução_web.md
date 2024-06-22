# Introdução ao Uso de Ferramentas Web no Terminal        

## 1. Utilizar localmente no terminal 👨🏽‍💻

O `httpie` é uma ferramenta de linha de comando para fazer requisições HTTP de maneira simples e intuitiva. Ele é utilizado principalmente para testar APIs e interagir com serviços web diretamente do terminal.

```powershell title="$ Shell"
pipx install httpie
```

### 1.1 Chamando um servidor

```powershell title="$ Shell"
http :8000/
```

- Caso não consiga chamar um servidor utilizando o Windows, adicione ao `path`

- Edite o nome `sharm` de acordo com o seu usuário:

```powershell title="$ Shell"
[System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Users\sharm\.local\bin", [System.EnvironmentVariableTarget]::User)
```

- Alternativamente, você pode abrir as "Propriedades do Sistema" na guia "Avançado" com o seguinte comando

```powershell title="$ Shell"
systempropertiesadvanced
```

1. Clique em "Variáveis de Ambiente..." na parte inferior da janela.
2. Encontre a variável "Path" na coluna "Variável", clique em editar.
3. Na janela de edição, clique em "Novo" e adicione C:\Users\nome_do_seu_usuario\.local\bin.
4. Clique em "OK" para fechar todas as janelas.

### 1.2 Vericiando o Path

Para verificar se o caminho foi corretamente configurado:

```powershell title="$ Shell"
echo $env:Path
```

Para exibir cada elemento do Path em linhas separadas:.

```powershell title="$ Shell"
$env:Path -split ';' | ForEach-Object { $_ }
```

## 2 Exibindo o Ip com Python 🐍

Para exibir o IP usando Python, inicie o interpretador interativo digitando:

```powershell title="$ Shell"
python
```

### 2.1 Exibindo o IP com Python

```py title="𓆚 Python"
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.connect(("8.8.8.8", 80))
s.getsockname()[0]
```
