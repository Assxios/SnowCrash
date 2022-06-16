HINT:
open the pcap with wireshark and look at the packets

ANSWER:
scp the file thnen open it with wireshark, we see at packet 43 the content is:
"pas word:"
then it is followed by the password written one packet at a time every other packet: (ignore the ACK only look at the PSH/ACK), hexa conveted to ascii for convenience (when possible)
"f t _ w a n d r 7f 7f 7f N D R e l 7f L 0 L"
7f = 127 which is delete control character therefore we can delete the characters affected by them and we get:
ft_waNDReL0L
su flag02 then getflag and we get:
kooda2puivaav1idi4f57q8iq