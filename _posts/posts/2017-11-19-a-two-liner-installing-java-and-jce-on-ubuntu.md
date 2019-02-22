---
layout: post
category: post
title: A two-liner for installing Java and JCE
date: 2017-11-19 20:06
excerpt: "Life can be so easy ;)"
tags: [Java, JDK, installer, JCE, Ubuntu]
comments: true
---

# Installing Java JDK 7/8 and JCE policy

I've heard installing the Java Cryptography Extension (JCE) requires some hands on - not on Ubuntu ;).

```
# Source: http://askubuntu.com/a/55960, section "The easy way"
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
# Java 9
sudo apt-get install oracle-java9-installer
# Java 8
sudo apt-get install oracle-java8-installer

# also install JCE: http://askubuntu.com/a/780347
sudo apt install oracle-java9-unlimited-jce-policy
sudo apt install oracle-java7-unlimited-jce-policy
```

And don't forget setting your JAVA_HOME:

```
# source: http://www.wikihow.com/Set-Up-Your-Java_Home-Path-in-Ubuntu
vi ~/.bashrc
# add at the end in ~/.bashrc:
JAVA_HOME=/usr/lib/jvm/java-8-oracle
export JAVA_HOME
PATH=$PATH:$JAVA_HOME
export PATH
```

That might be also interesting for setting your env vars permanently: https://help.ubuntu.com/community/EnvironmentVariables#Persistent_environment_variables

