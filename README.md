import csv
import logging
from datetime import datetime

logging.basicConfig(level=logging.ERROR)

def clean_date(date_str):
    date_str = date_str.replace('*', '/').replace('-', '/').replace('//', '/')
    parts = date_str.split('/')
    if len(parts) != 3 or not all(p.isdigit() for p in parts):
        return None
    return date_str

def processData(file_content):
    personData = []
    reader = csv.reader(file_content)
    next(reader)  # skip header
    for row in reader:
        try:
            id_num = int(row[0])
            date_str = clean_date(row[2])
            if not date_str:
                raise ValueError(f"Invalid date: {row[2]}")
            birthday = datetime.strptime(date_str, "%d/%m/%Y")
            personData.append({"id": id_num, "name": row[1], "birthday": birthday})
        except Exception as e:
            logging.error(f"Error processing line {row}: {e}")
    return personData
