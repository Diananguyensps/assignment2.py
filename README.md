import csv

def downloadData(file_path):
    """
    Reads the CSV file and returns its content as a list of rows.
    """
    with open(file_path, 'r') as f:
        reader = csv.reader(f)
        data = list(reader)
    return data

def processData(file_content):
    """
    Converts the raw CSV data into a list of dictionaries with id, name, and birthday.
    """
    processed = []
    for row in file_content:
        if len(row) == 3:  # Ensure the row has all columns
            person = {
                "id": int(row[0]),
                "name": row[1],
                "birthday": row[2]
            }
            processed.append(person)
    return processed

def displayPerson(person_id, personData):
    """
    Prints the information of the person with the given ID.
    """
    for person in personData:
        if person["id"] == person_id:
            print(f'ID: {person["id"]}, Name: {person["name"]}, Birthday: {person["birthday"]}')
            return
    print(f"No person found with ID {person_id}")

def main(file_path):
    file_content = downloadData(file_path)
    personData = processData(file_content)
    
    # Example: display all people
    for person in personData:
        displayPerson(person["id"], personData)

# Run the script
if __name__ == "__main__":
    csv_file = "birthdays100.csv"  # CSV file in the same repo
    main(csv_file)
