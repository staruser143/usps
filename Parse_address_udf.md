Certainly! Below is the code that uses the parse_address UDF to parse the address field into separate columns for street, city, state, and zip.

Step-by-Step Implementation

1. Import Necessary Modules

First, import the necessary modules:

from pyspark.sql import SparkSession
from pyspark.sql.functions import udf
from pyspark.sql.types import StructType, StructField, StringType

2. Initialize SparkSession

Create the SparkSession:

spark = SparkSession.builder \
    .appName("AddressParser") \
    .getOrCreate()

3. Define the UDF

Define the parse_address UDF. This UDF will return a tuple containing street, city, state, and zip:

def parse_address(address):
    # Split the address into components
    parts = address.split(',')
    
    # Handle cases where the address might not have all parts
    if len(parts) == 4:
        street = parts[0].strip()
        city = parts[1].strip()
        state_zip = parts[2].strip().split()
        state = state_zip[0]
        zip_code = state_zip[1] if len(state_zip) > 1 else None
    else:
        street, city, state, zip_code = None, None, None, None
    
    return street, city, state, zip_code

# Register the UDF with Spark
parse_address_udf = udf(parse_address, 
                        returnType=StructType([
                            StructField("street", StringType(), True),
                            StructField("city", StringType(), True),
                            StructField("state", StringType(), True),
                            StructField("zip", StringType(), True)
                        ]))

Explanation

parse_address Function: This function splits the address string based on commas. It returns a tuple with four parts: street, city, state, and zip. If the input does not match the expected format, it returns None for each part.

StructType for UDF Return: The return type of the UDF is defined as a StructType, which includes the four fields: street, city, state, and zip.


4. Create a Sample DataFrame

Create a sample DataFrame with a single address field:

data = [("1", "123 Main St, Springfield, IL, 62704"),
        ("2", "456 Elm St, Anytown, NY, 12345")]

df = spark.createDataFrame(data, ["id", "address"])
df.show(truncate=False)

5. Apply the UDF and Create New Columns

Now, apply the UDF to parse the address and create new columns:

# Apply the UDF and select the new columns
df_parsed = df.withColumn("parsed", parse_address_udf(df["address"]))

# Extract the fields from the parsed struct into separate columns
df_final = df_parsed.select("id", "address", 
                            "parsed.street", 
                            "parsed.city", 
                            "parsed.state", 
                            "parsed.zip")

# Show the result
df_final.show(truncate=False)

Explanation

withColumn("parsed", parse_address_udf(df["address"])): This line applies the parse_address UDF to the address column and stores the result in a new column called parsed. The parsed column is a struct with fields street, city, state, and zip.

select("id", "address", "parsed.street", "parsed.city", "parsed.state", "parsed.zip"): This line selects the original id and address columns along with the new columns (street, city, state, zip) extracted from the parsed struct.


6. Review the Output

Finally, you can see the result:

+---+---------------------------------+-----------+-----------+-----+-----+
|id |address                          |street     |city       |state|zip  |
+---+---------------------------------+-----------+-----------+-----+-----+
|1  |123 Main St, Springfield, IL, 62704|123 Main St|Springfield|IL   |62704|
|2  |456 Elm St, Anytown, NY, 12345   |456 Elm St |Anytown    |NY   |12345|
+---+---------------------------------+-----------+-----------+-----+-----+

Summary

This code demonstrates how to:

1. Define a UDF: Parse a complex string (like an address) into multiple components.


2. Apply the UDF: Use the UDF to transform a DataFrame column.


3. Extract and Create New Columns: Extract fields from the UDF’s output and create new DataFrame columns.



This approach is helpful for complex transformations where built-in functions like split() may not be sufficient.

