# Armenian-ASR
Առաջին հայկական open-source ձայնի ճանաչման մասին անհրաժեշտ ամբողջ տեղեկությունը։
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
```
Եվ վերջապես ներբեռնում ենք SRILM-ը և տեղադրում համակարգչի վրա։
https://hovinh.github.io/blog/2016-04-22-install-srilm-ubuntu/ հետևեք այս հղմանը և հաջողությամբ կտեղադրեք այն միայն ներբեռնելուց հետո վերանվանեք արխիվը srilm.tar.gz

## Ֆայլերի նախապատրաստում
Կլոնավորեք այս repo֊ն kaldi/egs -ում
```shell
git clone git@github.com:arsen2019/Armenian-ASR.git
```
Սեփական աուդիոյով Kaldi֊ն սովորեցնելու համար անհրաժեշտ է՝
* Մի քանի մարդու կողմից կրկնվող բառերի ձայնագրություններ, որոնք դասավորված կլինեն պապկեքով՝ ըստ արտասանողի
* wav.scp(ճանապարհը դեպի ձայնագրություններ)
* spk2gender(արտասանողների սեռը)
* utt2spk(ցույց է տալիս թե նաղադասությունը ով է արտասանում)
* text(յուրաքանչյուր ձայնագրության տեքստային տարբերակը)
* corpus.txt(ASR֊ում հանդիպող բոլոր բառերը)
* lexicon.txt(բառերը ֆոնետիկ վերլուծությամբ)
* nonsilence_phones.txt, silence_phones.txt, optional_silence.txt(հանդիպող ֆոնեմները)
Ավելի մանրամասն ամեն ֆայլի և նրա տեղակայման մասին կարող եք գտնել ՝ http://kaldi-asr.org/doc/kaldi_for_dummies.html կայքում։

Բոլոր ֆայլերը պատրաստելուց հետո ուղակի պետք է ՝
```bash
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/egs/armenian$ bash run.sh

```

##Օգտակար խորհուրդներ և հղումներ

1. Կարևոր է ձայնագրությունները և պապկեքը անվանել միայն թվերի և տառերի օգնությամբ։Այս օրինակում օրինակ 01M անվանված է առաջին արտասանողը, իսկ 01M0(1-5) իր արտասանաց 5 բառերը։Այսպես ավելի հեշտ է բաժանել ձայնագրությունները, քանզի առաջին երկու թիվը արտասանողին է նկարագրում, տառը(M կամ F)՝ սեռը, վերջին երկու թիվը՝ արտահայտությունը։
2. Օգտվել ./utils/validate_dict_dir.pl և ./utils/fix_data_dir.sh գորցիքներից, որոնցից առաջինը ստուգում է արդյոք ճիշտ են ստեղծված և տեղակայված անհրաճեշտ ֆայլերը, իսկ երկրորդը ուղղում է պրոբլեմ առաջացնող կոդի մասը՝ ջնջելով այն։Ջնջելուց հետո արդեն պարզ է դառնում թե կոդի որ մասը փոխելու անհրաժեշտություն կա։
3. Ֆայլերի վերջում դատարկ տողի առկայությունը բերում է սխալի։
4. Տերմինալում առաջացող սխալների դեպքում ուշադրություն դարձրեք տերմինալի առաջին տողերում առաջացող սխալներին, վերջում արտատպված սխալներում հիմնականում հետևանք են և կանհետան առաջին արտատպված սխալը վերացնելուց հետո։
5. https://groups.google.com/g/kaldi-help/ այս հղմամբ կգտնեք ձեր շատ հարցերի պատասխանները։

