EXP 5 : 
1. LOGIN BYPASS : * google sql payload script git hub link or open https://github.com/payloadbox/sql-injection-payload-list
                * go intruder
                * exploit
                * MySQL
                * mysql-injection-login-bypass.txt (Intruder/exploit/MySQL/mysql-injection-login-bypass.txt)
                * that are the scripts
                * go to http://testphp.vulnweb.com/login.php
                * take any script from GitHub and place it in login username box and enter a space  and enter any password


2. SQLMAP : * Go to http://testphp.vulnweb.com/listproducts.php?cat=1
            * copy that url 
            * sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --dump in terminal
            * copy the location paste in files

3. BURPSUIT : *copy the url http://testphp.vulnweb.com/listproducts.php?cat=1
              * open burp suit
              *  Go to Target -> open browser -> paste url -> send to intruder
              * https://github.com/payloadbox/sql-injection-payload-list -> intruder -> detect -> MySQL -> MySQL.txt (Intruder/detect/MySQL)and copy that command
              * paste on payload config before that hit add&&
              * snipper attack -> start

EXP 3 : HYDRA
1. sudo systemctl start vsftpd
2. create a passlist
3. hydra -l username -p passlist ftp://ipaddress

EXP ZAP : 
         * open ZAP 
         * Open Firefox -> Settings -> Search Proxy
         * You will see Network Settings click the Settings of that 
         * Change to Manual proxy configuration 
         * HTTP Proxy : 127.0.0.1 
         * Port : 8080
         * click okay.
         * open new tab on Firefox and https://sjcetpalai.ac.in/ 
         * open ZAP and in the left side you will see site option in that You see https://sjcetpalai.ac.in/ (This passive scan).
         * In ZAP, Right click on https://sjcetpalai.ac.in/ -> Attack -> Active Scan/ Spider. 

EXP Recon-ng:
             * open termainal and paste the commands
             * recon-ng
             * workspaces set msn
             * workspaces create msn
             * modules load recon/domains-hosts/brute_hosts , if this command shows invalid module name error the type : marketplace update
               marketplace install recon/domains-hosts/brute_hosts , and after installing again type : modules load recon/domains-hosts/brute_hosts
             * options set SOURCE msn.com
             * run (this command will run the module)
             * show hosts ( shows the collected data)
             * copy edit (export the collecetd data)

wpscan --url "URL" --api-token=token

 
