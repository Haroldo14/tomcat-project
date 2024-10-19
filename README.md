# tomcat-project
****************************************************************
                        Installation TOMCAT
****************************************************************
## Mise a jour librairies
apt-get update

## Verification installation Java
```bash
java --version

# Pour choisir version si plusieurs installes**
sudo update-alternatives --config java 
```
## telecharger tomcat et l'extraire
```bash
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.31/bin/apache-tomcat-10.1.31.tar.gz
sudo tar -xvzf apache-tomcat-10.1.31.tar.gz
```

## Creation utiisateur specifique avec droit limites
sudo useradd -r -m -U -d /opt/tomcat -s /bin/false tomcat
sudo chown -R tomcat: /opt/tomcat
sudo chmod +x /opt/tomcat/bin/*.sh

## Lancer Tomcat
sudo /opt/tomcat/bin/startup.sh
http://localhost:8080 ***(http://XXX.YYY.ZZZ.222:8080/)** pour y acceder dans mon cas.

***En cas d'erreur: The JAVA_HOME environment variable is not defined correctly...**
## Configurer JAVA_HOME sur la machine hote
nano ~/.bashrc
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH <!-- Puis Ajouter ces lignes en fin de fichier -->

## Configurer JAVA_HOME de TOMCAT
source ~/.bashrc
sudo nano /opt/tomcat/bin/setenv.sh
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
sudo chmod +x /opt/tomcat/bin/setenv.sh
sudo /opt/tomcat/bin/shutdown.sh
sudo /opt/tomcat/bin/startup.sh

## Configuration en tant que service pour etre demarer avec le systeme
sudo nano /etc/systemd/system/tomcat.service

[Unit]
Description=Apache Tomcat Web Application Container
After=network.target
[Service]
Type=forking
User=tomcat
Group=tomcat
Environment="JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh
Restart=on-failure
[Install]
WantedBy=multi-user.target

## Liste commandes redemarrage et activation tomcat
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat

## Ajout des utilisateurs & configurations 
```bash
sudo nano /opt/tomcat/conf/tomcat-users.xml
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
```

## Pour changer port de demarrage de Tomcat
```sydo nano /opt/tomcat/conf/server.xml```

***INSTALLATION JENKINS**
```sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key``` 
```echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]"  https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null```
sudo apt-get updateq
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

