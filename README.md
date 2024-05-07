# cross-clock-domains---sync-with-ack
This module contains two clock domains, where the upstream clock domain is faster. A valid_in pulse is passed between clock domains. Busy signal is used for further flow control.
A reset synchronizer is used to asynchronisly assert the reset and de-assert the reset to the applicable clock domain synchronously two clock cycles laters.
