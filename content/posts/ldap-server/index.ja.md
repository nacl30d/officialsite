---
title: "LDAPサーバの構築"
date: 2019-08-30T14:40:40+09:00
slug:  "ldap-server"
tags: ["develop", "ldap", "server"]
draft: false
---


研究室で扱うサーバが増えて毎回ユーザを作成するのが面倒になったので，LDAPを導入することにした．  
内容的には先人たちのエントリのリメイクだが，単なる操作ログではなく意味がわかるようにしたつもり．  

環境
===

Centos 7
---

```
$ cat /etc/redhat-release
CentOS Linux release 7.6.1810 (Core)
```

LDAPとは
===

このサイトがわかりやすい
 [LDAPのざっくり説明（LDAPとはなにか？）](https://yoshinorin.net/2018/10/24/about-ldap/)  

インストール
===

このサイトが解説が詳しい（英語）
[Step-by-Step Tutorial: Install and Configure OpenLDAP in CentOS 7 Linux](https://www.golinuxcloud.com/install-and-configure-openldap-centos-7-linux/)  

補完的にこのサイトも参考にした
[OpenLDAP構築 まとめ](https://www.tanchallenge-glory40.com/ldap_command/)  

必要なパッケージのインストール
---

```.sh
$ yum -y install openldap openldap-servers
```

Firewallに穴をあける
---

```.sh
$ firewall-cmd --add-service=ldap --parmanent
$ firewall-cmd --reload
```

`firewall-cmd --list-all`でチェックできる  

LDAPデータベースの設定（チューニング）
---

```.sh
$ cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
```

`ls -l /var/lib/ldap/DB_CONFIG`とかでチェックできる  
とりあえずデフォルトのままで良いと思う．  

Serviceの起動
---

```.sh
$ systemctl start slapd
$ systemctl enable slapd
```

`systemctl status slapd`でチェックできる  


作業用ディレクトリの作成
---

CentOS 6以降では，OLC (On-Line Configuration)が推奨されている（以前はslapd.confに直接書き込んでいたらしい）[^1]  
OLCでは作業用ldifファイルをいくつか作成することになるので，そのためのディレクトリを作る  

```.sh
$ mkdir ldif && $_
```

ベースエントリの設定
---

```.sh
$ cat << EOF > init.ldif
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=foo,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=admin,dc=foo,dc=com

dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0} to * by dn.base="gidNumber=0+uidNumber=0,cn=external,cn=auth" read by dn.base="cn=admin,dc=foo,dc=com" read by * none

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {slappasswdコマンド実行結果}
EOF

ldapmodify -Y EXTERNAL -H ldapi:/// -f init.ldif
```

`ldapsearch -Y EXTERNAL -H ldapi:/// -b cn=config olcDatabase=\*`でチェックできる  

### スキーマの設定

unixユーザを定義するために必要になる  

```.sh
$ ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
$ ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
$ ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```

設定
===

オブジェクトの作成（階層の定義）
---

```.sh
$ cat << EOF > base.ldif
dn: dc=foo,dc=com
objectClass: dcObject
objectClass: organization
dc: foo
o: foo bar Inc.

dn: ou=users,dc=foo,dc=com
objectClass: organizationalUnit
ou: users

dn: ou=groups,dc=foo,dc=com
objectClass: organizationalUnit
ou: groups
EOF

$ ldapadd -f base.ldif -WD cn=admin,dc=foo,dc=com
```

`ldapxxx` コマンドは-fでファイルを指定，-Dで実行するユーザのdnを指定，-Wでパスワードアリ  

グループの作成
---

```.sh
$ cat << EOF > economist.ldif
dn: cn=philosopher,ou=groups,dc=foo,dc=com
cn: economist
objectClass: posixGroup
gidNumber: 3001

dn: cn=economist,ou=groups,dc=foo,dc=com
cn: economist
objectClass: posixGroup
gidNumber: 3002
EOF

ldapadd -f economist.ldif -WD cn=admin,dc=foo,dc=com

```

ユーザの作成
---

```.sh
$ cat << EOF > user.ldif
dn: cn=adam,ou=users,dc=foo,dc=com
cn: adam
sn: Smith
objectClass: inetOrgPerson
objectClass: posixAccount
uidNumber: 1001
gidNumber: 3001
homeDirectory: /home/adam
loginShell: /bin/bash
uid: adam
userPassword: {SSHA}Lql641a/m57MMEbNKmCxHIgztaXPKlyV
EOF

$ ldapadd -f user.ldif -WD cn=admin,dc=foo,dc=com
```

サブグープに所属する [^2]
---

```.sh
$ cat << EOF > subgroup.ldif
dn: cn=economist,ou=groups,dc=foo,dc=com
changetype: modify
add: memberUid
memberUid: adam
EOF

$ ldapmodify -f subgroup.ldif -WD cn=admin,dc=foo,dc=com
```


[^1]: [CentOS7.0でOpenLDAP構築](https://qiita.com/kazukikudo/items/703d6e6664e13882fa0b)  
[^2]: [LDAPのグループへのユーザの追加](https://docs.oracle.com/cd/E77565_01/E54669/html/ol7-s12-auth.html)  
