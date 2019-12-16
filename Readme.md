##Description
1.List all MAC addresses and IP addresses of the all interfaces of running VMs.
2.Resolve all IP and MAC conflicts and update the configuration of each VM.
3.Mention the list of hypervisors as the input to your application (its taken as a list of all the hypervisor ip) and find the duplicate MAC and IP addresses of all running VMs.


##Usage
*To run the script on our hypervisor for part 1 & 2 use the q1_1.py
	- python q1_1.py

*To run the script across multiple hypervisors from a single host use the q1_3.py or App2.py

	#With tls certificate.
	- Both q1_3.py and resolve_conflict.py files should be present on the location from where we are accessing the multiple hypervisors envt.
	- The list of hypervisor ips need to be added in the hypervisor_list = [] in the code.
	- paramiko package needs to be installed using 'pip install paramiko'
	- For this to execute sucessfully the tls certificates need to installed on 
	  both the hypervisor and the host, the directory to add the certificates is /etc/pki/CA/ in the hypervisor
	  Reference: https://wiki.libvirt.org/page/TLSCreateCACert
	- python q1_3.py
	
	#Without tls certificate
	- python App2.py
	- Enter the hypervisor ips in the list.





**Steps for installation of the tls certificates on both the hypervisor and hosts:
	  
sudo apt-get install gnutls-bin

Create a template file named as certificate_authority_template.info
umask 277 && certtool --generate-privkey > certificate_authority_key.pem

##Generating a 3072 bit RSA private key...

ls -la certificate_authority_key.pem

certtool --generate-self-signed --template certificate_authority_template.info --load-privkey certificate_authority_key.pem --outfile certificate_authority_certificate.pem

ls -la certificate_authority_certificate.pem

rm certificate_authority_template.info

**Now the Certificate has been created, it needs to be copied to both virtualisation hosts and the administration desktop**
**The default location for the Certificate file on each host is /etc/pki/CA/cacert.pem**

scp -p certificate_authority_certificate.pem someuser@host1:cacert.pem

**login to the specified hypervisor machine**

mv cacert.pem /etc/pki/CA
chmod 444 /etc/pki/CA/cacert.pem
restorecon /etc/pki/CA/cacert.pem


# Version
 - 1.0

#Authors
----
Kashish Singh, Sathwik Kalvakuntla

# License

  - All rights reserved by the owner and NC State University.
  - Usage of the code can be done post approval from the above.




