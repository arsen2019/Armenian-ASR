# Armenian-ASR
Առաջին հայկական օպեն֊սուրս ձայնի ճանաչման մասին անհրաժեշտ բոլոր տեղեկությունը
Սա կլինի կարճ կուրս տե ինչպես ստեղծել սեփական ASR֊ը Kaldi գործիքի միջոցով։
## Միջավայրի ստեղծումը
Սկզբում անհրաժեշտ է տեղադրել բոլոր անհրաժեշտ փաթեթները(packages) որոնք պետք կգան թե հիմա, թե աշխատանքի հետագա էտապներում։
``` shell
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~$ sudo apt update && sudo apt upgrade
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~$ yes | sudo apt install unzip git-all
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~$ pkgs="wget
g++
make
automake
autoconf
sox
gfortran
libtool
subversion
python2.7
python3.8
zlib1g-dev")
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~$ yes | sudo apt-get install $pkgs
```
Կլոնավորում ենք Kaldi-ն (Պատրաստ եղեք տրամադրել 9ԳԲ հիշողություն) և տեղադրում բոլոր անհրաժեշտ գործիքները: Կախված համակարգչի հնարավորություններից այս պրոցեսը կարող է տևել 2-4 ժամ։
```shell
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~$ git clone https://github.com/kaldi-asr/kaldi.git kaldi --origin upstream
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~$ cd ./kaldi/tools
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/tools$ yes | extras/install_mkl.sh
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/tools$ extras/check_dependencies.sh
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/tools$ make CXX=g++
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/tools$ cd ../src
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/src$ ./configure --shared
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/src$ make depend CXX=g++
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/src$ make CXX=g++



