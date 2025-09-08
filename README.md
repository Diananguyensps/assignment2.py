import argparse
import urllib.request
import logging
import datetime
import csv

def downloadData(url):
    """Downloads the data and returns it as a list of lines"""
    response = urllib.request.urlopen(url)
    data = response.read().decode('utf-8').splitlines()
    return data

def processData(file_content):
    """Processes the CSV data into a dictionary"""
    reader = csv.reader(file_content)
    data_dict = {}
    for line in reader:
        try:
            id = int(line[0])
            name = line[1]
            birthday = datetime.datetime.strptime(line[2], "%m/%d/%Y")
            data_dict[id] = (name, birthday)
        except Exception as e:
            logging.error(f"Error processing line {line}: {e}")
    return data_dict

def displayPerson(id, personData):
    """Prints a person's name and birthday by ID"""
    if id in personData:
        name, birthday = personData[id]
        print(f"Person #{id} is {name} with a birthday of {birthday.strftime('%Y-%m-%d')}")
    else:
        print(f"No user found with ID {id}")

def main(url):
    print(f"Running main with URL = {url}...")
    data = downloadData(url)
    personData = processData(data)
    displayPerson(1, personData)  # Example: show Person #1

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--url", help="URL to the datafile", type=str, required=True)
    args = parser.parse_args()
    main(args.url)
