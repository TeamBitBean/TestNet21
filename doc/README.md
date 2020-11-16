<a href = "https://www.beancash.org"><img width = "61%" height = "auto" src = "https://img.shields.io/static/v1?style=social&logo=&label=Bean%20Cash&message=TestNet21" alt = "More Than Digital Cash">

</a>

##### ----------Official Testnet implementation of the Bean Cash protocol.----------

[![Discord](https://img.shields.io/discord/547402601885466658.svg?label=live.beancash.org)](https://live.beancash.org/teambean/channels/beta-core)  [![Discord](https://img.shields.io/discord/598173996109791232?color=g&label=BITB%20Hangout)](https://discord.gg/UBAcq3v)  [![Twitter Follow](https://img.shields.io/twitter/follow/BeanCash_BEAN?label=Bean%20Cash&style=social)](https://twitter.com/BeanCash_BEAN)  [![Twitter Follow](https://img.shields.io/twitter/follow/cryptolatino1?label=Cryptolatino&style=social)](https://twitter.com/CryptoLatino1)

  

<img width =600 img src="http://www.beancash.org/downloads/marketing/banners/MrBitBean.png">

  

## "Fast..Simple..Secure..More Than Digital Cash!"

### Bean Cash Specifications

  

* ****Consensus****: Proof of Bean (PoB) & Proof of Work (PoW)

* ****Proof of Work Algorithm****: SHA-256

* ****PoW Block Reward****: 100,000 BITB

* ****PoW Blocks****: 10,000

* ****PoB Block Reward****: 1000 BITB (1 SPROUT)

* ****Target Blocktime****: 1 minute

* ****RPC Port****: 22461 (Testnet: 22463)

* ****Port****: 22460 (Testnet: 22462)

* ****Time to Maturity****: 110 Confirmations

* ****Minimum Time for Beans To Be Elgible to Sprout****: 6 hours

* ****Maximum Supply (over 95+ years)****: 50 Billion BITB

  

The Bean Cash Testnet ([`BITB`](https://www.coingecko.com/en/coins/bean-cash)) brings scalability, speed, and lightening fast transactions! It will be the foundation on how `BITB` will be shaped.

### Development

The following instructions are geared towards developers, intending to contribute to the Bean Cash Testnet. C++11 minimum compiler support required.

## Build the source

  

For prerequisites and detailed build instructions please read the [Installation Instructions](https://github.com/CryptoLatino/TestNet21/blob/master/doc/readme-qt.rst) .

## Docker Image setup

#### * This image runs Ubuntu with Beancashd pre-installed.

  

1. Install Docker

  

`https://www.docker.com`

  

2. Create a Docker Volume

  

`docker volume create Beancash_testy2`

  

3. Get Repository

  

`docker pull cryptolatino/beancash_testy2`

  

4. Run Docker Image and Mount Volume to Container

  

`docker run -it -v Beancash_testy2:/root/.BitBean <YOUR_DIR>/beancash_testy2 bash`

  

5. Configure your Beancash.conf

  

`cd /root/.BitBean`

  

`nano Beancash.conf`

```

testnet=1

rpcuser=Beancashrpc

rpcpassword=ahardtoguesspasswordhere

addnode=5.199.130.20

addnode=157.245.130.170

addnode=199.192.28.124

addnode=178.142.51.17

addnode=216.107.205.190

addnode=173.30.94.73

addnode=216.107.220.54

addnode=216.107.220.59

addnode=216.107.220.57

```

6. Finally go to your home directory and run Beancashd

  

`./Beancashd -printtoconsole`

  

Thats it, your all set! Enjoy running Bean Cash in a docker container.

  

*****_" Don't be shy Visit our_** [**_Official Forum_**](http://www.beancash.org/forum/) **_and get some Test Bean "_*****

## Contribution

  

Thank you for considering to help out with the source code! We welcome contributions

from anyone on the Internet, and are grateful for even the smallest of fixes!

  

If you'd like to contribute to Bean Cash, please fork, fix, commit and send a pull request ,

for the maintainers to review and merge into the main code base. If you wish to submit

more complex changes though, please check up with the core devs first on [our mattermost channel](https://live.beancash.org/teambean/channels/beta-core)

to ensure those changes are in line with the general philosophy of the project and/or get

some early feedback which can make both your efforts much lighter as well as our review

and merge procedures quick and simple.

  

Please make sure your contributions adhere to our coding guidelines:

  

* Pull requests need to be based on and opened against the `master` branch.

* Commit messages should be prefixed with the package(s) they modify.

  

For more details on configuring your environment, managing project dependencies, and

testing procedures please contact us at: [support@beancash.org](mailto:support@beancash.org).

  

Alternatively, if you are a merchant with questions or interested in adopting Bean Cash in your business, you may contact Bean Core via telephone: <a href="tel:+1-406-213-4656">1-406-213-4656</a>

  

---

*****_Copyright (c) 2015-2017 Bean Core, Team Bean,_** [**_bitbean.org_**](https://www.beancash.org)***

<br>

*****_Copyright (c) 2017-2020 Bean Core, Team Bean,_** [**_beancash.org_**](https://www.beancash.org)**_,_** [**_beancash.space_**](https://www.beancash.space)***

---
