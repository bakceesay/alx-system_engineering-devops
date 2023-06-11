0x19. Postmortem An ALX - Software Engineering Task
Issues Summary:
June 05, 2023, 1:30 p.m. to 4:45 p.m. (GMT), In
Transactions not posting in Epicor web application.
Transaction arises on the IFMIS system not posting due to post scheduler not responding, all transaction where stop for a good amount of time.

Image description

Impact:
The outage affected transaction processing on the IFMIS system purchase orders where affected, approval process, all check and bank transfer payment were put to a stop for close to a whole day, the posting script always invoke an error message, the scheduler was overwhelmed with queues of transactions stuck on the task agent.
This causes a total shutdown of the Epicor instance (Accounting software) access by MDA’s, about 95% of users in this MDA’s were affected.

Root Cause:
The transaction log files overwhelmed the app server disk space, taking up all the resources on the server hard drive.
Under normal circumstances, these log files are automatically copied to a network area storage on the network, but the script and the batch file triggering the copy got corrupted through a malicious code.

Timeline:

1:30 PM — The issue was first detected by our monitoring system, Nagios which sent out an alert to the technical support team.
2:35 PM — The technical support team attempted to restart the web application server ERP 10, but this did not resolve the issue.
2:40 PM — The team began investigating the issue, assuming it was a problem with the server’s configuration.
3:00 PM — Further investigation revealed that the ERP 10 server was out of disk space due to it been overwhelmed by transaction log files.
3:15 PM — More investigations were done to find out the cause space issue on the servers.
3:45 PM — It was discovered that the script and the batch file that copied transaction log files to Network area storage were corrupted with malicious code. The scripts were not responding.
4:30 PM — System scan was done to remove the malicious code, a backup of the script and log files were deployed, log files were copied to Network area storage, and the restarted.
4:45 PM — The ERP system was back online and fully operational.
Image description

Misleading Investigation/Debugging Paths:

The technical support team initially assumed that the problem was related to the server configuration and invested valuable time investigating. This delayed determining the true cause of the problem.

Incident Escalation:
The technical support team were initially tasked to handle the issue, but when it was discovered that the issue was with the application server, it was escalated to the application support team for further investigation and corrective action.

Resolution:
Scan tools were used to remove the malicious code from the server, a copy of the script and a batch file were deployed, logs move to the network area storage, the server was restarted and became functional and normal operation resumes.

Corrective and Preventative Measures:
The following corrective and preventive measures will be put in place to avoid the occurrence of such issues in the future.

Perform regular scanning of the server for malicious code.
Regular monitoring of server disk space and log movement.
Implement more robust monitoring of server performance and resource usage, particularly disk space. -Improve change management and documentation for both the technical support and application support team, that will include training to better equip staff to handle such incidents in future.
