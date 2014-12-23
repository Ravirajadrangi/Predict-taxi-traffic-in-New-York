Description
==============
This repository hosts code and data for our final project for CS221 (Stanford class on artificial intelligence)
in autumn 2014.

The ability to predict taxi ridership could present valuable insights to city planners and taxi dispatchers in
answering questions such as how to position cabs where they are most needed, how many taxis to dispatch, and how
ridership varies over time. Our project focuses on predicting the number of taxi pickups given a one-hour time
window and a location within New York City. This project concept is inspired by the MIT 2013-2014 Big Data
Challenge, which proposed the same problem for taxicabs in Boston.


Instructions on how to run
===========================

#### Set up local MySQL database
```MySQL
mysql -u root
CREATE DATABASE taxi_pickups;
use taxi_pickups;

# Note: before running any sql script, make sure to edit the source to have
# file paths that are correct on your computer.

# To use our already aggregated data:
source /absolute/path/to/repo/sql/load_aggregated_from_csv.sql;

# Alternatively, to use raw data that you have downloaded from Chris Whong's website
# (http://chriswhong.com/open-data/foil_nyc_taxi/):
source /absolute/path/to/repo/sql/load_raw_from_csv.sql;
source /absolute/path/to/repo/sql/aggregate_pickups.sql;
```

#### Set up remote MySQL database using AWS (optional)
First, set up an AWS instance.

Next, copy local MySQL database table to the AWS instance.
```bash
sudo mysqldump -u root --single-transaction --compress --order-by-primary taxi_pickups \
pickups_aggregated | mysql -h <instance name>.rds.amazonaws.com -P 3306 -u nyc -p taxi_pickups
```

Finally, connect to the remote MySQL instance from your machine.
```bash
mysql -h <instance name>.rds.amazonaws.com -P 3306 -u nyc -p taxi_pickups
```

#### Download Python dependencies using [pip](https://pip.pypa.io/en/latest/)
```bash
sudo pip install -r requirements.txt
```

#### Run the program
```bash
cd src

# Sample usages...
# ... train and test a model.
python taxi_pickups.py --model linear --local --features feature-sets/features1.cfg --verbose

# ... plot a learning curve.
python plot_learning_curve.py --model linear --local --features feature-sets/features1.cfg --verbose

# ... create plots describing the data set (e.g. plot number of pickups by hour of day).
python plot.py --local

# ... submit a job to Stanford's SGE system on the barley machines.
python submit_job_barley.py <model-parameters>

# For more options and details about parameters:
python taxi_pickups.py --help
python plot_learning_curve.py --help
python plot.py --help
```

