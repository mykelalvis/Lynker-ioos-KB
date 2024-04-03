```
Zachary Wills - NOAA Affiliate

12:24 PM

~/Desktop/projects/coastal-sandbox/Cloud-Sandbox> cat misc/install_intel.sh 10/27/2023 11:23:58 AM #!/bin/bash # -a --install-dir /save/intel sudo sh ./l_BaseKit_p_2021.3.0.3219.sh -a --install-dir /save/intel sudo sh ./l_HPCKit_p_2021.3.0.3230.sh -a --install-dir /save/intel

~/Desktop/projects/coastal-sandbox/Cloud-Sandbox> cat misc/srweather-setup-notes.txt 10/27/2023 11:24:41 AM git clone [https://github.com/NOAA-EMC/hpc-stack.git](https://github.com/NOAA-EMC/hpc-stack.git) spack load cmake@3.23.1%intel@2021.3.0 sudo yum install Lmod sudo yum install libtiff-devel spack load intel-oneapi-compilers@2021.3.0 spack load intel-oneapi-mpi@2021.3.0%intel@2021.3.0 spack install libjpeg spack install libpng spa

_send_

Send message

Checking who can access file

Sandbox tag up - you're it!
```