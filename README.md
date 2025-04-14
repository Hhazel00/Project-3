# CIS-117 Lab4
# This script splits countries into regional CSV files from country_full.csv. The code writes separate files like Europe.csv, Asia.csv, etc.
# Hazel Juarez
# Group 3, Partner: Lizbet Caballero Zarate
# Collaboration: My partner worked on reading and spliting the data from the original csv. I tested the script and added exception blocks.


# Run this cell to mount your Google Drive
from google.colab import drive
drive.mount('/content/gdrive', force_remount=True)
%cd /content/gdrive/MyDrive/Colab Notebooks/Project

from google.colab import files
uploaded = files.upload()

import pandas as pd
import os


def split_by_region(file_path):
  """Creates different files by region"""
  try:
        df = pd.read_csv(file_path)

        if 'name' not in df.columns or 'region' not in df.columns:
            raise ValueError("CSV must contain 'name' and 'region' columns.")

        regions = df['region'].dropna().unique()

        for region in regions:
            region_df = df[df['region'] == region][['name', 'region']]
            safe_region = "".join(c for c in region if c.isalnum() or c in (" ", "_")).strip().replace(" ", "_")
            file_name = f"{safe_region}.csv"
            if os.path.exists(file_name):
                print(f"File already exists: {file_name}")
                continue
            region_df.to_csv(file_name, index=False)
            print(f" Created: {file_name}")

  except FileNotFoundError:
    print(f"Error: File not found")
  except PermissionError:
    print(f"Error: Permission denied for file: {file_path}")
  except ValueError:
    print(f"Error: Value error. Inappropriate value")
  except IOError:
        print(f"I/O Error: input/output error")
  except Exception as e:
    print(f" Error: {e}")


split_by_region("country_full.csv")
