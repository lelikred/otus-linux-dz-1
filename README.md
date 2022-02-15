# ДЗ1

## 1. Устанавливаем virtualbox, vagrant, packer

```
curl -O https://download.virtualbox.org/virtualbox/6.1.32/virtualbox-6.1_6.1.32-149290~Ubuntu~eoan_amd64.deb
sudo dpkg -i virtualbox-6.1_6.1.32-149290~Ubuntu~eoan_amd64.deb

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update
sudo apt-get install vagrant
sudo apt-get install packer

```

## 2. Установливаем GIT, создаем репозиторий

```sudo apt install git```

**Создаем пару ssh-ключей по данной инструкции и добавляем на github**

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

**Клонируем репозитории**

```
mkdir /home/user/git_repo
cd /home/user/git_repo

git clone git@github.com:<username>/manual_kernel_update.git
git clone git@github.com:<username>/otus-linux-dz-1.git
```

**Копируем все файлы из директории /manual_kernel_update в /otus-linux-dz-1**  
**Редактируем Vagrantfile - меняем имя образа**

## 3. Создаем образ
```
cd /home/user/git_repo/otus-linux-dz-1/packer
packer fix centos.json > centosfix.json" (версия packer обновилась, необходимо выполнить данную операцию)
```
Редактируем файл **centosfix.json** добавляя в секцию **builders** строчку **"headless": true** , чтобы виртуальная машина загружалась без **графического режима**

```packer build centosfix.json```

На выходе получаем файл **centos-7.7.1908-kernel-5-x86_64-lelikred-Minimal.box**

## 4. Загружаем образ в Vagrant cloud

```
vagrant cloud auth login (вводим имя пользователя и пароль)
vagrant cloud publish --release lelikred/centos-7-5-red 1.1 virtualbox centos-7.7.1908-kernel-5-x86_64-lelikred-Minimal.box
```
