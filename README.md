# check_netbackup-storage
Nagios plug-in to check Storage on Netbackup Appliance

check_netbackup-storage is a python2.7/nagios plugin that allow you to check partition capacity, according thresholds defined.
It use SSH connection to connect to the appliance, and cat a predefined df output (from local cron)
<br><br>

    Example of run : 
        check_netbackup-storage.py -H 1.2.3.4 -W 85 -C 95 -U supervision -P supervisionPassword


##Todo before use

#####On the Netbackup appliance

We need a to add a supervision account on the appliance.
This can be done in maintenance mode.

Then, we have to create a bash script to output the DF command
Here is mine :
--
    OutFile=/home/nbusers/dfout.log
    >$OutFile
    df -Pkh | awk '{if (NR!=1) print $5,$6}' | sed 's/%//g'>$OutFile
--

Finally, add a cron job to execute the script (every 5 minutes should be good) 

  */5 * * * * /script_dir/df.sh

<br>
#####On the Nagios host

check_netbackup-storage is a python script that use a very limited number of modules.
Before running the script, you have to install the 'fabric' module, with pip :

    pip install fabric
  
If you use python version before 2.7.9, you must install pip manually :
    https://pip.pypa.io/en/stable/installing/
  
  
<br>  
#####On the script header (optional)

there is a couple of variable that you can define (local directory use to store the SSH stdout, and the file name) 

see the help script for more details

<br>

#####Network issue
Ensure that firewall and other network security tools are opened between Nagios host and Filers
