## On mininet:
- Previously install maven http://javedmandary.blogspot.com/2016/09/install-maven-339-on-ubuntu.html
- Previously install jdk 11 https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html http://ubuntuhandbook.org/index.php/2018/11/how-to-install-oracle-java-11-in-ubuntu-18-04-18-10/
- Previously install onos-admin https://wiki.onosproject.org/display/ONOS/ONOS+Remote+Admin+Tools (descarga https://drive.google.com/file/d/1E-OqKNFuzSa_iydN04tNKb7kz9ozAb7z/view, `tar xvzf archivo.tar.gz, ejecuta)
- Previously install curl `sudo apt-get install curl`

- Generate the app `mvn archetype:generate -DarchetypeGroupId=org.onosproject -DarchetypeArtifactId=onos-bundle-archetype -DarchetypeVersion=2.2.1-b2` the archetype version might change overtime, you can check the current version on this link https://mvnrepository.com/artifact/org.onosproject/onos-archetypes
- Define value for property 'groupId': : mx.itesm.httpddosdetector           
- Define value for property 'artifactId': : httpddosdetector
- Leave default for version and package, just click enter
- The app should be generated in the folder httpddosdetector

- Add functionality to the app

- Compile the app `mvn clean install`
- Move the app to the container `cat ./httpddosdetector-1.0-SNAPSHOT.oar | sudo docker exec -i 3c6b8d8fddf9 sh -c 'cat > /root/onos/httpddosdetector-1.0-SNAPSHOT.oar'`
- Check if the file is copied by running `ls` inside the container `sudo docker exec -it 3c6b8d8fddf9 /bin/bash`
- Install the onos app `./onos-admin-1.12.1-SNAPSHOT/onos-app 172.17.0.1 install! ./http-ddos-detector/target/httpddosdetector-1.0-SNAPSHOT.oar`
- Activate app `karaf@root > app activate mx.itesm.httpddosdetector`
- check it is activated should see:
    ```
    * id=192, name=mx.itesm.httpddosdetector, version=1.0.SNAPSHOT, origin=ITESM, category=default, description=ONOS detector for denial of service attacks on HTTP, features=[httpddosdetector], featuresRepo=mvn:mx.itesm.httpddosdetector/httpddosdetector/1.0-SNAPSHOT/xml/features, apps=[], permissions=[], url=http://onosproject.org
    ```
    when running `karaf@root > apps -a`
- you can check the logs displaying the installation and activation of the `karaf@root > log:display` app
    ```
    03:05:31.416 INFO [ApplicationManager] Application mx.itesm.httpddosdetector has been installed
    03:05:31.435 INFO [FeaturesServiceImpl] Adding features: httpddosdetector/[1.0.0.SNAPSHOT,1.0.0.SNAPSHOT]
    03:05:33.469 INFO [FeaturesServiceImpl] Changes to perform:
    03:05:33.469 INFO [FeaturesServiceImpl]   Region: root
    03:05:33.470 INFO [FeaturesServiceImpl]     Bundles to install:
    03:05:33.470 INFO [FeaturesServiceImpl]       mvn:mx.itesm.httpddosdetector/httpddosdetector/1.0-SNAPSHOT
    03:05:33.473 INFO [FeaturesServiceImpl] Installing bundles:
    03:05:33.473 INFO [FeaturesServiceImpl]   mvn:mx.itesm.httpddosdetector/httpddosdetector/1.0-SNAPSHOT
    03:05:33.517 INFO [FeaturesServiceImpl] Starting bundles:
    03:05:33.520 INFO [FeaturesServiceImpl]   mx.itesm.httpddosdetector.httpddosdetector/1.0.0.SNAPSHOT
    03:05:33.544 INFO [AppComponent] Started
    03:05:33.547 INFO [FeaturesServiceImpl] Done.
    03:05:33.550 INFO [ApplicationManager] Application mx.itesm.httpddosdetector has been activated
    03:05:33.806 INFO [AppComponent] Reconfigured
    03:06:22.228 INFO [ServerUserAuthService] Session karaf@/172.17.42.1:44528 authenticated
    ```

- run simple server on host `python -m SimpleHTTPServer 80`

# References:
https://wiki.onosproject.org/display/ONOS/Creating+and+deploying+an+ONOS+application
https://www.youtube.com/watch?v=mzQubYhJhro