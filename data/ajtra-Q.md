# L1.- Floating pragma
Contracts should be deployed using the same compiler version/flags with which they have been tested. Locking the floating pragma, i.e. by not using ^ in pragma solidity ^0.8.0, ensures that contracts do not accidentally get deployed using an older compiler version with unfixed bugs.

# L2.- Outdated pragma
The actual is 0.8.16 and the code is using ^0.8.0 what means it's possible to use an older version

