```python
import zipfile
import os
import tkinter as tk
from tkinter import filedialog, messagebox

def convert_kmz_to_kml(kmz_file_path):
    if not kmz_file_path.lower().endswith('.kmz'):
        print("File yang dipilih bukan file KMZ.")
        return

    kml_file_path = kmz_file_path.replace('.kmz', '.kml')
    
    with zipfile.ZipFile(kmz_file_path, 'r') as kmz:
        for file in kmz.namelist():
            if file.endswith('.kml'):
                with kmz.open(file, 'r') as kml_file:
                    with open(kml_file_path, 'wb') as output_file:
                        output_file.write(kml_file.read())
                print(f"File KML telah diekstrak ke: {kml_file_path}")
                messagebox.showinfo("Sukses", f"File KML telah diekstrak ke: {kml_file_path}")
                return

    print("Tidak ada file KML ditemukan dalam file KMZ.")
    messagebox.showinfo("Gagal", "Tidak ada file KML ditemukan dalam file KMZ.")

def convert_kml_to_kmz(kml_file_path):
    if not kml_file_path.lower().endswith('.kml'):
        print("File yang dipilih bukan file KML.")
        return

    kmz_file_path = kml_file_path.replace('.kml', '.kmz')
    
    with zipfile.ZipFile(kmz_file_path, 'w', zipfile.ZIP_DEFLATED) as kmz:
        kmz.write(kml_file_path, os.path.basename(kml_file_path))
    
    print(f"File KML telah dikonversi ke KMZ: {kmz_file_path}")
    messagebox.showinfo("Sukses", f"File KML telah dikonversi ke KMZ: {kmz_file_path}")

def open_file_dialog():
    root = tk.Tk()
    root.withdraw()  # Sembunyikan jendela utama

    # Tampilkan informasi cara pakai
    messagebox.showinfo("Informasi cara pakai", "Cara pakai Convert KMZ to KML Either ini adalah dengan cara memilih file KML ataupun KMZ.\nJika file yang dipilih adalah KMZ maka akan dikonversikan secara otomatis ke KML, begitu juga sebaliknya.\nFile baru akan disimpan tepat di lokasi file yang dipilih.")

    file_path = filedialog.askopenfilename(
        title="Pilih file KML atau KMZ",
        filetypes=(("KML/KMZ files", "*.kml;*.kmz"), ("All files", "*.*"))
    )

    if not file_path:
        messagebox.showinfo("Info", "Tidak ada file yang dipilih.")
        return

    if file_path.lower().endswith('.kmz'):
        convert_kmz_to_kml(file_path)
    elif file_path.lower().endswith('.kml'):
        convert_kml_to_kmz(file_path)
    else:
        messagebox.showinfo("Info", "File yang dipilih bukan file KML atau KMZ.")

    # Tampilkan informasi pembuat program
    messagebox.showinfo("Info", "Program ini dibuat oleh: Syaiful Wachid > Fiberhome Designer\nLinkedIn Account: Syaiful Wachid\nCP:082230696953")

if __name__ == "__main__":
    open_file_dialog()
```
