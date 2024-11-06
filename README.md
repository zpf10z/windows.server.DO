Aqui está a tradução do arquivo README.md sem modificar os links:

## Passos de Instalação

### 1. Baixe e Instale o Arquivo Installer
Execute o comando abaixo para baixar o instalador:

```bash
wget https://raw.githubusercontent.com/zpf10z/windows.server.DO/main/windows-server-autoinstaller.sh
```

### 2. Conceda Permissão de Execução ao Arquivo
Após o download, conceda permissão para que o arquivo possa ser executado:

```bash
chmod +x windows-server-autoinstaller.sh
```

### 3. Execute o Instalador
Execute o instalador com o seguinte comando:

```bash
./windows-server-autoinstaller.sh
```

### 4. Execute o QEMU
Após a instalação, execute o QEMU para iniciar o Windows Server. Substitua `xx` pela versão do Windows escolhida (por exemplo, `windows10`):

```bash
qemu-system-x86_64 \
-m 4G \
-cpu host \
-enable-kvm \
-boot order=d \
-drive file=windowsxx.iso,media=cdrom \
-drive file=windowsxx.img,format=raw,if=virtio \
-drive file=virtio-win.iso,media=cdrom \
-device usb-ehci,id=usb,bus=pci.0,addr=0x4 \
-device usb-tablet \
-vnc :0
```

**Nota: Pressione Enter duas vezes para continuar.**

### 5. Acesse via VNC
Depois que o QEMU estiver em execução, siga estas etapas para acessar e configurar o Windows Server:

1. Ative o **Área de Trabalho Remota** nas configurações do Windows Server.
2. Desative o **CTRL+ALT+DEL** em Segurança Local.
3. Defina o Windows Server para **nunca hibernar**.

### 6. Comprimir o Arquivo do Windows Server
Após concluir a configuração, comprima a imagem do Windows Server. Substitua `xxxx` pela versão do Windows escolhida (por exemplo, `windows10`):

```bash
dd if=windowsxxxx.img | gzip -c > windowsxxxx.gz
```

### 7. Instale o Apache
Instale o Apache para servir o arquivo via web:

```bash
apt install apache2
```

### 8. Conceda Acesso do Firewall ao Apache
Permita o acesso do Apache através do firewall:

```bash
sudo ufw allow 'Apache'
```

### 9. Mova o Arquivo do Windows Server para o Local Web
Copie o arquivo do Windows Server compactado para o diretório web do Apache:

```bash
cp windowsxxxx.gz /var/www/html/
```

### 10. Link para Download
Após mover o arquivo, acesse-o pelo endereço IP do seu droplet:

```
http://[IP_Droplet]/windowsxxxx.gz
```

**Exemplo:**
```
http://188.166.190.241/windows10.gz
```

## Executando o Windows Server em um Novo Droplet

Para executar o Windows Server em um novo droplet, use o comando abaixo. Substitua `LINK` pelo link de download do arquivo compactado:

```bash
wget -O- --no-check-certificate LINK | gunzip | dd of=/dev/vda
```

### Nota Importante:
- Certifique-se de substituir o placeholder `xxxx` pela versão correta do Windows.
- Não esqueça de substituir `LINK` pelo URL real do seu arquivo.
