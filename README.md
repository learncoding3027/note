import os
import shutil

# Source and destination folder paths
source_folder = "path_to_source_folder"
destination_folder = "path_to_destination_folder"

# Function to copy Python files from source to destination
def copy_python_files(src, dst):
    for root, dirs, files in os.walk(src):
        for file in files:
            if file.endswith('.py'):
                source_file_path = os.path.join(root, file)
                relative_path = os.path.relpath(source_file_path, src)
                destination_file_path = os.path.join(dst, relative_path)
                os.makedirs(os.path.dirname(destination_file_path), exist_ok=True)
                shutil.copy(source_file_path, destination_file_path)
                print(f"Copied {source_file_path} to {destination_file_path}")

# Call the function with the source and destination folder paths
copy_python_files(source_folder, destination_folder)







import os
import shutil
import glob

# Source and destination folder paths
source_folder = "path_to_source_folder"
destination_folder = "path_to_destination_folder"

# Function to copy Python files from source to destination
def copy_python_files(src, dst):
    for root, dirs, files in os.walk(src):
        for file in files:
            if file.endswith('.py'):
                source_file_path = os.path.join(root, file)
                relative_path = os.path.relpath(source_file_path, src)
                destination_file_path = os.path.join(dst, relative_path)
                destination_directory = os.path.dirname(destination_file_path)
                os.makedirs(destination_directory, exist_ok=True)
                shutil.copy(source_file_path, destination_file_path)
                print(f"Copied {source_file_path} to {destination_file_path}")

# Call the function with the source and destination folder paths
copy_python_files(source_folder, destination_folder)


