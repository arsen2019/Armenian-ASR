# Armenian-ASR
This will be your guide through ASR voice recognition via Kaldi
## Install all the necessary tools and packages
First of all, we need to install all the packages and tools that we're going to use in the development process
``` sh
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
For the second we need to set up Kaldi. It may take from 2-4 hours so get patient and enjoy the action.
```sh
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
And finally, we have to download Srilm and install it in our Kaldi directory.
https://hovinh.github.io/blog/2016-04-22-install-srilm-ubuntu/ follow this link to avoid issues. Gentle hint rename your downloaded srilm archive to srilm.tar.gz.

## Creating all the mandatory files
Clone this repository to kaldi/egs.
```sh
git clone git@github.com:arsen2019/Armenian-ASR.git
```
For training Kaldi with custom audio data we will need.
* Audios from several people pronouncing some certain words that we want to train. Audios have to be sorted in folders with names that represent some unique id for that speaker
* wav.scp(path to the audios)
* spk2gender(speaker gender)
* utt2spk(shows who is pronouncing the word)
* text(each audio's text option)
* corpus.txt(All the possible words in ASR)
* lexicon.txt(words with phonemes)
* nonsilence_phones.txt, silence_phones.txt, optional_silence.txt(all the phonemes)
More detailed information about file preparation and more specific explanation for each file you can find here http://kaldi-asr.org/doc/kaldi_for_dummies.html

After completing all the above steps we just run this line
```sh
(base) arsen@arsen-HP-Pavilion-Laptop-15-eh1xxx:~/kaldi/egs/armenian$ bash run.sh

```
And we have trained Kaldi with our custom dataset.

## Useful pieces of advice and links
1. It's mandatory to name all the audios and folders with numbers and letters of the same length without any symbols. Kaldi uses a sorting mechanism wich is sensitive to _,-,., and etc. For a better understanding, in this project I have named folders such as 01M...09F. First 2 digits stands for the number of speaker and M or F is the gender. Audios are named 01M01...09F05 and intuitively first three positions are for the speaker and the last two numbers are for audio. With this pattern, I can have 100 speakers with 100 words and give them unique IDs.
2. Use ./utils/validate_dict_dir.pl and ./utils/fix_data_dir.sh tools, the first is validating the data that you have prepared and gives a warning whether it's not valid for further training. The second is fixing invalid lines after that, you can clearly understand which part of the code is causing the problem.
3. Don't leave empty lines at the end of the files.
4. In case of errors pay attention to the first error that appears in the terminal the further errors may be the result of the first one. Don't try to fix all errors give your priority to the first one.
5. Audios have to be mono and 16KHz otherwise Kaldi won't consider it as a valid data and will give "empty archive" error.
6. https://groups.google.com/g/kaldi-help/ this is a wonderful site for those who will not have ordinary problems.

