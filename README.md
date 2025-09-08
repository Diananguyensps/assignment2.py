import csv
import datetime

def downloadData(file_path):
    with open(file_path, 'r', newline='') as f:
        reader = csv.reader(f)
        data = list(reader)
    return data

def processData(file_content):
    data_dict = {}
    for row in file_content:
        try:
            id = int(row[0])
            name = row[1]
            birthday = datetime.datetime.strptime(row[2], "%d/%m/%Y")
            data_dict[id] = (name, birthday)
        except Exception:
            continue
    return data_dict

def displayPerson(id, personData):
    if id in personData:
        name, birthday = personData[id]
        print(f"Person #{id} is {name} with a birthday of {birthday.strftime('%Y-%m-%d')}")
    else:
        print(f"No person found with ID {id}")

def main():
    csv_file = "birthdays100.csv"
    data = downloadData(csv_file)
    personData = processData(data)
    displayPerson(1, personData)

if __name__ == "__main__":
    main()
