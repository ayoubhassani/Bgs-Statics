### 1st step :  Create 2 key pairs SHA1withRSA
#### requirement :  
[Download and install KeyStoreExplorer lastVersion.](http://keystore-explorer.org/downloads.html)
#### Creating Keys :  
1. Click New Key Store 

![Demarrage IIS Manager](https://github.com/ayoubhassani/Bgs-Statics/blob/master/Dtr3/2.png?raw=true)

2. Select JKS Forlmat 

![Demarrage IIS Manager](https://github.com/ayoubhassani/Bgs-Statics/blob/master/Dtr3/3.png?raw=true)

3.  Select RSA with desired size 

![Demarrage IIS Manager](https://github.com/ayoubhassani/Bgs-Statics/blob/master/Dtr3/4.png?raw=true)

4. Select V3 and the SHA-1 With RSA algorythm

![Demarrage IIS Manager](https://github.com/ayoubhassani/Bgs-Statics/blob/master/Dtr3/5.png?raw=true)

5. after creating key Pair export certificate chaine as folowing

![Demarrage IIS Manager](https://github.com/ayoubhassani/Bgs-Statics/blob/master/Dtr3/6.png?raw=true)

At this step ok the configuration we have two jks and two .cer
 * one jks and the cert exported from the other are for theBgs Portal the segonds are for Dtr3 
 * Still have to configure dtr and the portal to make it work 



### 2nd Step Configuring decrypt.jar coté DTR3  `decrypt.properties`

open or create a file named decrypt.properties in dtr inetpub\wwwroot\dtr\ldom directory this file willl contain the following

~~~~~~text
debug=0
cddoc.keyStorePath=c:/CDDOC/dtrSSO/keys/Cds.jks
cddoc.keyStorePassword=123456

cddoc.keyStoreSectionName=cds
cddoc.keyStoreSectionPassword=123456

Client.publicKeyCertFile=c:/CDDOC/dtrSSO/keys/bgs.cer

~~~~~~


### 3 th Step  Configiuration Dtr3 `config.ini` : 
open C:\inetpub\wwwroot\dtr\admin\app\config\cddoc.ini

~~~~~~text

[sso]
ssoActivated="0|LDOM"
ssoEncoding="none"
ssoEncodingWithIdExterne="0"
ssoActiveDirectoryDomaine=""
ssoSeedControle="1"
ssoDomaineUrl=""
ssoPathJavaInc4IVTK="C:/CDDOC/dtrSSO/decrypt.jar"
ssoDeltaSecondesGranted4IVTK="0"

~~~~~~


the Bgs Portal Jar is preconfigured to work with a directory named `sso` next to the jar file this directory has to contain the jks file `.jks` and the dtr certificate export `.cer`


### php code to encrypt a login

~~~~~~php

 $handle = popen("java -jar $jar $login", 'r');
                $objResult = '';
                while (!feof($handle))
                {
                    $objResult .= fgets($handle);
                }

pclose($handle);
// the result will be $objResult // false if there is a problem or ivth=`cryptedChain1`&ivsn=`cryptedChain2`

~~~~~~


### PHP code to decrypt a ivsn and ivtk 

~~~~~~php

                $handle = popen("java -jar $jar $ivtk $ivsn", 'r');
                $objResult = '';
                while (!feof($handle))
                {
                    $objResult .= fgets($handle);
                }
                pclose($handle);
//the user Login is in $objResult

~~~~~~

