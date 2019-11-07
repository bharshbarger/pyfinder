#!/usr/bin/env python

import sched, time, os, sys, subprocess, time, re
import gps_poll 


class Scanner(object):

    def __init__(self):
        self.iwApDict = {}
        self.chan = ''
        self.security = ''
        self.essid = ''
        self.bssid = ''    
        self.encryption = ''
        self.signal = ''
        self.wireless_interface = ''
    

    def signal_handler(self, signal, frame):
        print('You pressed Ctrl+C! Exiting...')
        sys.exit(0)


    def monitor_mode(self):
        

    def cmdargs(self):

        parser = argparse.ArgumentParser()
        parser.add_argument('-i', '--interface', nargs=1, metavar='wlan0', help='Specify wlan interface')
        #parser.add_argument('-s', '--ssid', nargs=1, metavar='foo', help='Specify SSID to filter on')

        args = parser.parse_args()
    
        print('Started at: %s' % (time.strftime("%d/%m/%Y - %H:%M:%S"))) #append to log


        if args.interface is None:
            self.wireless_interface = 'wlan0'
        else:
            self.wireless_interface =  args.interface

        #check dependencies...iwlist? different?

    def scan(self):
        # Create iwlist sub process.
        p1 = subprocess.Popen(['/bin/bash', '-c','iwlist %s scan' % \
            (self.wireless_interface)], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
        # Display stdout and decode as utf-8
        iwlistOutput = p1.stdout.read().decode('utf-8')
        lines = iwlistOutput.split('\n')

        # Iterate lines
        for line in lines:
            # Strip weirdness
            line = line.strip()
            if 'Address:' in line: 
                self.bssid = str(':'.join(line.split(':')[1:7]))
            if 'Channel:' in line: 
                self.chan= str(line.split(':')[1])
            if 'Signal level' in line: 
                self.signal=str(line.split('=')[2])
            if 'ESSID:' in line:
                self.essid = str(line.split(':')[1])
            # Determines if Encryption is enable, if not sets the AUTH value to OPEN.
            if 'Encryption key:' in line:
                self.encryption = str(line.split(':')[1])
            if self.encryption == "on":
                if 'Authentication' in line: 
                    self.security= str(line.split(':')[1])
            else:
                self.security= ' OPEN'
            # Populate dictionary
            self.iwApDict[self.bssid]=(self.chan, self.signal, self.security, str(self.essid.replace("\x00",' ')))
        self.output()

    def gps(self):


    def output(self):
        # Display Output to end User
        print(' %-20s %-4s %-6s %-7s %s' %\
             ('BSSID', 'CH', 'PWR', 'AUTH', 'ESSID'))
        print(' %-20s %-4s %-6s %-7s %s' % \
             ('-----------------', '--', '---', '----', '----------------'))
        for key in self.iwApDict.items()[1:]:
            mac=key[0].replace(' ','') # Clean up leading whitespace with replace
            vals=key[1]
            ch, pwr, auth, essid = vals
            print(' %-20s %-4s %-6s %-7s %s'%\
            (mac, ch, pwr.replace(' dBm',''), auth, essid))
        print('\n')

def main():

    run = Scanner()
    run.gps()

if __name__ == '__main__':
    main()
