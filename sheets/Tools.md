### **NPM / Yarn**
[Download Node.js/NPM](https://nodejs.org/en/)  
[install-node-js-npm-windows](https://blog.teamtreehouse.com/install-node-js-npm-windows)  
[install Yarn](https://classic.yarnpkg.com/en/docs/install/)  
&nbsp;  
**Or using NVM**  
[NVM for Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)  
[Download NVM Windows Installer](https://github.com/coreybutler/nvm-windows/releases)  
**Note:** nvm-windows runs in an Admin shell. When running *nvm install* or *nvm use* etc., you'll need to start powershell (or CMD) as Administrator to use nvm-windows.

----

### **VSCode + Extensions**  
[Download VSCode](https://code.visualstudio.com/download)  
**Extensions:**  
- vscode-icons  
  (File > Preferences > File Icon Theme > VSCode Icons)  
  Unter File -> Preferences -> Settings -> Extensions -> vscode-icons-configuration  
  Enable "Vsicons Presets: Folder All Default Icon"
- Live Server
- Auto Close Tag / Auto Rename Tag
- (ESLint)

----

### **Git + TortoiseGit**  
[Download GIT](https://git-scm.com/download/win)  
[Download TortoiseGit](https://tortoisegit.org/download/)  
[set-up-git-on-windows-with-tortoisegit](https://articles.assembla.com/en/articles/748191-set-up-git-on-windows-with-tortoisegit)  

----

### **PuTTYgen SSH Key**
[tortoisegit-how-to-create-and-upload-your-public-key-to-github](https://medium.com/chaya-thilakumara/tortoisegit-how-to-create-and-upload-your-public-key-to-github-884b7b619329)  

----

### **GPG Key** 
[Download gpg4win/Kleopatra](https://www.gpg4win.de/download-de.html)  
[github-verified-commits-with-gpg](https://pete.akeo.ie/2018/10/github-verified-commits-with-gpg.html)  

1. GPG Key generieren mit Kleopatra Tool:  
    * Datei -> Neues Schlüsselpaar...  
    * OpenPGP Schlüsselpaar erstellen
    * Public Key exportieren für GitHub etc. (Doppelklick auf Zertifikat im Fenster, im neuen Fenster unten der 'Exportieren'Button)
2. Im lokalen Git Repository in Datei .git/config die Key Id als 'signingkey' verwenden

```bash
[user]
    signingkey = 6CF9AE6A2F07C1D4
[commit]
    gpgsign = true
[gpg]
    program = "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```

----

### **Festplatte partitionieren**  
[windows-10-festplattenpartitionen-erstellen-und-bearbeiten](https://www.pctipp.ch/praxis/windows/windows-10-festplattenpartitionen-erstellen-und-bearbeiten-1990664.html)  