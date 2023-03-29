# gifmaker

Certainly! Here's a complete guide to setting up and running the application, including installing necessary libraries and organizing your files:

Install the required libraries:

bash
```pip install Pillow```
Create a new folder named "GIFCreator" for your project. Inside the "GIFCreator" folder, create another folder named "images" where you'll store your PNG files.

Inside the "GIFCreator" folder, create a Python file named gif_creator.py and paste the following code:

python

```import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import csv

def read_csv_file(csv_file):
    layers_data = []
    with open(csv_file, 'r') as file:
        csv_reader = csv.reader(file)
        for row in csv_reader:
            layers_data.append(row)
    return layers_data

def create_gif_from_layers(image_folder, layers_data, output_gif):
    frames = []
    for layers in layers_data:
        base_image = None
        for layer in layers:
            if base_image is None:
                base_image = Image.open(f"{image_folder}/{layer}.png")
            else:
                overlay = Image.open(f"{image_folder}/{layer}.png")
                base_image.paste(overlay, (0, 0), overlay)
        frames.append(base_image)

    frames[0].save(output_gif, format='GIF', append_images=frames[1:], save_all=True, duration=400, loop=0)

def browse_csv():
    file_path = filedialog.askopenfilename(filetypes=[("CSV Files", "*.csv")])
    if file_path:
        csv_entry.delete(0, tk.END)
        csv_entry.insert(tk.END, file_path)

def browse_image_folder():
    folder_path = filedialog.askdirectory()
    if folder_path:
        image_folder_entry.delete(0, tk.END)
        image_folder_entry.insert(tk.END, folder_path)

def browse_output_gif():
    file_path = filedialog.asksaveasfilename(defaultextension=".gif", filetypes=[("GIF Files", "*.gif")])
    if file_path:
        output_gif_entry.delete(0, tk.END)
        output_gif_entry.insert(tk.END, file_path)

def create_gif():
    csv_file = csv_entry.get()
    image_folder = image_folder_entry.get()
    output_gif = output_gif_entry.get()

    layers_data = read_csv_file(csv_file)
    create_gif_from_layers(image_folder, layers_data, output_gif)
    tk.messagebox.showinfo("Success", "GIF created successfully!")

# GUI setup
root = tk.Tk()
root.title("GIF Creator")

# CSV file selection
csv_label = tk.Label(root, text="CSV File:")
csv_label.grid(row=0, column=0, padx=10, pady=10, sticky="e")
csv_entry = tk.Entry(root, width=40)
csv_entry.grid(row=0, column=1)
csv_button = tk.Button(root, text="Browse...", command=browse_csv)
csv_button.grid(row=0, column=2)

# Image folder selection
image_folder_label = tk.Label(root, text="Image Folder:")
image_folder_label.grid(row=1, column=0, padx=10, pady=10, sticky="e")
image_folder_entry = tk.Entry(root, width=40)
image_folder_entry.grid(row=1, column=1)
image_folder_button = tk.Button(root, text="Browse...", command=browse_image_folder)
image_folder_button.grid(row=1, column=2)

# Output GIF selection
output_gif_label = tk.Label(root, text="Output GIF:")
output_gif_label.grid(row=2, column=0, padx=10, pady=10, sticky="e")
output_gif_entry = tk.Entry(root, width=40)
output_gif_entry.grid(row=2, column=1)
output_gif_button = tk.Button(root, text="Browse...", command=browse_output_gif)
output_gif_button.grid(row=2, column=2)

# Create GIF button
create_gif_button = tk.Button(root, text="Create GIF", command=create_gif)
create_gif_button.grid(row=3, column=1, pady=10)

root.mainloop()
```
Place the PNG files inside the "images" folder. Make sure the names of the PNG files correspond to the names in the CSV file.

Create a CSV file (layers_order.csv) in the "GIFCreator" folder with your desired layer combinations. Here's a sample content for the CSV file:

arduino

```background,face1,eyes1,mouth1
background,face1,eyes1_v2,mouth1
background,face1,eyes2,mouth1
background,face1,eyes2_v2,mouth
```
...
To run the application, open a terminal/command prompt, navigate to the "GIFCreator" folder, and execute the following command:
bash

```python gif_creator.py```
This command will start the GUI for your GIF creator application. Use the "Browse..." buttons to select the CSV file, the image folder, and the output GIF file. Click on "Create GIF" to create your GIF with the specified settings.
