## idea about thread ctl

### run.sh
command=python /tmp/lola/src/main.py -c /tmp/conf/mpki_test_server.conf -s 

### argparse way1
>import argparse
parser = argparse.ArgumentParser(prog='ctl_thread', description='ctl cmd',
formatter_class=argparse.ArgumentDefaultsHelpFormatter)
parser.add_argument("-c", "--cmdconf", type=str, help="issuing file context",
                    default='/tmp/conf/ctl_thread.conf')
### ctl_thread.conf
[issuing file context]
thread_status = stop

### argparse way2
parser.add_argument("-a", "--start", type=str, help="thread status ctl 1",
                        default='start')
parser.add_argument("-o", "--stop", type=str, help="thread status ctl 2",
                        default='stop')




### Invoking argparse
##### way1
    args = parser.parse_args()
    ctl_status = str(args.cmdconf).split('/')[-1]
    cmd = status
    ctl_func = CtlClass(status=ctl_status)
###### way2
    args = parser.parse_args()
    ctl_func = CtlClass(status=args.stop)
                                               

### thread ctl
[python - How to start and stop thread? - Stack Overflow](https://stackoverflow.com/questions/15729498/how-to-start-and-stop-thread)

>import threading
import time

    class Concur(threading.Thread):
    def __init__(self):
        super(Concur, self).__init__()
        self.iterations = 0
        self.daemon = True  # Allow main to exit even if still running.
        self.paused = True  # Start out paused.
        self.state = threading.Condition() 
 
    def run(self):
        self.resume()
        while True:
            with self.state:
                if self.paused:
                    self.state.wait()  # Block execution until notified.
            # Do stuff...
            time.sleep(.1)
            self.iterations += 1

    def pause(self):
        with self.state:
            self.paused = True  # Block self.

    def resume(self):
        with self.state:
            self.paused = False
            self.state.notify()  # Unblock self if waiting.

class Stopwatch(object):
    """ Simple class to measure elapsed times. """
    def start(self):
        """ Establish reference point for elapsed time measurements. """
        self.start_time = time.time()
        return self

    @property
    def elapsed_time(self):
        """ Seconds since started. """
        try:
            return time.time() - self.start_time
        except AttributeError:  # Wasn't explicitly started.
            self.start_time = time.time()
            return 0
            
