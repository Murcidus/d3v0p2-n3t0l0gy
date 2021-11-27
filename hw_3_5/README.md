## Домашнее задание к занятию "3.5. Файловые системы"

**1. Узнайте о [sparse](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB) (разряженных) файлах.**  
Узнал:  
![](img/sc_01.png)

**2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?**  
Не могут.
Жесткая ссылка, по сути - это другое имя того же файла (блока данных).  

![](img/sc_02.png)

**3. Сделайте `vagrant destroy` на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:**  

    ```bash
    Vagrant.configure("2") do |config|
      config.vm.box = "bento/ubuntu-20.04"
      config.vm.provider :virtualbox do |vb|
        lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
        lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
        vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
        vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
      end
    end
    ```
**Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.**  
Создал:  
![](img/sc_03.png)


**4. Используя `fdisk`, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.**  
Создал разделы:  
![](img/sc_04.png)

**5. Используя `sfdisk`, перенесите данную таблицу разделов на второй диск.** 
Перенес таблицу разделов на второй диск:    
![](img/sc_05_1.png)  
![](img/sc_05_2.png)

**6. Соберите `mdadm` RAID1 на паре разделов 2 Гб.**  
Собрал RAID1:  
![](img/sc_06.png)

**7. Соберите `mdadm` RAID0 на второй паре маленьких разделов.**  
Собрал RAID0:  
![](img/sc_07_1.png)  
И сохранил конфигурациюо:  
![](img/sc_07_2.png)  


**8. Создайте 2 независимых PV на получившихся md-устройствах.**  
Создал:    
![](img/sc_08.png)     


**9. Создайте общую volume-group на этих двух PV.**   
Создал:    
![](img/sc_09.png)   

**10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.**   
Создал:   
![](img/sc_10_1.png)    

![](img/sc_10_2.png)  


**11. Создайте `mkfs.ext4` ФС на получившемся LV.**  
Создал:   
![](img/sc_11.png)    

**12. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`.**  
Смонтировал:   
![](img/sc_12.png)    


**13. Поместите туда тестовый файл, например `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.**  
Скачал файл в `/tmp/new`:     
![](img/sc_13.png)    

**14. Прикрепите вывод `lsblk`.**  
![](img/sc_14.png)    


**15. Протестируйте целостность файла:**  

     ```bash
     root@vagrant:~# gzip -t /tmp/new/test.gz
     root@vagrant:~# echo $?
     0
     ```
Протестировал:   
![](img/sc_15.png)   

**16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.**  
Переместил:   
![](img/sc_16.png)   


**17. Сделайте `--fail` на устройство в вашем RAID1 md.**   
Сделал:  
![](img/sc_17.png)     

**18. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.**  
![](img/sc_18.png)   

**19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:**

     ```bash
     root@vagrant:~# gzip -t /tmp/new/test.gz
     root@vagrant:~# echo $?
     0
     ```
Протестировал:   
![](img/sc_19.png)    


**20. Погасите тестовый хост, `vagrant destroy`.**  
Погасил:   
![](img/sc_20.png)    