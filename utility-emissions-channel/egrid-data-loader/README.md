# egrid-data-loader

This project imports the Data from https://www.epa.gov/sites/production/files/2020-01/egrid2018_all_files.zip into Amazon DynamoDB.

# Requirements

1. Install the dependencies with `npm`::

   \$ npm install

# Setup AWS credentials

### Running Locally

1. If running locally, the only config change is to set AWS_ENDPOINT in aws-config.js to http://localhost:8001

2. After following the seeding instructions under "Running the Code", set AWS_ENDPOINT back to "http://localdynamodb:8001"

### Running Remotely

1. If running remotely, set AWS_ENDPOINT in aws-config.js to your respective aws dynamodb endpoint.

# Running the Code

Make sure to setup the AWS credentials in `chaincode/node/lib/aws-config.js`::
exports.AWS_ACCESS_KEY_ID = 'your_access_key';
exports.AWS_SECRET_ACCESS_KEY = 'your_secret_key';

Initialize the database::

    $ node index.js initdb

Download and extract the data from https://www.epa.gov/sites/production/files/2020-01/egrid2018_all_files.zip, for example::

    $ wget https://www.epa.gov/sites/production/files/2020-01/egrid2018_all_files.zip
    $ unzip egrid2018_all_files.zip

Load utility emssions data from the XLSX files, for now two different sheets are supported::

    $ node index.js load_utility_emissions eGRID2018_Data_v2.xlsx NRL18
    $ node index.js load_utility_emissions eGRID2018_Data_v2.xlsx ST18

Download the utility identifiers from https://www.eia.gov/electricity/data/eia861/ Unzip and load utility lookup data from the XLSX file Utility_Data_2019_Data_Early_Release.xlsx ::

    $ node index.js load_utility_identifiers Utility_Data_2019.xlsx

See the data that was loaded::

    $ node index.js list

Query for emssions factor for a given utility from its utility number and year, for example::

    $ node index.js get_emmissions_factor 34 2018

    $ node index.js get_emmissions_factor 11208 2018

Query for CO2 emssions factor for a given utility given the usage, for example::

    $ node index.js get_co2_emissions 34 2018 1500

    $ node index.js get_co2_emissions 11208 2018 3000 MWh KG
