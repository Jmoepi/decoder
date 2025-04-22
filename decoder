import requests
from bs4 import BeautifulSoup

def decode_secret_message_from_text(text):
    # Initialize variables to store data and track grid dimensions
    data = []
    max_x = 0
    max_y = 0

    # Split the input text into lines and remove empty lines
    lines = [line.strip() for line in text.splitlines() if line.strip()]

    # Try to find the start of the actual data (skip headers)
    # Look for three consecutive lines that match: int, str, int
    i = 0
    while i + 2 < len(lines):
        try:
            # Check if the current line is an integer
            int(lines[i])
            # Check if the next line is a string (any value is fine)
            lines[i+1]
            # Check if the third line is an integer
            int(lines[i+2])
            break  # Found the start of the data
        except Exception:
            i += 1  # Move to the next line if the current triplet is invalid
    else:
        # If no valid data is found, print a message and exit the function
        print("\nSecret Message:\n\nNo valid data found")
        return

    # Parse the data in triplets (x, char, y)
    data_lines = lines[i:]  # Start from the detected data lines
    triplets = [data_lines[j:j+3] for j in range(0, len(data_lines), 3)]  # Group lines into triplets


    for triplet in triplets:
        if len(triplet) < 3:
            continue  # Skip incomplete triplets
        try:
            # Extract x, char, and y from the triplet
            x = int(triplet[0])
            char = triplet[1]
            y = int(triplet[2])
        except Exception:
            continue  # Skip invalid triplets
        # Add the parsed data to the list
        data.append((x, y, char))
        # Update the maximum x and y values to determine grid size
        max_x = max(max_x, x)
        max_y = max(max_y, y)

    if not data:
        # If no valid data was parsed, print a message and exit
        print("\nSecret Message:\n\nNo valid data found")
        return

    # Create a grid of empty spaces with dimensions based on max_x and max_y
    rows = max_y + 1
    cols = max_x + 1
    grid = [[' ' for _ in range(cols)] for _ in range(rows)]

    # Fill the grid with characters from the parsed data
    for x, y, char in data:
        if 0 <= y < rows and 0 <= x < cols:  # Ensure coordinates are within bounds
            grid[y][x] = char

    # Print the grid row by row to display the secret message
    print("\nSecret Message:\n")
    for row in grid:
        print(''.join(row))

if __name__ == "__main__":
    # Prompt the user to enter the URL of the Google Doc
    url = input("Enter Google Doc URL: ")
    try:
        # Fetch the content of the URL
        response = requests.get(url)
        response.raise_for_status()  # Raise an error if the request fails
    except Exception as e:
        # Print an error message if the URL cannot be accessed
        print(f"Error accessing document: {e}")
        print("Could not retrieve document content")
        input("\nPress Enter to exit...")
        exit(1)

    # Parse the HTML content of the document
    soup = BeautifulSoup(response.text, "html.parser")
    # Try to find a <pre> tag (often used for preformatted text)
    pre = soup.find("pre")
    if pre:
        # If a <pre> tag is found, extract its text
        text = pre.get_text()
    else:
        # Otherwise, extract all text from the body, separating lines with newlines
        text = soup.body.get_text(separator="\n")

    # Decode the secret message from the extracted text
    decode_secret_message_from_text(text)

    # Wait for the user to press Enter before exiting
    input("\nPress Enter to exit...")
