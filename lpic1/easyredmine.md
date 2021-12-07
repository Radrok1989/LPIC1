
```sql
 create user 'redmine'@'localhost' identified with sha256_password by '8QSEDWTVf3cV'
grant all privileges on  redmine.*  to 'redmine'@'localhost';
```

```zsh
https_proxy=http://wsa.terreactive.ch:3128 http_proxy=http://wsa.terreactive.ch:3128 bundle install
```

```ruby
source "https://rubygems.org"
#source "https://dis.terreactive.ch/rubygems-proxy/"

gem "bundler"
gem "redmine-installer"
gem "unicorn"
```

```dockerfile
FROM ruby:2.5.1
  
WORKDIR /build

COPY Gemfile /build/

ENV https_proxy wsa0.terreactive.ch:3128
ENV http_proxy  wsa0.terreactive.ch:3128

RUN wget --no-proxy http://dis.terreactive.ch/files/ca-ta.pem && \
    #A unneeded, we do not use tA servers:
    mkdir -p /usr/local/share/ca-certficates/ && \
    cp ca-ta.pem /usr/local/share/ca-certficates/ && \
    update-ca-certificates

RUN cd /build && \
    bundle install
```

```bash
docker build -t easyredmine-deps .
docker run -it -v $PWD/gems:/gems easyredmine-deps /bin/bash
```

```bash
cp -a /usr/local/bundle/cache/* /gems/
cp -a /usr/local/lib/ruby/gems/2.5.0/cache/* /gems/ruby/
find /usr/local/bundle/extensions/x86_64-linux/2.5.0/ -name "*.so"
(find /usr/local/bundle/gems/  /usr/local/bundle/extensions/x86_64-linux/2.5.0/ -name "*.so" -exec ls -l {} \; | sort) > /gems/solibs/BOM.txt
```

```bash
tar czf ezrmgems.tgz gems/
```

On dis, latest_dev-dis manuell konfigurieren:

```bash
pab3genupdate -e -a tac-lw14.fp -- ruby2.5-dev

```

... (UPD)

```
apt install ruby2.5-dev
```






No UPD on Lab system, juste use dis as SW src:
```
aptitude install postgresql-10 apache2 ruby2.5 ruby-bundler git` # git erst für redmine command nötig, passenger really needed???
# not done: /usr/lib/postgresql/10/bin/pg_ctl -D /var/lib/postgresql/10/main -l logfile start
pab3swsrc latest_open-dev-dis
aptitude install bzip2 imagemagick patch libtool automake g++ autoconf bison build-essential make zlib1g-dev libssl-dev libyaml-dev libffi-dev uuid-dev libreadline-dev libapache2-mod-fcgid libapache2-mod-authnz-jwt libapache2-mod-auth-mellon
Gem-Datei:
---8<---
source "http://dis.terreactive.ch/rubygems-proxy"
gem "unicorn"
gem "redmine-installer"

---8<---
Gemfile Datei erzeugt imter /root/ und bundle install ausgeführt
   Korrekturen wharscheinlich wegen pab3 restriktives umask:
chmod a+rx /usr/local/bin/redmine
chmod -R go+rX /var/lib/gems/2.5.0/
chown -R redmine: /var/redmine_gem/current
chmod -R go+rX /var/redmine_gem/
redmine install als redmine User ausführen, nicht root
```

```sql
psql -c "create database redmine"; psql -c "create user redmine with encrypted password '$SQL_USER_PW'; grant all privileges on database redmine to redmine;  
```
```bash
docker run -v `pwd`:/src -it ezredmine /bin/bash
```
passwd root 
```bash
su - redmine
```

```bash
passwd redmine
redmine install
#start install redmine until fail
cd tmp
vi lustigezahlen/Gemfile
#change to
source `http://dis.terreactive.ch/rubygems-proxy`
#try again

gde@gde:~$ cat /etc/postgresql/10/main/conf.d/redmine.conf
listen_addresses = '0.0.0.0'
gde@gde:~$ tail -1 /etc/postgresql/10/main/pg_hba.conf
host redmine redmine 0.0.0.0/0 trust #trust is just for testing
sudo service postgresql restart

docker commit -a "Carlo und Guillaume" -m "Install Redmine FTW!" boring_euclid ezredmine-installed
```
**exec bundle exec puma as user redmine**
```bash
bundle exec puma -C /home/easy/puma.rb -e production #exec in /var/redmine_gen/current

G::ConnectionBad: FATAL:  no pg_hba.conf entry for host "172.17.0.2", user "redmine", database "redmine", SSL on
FATAL:  no pg_hba.conf entry for host "172.17.0.2", user "redmine", database "redmine", SSL off


