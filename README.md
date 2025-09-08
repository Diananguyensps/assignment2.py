import csv
import datetime

def downloadData(file_path):
    """Reads the CSV file from the local path and returns it as a list of rows"""
    with open(file_path, 'r', newline='') as f:
        reader = csv.reader(f)
        data = list(reader)
    return data

def processData(file_content):
    """Processes CSV rows into a dictionary keyed by ID"""
    data_dict = {}
    for row in file_content:
        try:
            id = int(row[0])
            name = row[1]
            birthday = datetime.datetime.strptime(row[2], "%d/%m/%Y")  # Adjust to CSV format
            data_dict[id] = (name, birthday)
        except Exception:
            continue  # skip bad rows
    return data_dict

def displayPerson(id, personData):
    """Prints a person's name and birthday by ID"""
    if id in personData:
        name, birthday = personData[id]
        print(f"Person #{id} is {name} with a birthday of {birthday.strftime('%Y-%m-%d')}")
    else:
        print(f"No person found with ID {id}")

def main():
    csv_file = "birthdays100.csv"  # Make sure this file is in your repo
    data = downloadData(csv_file)
    personData = processData(data)
    displayPerson(1, personData)  # Example: show person #1

if __name__ == "__main__":
    main()
