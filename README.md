# Python project for replication administration of a Mongo replica set.
# Classification (U)

# Description:
  Adminstrates a Mongo replica set, this includes checking the master status, checking membership status, checking replication time lag between primary and secondaries databases, and displaying the replication configuration status.


###  This README file is broken down into the following sections:
  * Features
  * Prerequisites
    - FIPS Environment
  * Installation
  * Configuration
  * Program Help Function
  * Testing
    - Unit


# Features
  * Check replication lag.
  * Show current members in replication set.
  * Show priority for members in replication set.
  * Check status of replication for members in replica set and print errors.
  * Check status of replication for members in replica set and print all.
  * Ability to sent output to standard out, file, and/or insert results into a Mongo database.

# Prerequisites:

  * List of Linux packages that need to be installed on the server.
    - git
    - python-pip

  * Local class/library dependencies within the program structure.
    - lib/arg_parser
    - lib/gen_libs
    - mongo_lib/mongo_class
    - mongo_lib/mongo_libs

  * FIPS Environment
    If operating in a FIPS 104-2 environment, this package will require at least a minimum of pymongo==3.8.0 or better.  It will also require a manual change to the auth.py module in the pymongo package.  See below for changes to auth.py.
    - Locate the auth.py file python installed packages on the system in the pymongo package directory.
    - Edit the file and locate the \_password_digest function.
    - In the \_password_digest function there is an line that should match: "md5hash = hashlib.md5()".  Change it to "md5hash = hashlib.md5(usedforsecurity=False)".
    - Lastly, it will require the configuration file entry auth_mech to be set to: SCRAM-SHA-1 or SCRAM-SHA-256.


# Installation:

Install these programs using git.
  * Replace **{Python_Project}** with the baseline path of the python program.

```
umask 022
cd {Python_Project}
git clone git@github.com:deepcoder42/mongo-rep-admin.git
```

Install/upgrade system modules.

```
cd mongo-rep-admin
sudo bash
umask 022
pip install -r requirements.txt --upgrade --system
exit
```

Install supporting classes and libraries.

```
pip install -r requirements-python-lib.txt --target lib --system
pip install -r requirements-mongo-lib.txt --target mongo_lib --system
pip install -r requirements-python-lib.txt --target mongo_lib/lib --system
```

# Configuration:

Create Mongodb configuration file.

Make the appropriate change to the environment.
  * Make the appropriate changes to connect to a Mongo database.
    - user = "USER"
    - japd = "PWORD"
    - host = "HOST_IP"
    - name = "HOSTNAME"
    - port = 27017
    - conf_file = None
    - auth = True
    - auth_db = "admin"
    - auth_mech = "SCRAM-SHA-1"
    - use_arg = True
    - use_uri = False

  * Notes for auth_mech configuration entry:
    - NOTE 1:  SCRAM-SHA-256 only works for Mongodb 4.0 and better.
    - NOTE 2:  FIPS 140-2 environment requires SCRAM-SHA-1 or SCRAM-SHA-256.
    - NOTE 3:  MONGODB-CR is not supported in Mongodb 4.0 and better.

  * If connecting to a Mongo replica set.  By default set to None to represent not connecting to replica set.
    - repset = "REPLICA_SET_NAME"
    - repset_hosts = "HOST_1:PORT, HOST_2:PORT, ..."
    - db_auth = "AUTHENTICATION_DATABASE"

```
cd config
cp mongo.py.TEMPLATE mongo.py
vim mongo.py
chmod 600 mongo.py
```

If inserting the results into a different Mongo database then create another mongo configuration file and use this file with the -m option.

Make the appropriate change to the environment.
  * Make the appropriate changes to connect to a Mongo database.
    - user = "USER"
    - japd = "PSWORD"
    - host = "HOST_IP"
    - name = "HOSTNAME"
    - port = 27017
    - conf_file = None
    - auth = True
    - auth_db = "admin"
    - auth_mech = "SCRAM-SHA-1"
    - use_arg = True
    - use_uri = False

  * Notes for auth_mech configuration entry:
    - NOTE 1:  SCRAM-SHA-256 only works for Mongodb 4.0 and better.
    - NOTE 2:  FIPS 140-2 environment requires SCRAM-SHA-1 or SCRAM-SHA-256.
    - NOTE 3:  MONGODB-CR is not supported in Mongodb 4.0 and better.

  * If connecting to a Mongo replica set.  By default set to None to represent not connecting to replica set.
    - repset = "REPLICA_SET_NAME"
    - repset_hosts = "HOST_1:PORT, HOST_2:PORT, ..."
    - db_auth = "AUTHENTICATION_DATABASE"

```
cp mongo.py.TEMPLATE mongo_insert.py
vim mongo_insert.py
chmod 600 mongo_insert.py
```


# Program Help Function:

  The program has a -h (Help option) that will show display an usage message.  The help message will usually consist of a description, usage, arugments to the program, example, notes about the program, and any known bugs not yet fixed.  To run the help command:
  * Replace **{Python_Project}** with the baseline path of the python program.

```
{Python_Project}mongo-rep-admin/mongo-rep-admin.py -h
```


# Testing:

# Unit Testing:

### Installation:

Install these programs using git.
  * Replace **{Python_Project}** with the baseline path of the python program.
  * Replace **{Branch_Name}** with the name of the Git branch being tested.  See Git Merge Request.

```
umask 022
cd {Python_Project}
git clone --branch {Branch_Name} git@github.com:deepcoder42/mongo-rep-admin.git
```

Install/upgrade system modules.

```
cd mongo-rep-admin
sudo bash
umask 022
pip install -r requirements.txt --upgrade --system
exit
```

Install supporting classes and libraries.

```
pip install -r requirements-python-lib.txt --target lib --system
pip install -r requirements-mongo-lib.txt --target mongo_lib --system
pip install -r requirements-python-lib.txt --target mongo_lib/lib --system
```


### Testing:
  * Replace **{Python_Project}** with the baseline path of the python program.

```
cd {Python_Project}/mongo-rep-admin
test/unit/mongo_rep_admin/unit_test_run.sh
```

### Code Coverage:
```
cd {Python_Project}/mongo-rep-admin
test/unit/mongo_rep_admin/code_coverage.sh
```

