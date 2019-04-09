#!/bin/sh -
clear

#------------------------------
#          Creditos
#   Miner by tpruvot
#   Source by FENIX_LINUX
#   Edit by HumansLothbrok
#------------------------------

bash=$(echo $BASH)


if [ "$bash" = "/bin/bash" ]
then
exit 0
fi

# cada numero representa uma palavra no script, em ordem vem a carteira o email e no final os nucleos 
#
core=$3
email=$2
wallet=$1


# Isso serve para quando a Wallet estiver vazia ele retornar uma mensagem e não dar procedimento a mineração.
#
if [ "$1" = ""  ] 
then
echo "			\033[41;1;37m Minerar XMR  $versao \033[0m "				
sleep
echo "\033[34m Você deve adicionar nessa ordem \033[0m"
echo "sudo sh $0 \033[31m Carteira EMAIL\033[32m Numero de nucleos \033[0m  \n "
echo "\033[34m ao final do script para que possa minerar corretamente \033[0m"
exit 0
fi


# Aqui se encontra a atualizaão do sistema e a instalação dos plugins e download do minerador.
#
echo "CARTEIRA CONFIGURADA : \033[01;32m $wallet\033[0m   "
echo "\033[01;31m	 * Minerar Monero na http://supportxmr.com \033[0m   \n"
echo "\033[44;1;37m Baixando Pacotes....     \033[0m "
sudo apt-get update
sudo apt-get install git -y
sudo apt-get install screen -y
sudo  apt-get install automake autoconf pkg-config libcurl4-openssl-dev libjansson-dev libssl-dev libgmp-dev make g++ -y
sudo rm -r cpuminer-multi 
git clone https://github.com/lucasjones/cpuminer-multi
clear


# Aqui esta a configuração do minerador para script cryptonight
#
echo "\033[44;1;37m Configurando e Compilando Recursos..... \033[0m "
cd cpuminer-multi
./autogen.sh
./configure CFLAGS="-march=native" --with-crypto 
./build.sh
cd cpuminer-multi
clear
sudo mv cpuminer /usr/local/bin/
clear

# Comando iniciador da mineração
#
echo "\033[37;41mSua Mineraçao Foi Iniciada\033[01;0m \n"
cpuminer -a cryptonight -o stratum+tcp://pool.supportxmr.com:3333 -u $wallet -p x:$email -t $core -R 30 -B


#             | | | 
# Screen aqui v v v
# screen desativado, mas ainda sim funciona sem precisa do comando....
# FIM ?
