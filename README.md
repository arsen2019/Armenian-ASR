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


