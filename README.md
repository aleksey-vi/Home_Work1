
Установка ПО
Vagrant

Переходим на https://www.vagrantup.com/downloads.html выбираем соответствующую версию. В данном случае Debian 64-bit и версия 2.2.6. Копируем ссылку и в консоли выполняем:

curl -O https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.deb && \
sudo dpkg -i vagrant_2.2.6_x86_64.deb

После успешного окончания будет установлен Vagrant.
Packer

Переходим на https://www.packer.io/downloads.html выбираем соответствующую версию. В данном случае Linux 64-bit и версия 1.4.4. Копируем ссылку и в консоли выполняем:

curl https://releases.hashicorp.com/packer/1.4.4/packer_1.4.4_linux_amd64.zip | \
sudo gzip -d > /usr/local/bin/packer && \
sudo chmod +x /usr/local/bin/packer

После успешного окончания будет установлен Packer.
Kernel update
Клонирование и запуск

Для запуска рабочего виртуального окружения необходимо зайти через браузер в GitHub под своей учетной записью и выполнить fork данного репозитория: https://github.com/dmitry-lyutenko/manual_kernel_update

После этого данный репозиторий необходимо склонировать к себе на рабочую машину. Для этого воспользуемся ранее установленным приложением git, при этом в <user_name> будет имя уже вашего репозитрия:

git clone git@github.com:<user_name>/manual_kernel_update.git

В текущей директории появится папка с именем репозитория. В данном случае manual_kernel_update. Ознакомимся с содержимым:

cd manual_kernel_update
ls -1
manual
packer
Vagrantfile

Здесь:

    manual - директория с данным руководством
    packer - директория со скриптами для packer'а
    Vagrantfile - файл описывающий виртуальную инфраструктуру для Vagrant

Запустим виртуальную машину и залогинимся:

vagrant up
...
==> kernel-update: Importing base box 'centos/7'...
...
==> kernel-update: Booting VM...
...
==> kernel-update: Setting hostname...

vagrant ssh
[vagrant@kernel-update ~]$ uname -r
3.10.0-957.12.2.el7.x86_64

Теперь приступим к обновлению ядра.
kernel update

Подключаем репозиторий, откуда возьмем необходимую версию ядра.

sudo yum install -y http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm

В репозитории есть две версии ядер kernel-ml и kernel-lt. Первая является наиболее свежей стабильной версией, вторая это стабильная версия с длительной поддержкой, но менее свежая, чем первая. В данном случае ядро 5й версии будет в kernel-ml.

Поскольку мы ставим ядро из репозитория, то установка ядра похожа на установку любого другого пакета, но потребует явного включения репозитория при помощи ключа --enablerepo.

Ставим последнее ядро:

sudo yum --enablerepo elrepo-kernel install kernel-ml -y
