# Vagrant, VirtualBox, Ansible & Pimcore for EHL

Vagrant kan användas för att skapa en virtuella maskin för EHL DEV-miljö lokalt i datorn som ska likna EHLs TEST och PROD miljö (server-side).

* `Vagrant` - Vagrant är ett verktyg för att bygga och hantera miljöer för virtuella maskiner i ett enda arbetsflöde. [Mer om Vagrant](https://www.vagrantup.com/).
  
* `VirtualBox` - VirtualBox är en universal virtualizer för x86-hårdvara, riktad mot server, skrivbord och inbäddad användning. Vagrant levereras med support out of the box för VirtualBox. [Mer om VertualBox](https://www.virtualbox.org/).

* `Ansible` - Ansible är ett open source community-projekt sponsrat av Red Hat, det är det enklaste sättet att automatisera IT. [Mer om Ansible](https://www.ansible.com/).

* `Pimcore` - Open Source Digital Experience Platform: MDM/PIM, CDP, DAM, CMS/UX & eCommerce. [Mer om Pimcore](https://pimcore.com/).

## Progress
 _**Vagrant**_ med hjälp av _**VirtualBox**_ skapar en virtuell maskin (server) lokalt i våran dator. _**Ansible**_ som installeras och körs av Vagrant ta hand om alla program/paket som behövs (Ubuntu, Apache, MySQL, PHP ...) för att skapa en DEV-miljö som är anpassat för _**Pimcore**_ och som liknar EHLs TEST och PROD miljö.

## Komma igång
> _**OBS!** Detta installation är baserade för MAC OS X_ anvämdare.

1. Installera **VertualBox** och **Vagrant**:
    * Installera den senaste versionen av **VertualBox** genom att gå in på [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads) och välj `OS X hosts`.

      > _❕ Kontrollera att inget behöver accepteras under datorns System Performances -> Security & Privacy -> General._
      
    * Installera den senaste versionen av **Vagrant** genom att gå in på [Download Vagrant](https://www.vagrantup.com/downloads) och välja `MAC OS X`.

    > _❕ **MÅSTE!!!** Nu måste vi starta om datorn._

2. Hämta *vagrant-mappen* från gitlab:
    ```bash
    cd Applications/ehl/pimcore-x
    git clone git@git.lu.se:ehl/pimcore-x/vagrant.git
    cd vagrant
    ```

3. Ladda ner senaste **prod-data-dump-for-dev-XXXXXXXX** zip-filen från [EHL Box](https://lu.app.box.com/folder/7118153617).

4. Hämta **source-mappen** från gitlab:
    ```bash
    cd Applications/ehl/pimcore-x/vagrant/project
    git clone git@git.lu.se:ehl/pimcore-x/source.git ehl
    ```
    * Öppna **prod-data-dump-for-dev-XXXXXXXX** filen som vi laddade ner från EHL box.
    * Kopiera(replace) hela `var`, `web` och `app/config/local` mappar från `data-dump filen` till `Applications/ehl/pimcore-x/vagrant/project/ehl`

5. Redigera `hosts` i MAC
   ```bash
   sudo vim /etc/hosts
   i # (i = insert) på tangentbordet  för att kunna skriva/redigera text i vim
   ```
   * Ta bort alla gamla EHL hosts och ersätt med följande:
   ```yaml
   192.168.56.21 dev.publicera.ehl.lu.se
   192.168.56.21 dev.ehl.lu.se
   192.168.56.21 dev.lusem.lu.se
   192.168.56.21 dev.har.lu.se
   192.168.56.21 dev.fek.lu.se
   192.168.56.21 dev.ekh.lu.se
   192.168.56.21 dev.nek.lu.se
   192.168.56.21 dev.stat.lu.se
   192.168.56.21 dev.staff.lusem.lu.se
   192.168.56.21 dev.lusax.ehl.lu.se
   192.168.56.21 dev.entrepreneur.lu.se
   192.168.56.21 dev.ed.lu.se
   192.168.56.21 dev.kefu.se
   192.168.56.21 dev.ics.lu.se
   192.168.56.21 dev.handel.lu.se
   ```
   * Spara och avsluta som följande:
   ```bash
   esc # på tangentbordet för att avsluta (insert) mode
   :wq # för att spara och avsluta
   Enter ⏎
   ```

6. ### Bygga DEV-miljön för EHL med Vagrant:
   > *❕ Det kan ta ca 5-15 min.*
    ```bash
   cd Applications/ehl/pimcore-x/vagrant
   vagrant up
    ```
    > *❕ Det ska INTE komma upp något error/felmeddelande, annars ska alla fixas först innan vi kan fortsätta.*

7. Läsa in databas-dumpen som vi sparade:
   * Ta bort den gamla known_hosts:
     * öppna en ny terminal
     * $ `rm ~/.ssh/known_hosts`
   * Öppna Sequel Pro i din dator och koppla till MySQL databasen i genom följande Konfigurationer:
     * Välj `SSH`
     * Name = dev-vagrant-pimcore
     * MySQL Host = 127.0.0.1
     * Username = pimcore
     * Password = pimcore
     * Database = pimcore
     * SSH Host = 192.168.56.20
     * SSH User = vagrant
     * SSH Password = ~/Applications/ehl/pimcore-x/vagrant/.vagrant/machines/default/virtualbox/private_key
     * Spara/Save
     * Connect
     * Det ska komma upp en fråga
       * Svara `yes` sen `Enter ⏎`
     * File -> import -> från `data-dump-filen DATABASE_NAME.sql`
     > *❕ Det kan ta ca 5-15 min.*
   
8. SSH till den nya Vagrant-maskin som vi precis byggde:
   ```bash
   cd Applications/ehl/pimcore-x/vagrant
   vagrant ssh
   cd /var/www/project/ehl
   sudo -u www-data composer install --no-plugins
   ```
   > *❕ Det kan ta ca 5-15 min.*

   > *❕ Det ska INTE komma upp något error/felmeddelande, annars ska alla fixas först innan vi kan fortsätta.*

9. Testa:
   * Alla sidor fungerar och visas ut
   * Synka Lucat/lucris
   * Skapa nya sidor
   * Redigera sidor och spara/publisera
   * Och allt annat som är viktigt för oss att vara fungerande

## Vagrant viktigaste Kommando
    
Nästan all interaktion med Vagrant sker via kommando-gränssnittet.

```text
- Start vagrant: `vagrant up`
- Stäng vagrant: `vagrant suspend`

- Ssh:a in i vagrant: `vagrant ssh`
- Logga ut från Ssh: `logout` 
```
Alla Vagrant kommando hittar man under [Command-Line Interface](https://www.vagrantup.com/docs/cli).
